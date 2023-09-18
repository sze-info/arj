---
layout: default
title: ROS2 launch
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



# `ROS 2` launch és `ROS 2` egyéb haladó koncepciók


## Bevezető

Az ROS 2 launch rendszere segíti a felhasználó által definiált rendszer konfigurációjának megadását, majd a konfiguráció szerinti végrehajtását. A konfigurációs lépés a következőket tartalmazza:
  - mely programok kerüljenek futtatásra,
  - milyen argumentumokat kapjanak a futtatott programok,
  - ROS-specifikus konvencióknak megfelelő összetevők, amelyek a komponensek könnyű újrahasznosíthatóságát teszik lehetővé.

  Fontos megemlíteni továbbá, hogy a megoldás felügyeli az elindított folyamatokat, és képes reagálni a folyamatok futási állapotában bekövetkező változásokra.

  Launch fájlok készítése történhet Python, XML, vagy YAML segítségével.


## Előkészületek

Hozzunk létre egy mappát a launch fájlok részére:

``` bash
mkdir launch
```

## Launch fájl létrehozása

Állítsunk össze egy launch fájlt a ```turtlesim``` csomag elemeivel, Python nyelv alkalmazásával.

``` py
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            namespace='turtlesim1',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='turtlesim',
            namespace='turtlesim2',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
            remappings=[
                ('/input/pose', '/turtlesim1/turtle1/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        )
    ])

```
