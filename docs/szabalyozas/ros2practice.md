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

A gyakorlaton 

Humble
{: .label .label-yellow }


## Videó

A videóhoz hasonló módon szeretnénk szemléltetni a szabályozás kérdéskörét, azonban mi Plotjuggler helyett Foxglove Studio-t használunk.

<iframe width="560" height="315" src="https://www.youtube.com/embed/G-f2eyPifbc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## PID hangolás

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

# Források / Sources
- [github.com/dottantgal/ros2_pid_library](https://github.com/dottantgal/ros2_pid_library/) - [MIT license](https://github.com/dottantgal/ros2_pid_library/blob/main/LICENSE)