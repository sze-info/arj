---
layout: default
title: ROS2 Gyakorlat
parent: Szabályozás
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

A gyakorlat első részében egy példa első illetve másodrendű rendszert fogunk használni, erre fogunk PID szabályzót alkalmazni, majd hangolni.
A gyakorlat második részében egy szimulált trajektóriakövető robot / jármű működését nézzük át és hangoljuk.

Humble
{: .label .label-yellow }


## Videó

A videóhoz hasonló módon szeretnénk szemléltetni a szabályozás kérdéskörét, azonban mi Plotjuggler helyett Foxglove Studio-t használunk.

<iframe width="560" height="315" src="https://www.youtube.com/embed/G-f2eyPifbc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## `1. feladat`: PID hangolás

A következő leírás azzal a feltételezéssel él, hogy a ROS 2 workspace a `~/ros2_ws/` helyen található.

### `Terminal 1` clone

Klónozzuk a repositoryt:

```
cd ~/ros2_ws/src
git clone https://github.com/dottantgal/ros2_pid_library
```

### `Terminal 1` build

Lépjünk vissza a workspace gyökerébe és build:

```
cd ~/ros2_ws
colcon build --packages-select use_library pid_library example_system
```

### `Terminal 2` run

Új terminált nyissunk és futtassuk a következő parancsokat:

```
source ~/ros2_ws/install/local_setup.bash && source ~/ros2_ws/install/setup.bash
ros2 launch example_system example_sys_launch.py
```

### `Terminal 3` set point

```
ros2 topic pub -r 1  /set_point_topic std_msgs/msg/Float32 "data: 0.0"
ros2 topic pub -r 1  /set_point_topic std_msgs/msg/Float32 "data: 1.0"
ros2 topic pub -r 1  /set_point_topic std_msgs/msg/Float32 "data: 1.4"
ros2 topic pub -r 1  /set_point_topic std_msgs/msg/Float32 "data: 0.6"
```

### `Terminal 4` foxglove

Ha esetleg eddig nem lett volna telepítve:
```
sudo apt install ros-humble-foxglove-bridge
```
Maga bridge így indítható:
```
source ~/ros2_ws/install/local_setup.bash && source ~/ros2_ws/install/setup.bash
ros2 launch foxglove_bridge foxglove_bridge_launch.xml
```
Ezután Foxglove Studió segítségével `ws://localhost:8765` címen elérhető minden adat.

![](pid_plot02.png)

### VS code

Szerkesszük a `example_sys_launch.py` fájlt, majd `colcon build` (terminal 1) `source` és futtatás.

```
code ~/ros2_ws/src/ros2_pid_library/
```

![](vs_code01.png)

Futtassuk és figyeljük meg az eredményeket a beavatkozó jel (`control_value`) enyhén más jelleget mutat:

![](pid_plot03.png)

## `2. feladat`: Trajektóriakövetés

![](https://raw.githubusercontent.com/jkk-research/sim_wayp_plan_tools/main/img/gz_rviz01.gif)


[github.com/jkk-research/sim_wayp_plan_tools](https://github.com/jkk-research/sim_wayp_plan_tools)

### Követelmények
A gyakorlat hibamentes lefutásához a következő programok telepítése szükséges: 
- ROS 2 Humble: [docs.ros.org/en/humble/Installation.html](https://docs.ros.org/en/humble/Installation.html)
- Gazebo Fortress: [gazebosim.org/docs/fortress/install_ubuntu](https://gazebosim.org/docs/fortress/install_ubuntu), Több információ az integrálásról: [gazebosim.org/docs/fortress/ros2_integration](https://gazebosim.org/docs/fortress/ros2_integration)
- `ros-gz-bridge` Egy parancsal installálható: `sudo apt install ros-humble-ros-gz-bridge`
- Ellenőrizük, hogy a [`colcon_cd`](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html#setup-colcon-cd) megfelelően van telepítve. A csv fájlok a `colcon_cd`-vel töltődnek be.

### Package-ek és build

 Az alapértelmezett workspace a következő legyen:`~/ros2_ws/`.

### Klónozuk le a package-eket
```
cd ~/ros2_ws/src
git clone https://github.com/jkk-research/wayp_plan_tools
git clone https://github.com/jkk-research/sim_wayp_plan_tools
```

### ROS 2 -es package-ek buildelése
```
cd ~/ros2_ws
colcon build --packages-select wayp_plan_tools sim_wayp_plan_tools
```
### `wayp_plan_tools` használata szimulátorként

#### 1. A gazebo indítása
```
ign gazebo -v 4 -r ackermann_steering.sdf
```

#### 2. A Gazebo bridge indítása

Ne felejtsünk el `source`-olni az ROS-es parancsok előtt.

``` r
source ~/ros2_ws/install/local_setup.bash
```

``` r
ros2 launch sim_wayp_plan_tools gazebo_bridge.launch.py
```

Ez a `launch` fájl a következő node-okat indítja el egyben:

``` r
ros2 run ros_gz_bridge parameter_bridge /world/ackermann_steering/pose/info@geometry_msgs/msg/PoseArray[ignition.msgs.Pose_V
ros2 run ros_gz_bridge parameter_bridge /model/vehicle_blue/cmd_vel@geometry_msgs/msg/Twist]ignition.msgs.Twist
ros2 run ros_gz_bridge parameter_bridge /model/vehicle_blue/odometry@nav_msgs/msg/Odometry[ignition.msgs.Odometry --ros-args -r /model/vehicle_blue/odometry:=/odom
```
Több információ a bridge-ről: [github.com/gazebosim/ros_gz/blob/ros2/ros_gz_bridge/README.md](https://github.com/gazebosim/ros_gz/blob/ros2/ros_gz_bridge/README.md)

Ez a `launch` a `PoseArray`-ből egy `/tf`-et is készít a `pose_arr_to_tf`.

#### *Opcionális*: A gazebo-ban lévő robot irányítása billentyűzettel:

``` r
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/model/vehicle_blue/cmd_vel
```

#### 3. Waypointok betöltése

**Megjegyzés:** A waypointok egy ponthalmaz, amely az útvonal pozíció, orientáció és sebesség adatait tartalmazza diszkrét pontokra osztva. Ezeket az adatokat jellemzően úgy nyerjük ki hogy az útunk során ROS-ben rögzítjük a gps-től vagy az odometriától az x,y esetleg z koordinátákat, az aktuálishoz képest a következő pontra mutató orientációt és az éppen aktuális sebesség adatot. Végül az imént felsoroltakat csv fájlokban rögzítjük.  

Használjuk a ROS 2-es workspacet `file_dir`-ként:
``` r
ros2 run wayp_plan_tools waypoint_loader --ros-args -p file_name:=sim_waypoints1.csv -p file_dir:=$HOME/ros2_ws/src/sim_wayp_plan_tools/csv -r __ns:=/sim1
```
Vagy az alapparaméterekel:

``` r
ros2 launch sim_wayp_plan_tools waypoint_loader.launch.py
```
#### 4. Waypoint goal pose-ként
Ahogy az elméleti rész 4. fejezetben az ábrákon látható, minden szabályozási algoritmushoz tartozik egy vagy több goal pose amire az éppen működő szabálzó szabályoz. 

``` r
ros2 run wayp_plan_tools waypoint_to_target --ros-args -p lookahead_min:=2.5 -p lookahead_max:=4.5 -p mps_alpha:=1.5 -p mps_beta:=3.5 -p waypoint_topic:=waypointarray -p tf_frame_id:=base_link -p tf_child_frame_id:=map -r __ns:=/sim1
```
Vagy az alapparaméterekel:

``` r
ros2 launch sim_wayp_plan_tools waypoint_to_target.launch.py
```
#### 5. A szabályzás indítása:

Több lehetőség van:
- `single_goal_pursuit`: Pure pursuit (for vehicles / robots), a simple cross-track error method
- `multiple_goal_pursuit`: Multiple goal pursuit for vehicles / robots an implementation of our [paper](https://hjic.mk.uni-pannon.hu/index.php/hjic/article/view/914)
- `stanley_control`: Stanley controller, a heading error + cross-track error method
- `follow_the_carrot`: Follow-the-carrot, the simplest controller

Egy példa a pure pursuit-ra :

``` r
ros2 run wayp_plan_tools single_goal_pursuit --ros-args -p cmd_topic:=/model/vehicle_blue/cmd_vel -p wheelbase:=1.0 -p waypoint_topic:=targetpoints -r __ns:=/sim1
```
Vagy az alapparaméterekel:

``` r
ros2 launch sim_wayp_plan_tools single_goal_pursuit.launch.py
```
#### 6. Az eredmények vizualizálása `RViz2`-ben:
``` r
ros2 launch sim_wayp_plan_tools rviz1.launch.py
```
**Vagy futtasunk mindent együtt egyetlen parancsal:**

After `ign gazebo -v 4 -r ackermann_steering.sdf` (terminal 1) and `source ~/ros2_ws/install/local_setup.bash` (terminal 2), run this command (also in terminal 2): 
``` r
ros2 launch sim_wayp_plan_tools all_in_once.launch.py
```

# Hibaelhárítás:

A `ign gazebo server` leállítása:

``` r
ps aux | grep ign
```

``` r
ab  12345 49.9  1.2 2412624 101608 ?      Sl   08:26  27:20 ign gazebo server
ab  12346  518  6.6 10583664 528352 ?     Sl   08:26 283:45 ign gazebo gui
ab  12347  0.0  0.0   9396  2400 pts/2    S+   09:21   0:00 grep --color=auto ign
```

Ha azonosítva van a PID a folyamat leállításához használd a kill parancsot. Például a gazebo szerver leállításához:

``` r
kill 12345
```


# Források / Sources
- [github.com/dottantgal/ros2_pid_library](https://github.com/dottantgal/ros2_pid_library/) - [MIT license](https://github.com/dottantgal/ros2_pid_library/blob/main/LICENSE)
