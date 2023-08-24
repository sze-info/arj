---
layout: default
title: Transzformációk
has_children: true
---

{: .no_toc }

<details markdown="block">
  <summary>
    Tartalom
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---



# Bevezetés

ROS-ben (és álatalában robotikában) a transformok határozzák meg a hogy, mi merre található az adott vonatkozatási ponttól (frame). Több transzform leírhatja például egy robotkar mozgását vagy épp egy jármű és szenzorai helyzetét a térben.

# Transzformációk
Például a Nissan Leaf `base_link` framejéhez képest a következő fontosabb framek találhatóak meg:

![img](leaf_tf01.png)
*1. ábra - Frame-k a járművön*

Szeretnénk tartani magunkat ahhoz a konvencióhoz, hogy a globális térképet `map` frame-nek, a jármű hátsó tengelyép `base_link`-nek hívjuk. A `map` és a `base_link` közötti megfeleltetés történehet GPS, NDT matching, Kálmán filter, odometria és számos további módon. Ezt a követező példa szemlélteti:

![img](tf_examples01.svg)
*2. ábra - Példa TF tree*

GPS használata esetén nagyvonalakban a következő példa alapján kell elképzelni a frameket. A `map` a globális térkép, viszont a `gps` helyzetét is tudjuk ehhez képest. (**Megjegyzés**: *a `2020.A` senzor összeállításban 2 GPS is van, ezek kölönböző helyen találhatóak, mérni tudnak párhuzamosan, de csak egy transzform határozhatja meg a `base_link` helyzetét. Ezt az 1. ábrán a szaggatott nyilak jelzik.*) Innen már egy további (statikus) transzformációval kapható a `base_link` (a hátsó tengely). További statikus transzformációkkal kaphatók a szenzorok a példában a `left_os1/os1_sensor` látható.

![img](tf_examples02.svg)
*3. ábra - A TF tree 2d koordinátarendszerben, vizuális példa*

A transformok a `tf` topicaban hirdetődnek, azonban például az MPC szabályzó egy `current_pose` nevű topicot használ a szabályzás megvalósításához. Ezt úgy oldottuk meg, hogy a `base_link` frame értékeit `current_pose` topic-ként is hirdetjük. A frame transzlációja a topic pozícója, illetve a frame rotációja a topic orientációja.

Nagy transzformoknál az RVIZ megjelenítője nem működik pontosan (https://github.com/ros-visualization/rviz/issues/502). Mivel az ROS SI mértékegységeket használ, így métert is, a GPS esetén célszerű az UTM ([wikipedia-utm](https://en.wikipedia.org/wiki/Universal_Transverse_Mercator_coordinate_system)) koordinátarendszer használata. Ez értelemszerűen nagy értékű koorinátákkal számol. Ahhoz, hogy ezt az ellentmondást feloldjuk célszerű kisebb traszformokat megjeleníteni. Így például Győrhöz (`map_gyor_0`) és Zalához (`map_zala_0`) egy fix statikus transformot, hirdetni, amihez képest már szépen működik az RVIZ megjelenítője. A következő ábra ezt szemlélteti, illetve egy kicsit részletesebb szenzorrendszert mutat be.
![img](tf_examples03.svg)
*4. ábra - Az `rqt_tf_tree` által megjelenített TF fa*

Az ábrán csak a `map` `gps` transzform változó, a többi statikus. Statikus transzformot hirdetni launch fájlban például a `/base_link` és a `left_os1/os1_sensor` következőképp lehet (lásd 3. ábra)

``` python
Node(
    package='tf2_ros',
    executable='static_transform_publisher',
    name='ouster_left_tf_publisher',
    output='screen',
    arguments=['1.769', '0.58', '1.278','3.1415926535', '0', '0', '/base_link', 'left_os1/os1_sensor'],
),

```


Ugyanez régen `ROS 1`-ben:
``` xml
<node args="1.769 0.58 1.278 3.1415926535 0.0 0.0 /base_link left_os1/os1_sensor 50" name="ouster_left_tf_publisher" pkg="tf" type="static_transform_publisher"/>
``` 
Vagy ugyanez parancsként `ROS 2`-ben statikus transzformként quaternionokkal: 
``` c
ros2 run tf2_ros static_transform_publisher --x 1.769 --y 0.58 --z 1.278 --qx 0.0 --qy 0.0 --qz 1.0 --qw 0.0 --frame-id left_os1/os1_sensor --child-frame-id base_link
``` 

Vagy ugyanez parancsként `ROS 2`-ben roll/pitch/yaw: 
``` c
ros2 run tf2_ros static_transform_publisher --x 1.769 --y 0.58 --z 1.278 --roll 0.0 --pitch 0.0 --yaw 3.1415926535 --frame-id left_os1/os1_sensor --child-frame-id base_link
``` 

Ugyanez régen `ROS 1`-ben (50 ms = 20 Hz):
``` c
rosrun tf static_transform_publisher 1.769 0.58 1.278 3.1415926535 0.0 0.0 /base_link left_os1/os1_sensor 50  
``` 
Itt az utolsó argumentum `ROS 1`-nél 50 ms, tehát 20 Hz-en hirdette a ugyanazt a transzformációt. Ez nem a legszerencsésebb, az `ROS 2` ebben is fejlődött, ott elég egyszer hirdetni ugyanezt.

Példa a statikus transzform launch fájlra: [tf_nissanleaf_statictf.launch](https://github.com/szenergy/nissan_leaf_ros/blob/master/nissan_bringup/launch/tf_setup/tf_nissanleaf_statictf.launch)


# Quaternion (kvaterniók)
A roll pitch yaw alternatívája, a komplex számokhoz hasonló kiterjesztéssel. 

Demonstáció: [www.quaternions.online](https://quaternions.online/)

Előnyei:
- Numerikus stabilitás
- Gyors számítás
- Pontosság
- Gimbal lock

Hátrány:
- Nem intuitív az ember számára

$$tan(\frac{\pi}{2}) = \infty $$

<iframe width="560" height="315" src="https://www.youtube.com/embed/zjMuIxRvygQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Konvenciók

![rh](right_hand_rule01.svg)
![va](vehicle_axes01.svg)

# Olvasnivaló
- [articulatedrobotics.xyz/ready-for-ros-6-tf](https://articulatedrobotics.xyz/ready-for-ros-6-tf/)