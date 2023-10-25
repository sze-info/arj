
# Start

- Tantárgy neve magyarul: **Autonóm robotok és járművek programozása**
- Tantárgy neve angolul: **Autonomous robots and vehicles software engineering**
- Neptun tárgykód: `GKNB_AUTM078`

{: .important-title }
> A tantárgy célja
>
> A tárgy sikeres teljesítése után a hallgatók átfogó képet kapnak az önvezető (autonóm) járművek szoftver moduljairól és további lényeges összetevőiről.
>
> C++ és python nyelven egyszerűbb ehhez kapcsolódó szoftvermodulokat tudnak fejleszteni. Ezen túlmenően a hallgatók képesek a megfelelő kód alkalmazására, valamint annak funkcióinak értelmezésére és továbbfejlesztésére ROS 2 keretrendszerben.


A tárgy összeállítása során olyan nemzetközileg is elismert egyetemek tananyagát vettük alapul, mint az MIT, ETH Zürich, TU München, Univerisity of Virginia vagy a Stanford University. A megfelelő licenc betarása mellett bizonyos tanangyag részeket át is vettünk, másokat inspirációként használtunk, [erről részletesen itt](https://github.com/sze-info/arj#acknowledgement) lehet olvasni. 2023-ban a tantárgy az ROS 2 Humble verzióját használja, ez a disztribúció 2027 májusáig támogatott.


{: .highlight }
A tárgyban bemutatott ismeretekre alapozva diplomamunka, szakdolgozat, projektmunka, TDK dolgozat is készíthető, illetve van lehetőség a kötelező szakmai gyakorlat teljesítésére is.
 
Érintett témakörök (nem a feldolgozás szerinti sorrendben)

- Bevezetés: Önvezető / autonóm járművek bevezetése: az aktuális helyzet, múlt és jövő. Szenzorok, aktuátorok kommunikációs technológiák. (LIDAR, radar, aktív és passzív kamera, GPS, odometria, IMU, CAN) Foxglove studio és saját mérések szemlétetésképp				
- Szoftverrendszer: Önvezető / autonóm járművek szoftverei: érzékelés, észlelés, tervezés, követés. Szimulációs technológiák, felhasználói felületek. Keretrendszerek: ROS/ROS2/MATLAB/LabVIEW szererepe, valós idejű rendszerek (FPGA, real-time operációs rendszerek).				
- Érzékelés: SLAM, objektumdetekció, objektumkövetés és előrejelzés. Padkadetekció, sávdetekció, úthibadetekció, jármű és gyalogosdetekció/követés stb. Mesterséges intelligencia (különösen neurális hálózatok) és hagyományos (pl C++ nyelven készült) algoritmusok előnyei hátrányai, fúziója.				
- Technológiai ismeretek: Linux, Git: Linux ismeretek: Terminal kezelése, Git kezelése, VS code, ROS telepítése				
- Technológiai ismeretek: ROS 2 alapok: topicok és üzenetek, MCAP (Rosbag) visszajátszása, Topicok kezelése, Topic tartalmának elérése pythonból, rviz, rqt_plot, MCAP (Rosbag) készítés. 
ROS 2 ökoszisztéma és fejlesztés ROS 2 node-ok készítése pythonban és C++-ban: ROS 2 node-ok, rqt_graph, Publisher / Subscriber node pythonban, Publisher / Subscriber node C++-ban. 
Első egyeztetés az egyéni projektfeladatról.
- ROS 2 programozás:ROS szenzoradatok feldolgozása C++ node-al: ROS node-ok írása, `visualization_msgs`, LIDAR szenzoradatok: `sensor_msgs/PointCloud2`, `sensor_msgs/LaserScan`, stb.				
- Szimuláció és szabályzás:	Szimuláció: ROS node-ok használata szimulációhoz (gazebo) F1/10, rviz, egyéni projektfeladatok véglegesítése. Tervezés blokk: trajektória tervezők típusai, kinematikai kihívások, Szabályzás blokk: járműmodellezés, szabályzók bemutatása, jármű- és aktuátorszintű szabályzás, a mozgás megvalósítása (fékrendszerek, kormányrendszerek…stb) 				
- Kitekintés, doktori kutatások, egyetemi hallgatói csapatok: Nissan Leaf, Lexus és Szenergy önvezető projektek, bemutatása, kiragadott kódrészletekkel
Érzékelés: pontfelhő kezelés vagy objektum detektálás kamera alapon
Észlelés / tervezés: útvonalmeghatározás, szabad terület meghatározás, trajektória tervezés
Szabályzás: zárthurkú modellezett jármű, szabályzó építése (pl PID vagy pure pursuit)				
- Mesterséges intelligencia: Önvezető / autonóm járművek szoftverei, összefoglalás, kitekintés neurális hálózatok (mesterséges intelligencia, AI)				
- Technológiai ismeretek: ROS 2	használata, újdonságai ROS-hez képest				
- Projektmunka:	Egyéni projektfeladat bemutatása				

2023/24 őszi félévében az `A2`-es teremben, illetve a `C100`-as gépteremben tartunk órákat.

## Elmélet

Óra | Dátum | Tananyag
-----|-----|-----
1 | szept. 6. | [Bevezetés](https://sze-info.github.io/arj/bevezetes/README.html): A tantárgy felépítése. Robotikai és önvezető járműves ismeretek. Érzékelés, észlelés, tervezés, szabályozás, aktuálás.
2 | szept. 13. | [ROS2 koncepciók](https://sze-info.github.io/arj/bevezetes/ros2.html): Egyetemi robotok és járművek ismertetése. `ROS 2` alapismeretek.
3 | szept. 20. | [Érzékelés](https://sze-info.github.io/arj/erzekeles/README.html): Kamera, LIDAR, GNSS (GPS), IMU, CAN szenzorok működése, jelfeldolgozása, főbb `ROS 2` topicok, `ROS 2` időkezelés.
4 | szept. 27. | [Féléves beadandó](https://sze-info.github.io/arj/feleves_beadando/): féléves beadandó ismertetése, osztályzási szempontok, ötletek, kérdések-válaszok
5 | okt. 4. | [Transzformációk](https://sze-info.github.io/arj/transzformaciok/README.html): Merev test mozgása, mátrix szorzás ismétlése, homogén koordináták szemléltetése rövid progamkódokkal, quaternion (kvaterniók) fogalma.
6 | okt. 11. | [Észlelés](https://sze-info.github.io/arj/eszleles/README.html): objektumfelismerés, objektumklasszifikáció, objektum követés és predikció, SLAM és LOAM
7 | okt. 25. | [Szimuláció](https://sze-info.github.io/arj/szimulacio/README.html): ROS 2 kompatibilis szimulátorok áttekintése (pl [Gazebo](http://gazebosim.org/), [Carla](https://carla.org/), [SVL](https://www.lgsvlsimulator.com/), [OSSDC SIM](https://github.com/OSSDC/OSSDC-SIM), [AirSim](https://microsoft.github.io/AirSim), [AWSIM](https://tier4.github.io/AWSIM), [CoppeliaSim](https://www.coppeliarobotics.com/coppeliaSim), [MVSim](https://mvsimulator.readthedocs.io/))
8 | nov. 8. | [Szabályozás](https://sze-info.github.io/arj/szabalyozas/README.html)
9 | - | [Mesterséges intelligencia](https://sze-info.github.io/arj/mesterseges_intelligencia/README.html)
10 | - | [Tervezés](https://sze-info.github.io/arj/tervezes/README.html)


## Gyakorlat

Óra | Dátum | Tananyag
-----|-----|-----
1| szept. 6. | [Bevezetés](https://sze-info.github.io/arj/bevezetes/practice.html) + [Linux](https://sze-info.github.io/arj/bevezetes/linux.html) + [Géptermi ismeretek](https://sze-info.github.io/arj/bevezetes/gepterem.html): WSL2 használata Windows operációs rendszeren. Géptermi alapismeretek. Linux parancsok, amelyek szükségesek lehetnek a későbbiekben.
2| szept. 13. | [Telepítés](https://sze-info.github.io/arj/telepites/ros_humble.html)+ [Fejlesztőkörnyezet beállítása](https://sze-info.github.io/arj/bevezetes/vscode.html) + [ROS2 kommunikáció](https://sze-info.github.io/arj/bevezetes/ros2gyak.html): Első `ROS 2` node-ok, ROS parancsok használata, build és source.
3| szept. 20. | [Érzékelés gyakorlat](https://sze-info.github.io/arj/erzekeles/practice.html): Szenzor adatok jellemzőbb formátumai: `sensor_msgs/PointCloud2`, `sensor_msgs/Image`, `geometry_msgs/Pose`, stb. Bag `.mcap` fájlok kezelése, lejátszása. Egyszerű pacakge készítése, amely pozíció adatokra iratkozik fel. 
4| szept. 27. | [Verziókezelés, Git](https://sze-info.github.io/arj/onallo/ros2git.html), [Copilot](https://sze-info.github.io/arj/bevezetes/copilot.html), [vs code](https://sze-info.github.io/arj/bevezetes/vscode.html), [ROS 2 launch](https://sze-info.github.io/arj/ros2halado/ros2launch.html): Copilot használata ROS 2 fejlesztéshez, Template repo ismertetése, használata, launch fájlok írása python nyelven
5| okt. 4. | [Transzformációk gyakorlat](https://sze-info.github.io/arj/transzformaciok/practice.html): Node létrehozása, amely transzformációkat hirdet. Markerek megjelenítése, launch önálló feladat.
6| okt. 11. | [Észlelés gyakorlat](https://sze-info.github.io/arj/eszleles/practice.html): egyszerű LIDAR szűrés, X, Y és Z koordináták szerint.
7| okt. 25. | [Szimuláció bevezetés](https://sze-info.github.io/arj/szimulacio/gazebo_fortress.html): Gazebo Fortress és ROS 2, [szimuláció gyakorlat](https://sze-info.github.io/arj/szimulacio/gyakorlat.html): saját robotszimuláció létrehozása.
8| nov. 8. | [Szabályozás gyakorlat](https://sze-info.github.io/arj/szabalyozas/ros2practice.html)
9| - | [Mesterséges intelligencia gyakorlat](https://sze-info.github.io/arj/mesterseges_intelligencia/practice.html)
10| - | [Tervezés gyakorlat](https://sze-info.github.io/arj/tervezes/practice.html)


Oktatók | | | .
-----|-----|-----|-----
<img src="https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/okt_he01.png" width="80px"/> | Dr. Horváth Ernő <br/> <i>Tárgyfelelős</i> <br/>[github.com/horverno](http://github.com/horverno) | <img src="https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/okt_ba01.png" width="80px"/>| Dr. Ballagi Áron <br/> <i>Tematika, nem oktat</i><br/> [github.com/aronball](http://github.com/aronball)
<img src="https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/okt_kr01.png" width="80px"/> | Krecht Rudolf <br/> <i>Szimuláció, robotika</i> <br/> [github.com/rudolfkrecht](http://github.com/rudolfkrecht) | <img src="https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/okt_um01.png" width="80px"/> | Unger Miklós <br/> <i>Környezetérzékelés</i> <br/> [github.com/umiklos](http://github.com/umiklos)
<img src="https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/okt_ig01.png" width="80px"/>| Ignéczi Gergő <br/> <i>Szabályozástechnika</i> <br/> [github.com/gfigneczi1](http://github.com/gfigneczi1)|<img src="https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/okt_mn01.png" width="80px"/> | Markó Norbert <br/> <i>AI, neurális hálók</i>  <br/> [github.com/norbertmarko](http://github.com/norbertmarko)


![](https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/technology01.svg)

<br><br><br>
<center>
<h2><a href="https://sze-info.github.io/arj/">sze-info.github.io/arj</a></h2>
</center>

<br><br><br>