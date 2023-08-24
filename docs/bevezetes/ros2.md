---
layout: default
title: ROS 2 alapfogalmak
parent: Bevezetés
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


# `ROS 2` alapfoglamak

[ROS verziók és telepítés](https://sze-info.github.io/arj/telepites/README.html)



Az `ROS 2`, a `ROS` legújabb kiadása, olyan szoftverkönyvtárak és eszközök készlete (middleware), amelyek segítenek robotalkalmazások fejlesztésében. Definíció szerint a middleware egy szoftver komponenseket összekötő szoftver. Ez egy olyan réteg, amely az operációs rendszer és az alkalmazások között helyezkedik el az elosztott számítógépes hálózat mindkét oldalán. Az `ROS 2` megengedő, nyílt forráskódú, [Apache 2.0](https://choosealicense.com/licenses/apache-2.0/) licenszelést használ. 

A `ROS` 2007-es kiadása óta inkrementális frissítéseken esett át, tehát fundamentális változások nem, nagyobb fejlesztések viszont folyamatosan történtek. 2017-ben jött rá a robotikai közösség, hogy olyan alapvető limitációi vannak az eredeti 2007-es elképzelésnek, amit ilyen inkrementális módon sajnos nem lehet javítani. Így végül a Noetic Ninjemis (2025-ig támogatva) az `ROS 1` utolsó kiadása, helyette párhuzamosan elkezdték fejleszteni az `ROS 2`-t. Ez egyben azt is jelenteti, hogy a korábbi forráskódokat nehezebben lehet portolni az új verzióra, cserébe rengeteg újdonságot, javítást, támogatást kaphatunk a fejlesztenő robotok, járművek számára.


A fentiek hatására tehát az `ROS 2` átlépett az akadémiai kutatások világából az ipari fehasználásra. Érdekesség, hogy a NASA VIPER nevű holdjárója is `ROS 2`-t futtat. Emellett olyan autóipari óriások is használják, mint a Bosch, a BMW vagy a Volvo. Robotikai cégek közül pedig számos további példát lehetne hozni. Linkek: [www.nasa.gov/viper/lunar-operations](https://www.nasa.gov/viper/lunar-operations), [rosindustrial.org/ric/current-members](https://rosindustrial.org/ric/current-members/), [www.bosch.com/stories/bringing-robotics-middleware-onto-tiny-microcontrollers](https://www.bosch.com/stories/bringing-robotics-middleware-onto-tiny-microcontrollers/). 

![ROS 2 in space](ros2space01.png)

Kép forrása: [Robot Operating System 2: Design, Architecture, and Uses In The Wild:
Steve Macenski et al.](https://arxiv.org/pdf/2211.07752.pdf)

## Különbségek az `ROS 1` és `ROS 2` között 

- *Változások a Middleware-ben*  
  A `ROS 1` a Master-Slave architektúrát és az XML-RPC middleware-t használja. A `ROS 2` ezzel szemben a  Data Distribution Service (DDS) használ, amely nagyobb hatékonyságot és megbízhatóságot, alacsony késleltetést és skálázhatóságot, valamint konfigurálható szolgáltatásminőségi (QoS) paramétereket biztosít. Többek között így nem kell `roscore`-t indítani. Az XML-RPC jobb az egyszerű távoli eljáráshívásokhoz, míg a DDS hozzáadott komplexitása lehetővé teszi, hogy jobban támogassa a valós idejű rendszereket.
- *Változások a ROS API-ban*  
  A `ROS 1` két különálló könyvtárral rendelkezik: a C++ nyelvhez készült `roscpp` és a Pythonhoz készült `rospy`. Ezek nem teljesen azonosak egymással a funkciók tekintetében. Ezzel szemben a `ROS 2` egy C nyelven írt alapkönyvtárral - `rcl` (ROS klienskönyvtár) - rendelkezik, amelyre könyvtárak épülnek. Ez biztosítja, hogy az alapvető funkciók hamarabb elérhetők legyenek a különböző API-kban. Ez az egyik fő oka annak, hogy a `ROS 2` a korábbi Pythonon és a C++-on kívül több nyelvi támogatást is képes nyújtani: például  [rclada](https://github.com/ada-ros/rclada) Ada, [rclcpp](https://github.com/ros2/rclcpp) C++, [rclgo](https://github.com/juaruipav/rclgo) Go,  [rclpy](https://github.com/ros2/rclpy) Python, [rcljava](https://github.com/esteve/ros2_java/tree/master/rcljava)  Java, [rclnodejs](https://github.com/RobotWebTools/rclnodejs) Node.js,  [rclobjc](https://github.com/esteve/ros2_objc) Objective C (iOS),  [rclc](https://github.com/ros2/rclc) C, [ros2_rust](https://github.com/ros2-rust/ros2_rust) Rust, [ros2_dotnet](https://github.com/esteve/ros2_dotnet) .NET, [ros2cs](https://github.com/RobotecAI/ros2cs) ros2_dotnet alternatíva C# nyelven.
 
- *Változások az adatformátumban*  
  A `ROS 2` az `MCAP` formátumot használja, ami nem dedikáltan az ROS saját formátuma, hanem egy nyílt forráskódú konténerfájl-formátum multimodális log-adatokhoz. Támogatja az időbélyegzővel ellátott, előre sorba rendezett adatokat, és ideális a pub/sub vagy robotikai alkalmazásokban való használatra. Bővebben: [mcap.dev](https://mcap.dev/)

## Pár hasznos újítás
- *Valós idejű feldolgozás*  
  A fenti funkciók összegzése, valamint a DDS használata lehetővé teszi, hogy a `ROS 2` kiválóan alkalmas legyen a valós idejű (real time) feldolgozásra, különösen akkor, ha determinisztikus, alacsony késleltetésű kommunikációra van szükség.
- *QoS: Quality of Service* 
  A `ROS 2` lehetővé teszi az adatáramlás konfigurálását, ami befolyásolja az adatok küldésének és fogadásának módját. Ez magában foglalja az üzenetek megbízhatóságára, határidejére és prioritására vonatkozó beállításokat, amelyek biztosíthatják, hogy a kritikus üzenetek időben kézbesítésre kerüljenek. 
- *Többszálú végrehajtás*  
  A `ROS 2` támogatja a több csomópont valóban párhuzamos futtatását, így a modern többmagos processzorok sokkal jobban kihasználhatók, mint a `ROS 1` esetében. 

## Egyéb változások
- A Catkin eltűnt, helyére az Ament (Colcon) lépett, mint build rendszer. Az overlay-ek lehetővé teszik egy másodlagos munkaterület létrehozását, amely nem befolyásolja az elsődleges munkaterületet - ez akkor hasznos, ha új csomagokkal kell kísérletezni, de úgy, hogy ez ne befolyásolja az alapkonfigurációt (ezt "underlay"-nek hívják).
- A `ROS 2` visszafelé nem kompatibilis a `ROS 1`-gyel. Következésképpen a `ROS 1` csomagok valószínűleg nem fognak működni a `ROS 2`-vel, és átdolgozást igényelnének, és más szoftverek, amelyeket a `ROS 1`-gyel szoktál használni, már nem fognak működni.
- A `ROS 1` elsősorban az Ubuntu számára készült. A `ROS 2` fut MacOS, Windows, Ubuntu és más (akár Real-Time) operációs rendszereken is.

## Verziók

[ROS verziók és telepítés](https://sze-info.github.io/arj/telepites/README.html)

![Alt text](ros2distros.svg)

Distrok százalékos megoszlása az időben: [metrics.ros.org/rosdistro_rosdistro.html](https://metrics.ros.org/rosdistro_rosdistro.html)

Humble
{: .label .label-yellow }

A `Humble Hawksbill` vagy röviden `Humble` egy long term support (LTS) release, 5 évig támogatott (2022 májusától 2027 májusáig)

További release-ek: [docs.ros.org/en/humble/Releases.html](https://docs.ros.org/en/humble/Releases.html)

<iframe width="560" height="315" src="https://www.youtube.com/embed/0aPbWsyENA8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# Koncepciók

## Nodes 

A node legegyszerűbben fogalmazva ROS programot (magyarosítva csomópont) jelent. Jellemzőik:

- "Végrehajthatóak" (c++ / py).
- Minden node egy folyamat
- ROS kezeli a szálakat (threading).
- Egy node belsejében több szál is lehet.
- publish/subscribe topic-okra. 
- Több node is "közzétehet" egy topicra, és egy node több topicot is "meghallgathat".

## Topics

- A node-ok közötti információ áramlásért felel.
- Minden topic típusát az "üzenet" határozza meg
- A node-ok között megengedett a "many-to-many" kommunikáció 

## Messages

- Egy topic tartalmát és szerkezetét az üzenet határozza meg
- Alkalmazásprogramozási interfész (API) a Node-ok számára .msg  kiterjesztésű fájlokban vannak definiálva

### Üzenetek típusai 
- Primitive built-in types (std_msgs)
  - `bool`, `string`, `float32`, `int32`, `…`
- Higher-level built in types:
  - `geometry_msgs`: `Point`, `Polygon`, `Vector`, `Pose`, `PoseWithCovariance`, `…`
  - `nav_msgs`: `OccupancyGrid`, `Odometry`, `Path`, `…`
  - `sensors_msgs`: `Joy`, `Imu`, `NavSatFix`, `PointCloud`, `LaserScan`, …
- Támogatottak továbbá:
  - Konstansok
  - Felsorolások
  - Beágyazott definíciók

Példa:

``` c
$ ros2 interface show geometry_msgs/msg/Point
float64 x
float64 y
float64 z
```

``` c
$ ros2 interface show std_msgs/msg/Header
uint32 seq
time stamp
string frame_id
```

A `Header` és a`Point` a típusból épül fel a `PoseStamped` típus struktúrája:

``` c
$ ros2 interface show geometry_msgs/msg/PoseStamped
std_msgs/Header header
  uint32 seq
  time stamp
  string frame_id
geometry_msgs/Pose pose
  geometry_msgs/Point position
    float64 x
    float64 y
    float64 z
  geometry_msgs/Quaternion orientation
    float64 x
    float64 y
    float64 z
    float64 w
```

## Publishing / Subscribing

A következőkben az `urban_road_filt` nevű node a `points` adatokra iratkozik fel, ami `PointCloud2` típusú, és hirdet `PointCloud2`, `MarkerArray` típusú üzeneteket:

```mermaid
flowchart LR

P[points] -->|sensor_msgs/PointCloud2| U(urban_road_filt)
U --> |sensor_msgs/PointCloud2| A[curb]
U --> |sensor_msgs/PointCloud2| B[road] 
U --> |sensor_msgs/PointCloud2| C[road_probably]
U --> |sensor_msgs/PointCloud2| D[roi]
U --> |visualization_msgs/MarkerArray| E[road_marker]

```

## Parameters
- Publish/Subscribe segítségével nem írható le minden
- A Node-oknak néha szükségük lehet paraméterezésre
- Paraméterek lehetnek:
  - Vezérlő típusa
  - Szín küszöbértékek; 
  - Kamera felbontás, stb

## Launch fájlok

Több node (ROS program) kötegelt végehajtása. Megtartva az `ROS 1`konvenciókat lehet XML formátumú fájl, amely az ROS szinte minden aspektusát / műveletét meghatározhatja. Újabban viszont ezek `python` fájlok is lehetnek, így sokkkal nagyobb szabadságunk van. Node indítás, paraméterek beállítása / betöltése, topic leképezése, parancssori argumentumok átadása.

Erről egy rövid videó:

<iframe width="560" height="315" src="https://www.youtube.com/embed/PqNGvmE2Pv4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>