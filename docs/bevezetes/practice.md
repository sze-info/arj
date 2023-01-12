---
layout: default
title: Gyakorlat
parent: Bevezetés
---

{: .no_toc }

<details open markdown="block">
  <summary>
    Tartalom
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---


# Gyakorlat

A gyakorlat során meg fogunk ismerkedni az önvezető járművek jellemző tulajdonságaival és a rögzített adatok jellegzetességeivel.

# Foxglove Studio

Bevezetésképpen nézzük egy önvezető jármű jellemző adatait. Példaképp célszerű az **egyetemünk** egyik ilyen járművével készült adatokat vizsgálni. Foxglove Studio-t fogunk használni, hiszen telepítés nélkül, vagy ~150MB méretű telepíthető állományként is hozzáférhető, valamint képes vizualizálni a számunkra fontos adatokat. A vizsgált adatok hasonló képet fognak mutatni:

![foxglove01](foxglove01.png)

Töltsük le a fent vizualizált rosbag `.bag` fájlt és a Foxglove Studio layout-ot:

[Rosbag letöltése 3.37 GB](https://laesze-my.sharepoint.com/:u:/g/personal/herno_o365_sze_hu/EYl_ahy5pgBBhNHt5ZkiBikBoy_j_x95E96rDtTsxueB_A?download=1){: .btn .btn-green .mr-4 } 
[Layout letöltése](https://jkk-research.github.io/data/leaf01foxglove.json){: .btn .btn-blue }

{: .new }
A [https://jkk-research.github.io/#dataset](https://jkk-research.github.io/#dataset) oldalról további példa adatokat lehet letölteni.

## A Foxglove bemutatása

Amíg a `.bag` töltődik, röviden bemutatjuk a Foxglove Studio programot. A Foxglove Studio egy nyílt forráskódú, robotikai adatokat vizualizáló és hibakereső eszköz. Elérhető számos módon:
- önálló asztali alkalmazásként futtatható
- böngészőben hozzáférhető
- saját domainen, önállóan hostolható

A natív robotikai eszközök (mint például az ROS ecoszisztéma részei) általában csak Linux rendszeren támogatottak, de a Studio asztali alkalmazás Linuxon, Windowson és macOS-en is működik. Akár az ROS stack más operációs rendszeren fut, a Studio képes kommunikálni a robottal zökkenőmentesen.

A Studio gazdag vizuális elemeket és hibakereső panelokat kínál - interaktív diagramoktól, 3D vizuális elemekig, kameraképektől, és diagnosztikai adatfolyamokig. Legyen szó valós idejű robotkövetésről, vagy `.bag` fájlban történő hibakeresésről, ezek a panelok segítenek a különböző, általános robotikai feladatok megoldásában.

Ezek a panelok ezután egyedi elrendezésekben konfigurálhatók és összeállíthatók a projekt egyedi igényeinek és munkafolyamatainak meg.

## Az egytemi Nissan mérésadatainak leírása

ROS rendszerben (de más hasonló robotikai megoldásokban is) az egyes adatok [topic](http://wiki.ros.org/Topics)-okba szerveződve vannak publikálva. Egy topic lehet például egy szenzor kimenete, egy szabályzó bemenete, vizualizációs marker, stb. A topicoknak [típusuk](http://wiki.ros.org/Messages) van, rengeteg előre definiált típus létezik, de létrehozhatunk sajátot is, ha ezek nem lennének elegek. Példaképp pár előre definiált típus:
-  [`sensor_msgs/Image`](http://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/Image.html) - Tömörítés nélküli képi információ, jellemzően a kamerától jön, de lehet feldolgozott adat, ami pl jelölve vannak a gyalogosok is.
-  [`sensor_msgs/CompressedImage`](http://docs.ros.org/en/noetic/api/sensor_msgs/html/msg/CompressedImage.html) - Tömörített képi információ.
-  [`std_msgs/String`](http://docs.ros.org/en/noetic/api/std_msgs/html/msg/String.html) - Egyszerű szöveges üzenettípus.
-  [`std_msgs/Bool`](http://docs.ros.org/en/noetic/api/std_msgs/html/msg/Bool.html) - Egyszerű bináris üzenettípus.
-  [`geometry_msgs/Point`](http://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/Point.html) - XYZ 3D pont.
- [`geometry_msgs/Pose`](http://docs.ros.org/en/noetic/api/geometry_msgs/html/msg/Pose.html) - 3D pont és a hozzá tartozó orientáció.

Ahogy látszik a típusok különböző kategóriákba esnek, úgy mint: `std_msgs`,  `diagnostic_msgs`, `geometry_msgs`, `nav_msgs`, `sensor_msgs` stb. Nézzük milyen típusú üzenetek találhatók a letöltött fájlban:

| Topic | Típus | Hz | Szenzor |
| --- | --- | --- | --- |
/gps/duro/current_pose | `geometry_msgs/PoseStamped` | 10 | Duro GPS (UTM)
/gps/duro/imu | `sensor_msgs/Imu` | 200 | Duro GPS
/gps/duro/mag | `sensor_msgs/MagneticField` | 25 | Duro GPS
/gps/nova/current_pose | `geometry_msgs/PoseStamped` | 20 | Novatel GPS (UTM)
/gps/nova/imu | `sensor_msgs/Imu` | 200 | Novatel GPS
/left_os1/os1_cloud_node/imu | `sensor_msgs/Imu` | 100 | Ouster LIDAR
/left_os1/os1_cloud_node/points | `sensor_msgs/PointCloud2` | 20 | Ouster LIDAR
/right_os1/os1_cloud_node/imu | `sensor_msgs/Imu` | 100 | Ouster LIDAR
/right_os1/os1_cloud_node/points | `sensor_msgs/PointCloud2` | 20 | Ouster LIDAR
/velodyne_left/velodyne_points | `sensor_msgs/PointCloud2` | 20 | Velodyne LIDAR
/velodyne_right/velodyne_points | `sensor_msgs/PointCloud2` | 20 | Velodyne LIDAR
/cloud | `sensor_msgs/PointCloud2` | 25 | SICK LIDAR
/scan | `sensor_msgs/LaserScan` | 25 | SICK LIDAR
/zed_node/left/camera_info | `sensor_msgs/CameraInfo` | 30 | ZED kamera
/zed_node/left/image_rect_color/compressed | `sensor_msgs/Image` | 20 | ZED kamera
/vehicle_status | `autoware_msgs/VehicleStatus` | 100 | CAN adatok
/ctrl_cmd | `autoware_msgs/ControlCommandStamped` | 20 | Referencia sebesség és kormyánszög
/current_pose | `geometry_msgs/PoseStamped` | 20 | Aktuális GPS
/tf | `tf2_msgs/TFMessage` | 500+ | Transform