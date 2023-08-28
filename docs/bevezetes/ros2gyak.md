---
layout: default
title: ROS 2 gyakrolat
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


# `ROS 2` bevezetés és gyakorlat

## Emlékeztető

Pár alapfogalom az [előző](https://sze-info.github.io/arj/bevezetes/ros2.html) alkalomról: 

- **Node**: Gyakorlatilag ROS *program*ot jelent. (pl. `turtlesim_node`, `cmd_gen_node`, `foxglove_bridge`)
- **Topic** (topik): Nevekkel ellátott kommunikációs csatorna. (pl. `/turtle1/cmd_vel`, `/turtle1/pose`, `/raw_cmd`)
- **Message** (üzenet): (pl. `std_msgs/msg/Bool`, `geometry_msgs/msg/Twist`, `turtlesim/msg/Pose`)
- **Package** (csomag): ROS programok (node-ok) gyűjteménye (pl. `turtlesim`, `arj_intro_cpp`, `arj_transforms_cpp`)
- **Launch fájlok**: Több node paraméterezett elindítására alkalmas (pl. `multisim.launch.py`, `foxglove_bridge.launch.xml`, `foxglove_bridge.launch.py`)
- **Publish / subscribe**: Üzenetekre történő publikálás és feliratkozás. 
- **Build**: A package forráskódjából futtatható állományok készítésének folyamata. ROS2-ben a `colcon` az alapértelmezett build eszköz. 

# `1.` feladat - Node és publish

Nyissunk két terminált. Az első terminálból indítsuk a beépített `turtlesim_node` szimulátort, ami a `turtlesim` package-ben található.

``` r
ros2 run turtlesim turtlesim_node
```

*Megjegyzés*: ha esetleg valamiért hiányozna, telepíthető a `sudo apt install ros-humble-turtlesim` paranccsal.

A második ablakból publikáljunk egy parancsot, melynek hatására körbe fordul:

``` r
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 0.5, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.2}}'
```

<center><img src="turtlesim01.gif" width="80%" /></center>

A háttérben a `turtlesim_node` node (kerek jelölés) feliratkozik a `/turtle1/cmd_vel` topicra (szögletes jelölés), ennek hatására indul a mozgás. 


```mermaid
flowchart LR

C[ /turtle1/cmd_vel] -->|geometry_msgs/msg/Twist| S(turtlesim_node)

```

Ahogy a flowcharton is látszik, a `/turtle1/cmd_vel` típusa `geometry_msgs/msg/Twist`. Ezt a következő parancsból tudhatjuk meg:

``` r
ros2 topic type /turtle1/cmd_vel
```

A `geometry_msgs/msg/Twist` a üzenet strutúráját pedig ez a parancs adja:

``` r
ros2 interface show geometry_msgs/msg/Twist
```

``` r
Vector3  linear
        float64 x
        float64 y
        float64 z
Vector3  angular
        float64 x
        float64 y
        float64 z
```
Az összes topic-ot így lehet listázni:

``` r
ros2 topic list
```

Az adott topic tartalmát a következőképp lehet kiíratni:

``` r
ros2 topic echo /turtle1/pose
ros2 topic echo /turtle1/pose --csv
ros2 topic echo /turtle1/pose --csv > turtle_data_01.csv
```

# Workspace és build tudnivalók
Első lépésként az `ls | grep ros2` parancs segítségével ellenőrizzük, hogy létezik-e a workspace a home directoryban. A tantárgyban a workspace-t `ros2_ws`-nek nevezzük. A név igazából nem számít, de a legtöbb tutorial is ezt a nevet használja, így mi is követjük ezt a hagyományt. Több workspace is használható egyidejüleg, külön source-olható, nagyobb rendszereknél ez kényelmes megoldás lehet. Mi egyelőre maradunk az egytelen `ros2_ws`-nél. Ha nem létezne a `mkdir -p gyak_ws/src` parancs segítségével készíthetjük el a workspace és a source mappákat.

## Colcon
A legfontosabb parancs talán a `colcon build`. Említésre méltó még a `colcon list` és a `colcon graph`. Előbbi listázza az elérhető packageket, utóbbi pedig a függőségekről ad gyors nézetet.

A `colcon build` számos hasznos kapcsolóval érkezik:
- `--packages-select`: Talán az egyik leggyakrabban használt kapcsoló, utána meggadhatunk több package-t, amit buildelni szeretnénk. Ha nincs megadva, akkor az alapértelmezett, hogy a teljes workspace-t buildeli. A gyakorlatban lesz is egy `colcon build --packages-select arj_intro_cpp arj_transforms_cpp` parancs, ez a két arj package-t buildeli.
- `--symlink-install`: A fájlok forrásból való másolása helyett használjon szimbolikus hivatkozásokat. Így elkerülhető, hogy pl. minden egyes launch fájl módosítás esetén újra kelljen buildelni a package-t.
- `--parallel-workers 2`: A párhuzamosan feldolgozható feladatok maximális száma, ebben az esetben `2`. Ha nincs megadva, akkor az alapértelmezett érték a logikai CPU magok száma. Akkor érdemes korlátozni, ha a build nem fut végig erőforrás hiány miatt. 
- `--continue-on-error`: Nagyobb build esetén, ne álljon meg az első hibás package után. Így ha 100 packageből 1 nem működne, akkor is 99 buildelődik. Ha ez nincs megadva, akkor 0 és 99 közötti package buildelődik, a függőségek és egyéb sorrendiségek alapján. 

## Source
Ahhoz hogy az ROS2 futtatható fájlainkat valóban el tudjuk indítani, be kell állíani a be a környezetet (úgynevezett source-olás), tehát meg kell adni a bash számára, hogy hol keresse az adott futtatható fájlokat, azoknak milyen függőségei vannak stb. Ez egyszerűbb, mint hangzik, csak egy `source <útvonal>/<név>.bash` parancsot kell kiadni. Korábban írtuk, hogy a worksapce neve nem számít, és valóban, a source megadása után mindegy, hogy fizikailag hol található a futtatható állomány, kényelmesen elindítható egy paranccsal bármelyik mappából. 
Mivel a packagek különböző workspace-eken belül egymásra is épülhetnek, az ROS2 bevezette az overlay / underlay elvet. Ez azt jelenti, hogy egyik workspace buildelésekor egy másik workspace már be volt source-olva, annak valamely package-e függ a az előzőleg lebuildelt package-től. Tehát annak funkcionalitása, kódja szükséges a ráépülő package-nek. Ennek megfeleően a source-olás is kétféle lehet:
- A `local_setup.bash` script csak a jelenlegi workspace-ben állítja be a környezetet (source-ol). Tehát nem source-ol szülő (függő) workspace-t.
- A `setup.bash` szkript viszont a `local_setup.bash` parancsfájlt adja az összes olyan workspace-hez, amely a munkaterület létrehozásakor függőség volt. 

{: .highlight }
A tantárgyban nem kell ilyen összetett rendszereket használni, legtöbbször egy `ros2_ws` is elég.

# `2.` feladat - Package build és használat

[docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html)

Nyissunk négy terminált. Az első terminálból most is indítsuk a beépített `turtlesim_node` szimulátort, ami a `turtlesim` package-ben található.

``` r
ros2 launch turtlesim turtlesim_node
```

A második terminálban klónozzuk, majd buildeljük a példa package-t.

``` r
cd ~/ros2_ws/src
git clone https://github.com/sze-info/arj_packages
colcon build --packages-select arj_intro_cpp
```

A harmadik terminálban futtassuk a `cmd_gen_node` ROS node-ot.

Először `source`-olnunk kell, ha saját package-ket használunk:

``` r
source ~/ros2_ws/install/setup.bash
```

Ezután már futtatható a node:

``` r
ros2 run arj_intro_cpp cmd_gen_node
```

A következőképp mozog most a teknős:

<center><img src="turtlesim02.gif" width="80%" /></center>

Forráskódja elérhető a [github.com/sze-info/arj_packages](https://github.com/sze-info/arj_packages/blob/main/arj_intro_cpp/src/cmd_gen_node.cpp) repon.A lényeg, hogy a `loop` függvény 5 Hz (200 ms) frekvencián fut le, és 

``` cpp
void loop()
{
  // Publish transforms
  auto cmd_msg = geometry_msgs::msg::Twist();
  if (loop_count_ < 20)
  {
    cmd_msg.linear.x = 1.0;
    cmd_msg.angular.z = 0.0;
  }
  else
  {
    cmd_msg.linear.x = -1.0;
    cmd_msg.angular.z = 1.0;
  }
  cmd_pub_->publish(cmd_msg);
  loop_count_++;
  if (loop_count_ > 40)
  {
    loop_count_ = 0;
  }
}
```
{: .important-title }
> Python megfelelője
>
> A C++ kód python verziója szintén elérhető a [github.com/sze-info/arj_packages](https://github.com/sze-info/arj_packages/blob/main/arj_intro_py/arj_intro_py/cmd_gen_node.py) címen. Érdemes összehasonlítani a C++ és a python kódokat.

Nézzük meg az utolsó terminálban a Foxglove segítségével az élő adatokat (itt se felejtsük a `source`-t):

``` r
ros2 launch arj_intro_cpp foxglove_bridge.launch.py
```

```mermaid
flowchart LR

C[ /turtle1/cmd_vel] --> S(turtlesim_node)
C[ /turtle1/cmd_vel] --> F(foxglove_bridge)
G(cmd_gen_node)--> C

```
Mindehárom node-ot egyben a következőképp indíthatjuk:

``` r
ros2 launch arj_intro_cpp turtle.launch.py
```

# `3.` feladat - Saját package készítése

A feladat a hivatalos ROS2 dokumentáción alapul: [docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html). Készítsük el a `my_package` nevű ROS 2 package-t.


{: .important-title }
> Python megfelelője
>
> Jelenleg C++ package-t készítünk, de az [eredeti]((https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html)) tutorial is taralmazza a CMake(c++) package Python megfelelőit.

Első lépés, hogy a a workspace `src` mappájába lépjünk:

``` r
cd ~/ros2_ws/src
```

Készítsünk egy `my_package` nevű package-t és egy `my_node` navű node-ot.

``` r
ros2 pkg create --build-type ament_cmake --node-name my_node my_package
```

Buildeljük a szokásos módon:

``` r
cd ~/ros2_ws
colcon build --packages-select my_package
```


Majd source:

``` r
source ~/ros2_ws/install/setup.bash
```

És már futtatható is:

``` r
ros2 run my_package my_node


hello world my_package package
```

Vizsgáljuk meg a `my_package` tartalmát!

``` r
ls -R ~/ros2_ws/src/my_package

/home/he/ros2_ws/src/my_package:
  CMakeLists.txt  include  package.xml  src
/home/he/ros2_ws/src/my_package/include:
  my_package
/home/he/ros2_ws/src/my_package/include/my_package:
  [empty]
/home/he/ros2_ws/src/my_package/src:
  my_node.cpp
```

``` r
tree ~/ros2_ws/src/my_package

my_package
├── CMakeLists.txt
├── include
│   └── my_package
├── package.xml
└── src
    └── my_node.cpp
```

``` cpp
cat ~/ros2_ws/src/my_package/src/my_node.cpp


#include <cstdio>

int main(int argc, char ** argv)
{
  (void) argc;
  (void) argv;

  printf("hello world my_package package\n");
  return 0;
}
```

# Források
- [docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html)
- [docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html)