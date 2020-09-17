# Gazebo Actor

This repository demonstrates the usage of actor and related plugins in Gazebo.  
Move, animate, interact with robot with actor now!  

## Assumption

- You are running ROS Melodic
- You are running Gazebo Version 9
- You are on a Linux machine (e.g. Ubuntu18.04)

## Moving Actor Only

Follow the sdf format below to add an actor in Gazebo world.  
Note that animated models have no collision model nor does it interact with gravity.  
This example is taken from [servicesim_competition](https://github.com/osrf/servicesim/blob/master/servicesim_competition/worlds/follow_actor_demo.world#L32).  
For more detail please refer to official Gazebo [page](http://gazebosim.org/tutorials?tut=actor&cat=build_robot).  

```xml
<actor name="number5">
    <!-- Actor visual model -->
    <include>
        <uri>model://number5</uri>
    </include>

    <!-- Actor motion -->
    <script>
        <loop>true</loop>
        <auto_start>true</auto_start>
            <trajectory id="0" type="square">
                <waypoint>
                    <time>0.0</time>
                    <pose>-3 -3 1 0 0 0</pose>
                </waypoint>
                <waypoint>
                    <time>10.0</time>
                    <pose>-3 3 1 0 0 0</pose>
                </waypoint>
                <waypoint>
                    <time>20.0</time>
                    <pose>3 3 1 0 0 0</pose>
                </waypoint>
                <waypoint>
                    <time>30.0</time>
                    <pose>3 -3 1 0 0 0</pose>
                </waypoint>
                <waypoint>
                    <time>40.0</time>
                    <pose>-3 -3 1 0 0 0</pose>
                </waypoint>
            </trajectory>
    </script>
</actor>
```

## Moving Actor with Animation

Follow the sdf format below to animate an actor in Gazebo world.  
This example is take from [servicesim_competition](https://github.com/osrf/servicesim/blob/master/servicesim_competition/worlds/service.world#L41168).  
For more detail please refer to official Gazebo [page](http://gazebosim.org/tutorials?tut=actor&cat=build_robot).  

```xml
<actor name="human_94763">

  <!-- Starting pose, nice for when the world is reset -->
  <pose>
    12.1254
    5.385326
    1.0
    1.570796
    -0.0
    3.141593
  </pose>

  <!-- Actor visual model -->
  <skin>
    <filename>model://actor/meshes/SKIN_man_green_shirt.dae</filename>
  </skin>

  <!-- Actor animation -->
  <animation name="animation">
    <filename>model://actor/meshes/ANIMATION_walking.dae</filename>
    <interpolate_x>true</interpolate_x>
  </animation>

  <!-- Actor motion -->
  <script>
    <loop>true</loop>
    <auto_start>true</auto_start>
        <trajectory id="0" type="square">
            <waypoint>
                <time>0.0</time>
                <pose>-3 -3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>10.0</time>
                <pose>-3 3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>20.0</time>
                <pose>3 3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>30.0</time>
                <pose>3 -3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>40.0</time>
                <pose>-3 -3 1 0 0 0</pose>
            </waypoint>
        </trajectory>
    </script>
</actor>
```

## Actor with Plugins

Follow the sdf format below to animate an actor with plugins in Gazebo world.  
The focus of the tutorial will be placed on human actors, therefore, I will only explain plugins that are related to human actors.  
All the plugins can be found in [service_competition](https://github.com/osrf/servicesim/tree/master/servicesim_competition).  
For AutoActorPlugin, it can be found [here](https://github.com/BruceChanJianLe/gazebo-plugin-autonomous-actor).  

### Attach Collision Model to Actor (Plugin)

Follow the sdf format below to add the related plugin to your actor.  
This plugin will add a static model to your moving actor.  
Here a box collision model is attached to the human actor.  

```xml
<!-- Create a static collision model -->
<model name="human_94763_collision_model">
  <pose>0 0 -100 0 0 0</pose>
  <static>true</static>
  <link name="link">
    <collision name="link">
      <pose>0 -0.18 0.05 0 -1.5707963267948966 0</pose>
      <geometry>
        <box>
          <size>0.44 1.62 0.60</size>
        </box>
      </geometry>
    </collision>
  </link>
</model>

<!-- Create your actor -->
<actor name="human_94763">

  <!-- Starting pose, nice for when the world is reset -->
  <pose>
    12.1254
    5.385326
    1.0
    1.570796
    -0.0
    3.141593
  </pose>

  <!-- Actor visual model -->
  <skin>
    <filename>model://actor/meshes/SKIN_man_green_shirt.dae</filename>
  </skin>

  <!-- Actor animation -->
  <animation name="animation">
    <filename>model://actor/meshes/ANIMATION_walking.dae</filename>
    <interpolate_x>true</interpolate_x>
  </animation>

  <!-- Actor motion -->
  <script>
    <loop>true</loop>
    <auto_start>true</auto_start>
        <trajectory id="0" type="square">
            <waypoint>
                <time>0.0</time>
                <pose>-3 -3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>10.0</time>
                <pose>-3 3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>20.0</time>
                <pose>3 3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>30.0</time>
                <pose>3 -3 1 0 0 0</pose>
            </waypoint>
            <waypoint>
                <time>40.0</time>
                <pose>-3 -3 1 0 0 0</pose>
            </waypoint>
        </trajectory>
    </script>

    <!-- Actor Plugin -->
    <!-- Enable collisions -->
    <plugin name="attach_model" filename="libAttachModelPlugin.so">
      <link>
        <link_name>human_94763_pose</link_name>
        <model>
          <model_name>human_94763_collision_model</model_name>
        </model>
      </link>
    </plugin>

</actor>
```


## References
- https://github.com/onlytailei/gym_ped_sim
