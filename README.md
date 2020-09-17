# Gazebo Actor

This repository demonstrates the usage of actor and related plugins in Gazebo.  
Move, animate, interact with robot with actor now!  

## Table of Content

- [Assumption](#Assumption)
- [Moving Actor Only](#Moving-Actor-Only)
- [Moving Actor with Animation](#Moving-Actor-with-Animation)
- [Actor with Plugins](#Actor-with-Plugins)
    - [Attach Collision Model to Actor (Plugin)](#Attach-Collision-Model-to-Actor-(Plugin))
    - [Attach Detail Collision Model to Actor (Plugin)](#Attach-Detail-Collision-Model-to-Actor-(Plugin))
    - [Trajectory for Actor (Plugin)](#Trajectory-for-Actor-(Plugin))
    - [Autonomous Actor (Plugin)](#Autonomous-Actor-(Plugin))
    - [Trajectory and Collision Attach to Actor (Multiple Plugins)](#Trajectory-and-Collision-Attach-to-Actor-(Multiple-Plugins))

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

### Attach Detail Collision Model to Actor (Plugin)

Follow the sdf format below to add the related plugin to your actor.  
This plugin will add a group of linked model to your moving actor.  

```xml
<actor name="human_61866">

    <!-- Starting pose, nice for when the world is reset -->
    <pose>
        -0.46
        20.8
        0.0
        0.0
        0.0
        1.18
    </pose>

    <!-- Actor visual model -->
    <skin>
        <filename>model://actor/meshes/SKIN_man_green_shirt.dae</filename>
    </skin>

    <!-- Actor animation -->
    <animation name="animation">
        <filename>model://actor/meshes/ANIMATION_standing.dae</filename>
    </animation>

    <!-- Need one waypoint to idle at -->
    <script>
        <trajectory id='0' type='animation'>
        <waypoint>
            <time>100</time>
            <pose>
                -0.46
                20.8
                0.0
                0.0
                0.0
                1.18
            </pose>
        </waypoint>
        </trajectory>
    </script>

    <!-- Actor plugin -->
    <!-- Enable collisions -->
    <plugin name="actor_collisions_plugin" filename="libCollisionActorPlugin.so">
    <scaling collision="LHipJoint_LeftUpLeg_collision" scale="
        0.01
        0.001
        0.001
    "/>
    <scaling collision="LeftUpLeg_LeftLeg_collision" scale="
        8.0
        8.0
        1.0
    "/>
    <scaling collision="LeftLeg_LeftFoot_collision" scale="
        8.0
        8.0
        1.0
    "/>
    <scaling collision="LeftFoot_LeftToeBase_collision" scale="
        4.0
        4.0
        1.5
    "/>
    <scaling collision="RHipJoint_RightUpLeg_collision" scale="
        0.01
        0.001
        0.001
    "/>
    <scaling collision="RightUpLeg_RightLeg_collision" scale="
        8.0
        8.0
        1.0
    "/>
    <scaling collision="RightLeg_RightFoot_collision" scale="
        8.0
        8.0
        1.0
    "/>
    <scaling collision="RightFoot_RightToeBase_collision" scale="
        4.0
        4.0
        1.5
    "/>
    <scaling collision="LowerBack_Spine_collision" scale="
        12.0
        20.0
        5.0
    " pose="0.05 0 0 0 -0.2 0"/>
    <scaling collision="Spine_Spine1_collision" scale="
        0.01
        0.001
        0.001
    "/>
    <scaling collision="Neck_Neck1_collision" scale="
        0.01
        0.001
        0.001
    "/>
    <scaling collision="Neck1_Head_collision" scale="
        5.0
        5.0
        3.0
    "/>
    <scaling collision="LeftShoulder_LeftArm_collision" scale="
        0.01
        0.001
        0.001
    "/>
    <scaling collision="LeftArm_LeftForeArm_collision" scale="
        5.0
        5.0
        1.0
    "/>
    <scaling collision="LeftForeArm_LeftHand_collision" scale="
        5.0
        5.0
        1.0
    "/>
    <scaling collision="LeftFingerBase_LeftHandIndex1_collision" scale="
        4.0
        4.0
        3.0
    "/>
    <scaling collision="RightShoulder_RightArm_collision" scale="
        0.01
        0.001
        0.001
    "/>
    <scaling collision="RightArm_RightForeArm_collision" scale="
        5.0
        5.0
        1.0
    "/>
    <scaling collision="RightForeArm_RightHand_collision" scale="
        5.0
        5.0
        1.0
    "/>
    <scaling collision="RightFingerBase_RightHandIndex1_collision" scale="
        4.0
        4.0
        3.0
    "/>
    </plugin>

</actor>
```

### Trajectory for Actor (Plugin)

Follow the sdf format below to add trajectory plugin for your actor.  
This plugin will move your actor according to your specify target and will stop moving when your robot is blocking its way.  
Please specify your robot name at <obstacle> element.  

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

  <!-- Actor plugin -->
  <!-- Actor motion -->
  <plugin name="trajectory" filename="libTrajectoryActorPlugin.so">
    
      <target>
        12.1254
        5.385326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        10.3737
        3.89311
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        7.96763
        4.71297
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        6.41362
        5.11439
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        6.32939
        8.46126
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        7.37067
        9.849683
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        10.5193
        9.81355
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        12.1611
        8.39257
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        13.1988
        5.185326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.704
        5.185326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.846853
        7.988438
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        16.0257
        8.12129
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        16.195299
        1.99335
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.1699
        1.91747
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.176
        5.385326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    

    <velocity>1.0516294306711416</velocity>
    <obstacle_margin>1.5</obstacle_margin>
    <obstacle>my_robot_name</obstacle>

  </plugin>

</actor>
```

### Autonomous Actor (Plugin)

Please visit this [repository](https://github.com/BruceChanJianLe/gazebo-plugin-autonomous-actor) for more information.  

### Trajectory and Collision Attach to Actor (Multiple Plugins)

Follow the sdf format to add multiple plugins to your moving actor.  
Here we will be adding the box collision model to a trajectory actor.  

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

  <!-- Actor plugin 1 -->
  <!-- Actor motion -->
  <plugin name="trajectory" filename="libTrajectoryActorPlugin.so">
    
      <target>
        12.1254
        5.385326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        10.3737
        3.89311
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        7.96763
        4.71297
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        6.41362
        5.11439
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        6.32939
        8.46126
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        7.37067
        9.849683
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        10.5193
        9.81355
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        12.1611
        8.39257
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        13.1988
        5.185326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.704
        5.185326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.846853
        7.988438
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        16.0257
        8.12129
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        16.195299
        1.99335
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.1699
        1.91747
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    
      <target>
        14.176
        5.385326
        1.0
        1.570796
        -0.0
        3.141593
      </target>
    

    <velocity>1.0516294306711416</velocity>
    <obstacle_margin>1.5</obstacle_margin>
    <obstacle>my_robot_name</obstacle>

  </plugin>

  <!-- Actor plugin 2 -->
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
