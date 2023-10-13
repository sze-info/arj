---
layout: default
title: Ground filter
parent: Észlelés
---

# Ground filter


<img src="https://raw.githubusercontent.com/url-kaist/patchwork-plusplus/master/pictures/demo_000000.png" width="80%" />

[github.com/MohamedHussein736/patchwork-plusplus-ros/tree/ROS2](https://github.com/MohamedHussein736/patchwork-plusplus-ros/tree/ROS2)

``` r
cd ~/ros2_ws
```

``` r
colcon build --packages-select patchworkpp
```

``` r
ros2 launch patchworkpp demo.launch
```

``` r
ros2 bag play kitti_00_sample.db3
```