
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

<br>

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

A tárgy összeállítása során olyan nemzetközileg is elismert egyetemek tananyagát vettük alapul, mint az MIT, ETH Zürich, TU München, Univerisity of Virginia vagy a Stanford University. A megfelelő licenc betarása mellett bizonyos tanangyag részeket át is vettünk, másokat inspirációként használtunk, [erről részletesen itt](https://github.com/sze-info/arj#acknowledgement) lehet olvasni. 2023-ban a tantárgy az ROS 2 Humble verzióját használja, ez a disztribúció 2027 májusáig támogatott.


![](https://raw.githubusercontent.com/sze-info/arj/main/docs/_images/technology01.svg)

<br><br><br>
## [sze-info.github.io/arj](https://sze-info.github.io/arj)

<br><br><br>