---
layout: default
title: Gyakorlat
parent: Észlelés
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

A feladat egyszerű LIDAR szűrés, X, Y és Z koordináták szerint.

![](https://raw.githubusercontent.com/sze-info/arj_packages/main/arj_simple_perception/img/simple_filter01.gif)

Ha még nem tettük volna, klónozzuk az `arj_packages` repot és buildeljük az `arj_simple_perception` package-t.

``` r
cd ~/ros2_ws/src
git clone https://github.com/sze-info/arj_packages
```

Ha már létezik, akkor az előző lépés helyett, csak frissítsük.

``` r
cd ~/ros2_ws/src/arj_packages/
git status
git checkout -- .
git pull
```
A `git checkout -- .` az összes esetleges lokális változás visszavonására jó.

``` r
cd ~/ros2_ws
MAKEFLAGS="-j4" colcon build --packages-select arj_simple_perception --cmake-args -DCMAKE_BUILD_TYPE=Release
```

{: .note }
A klasszikus `colcon build --packages-select arj_simple_perception` is működik, csupán egy kicsit lassabb, ezért használjuk most a build flageket.


``` r
source ~/ros2_ws/install/setup.bash
ros2 run arj_simple_perception lidar_filter_simple
```

``` r 
ros2 bag play /mnt/c/bag/lexus3_2023-09-04.mcap --loop --clock
```


# `2.` feladat

Hasnolítsuk össze a `lidar_filter_simple_param.cpp`-t a `lidar_filter_simple.cpp`-vel. Vs code jobb kilikk a fájlon `Select for compare` és `Compare with Selected`.

![compare_vs_code01](compare_vs_code01.png)

Az előző feladatban használt egyszerű filter minimum és maximum X,Y,Z értékeit dinamikusan változtassuk.

``` r
source ~/ros2_ws/install/setup.bash
ros2 run arj_simple_perception lidar_filter_simple_param
```

``` r
source ~/ros2_ws/install/setup.bash
ros2 launch arj_simple_perception run_rviz1.launch.py
```

``` r
ros2 run rqt_reconfigure rqt_reconfigure
```

A 3 terminal helyett használhatunk egy `launch` fájlt is:
``` r
source ~/ros2_ws/install/setup.bash
ros2 launch arj_simple_perception run_all.launch.py
```