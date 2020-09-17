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
This example is taken from [servicesim_competition](https://github.com/osrf/servicesim/blob/master/servicesim_competition/worlds/follow_actor_demo.world).  
For more detail please refer to official Gazebo [page](http://gazebosim.org/tutorials?tut=actor&cat=build_robot).

```xml
<actor name="number5">
    <include>
    <uri>model://number5</uri>
    </include>
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
This example is take from [servicesim_competition](https://github.com/osrf/servicesim/blob/master/servicesim_competition/worlds/follow_actor_demo.world).  


## References
- https://github.com/onlytailei/gym_ped_sim
