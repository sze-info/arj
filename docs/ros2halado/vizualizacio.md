---
layout: default
title: ROS2 vizualizáció
parent: ROS 2 haladó 
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



# `ROS 2` vizualizáció


## `visualization_msgs/msg/Marker` típus

[`visualization_msgs/msg/Marker` dokumentáció](https://docs.ros2.org/foxy/api/visualization_msgs/msg/Marker.html)


## `visualization_msgs/msg/MarkerArray` típus
[`visualization_msgs/msg/MarkerArray` dokumentáció](https://docs.ros2.org/foxy/api/visualization_msgs/msg/MarkerArray.html)


## Mesh

``` r
git clone https://github.com/szenergy/rviz_markers
git checkout ros2-humble
cd ~/ros2_ws 
colcon build --packages-select rviz_markers
source ~/ros2_ws/install/setup.bash
```

![](https://raw.githubusercontent.com/wiki/szenergy/szenergy-public-resources/img/rviz02.gif)

# Források

- [docs.ros.org/en/humble/How-To-Guides/Visualizing-ROS-2-Data-With-Foxglove-Studio.html](https://docs.ros.org/en/humble/How-To-Guides/Visualizing-ROS-2-Data-With-Foxglove-Studio.html)
- [docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Creating-Launch-Files.html](https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Creating-Launch-Files.html)
- [docs.ros.org/en/humble/Tutorials/Intermediate/Tf2/Writing-A-Tf2-Broadcaster-Cpp.html](https://docs.ros.org/en/humble/Tutorials/Intermediate/Tf2/Writing-A-Tf2-Broadcaster-Cpp.html)