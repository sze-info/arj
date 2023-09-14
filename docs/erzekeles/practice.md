---
layout: default
title: Gyakorlat
parent: Érzékelés
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


# Gyakorlat

A gyakorlat Ubuntu 22.04 `ROS humble`, Windows 10/11 WSL `humble` mellett működik. A különböző verziók telepítésének leírása [itt található](https://sze-info.github.io/arj/telepites/README.html).

# Előkészületek

Az előző gyakorlaton megismerkedtünk a következő rosbag-gel (ROS 2-ben a formátum már `.mcap`):

![foxglove01](https://sze-info.github.io/arj/bevezetes/foxglove01.png)

Előkészületként nézzük meg, hogy létezik-e a `C:\temp` könyvtár

``` c
test -d "/mnt/c/temp" && echo Letezik || echo Nem letezik
```
Vagy egyszerűbben:
``` c
ls /mnt/c/temp
```

- Ha nem létezik (`No such file or directory`) akkor hozzuk létre: `mkdir /mnt/c/temp`
- Ha létezik, akkor nincs teendőnk, lépjünk a következő lépésre, másoljuk át ide az `.mcap` fájlokat


Tanteremben a másolás a következő parancsok **egyike** legyen:

``` r 
rsync -avzh --progress /mnt/kozos/measurement_files/lexus3sample01.mcap  /mnt/c/temp/
rsync -avzh --progress /mnt/kozos/measurement_files/lexus3sample02.mcap  /mnt/c/temp/
rsync -avzh --progress /mnt/kozos/measurement_files/lexus3sample03.mcap  /mnt/c/temp/
rsync -avzh --progress /mnt/kozos/measurement_files/lexus3sample04.mcap  /mnt/c/temp/
rsync -avzh --progress /mnt/kozos/measurement_files/lexus3sample05.mcap  /mnt/c/temp/
```

Listázzuk a **megfelelő** átásolt `.mcap` fájl alap információit, hasonlóan:  

``` r
ros2 bag info /mnt/c/temp/lexus3sample06.mcap

closing.

Files:             /mnt/c/temp/lexus3sample06.mcap
Bag size:          6.5 GiB
Storage id:        mcap
Duration:          54.102s
Start:             Jul 18 2023 15:37:09.211 (1689687429.211)
End:               Jul 18 2023 15:38:03.314 (1689687483.314)
Messages:          29598
Topic information: 
  Topic: /lexus3/zed2i/zed_node/right_raw/image_raw_color/compressed | Type: sensor_msgs/msg/CompressedImage | Count: 1025
  Topic: /lexus3/os_left/points | Type: sensor_msgs/msg/PointCloud2 | Count: 1044
  Topic: /lexus3/os_right/points | Type: sensor_msgs/msg/PointCloud2 | Count: 1061
  Topic: /lexus3/os_center/imu | Type: sensor_msgs/msg/Imu | Count: 5335
  Topic: /tf_static | Type: tf2_msgs/msg/TFMessage | Count: 14
  Topic: /lexus3/os_center/points | Type: sensor_msgs/msg/PointCloud2 | Count: 1062
  Topic: /tf | Type: tf2_msgs/msg/TFMessage | Count: 11218
  Topic: /lexus3/gps/duro/mag | Type: sensor_msgs/msg/MagneticField | Count: 1339
  Topic: /lexus3/gps/duro/imu | Type: sensor_msgs/msg/Imu | Count: 5336
  Topic: /lexus3/gps/duro/status_string | Type: std_msgs/msg/String | Count: 1082
  Topic: /lexus3/gps/duro/current_pose | Type: geometry_msgs/msg/PoseStamped | Count: 108 
```

# Játsszuk vissza az `.mcap` fájlt

A következőken a mérésadafájlt visszajátsszuk és ellenőrizzük milyen adatok jelennek meg, milyen típusban és sebességben. A `--loop` kapcsoló a végtelen ismétlést a `--clock` kapcsoló pedig egy `/clock` topic hirdetéséért felel, ehhez igazítja a lejátszást.

``` r
ros2 bag play /mnt/c/temp/lexus-2023-07-18-campus.mcap --clock --loop
```

A következő topic-ok jelennek meg:

``` r
ros2 topic list

/clock
/events/read_split
/lexus3/gps/duro/current_pose
/lexus3/gps/duro/imu
/lexus3/gps/duro/mag
/lexus3/gps/duro/status_string
/lexus3/os_center/imu
/lexus3/os_center/points
/lexus3/os_left/points
/lexus3/os_right/points
/lexus3/zed2i/zed_node/right_raw/image_raw_color/compressed
/parameter_events
/rosout
/tf
/tf_static
```

A `ros2 topic hz` az adott topic frekvenciáját mutatja. A pozíció pl. itt ~20Hz.

``` r
ros2 topic hz /lexus3/gps/duro/current_pose
average rate: 20.133
        min: 0.002s max: 0.101s std dev: 0.03451s window: 22
```

# `ROS 2` időkezelés

Az `ROS` idő kezelésre a Unix-idő vagy POSIX-időt használja. Ez a UTC (greenwichi idő) szerinti 1970. január 1. 00:00:00 óta eltelt másodpercek és nanoszekundumok számát jelenti (`int32 sec`, `int32 nsec`). Ez egyrészt relatív kis helyet fogla a memóriában, másrészt könnyen számolható az eltelt idő egy egyszerű kivonással. 

[`ros2time.ipynb`](https://github.com/sze-info/arj/blob/main/docs/erzekeles/ros2time.ipynb){: .btn .btn-red .mr-4 } 

Hátránya, hogy nem túl intuitív, nem olvasható az ember számára. Pl. a Foxglove Studio ezért is gyakran átalakítja olvashatóbb formátumra. A másodpercek és nanoszekundumok a következőképp képzelhetők el:

``` py
import rclpy
current_time = node.get_clock().now()
print(current_time.to_msg())

Output: 
sec=1694595162, nanosec=945886859
```

Az időbélyeg több helyen is szerepet kap:

``` r
ros2 topic echo /clock --once
clock:
  sec: 1689687476
  nanosec: 770421827
``` 

``` r
ros2 topic echo --once /lexus3/gps/duro/current_pose

header:
  stamp:
    sec: 1694595162
    nanosec: 945886859
  frame_id: map
pose:
  position:
    x: 640142.9676535318
    y: 5193606.439717201
    z: 1.7999999523162842
  orientation:
    x: 0.008532664424537166
    y: 0.0018914791588597107
    z: 0.44068499630505714
    w: 0.8976192678279703
```

Ha szeretnénk átválatni a másodperceket és nanoszekundumokat, azt pl a következő módon thetjük meg:

``` py
from datetime import datetime
current_time_float = current_time.to_msg().sec + current_time.to_msg().nanosec / 1e9 # 1e9 is 1,000,000,000: nanosec to sec
print("As a float:\t%.5f" % (current_time_float))
print("ISO format:", end="\t")
print(datetime.utcfromtimestamp(current_time_float).isoformat())


Output:
As a float:	1694595162.94589
ISO format:	2023-09-13T08:52:42.945887
```

**Emlékeztető**: a nanoszekundum a másodperc egy milliárdodrésze (10^-9 s).

# GNSS (GPS)

A köveztkezőkben átnézünk pár jellemző szenzort (GPS, kamer, LIDAR) és azok topic-jait, node-jait (driver package-ekbe szervezve). Vessünk egy pillantást a saját fejelsztésű Duro GPS (GNSS) driverre: [github.com/szenergy/duro_gps_driver](https://github.com/szenergy/duro_gps_driver/tree/ros2-humble). A GPS-t etherneten a számítógéphez csatlakoztatva, a ROS drivert indítva a következő topicokat fogja hirdetni:


|Topic|Type
|-|-|
`/gps/duro/current_pose` |[`[geometry_msgs/PoseStamped]`](http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/PoseStamped.html)
`/gps/duro/fix` |[`[sensor_msgs/NavSatFix]`](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/NavSatFix.html)
`/gps/duro/imu` |[`[sensor_msgs/Imu]`](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/Imu.html)
`/gps/duro/mag` |[`[sensor_msgs/MagneticField]`](http://docs.ros.org/en/melodic/api/sensor_msgs/html/msg/MagneticField.html)
`/gps/duro/odom ` |[`[nav_msgs/Odometry]`](http://docs.ros.org/en/melodic/api/nav_msgs/html/msg/Odometry.html)
`/gps/duro/rollpitchyaw` |[`[geometry_msgs/Vector3]`](http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Vector3.html)
`/gps/duro/status_flag` |[`[std_msgs/UInt8]`](http://docs.ros.org/en/melodic/api/std_msgs/html/msg/UInt8.html)
`/gps/duro/status_string` |[`[std_msgs/String]`](http://docs.ros.org/en/melodic/api/std_msgs/html/msg/String.html)
`/gps/duro/time_ref` |[`[sensor_msgs/TimeReference]`](http://docs.ros.org/en/api/sensor_msgs/html/msg/TimeReference.html)


# IMU

*Jellemző `ROS 2` topic típusok:* [`sensor_msgs/msg/Imu`](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Imu.msg)


# Kamera


*Jellemző `ROS 2` topic típusok:* [`sensor_msgs/msg/Image`](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/Image.msg), [`sensor_msgs/msg/CameraInfo`](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/CameraInfo.msg)

# LIDAR


*Jellemző `ROS 2` topic típusok:* [`sensor_msgs/msg/PointCloud2`](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/PointCloud2.msg), [`sensor_msgs/msg/LaserScan`](https://github.com/ros2/common_interfaces/blob/humble/sensor_msgs/msg/LaserScan.msg)

# Vizualizáció

## RVIZ2

``` r
ros2 run rviz2 rviz2
```

## Foxglove studio

![](https://foxglove.dev/images/blog/introducing-foxglove-studios-new-navigation/breakdown.jpeg)

Forrás: [foxglove.dev/blog/introducing-foxglove-studios-new-navigation](https://foxglove.dev/blog/introducing-foxglove-studios-new-navigation)