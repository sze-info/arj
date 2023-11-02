---
layout: default
title: Ground filter
parent: Észlelés
---

# Ground filter


<img src="https://raw.githubusercontent.com/url-kaist/patchwork-plusplus/master/pictures/demo_000000.png" width="80%" />

Elérhető:
- [github.com/MohamedHussein736/patchwork-plusplus-ros/tree/ROS2](https://github.com/MohamedHussein736/patchwork-plusplus-ros/tree/ROS2)
- [github.com/jkk-research/patchwork-plusplus-ros](https://github.com/jkk-research/patchwork-plusplus-ros) az eredeti repo forkja, ami csak az ROS 2-es brenchet tartalmazza

``` bash
cd ~/ros2_ws/src
```

``` bash
git clone https://github.com/jkk-research/patchwork-plusplus-ros
``` 

``` bash
cd ~/ros2_ws/src
```

``` bash
colcon build --packages-select patchworkpp
```

``` bash
ros2 launch patchworkpp demo.launch
```

TODO

``` bash
ros2 bag play kitti_00_sample.db3
```