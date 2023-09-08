---
layout: default
title: Gyakorlat
parent: Tervezés
---

 

<details markdown="block">
  <summary>
    Tartalom
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---




# `1.` feladat


Az első feladat a ROS 2 Navigation stack-jének beüzemelése szimulátorban, üres pályán. Részletes dokumentáció a [navigation.ros.org](https://navigation.ros.org/) oldalon.

<iframe width="560" height="315" src="https://www.youtube.com/embed/gjaXRG1d2Fw?si=Xf2iOuBe8ihZnKuV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Clone és build

``` r
cd ~/ros2_ws/src
git clone https://github.com/rosblox/nav2_outdoor_example
```

``` r
cd ~/ros2_ws
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

``` r
cd ~/ros2_ws
colcon build --packages-select nav2_outdoor_example
```

## Futtatás

``` r
source ~/ros2_ws/install/setup.bash
ros2 launch nav2_outdoor_example bringup.launch.py
```

# `2.` feladat


A második feladat a ROS 2 Navigation stack-jének beüzemelése szimulátorban, a turlebot egyik pályáján. Részletes dokumentáció a [navigation.ros.org](https://navigation.ros.org/) oldalon.

<video src="https://user-images.githubusercontent.com/2298371/226628768-818a7c3f-e5e1-49c6-b819-112c2cfa668b.webm" type="video/webm" width="560" controls>
</video>

[Videó direkt link](https://user-images.githubusercontent.com/2298371/226628768-818a7c3f-e5e1-49c6-b819-112c2cfa668b.webm)

Megjegyzés: előfordulhat, hogy az `ign_ros_control` package másik feladatban is buildelt package, ha ez már létezik, akkor a build / apt install kihagyható. A helyek, ahol ez lehetséges, hogy megtalálható:
``` r
ros2_ws/src/gz_ros2_control/ign_ros2_control
ros2_ws/src/navigation2_ignition_gazebo_example/src/gz_ros2_control/ign_ros2_control
/opt/ros/humble/share/ign_ros2_control
```


## Clone és build

```r
sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup ros-humble-turtlebot3-gazebo
```


``` r
cd ~/ros2_ws/src
git clone https://github.com/ros-controls/gz_ros2_control
git clone https://github.com/art-e-fact/navigation2_ignition_gazebo_example
cd ~/ros2_ws/src/gz_ros2_control
git checkout humble
cd ~/ros2_ws
rosdep install -y --from-paths src --ignore-src --rosdistro humble
```

``` r
cd ~/ros2_ws
colcon build --packages-select sam_bot_nav2_gz
```
## Futtatás

Gazebo, RViz2 és Navigation2
``` r
source ~/ros2_ws/install/setup.bash
ros2 launch sam_bot_nav2_gz complete_navigation.launch.py
```

Célpont kijelölése RViz2-ben:
``` r
source ~/ros2_ws/install/setup.bash
ros2 run sam_bot_nav2_gz follow_waypoints.py
source ~/ros2_ws/install/setup.bash
ros2 run sam_bot_nav2_gz reach_goal.py
``` 

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/rviz-not-started.png)

![](gazebo_turtlebot01.png)



![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/rviz_initial.png)

## Navigáció

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/rviz-set-initial-pose.png)

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/navstack-ready.png)

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/navigate-to-pose.png)
   
![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/navigation_with_recovery_behaviours.gif)

# Sources
- [navigation.ros.org/getting_started/index.html](https://navigation.ros.org/getting_started/index.html)
- [navigation.ros.org](https://navigation.ros.org)
- [github.com/ros-controls/gz_ros2_control](https://github.com/ros-controls/gz_ros2_control)
- [github.com/art-e-fact/navigation2_ignition_gazebo_example](https://github.com/art-e-fact/navigation2_ignition_gazebo_example)