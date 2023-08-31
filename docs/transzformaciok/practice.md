---
layout: default
title: Gyakorlat
parent: Transzformációk
---

# Gyakorlat

A következő gyakorlat a transzformációk ROS2-ben történő kezelését szemlélteti, C++-ban.

{: .important-title }
> Python megfelelője
>
> A C++ kód python verziója szintén elérhető a [github.com/sze-info/arj_packages](https://github.com/sze-info/arj_packages/tree/main/arj_transforms_py) címen. Érdemes összehasonlítani a C++ és a python kódokat.

Frissítsük a legújabb verzióra az `arj_packages` repo-t. Ha frissül, vagy `Already up to date.` üzenetet kapunk, akkor nem kell klónoznunk. Ha a `cd ~/ros2_ws/src/arj_packages` parancs után a `~/ros2_ws/src/arj_packages: No such file or directory` üzenetetet kaptuk, akkor klónozzuk a repo-t.

``` r
cd ~/ros2_ws/src/arj_packages
git pull
```

Amennyiben a `No such file or directory` üzenetetet kaptuk, klónozzuk a következő parancsokkal:

``` r
cd ~/ros2_ws/src
git clone https://github.com/sze-info/arj_packages
```

Ezután már buildelhetünk is:

``` r
cd ~/ros2_ws
colcon build --packages-select arj_transforms_cpp
```
Célszerű új terminalban source-olni, majd futtatni:

``` r
source ~/ros2_ws/install/setup.bash
ros2 run arj_transforms_cpp pub_transforms
```

Vizsgáljuk meg, nyers adatként milyen kimenetet kapunk:


``` r
ros2 topic echo /tf
``` 

Erre a válasz hasonló lesz: 

``` r
transforms:
- header:
    stamp:
      sec: 1693475112
      nanosec: 95339579
    frame_id: orbit1
  child_frame_id: orbit2
  transform:
    translation:
      x: -2.487199068069458
      y: 0.25266680121421814
      z: 0.0
    rotation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0
---
transforms:
- header:
    stamp:
      sec: 1693475112
      nanosec: 145005518
    frame_id: map
  child_frame_id: orbit1
  transform:
    translation:
      x: -4.109088897705078
      y: 2.8487515449523926
      z: 0.0
    rotation:
      x: 0.0
      y: 0.0
      z: -0.46381551598382736
      w: 0.8859318072699817

``` 
Ahogy láthatjuk a `map` frame `child_frame_id`-ja `orbit1`. Az `orbit1` frame `child_frame_id`-ja pedig az `orbit2`. Tehát, ha a `map`-et naygszülőnek tekintjük, akkor az `orbit2` az unoka. Szemléletesebb ezt az `rqt_tf_tree` segítségével megvizsgálni.

``` r
ros2 run rqt_tf_tree rqt_tf_tree
```

![frames01](frames01.svg)

Ha esetleg nem működne a fenti parancs, akkor telepíthető a `sudo apt install ros-humble-rqt-tf-tree` segítségével. Géptermi gépeken erre elvileg nincs szükség.


Nézzük meg RVIZ2 segítségével, így fog kinézni:

``` r
ros2 launch arj_transforms_cpp rviz1.launch.py
```

![transforms01](https://raw.githubusercontent.com/sze-info/arj_packages/main/arj_transforms_cpp/img/transforms01.gif)


Bővítsük `orbit3` statikus transformmal:

``` r
ros2 run tf2_ros static_transform_publisher --x 1.0 --y 0.2 --z 1.4 --qx 0.0 --qy 0.0 --qz 0.0 --qw 1.0 --frame-id orbit2 --child-frame-id orbit3
```

``` r
ros2 run rqt_reconfigure rqt_reconfigure
```

![](rqt_reconf01.png)

Hirdessünk egy markert, majd adjuk hozzá RVIZ2-ben.

``` r
ros2 topic pub --rate 40 /marker_topic2 visualization_msgs/msg/Marker '{header: {frame_id: "orbit2"}, ns: "markers", id: 1, type: 2, action: 0, pose: {position: {x: 0.0, y: 0.0, z: 0.0}, orientation: {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}, scale: {x: 1.0, y: 1.0, z: 1.0}, color: {r: 0.2, g: 0.4, b: 0.2, a: 1.0}}'
```

# További 

[Python notebook](https://nbviewer.org/github/horverno/sze-academic-python/blob/master/eload/ealeshtranszfromaciok.ipynb){: .btn .btn-blue }

<iframe width="560" height="315" src="https://www.youtube.com/embed/kYB8IZa5AuE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

![rh](right_hand_rule01.svg)

![lexus 3d](https://raw.githubusercontent.com/jkk-research/lexus_base/main/img/lexus3d01.gif)



# Olvasnivaló
- [articulatedrobotics.xyz/ready-for-ros-6-tf](https://articulatedrobotics.xyz/ready-for-ros-6-tf/)