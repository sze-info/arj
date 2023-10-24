---
layout: default
title: Szimuláció
has_children: true
---

# Szimuláció

 Az ROS-által leginkább támogatott szimulátor a Gazebo, de érdemes megemlíteni az [SVL](https://github.com/lgsvl/simulator)-t, ebből saját verziónk is van a [Nissan](https://github.com/szenergy/nissanleaf-lgsvl)-ra optimalizáva, a [Carla](https://github.com/carla-simulator)-t vagy a [CoppeliaSim](https://www.coppeliarobotics.com/)-et.


<iframe width="560" height="315" src="https://www.youtube.com/embed/QD9iCauN0K8?rel=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# További szimulátorok

- [OSRF Gazebo](http://gazebosim.org/) - ROS-által leginkább támogatott szimulátor, OGRE-alapú, ROS/ROS 2 kompatibilis.
  - [GitHub repository](https://github.com/osrf/gazebo)
- [CARLA](https://carla.org/) - Unreal Engine alapú automotive szimulátor. Autoware, Baidu Apollo, ROS/ROS 2 kompatibiltással
  - [GitHub repository ](https://github.com/carla-simulator/carla)
  - [YouTube channel](https://www.youtube.com/channel/UC1llP9ekCwt8nEJzMJBQekg)
- [LGSVL / SVL](https://www.lgsvlsimulator.com/) - Unity Enginealapú automotive szimulátor, Autoware, Baidu Apollo, ROS/ROS 2 kompatibiltással. *Fontos:* az LG [felfüggesztette](https://www.svlsimulator.com/news/2022-01-20-svl-simulator-sunset) az SVL Simulator aktív fejlesztését.
  - [GitHub repository](https://github.com/lgsvl/simulator)
  - [YouTube channel](https://www.youtube.com/c/LGSVLSimulator)
- [OSSDC SIM](https://github.com/OSSDC/OSSDC-SIM) - Unity Engine alapú szimulátor autóipari alkalmazásokhoz, a felfüggesztett LGSVL szimulátoron alapul, de aktív fejlesztés. Kompatibilis az Autoware, a Baidu Apollo és a ROS/ROS 2 szoftverekkel.
   - [GitHub repository](https://github.com/OSSDC/OSSDC-SIM)
   - [YouTube video](https://www.youtube.com/watch?v=fU_C38WEwGw)
- [AirSim](https://microsoft.github.io/AirSim) - Unreal Engine alapú szimulátor drónokhoz és autókhoz. ROS kompatibilis.
   - [GitHub repository](https://github.com/microsoft/AirSim)
   - [YouTube video](https://www.youtube.com/watch?v=gnz1X3UNM5Y)
- [AWSIM](https://tier4.github.io/AWSIM) - Unity Engine alapú szimulátor autóipari alkalmazásokhoz. Kompatibilis az Autoware-rel és a ROS 2-vel.
   - [GitHub repository](https://github.com/tier4/AWSIM)
   - [YouTube video](https://www.youtube.com/watch?v=FH7aBWDmSNA)
– [CoppeliaSim](https://www.coppeliarobotics.com/coppeliaSim) – Többplatformos, általános célú robotszimulátor (korábbi nevén V-REP).
   - [YouTube channel](https://www.youtube.com/user/VirtualRobotPlatform)

## Gazebo és ROS kompatibilitás

Az ROS és Gazebo kiválasztás a [Picking the "Correct" Versions of ROS & Gazebo](https://gazebosim.org/docs/garden/ros_installation) link alapján írjuk le.
*Megjegyzés*: a Gazebo 11 és előtt elévő verzióit Gazebo Classic néven, míg az utána lévőket Ignition Gazebo vagy röviden Ignition-ként említik.


| | **GZ Citadel (LTS)**  | **GZ Fortress (LTS)**   | **GZ Garden**   |
|---|---|---|---|
| **ROS 2 Rolling**       | 🔴 | 🟢 | 🟡 |
| **ROS 2 Humble (LTS)**  | 🔴 | 🟢 | 🟡 |
| **ROS 2 Foxy (LTS)**    | 🟢 | 🔴 | 🔴 |
| **ROS 1 Noetic (LTS)**  | 🟢 | 🟡 | 🔴 |


* 🟢 - Ajánlott kombináció
* 🟡 - Lehetséges, *de nem ajánott*. Plusz munkával működésre lehet bírni együtt a két szoftvert.
* 🔴 - Nem kompatibils / nem lehetséges.
