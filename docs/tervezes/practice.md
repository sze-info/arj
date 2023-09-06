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

## Clone és build

```r
sudo apt install ros-humble-navigation2
sudo apt install ros-humble-nav2-bringup
```

```r
sudo apt install ros-humble-turtlebot3-gazebo
```

## Futtatás

``` r
export TURTLEBOT3_MODEL=waffle
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
```


![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/rviz-not-started.png)

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/gazebo/gazebo_turtlebot1.png)



![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/rviz_initial.png)

## Navigáció

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/rviz-set-initial-pose.png)

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/navstack-ready.png)

![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/rviz/navigate-to-pose.png)
   
![](https://raw.githubusercontent.com/ros-planning/navigation.ros.org/master/images/navigation_with_recovery_behaviours.gif)

# Sources
- [navigation.ros.org/getting_started/index.html](https://navigation.ros.org/getting_started/index.html)
- [navigation.ros.org](https://navigation.ros.org)