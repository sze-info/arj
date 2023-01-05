---
layout: default
title: Szabályozás
has_children: true
---

# Szabályozás

Tartalom:
1. Motiváció a zárthurkú szabályozás mögött - bevezetés
2. Architekurális áttekintés, visszatekintés
3. Szabályzási alapok
4. Járműszabályzási megoldások
5. Aktuátorok

## 1. Motiváció a zárthurkú szabályozás mögött - bevezetés

Egy rendszer tervezett célállapotát úgy érhetjük el, ha célérték figyelembevételével a rendszerbe beavatkozunk. Például egy jármű esetén a célsebességet a gáz és fékpedál mozgatásával, közvetetten a motor nyomatékának és a fékerőnek a változtatásával érhetjük el. Kezdeti példának tekintsünk egy járművezetőt: a vezető általában tisztában van a megengedett legnagyobb sebességgel, ekörül alkalmaz egy számára megfelelő tűrési sávot. Ezen belül meghatároz egy számára biztonságos és kényelmes sebességet, amit tartani szeretne. A vezető addig gyorsít, amíg el nem éri a kívánt sebességet, majd a gázpedált kicsit visszábbengedve tartja a sebességet.
Helyes az az állítás, hogy amennyiben elértük a kívánt sebességet, a pedált elengedhetjük, nincs több dolgunk? Természetesen nem. Miért nem? Hisz a jármű a veszteségekből adódóan lassulni fog, lejtő esetén akár gyorsulhat is. Ahhoz, hogy tartani tudjuk a sebességet a vezetőnek folyamatosan figyelnie kell a jármű mozgását illetve a környezetet, és ez alapján beavatkoznia a pedálokon keresztül.
Ez az egyszerű példa legtöbb részét a tervezési és szabályzási komponenseknek lefedi. A szabályzás téren a következő megfigyeléseket tehetjük meg:
- a vezető érzékeli a jármű akutális állapotát (valamilyen pontossággal)
- a vezető tudja, hogyha beavatkozik a gáz- vagy fékpedállal, milyen hatást fog elérni, azaz mennyire lassul vagy gyorsul az autó (valamilyen pontossággal)
- a vezető különösebb érzékeléstől függetlenül (tehát anélkül, hogy tudná, pl. milyen erő hat az autóra) nagyjából (!) meg tudja határozni a kívánt gázpedál állást (nagy hibával)

Az utolsó pontot szokás ún. **előrecsatolt ágnak** is hívni (lásd 3. alfejezet), avagy **nyílthurkú szabályzásnak** (amennyiben nincs semmilyen infónk az érzékelésről).
Egy nagyon durva összehasonlítást tartalmaz az 1. Ábra. Képzeljünk el egy helyzetet, amikor nincs információnk arról, milyen gyorsan megy a jármű, csupán a pedált tudjuk kezelni. A feladat hogy álló helyből gyorsítva elérjük a 90 km/h sebességet, majd ezt a sebességet tartsuk. Ha nem tudjuk, épp mennyivel megyünk, honnan tudjuk, hogy kell-e még nyomni a pedált avagy nem? Ilyenkor arra tudunk alapozni, hogy ismerjük az útviszonyokat (pl. sík talaj, aszfaltos út), ismerjük az autónkat (milyen motor, milyen nyomatékviszonyok...stb.). Így *nagyjából* meg tudjuk határozni, milyen hosszan kell nyomni a gázpedált, majd amikor *nagyjából* elértük a sebességet, mennyire kell ott tartani a pedálon a lábunkat, hogy ne lassuljunk, ne gyorsuljunk. Az eredmény valószínűleg hasonlítani fog a kívánt sebességgörbéhez, de messze nem lesz pontos. Hiszen pontatlanul ismerjük az utat, a saját autónkat, befolyásolja a gyorsulást a hőmérséklet, emelkedő/lejtő, szembeszél...stb. Ezért általában nem, vagy nem csak ezt a **nyílthurkú** megközelítést használjuk, hanem minél pontosabb érzékelők segítségével *korrigáljuk* az általunk előre meghatározott pedál állásokat, és ezzel bármilyen **zavar** hatását le tudjuk kezelni. Ez utóbbi megközelítést nevezzük **zárthurkú szabályzásnak**, az érzékelésből kapott információkat pedig **visszacsatolásnak**.

A magyar terminológiában szokás a *nyílthurkú szabályzást* **vezérlésnek**, a *zárthurkú szabályzást* röviden csak **szabályzásnak** hívni. A kettőt együtt pedig **irányításnak**. Az angol terminológia ezzel szemben mindkettőt **controlnak**, azon belül is **closed loop controlnak** illetve **open loop controlnak** hívja. A visszacsatolást **feedbacknek**, az előrecsatolást **feed-forwardnak** szokás hívni. 

![image info](../_images/control/arj_control_01.png)

*1. Ábra: a zárthurkú szabályozás mögötti motiváció. Forrás: Autonomous Driving Software Engineering - Lecture 08: Control*

## 2. Architekturális áttekintés, visszatekintés

Ahogyan azt a korábbi fejezetekben is láttuk, a teljes járműirányítási lánc moduláris. A legfőbb feladatok:
- érzékelés
- észlelés
- tervezés
- szabályzás

Ez a fejezet a szabályzásról szól. A szabályzások alapjairól a 3. alfejezetben olvashatunk. A szabályzó rétegnek a tervezés biztosítja a bemenetet. Így - némileg kiegészítve - vessünk egy pillantást az architektúrára! Ezt a 2. Ábra mutatja.

![image info](../_images/control/arj_control_02.svg)
*2. Ábra: a legfőbb tervezési és szabályzási rétegek az architektúrában.*

A szabályzó réteg általában több szinre bomlik. Minimum két ilyen szintet megkülönböztetünk:
- járműszintű szabályzás
- aktuátor szabályzás

Mindkettő rétegnek meg van a feladata. A járműszintű szabályzás feladata, hogy a járművet a tervezett trajektórián végigvezesse a megfelelő sebességgel. Ehhez a jármű szintjén megfogalmazott célértékeket határoz meg. Ökölszabályként elfogadhatjuk, hogy azok a mennyiségek a járműszintűek, amelyek még nem kapcsolódnak egy kimondott aktuátorhoz. Azaz, pl. minden autóra jellemző annak gyorsulása vagy szögsebessége, attól függetlenül, milyen hajtással (elektromos, hibrid, belsőégésű) vagy kormányzással (elektromos szervó, hidraulikus szervó, lánctalpas...stb.) rendelkezik. Általában ez a szabályzó réteg a "leglassabb", beágyazott környezetben a ciklus idő 10-50ms.
Az aktuátot szabályzás feladata, hogy a járműszintű mennyiségeket lebontsa és megvalósítsa az aktuátorokon keresztül. Pl. a hosszirányú gyorsulást befolyásolhatjuk a motoron és a féken keresztül. A motor esetében a gyorsulást a motor nyomatékának szabályzásával érjük el, amit pedig a fojtószelep állásával érünk el. A fojtószelep állását pedig az azt mozgató pl. szervómotor pozíciójának, voltaképp a szervómotor kapocsfeszültségének szabályzásával érünk el. A fékrendszer esetén a fékerőt tudjuk befolyásolni, ami egyben a féknyomás szabályzását jelenti (pl. az ESP szelepein keresztül), amit pedig a hidraulikus fékrendszer motor szivattyújának nyomásával érhetünk el. Ezt pedig a szivattyú motorjának fordulatszámával tudjuk befolyásolni, ami megint a motor kapocsfeszültségének a szabályzásával érünk el...stb. Láthatjuk, hogy aktuátoroktól függően itt több (akár 4-6) egymásba ágyazott szabályzó hurokról beszélünk. 
Belülről kifelé érdemes a problémát megközelíteni, ahogy a legbelső szabályzás ciklusideje akár 1 ms (vagy az alatti) lehet, még ahogy kintebb lépünk ez nő, 5-10-20ms tartományban. 
Fontos, hogy automatizált vezetési rendszerek mozgásszabályzása esetén az aktuátor szabályzó rétegeket sokszor nem vesszük figyelembe, hanem feltételezzük, hogy azok teljesítménye, pontossága, sebessége megfelelő, "közel ideális". Persze a gyakorlatban ezt sokszor nehéz elválasztani, de arra törekszünk, hogy csak a járműszintű szabályzást kelljen megtervezni.

A legfelső szintű szabályzás feladata tehát az, hogy a jármű a trajektóriát lekövesse. Általában két dimenziót különböztetünk meg:
- hosszirányú mozgás: a jármű hosszirányú gyorsulásának, és végül a fék és motorerők meghatározása.
- keresztirányú mozgás: a jármű célszögesebességének, és végül a kormányszögnek a meghatározása.

A trajektória általában egy időben leírt sebességtarjektória illetve egy térben megadott célpont halmaz, azaz egy görbe (vagy annak egy reprezntációja). A cél, hogy a görbét minél kisebb pozícióhibával lekövessük, illetve a sebességet minél pontosabban tartsuk. Mindezt úgy, hogy ne lépjünk át semmilyen határértéket (pl. túl nagy gyorsulás, pályaelhagyás...stb).

Mielőtt rátérnénk a létező járműszabályzási megoldásokra, a 3. alfejezetben áttekintést adunk a szabályzási alapokról.

## 3. Szabályzási alapok

Ebben a fejezetben ismertetjük a szabályzástechnikához kapcsolódó alapfogalmakat, és erre példákat is hozunk a járműirányítás területéről.

### 3.1. Definíciók

**Valós fizikai rendszer**: egy olyan fizikai objektum, amely mérhető külső kényszer hatására mérhető módon megváltozik.

**Bemenetek**: A valós fizikai rendszerre ható és időben változni képes kényszereket nevezzük fizikai bemeneteknek.<br>
**Kimenetek**: A valós fizikai rendszernek a fizikai kényszerek hatására bekövetkező bármely változása lehet fizikai kimenet, ezek közül azt tekintjük fizikai kimenetnek, amelyet az adott vizsgálatban közvetlenül vagy közvetve mérünk.<br>
*Példa: ezt mindig a szabályzási feladatnak megfelelően határozzuk meg. Például járműszintű irányítás: a rendszer a jármű maga, bemenete pl. a hosszirányú és keresztirányú gyorsulás. Kimenete pl. a pozíció, sebesség...stb.*<br>
*Példa: alacsonyszintű szabályzás esetén ez lehet pl. a fékszabályzás, ahol a bemenet a főfékhenger pozíciója, a kimenet a féktárcsa és a fékpofa között fellépő erő, vagy a kerékre ható nyomaték...stb.*.

**Az absztrakt rendszer**: Az absztrakt rendszer egy valós fizikai rendszer valamilyen pontosságú és meghatározott működési tartományra érvényes absztrakt modellje, amely a bemenőjelek és a kimenőjelek között teremt matematikai kapcsolatot. Ez voltaképp egy modellezési eljárás eredménye, amely modell a valós fizikai rendszer matematikai leírása, jellemzően differenciálegyenletek formájában.<br>
*Példa: a jármű kinematikai bicikli modellje*.

**Rendszer paraméterek:** A valós fizikai rendszert leíró egyenletek együtthatóit paramétereknek nevezzük.<br>
**Időinvariancia:** Az időinvariáns rendszer esetén, ha a rendszer egy U(t) gerjesztésre adott válasza Q(t), akkor az időben eltolt U(t-tau) gerjesztésre adott Y(t-tau) válasz is egyszerű időbeni eltolással megkapható. Ez egyúttal azt is jelenti, hogy a rendszer modellje időben nem változik, azaz a rendszer paraméterei állandóak.<br>
*Példa: a jármű kinematikai bicikli modelljének paraméterei pl. a jármű tengelytávja, amely (remélhetőleg) időben nem változik, azaz ez a paraméter időinvariáns. Ha a rendszer modelljének összes paramétere időinvariáns, maga az absztrakt rendszer is invariáns.*<br>
*Példa: a jármű dinamikai bicikli modelljének egy paramétere a jármű tömege. Ez változhat "menet közben is", hiszen pl. többen ülnek az autóban, csomagokat pakolnak bele, az üzemanyag szintje változik, azaz ez a paraméter nem időinvariáns, tehát a fizikai rendszer sem időinvariáns. Ugyanakkor tekinthetjük **bizonyos feltételek mellett** időinvariánsnak, ekkor a modell pontatlansága nő.*

**Állapot**: Az állapot a memória jelleggel rendelkező dinamikai rendszerekben a múlt összesített hatása. A rendszer állapotának a következő két tulajdonsággal kell rendelkeznie
- Bármely T időpillanatban a kimenőjel az adott pillanatbeli állapot és bemenőjel együttes ismeretében egyértelműen meghatározható legyen. Kimeneti egyenletnek nevezzük azt az összefüggést, amely az állapotból és a bemenőjelből meghatározza a kimenőjeleket.
- Az állapot egy adott T időpillanatban egyértelműen meghatározható legyen a bemenőjel a t≤T időtartománybeli értékének ismeretében. Az állapot változását leíró egyenletet állapotegyenletnek nevezzük.

**Állapotváltozó**: Az állapotváltozók az állapot egyértelmű leírására szolgálnak. Az állapotváltozó lehet egy időfüggvény, az állapot mennyiségi változásainak leírására, illetve logikai változó az állapot minőségi váltásainak leírására.

**Rendszer dimenzió:** Egy rendszer állapotának egyértelmű leírásához minimálisan szükséges állapotváltozók számát a rendszer dimenziójának szokás nevezni.

**Kanonikus állapotváltozók:** Kanonikus állapotváltozónak nevezzük az állapotváltozók legkisebb olyan halmazának elemeit, amelyek segítségével az állapot egyértelműen leírható. A nem kanonikus állapotváltozókat származtatott állapotváltozóknak nevezzük.

**Ljapunov-féle stabilitás:** Egy nemlineáris autonóm működésű rendszert akkor mondunk Ljapunov értelemben stabilisnak, ha az egyensúlyi állapot bármely környezetéhez találunk egy olyan nullánál nagyobb maximális kitérítést, amelynél kisebb kitérítések esetén a rendszer garantáltan visszatér az eredetileg meghatározott környezetbe.

**BIBO stabilitás:** Korlátos bemenőjelre minden esetben korlátos kimenőjel a válasz. Ezt a feltételt teljesítő rendszert ismételten az angol név után BIBO (Bounded Input Bounded Output) stabilis rendszernek hívjuk.

Ezen fogalmak összessége elegendő a járműirányítási alapok megértéséhez. A szabályzási feladat sokszor két részre bontható:
- modellezés, az absztrakt matematikai modell elkészítése
- szabályzás, azaz a szabályzó megtervezése.



### 3.2. A szabályzási feladat megfogalmazása

**A cél mindig a valós fizikai rendszer irányítása úgy, hogy az az előírt célértéknek megfelelően viselkedjen.** Fontos, hogy minden célhoz meghatározzunk olyan **mérhető** mennyiségeket, amelyek alapján a szabályzás *jóságát* meg tudjuk határozni. A legtöbbször (de nem kizárólagosan) használt ilyen mennyiségek:
- beállási idő (gyorsaság)
- túllendülés mértéke
- állandósult állapotbeli hiba
- a szabályzás energiája.

A szabályzási láncot a 3. Ábrán látható módon írhatjuk fel. A következő jelöléseket használjuk:
- $r(t)$: a célérték, avagy referenciajel
- $y(t)$: a visszacsatolt érték.
- $m(t)$: a valós rendszeren mért érték. Megjegyzés: sokszor az érzékelőt ideálisnak tekintjük, így $y(t)=m(t)$, és így a visszacsatolt érték egyben az absztrakt rendszer kimenete.
- $e(t)$: hibajel, a szabályzó bemenete.
- $i(t)$: a szabályzó által meghatározott beavatkozó jel.
- $f(t)$: előrecsatolt ág.
- $u(t)$: a rendszer bemenete
- $d(t)$: külső zavarok.

<img src="../_images/control/arj_control_03.svg" width="600" height="260" /> <br>
3. Ábra: a szabályzási lánc blokkdiagramja.
   
A szabályzó feladata, hogy a bemenetén keletkező hibát minimalizálja. A teljes szabályzási lánc feladata, hogy a valós fizikai rendszer kimenete a lehető legnagyobb pontossággal kövesse le a referencia jelet. 
A következőkben egy egyszerű példán szemléltetjük egy rendszer modellezését, a zavarok hatását, továbbá példát adunk egy szabályzóra illetve az előrecsatolás lehetőségére. A példát MATLAB/Simulink környezetben készítettük el.


### 3.3. Példa a szabályzási alapokra

Ebben a példában egy egyszerű modellt fogunk felépíteni hogy szemléltessük a zárthurkú szabályzást. A feladat egy jármű sebességszabályzása. Ehhez a jármű egyszerű modelljét fogjuk elkészíteni. 
1. Feladat: határozzuk meg a rendszer be- és kimeneteit!
A rendszerre a járműre ható gyorsító erővel szeretnénk hatni, ezért ez a bemenet. A kimenet a jármű sebessége, hiszen ezt szeretnénk egy adott célértékre szabályozni. Mivel ennek a rendszernek 1 be- és 1 kimenete van, ezért szokás SISO (Single Input Single Output) rendszernek is hívni. Ennek analógiáján léteznek MIMO (Multiple Inputs Multiple Outputs) rendszerek is.
*Megjegyzés: a valóságban a járműre a gázpedállal és a fékpedállal hatunk, de ezek voltaképp a jármű gyorsulását befolyásolják, így az egyszerűség kedvéért a szakasz ezen részét nem modellezzük. Ezzel természetesen hibát viszünk a modellbe, de növeljük annak általánosságát.*

1. Feladat: készítsük el a jármű matematikai modelljét / absztrakt modelljét!
Olyan egyenletrendszert keresünk, amely összeköti a be- és kimenetet. Itt meg kell határoznunk, mennyire törekszünk pontos modellre. Az egyszerűség kedvéért éljünk a következő megkötésekkel:
- eltekintünk a hosszirányban fellépő kerékszliptől
- eltekintünk az aktuátorok dinamikai viselkedésétől, azaz a kért gyorsulás egyből megvalósul
- eltekintünk a mért mennyiségeket terhelő hibáktól, azaz a szenzorok pontatlanságától.
- eltekintünk a futómű és a felfüggesztés dinamikai tulajdonságaitól.

Az egyenletünk így egy koncentrált tömegpont lineáris mozgásává egyszerűsödik. A modellbe foglaljuk be a következő mennyiségeket:
- a jármű tömege
- a jármű légellenállása
- a jármű sebességarányos súrlódása.

Newton II. tételének megfelelően írjuk fel a következő dinamikai egyensúlyi egyenletet: <br>
$\ddot I = \sum F$ <br>
Azaz: <br>
$m*\ddot v(t) = F_{prop}(t) - F_{aero}(t) - F_{fric}(t)$  <br>
Láthatjuk, hogy mind a bemenet, mind a kimenet szerepel az egyenletünkben, így valóban megtaláltuk a rendszer modelljét. Alakítsuk tovább, hogy *csak* a ki- és bemenet szerepeljen benne:<br>
$m*\ddot v(t) = F_{prop}(t) - \frac{1}{2}*v(t)^2*\rho*c*A - v(t)*b$ <br>

Ebben a formában az időben változó jelek a be- és kimenet (a sebesség illetve a hajtóerő), és vannak időben állandó (időinvariáns) paraméterek:
- $\rho$: a levegő sűrűsége
- $A$: homlokfelület mérete
- $c$: jármű légellenállási együtthatója
- $b$: Coloumb-féle súrlódási tényező

<img src="../_images/control/arj_control_04.svg" width="800" height="260" /> <br>
4. Ábra: a jármű modellezett erőegyensúlya


Ahhoz, hogy megkapjuk a sebességet, mint választott kimenet, meg kell oldanunk a differenciálegyenletet. Ezt Simulinkben numerikusan végezzük el. A megoldást az 5. Ábra mutatja.

<img src="../_images/control/arj_control_06.png" width="400" height="130" /> <br>
5. Ábra: a differenciálegyenlet numerikus megoldása.

A teljes szabályzási lánc blokkdiagramját a 6. Ábra mutatja. Ezen az ábrán nem használjuk a visszacsatolt ágat. A kezdő sebességet 20 m/s-ra állítottuk. A hajtóerő ez esetben nulla, így voltaképp a jármű tehetetlenségénél fogva gurul, és folyamatosan lassul a terheléssel arányosan. 

<img src="../_images/control/arj_control_05.png" width="800" height="260" /> <br>
6. Ábra: a teljes szabályzási lánc blokkdiagramja.

A következő paraméter értékeket választottuk a szimulációhoz:
- $A = 1.2m^2$
- $b = 10 Ns/m$
- $c = 0.4$
- $\rho = 1 kg/m^3$
- $m = 1250 kg$

A 7. Ábrán látható a futtatás eredménye. 100s-ig futott a szimuláció, ez idő alatt 20 m/s-ról nagyjából 7 m/s-ra lassul a jármű.

<img src="../_images/control/arj_control_07.png" width="400" height="300" /> <br>
1. Ábra: a teljes szabályzási lánc blokkdiagramja.

A 8. Ábrán a visszacsatolt szabályzónak egy arányos szabályzót (P szabályzót) választunk, amely a hibával arányosan határozza meg a beavatkozó jelet. Az erősítést 100-ra választjuk, azaz 1 m/s sebességhiba 100N hajtóerőt eredményez. A kezdő sebesség 15 m/s, a célsebesség 20 m/s, így kezdetben 500N hajtóerőnk lesz.

<img src="../_images/control/arj_control_08.png" width="300" height="80" /> <br>
8. Ábra: arányos szabályzó, 100-as erősítéssel.

A 9. Ábrán látható a szabályzó karakterisztikája, P szabályzóval, 100-as erősítéssel. A maradandó hiba relatíve nagy (több mint 10%). Ennek oka, hogy az arányos szabályzó a hibával arányos bemeneti jelet állít elő, és mivel a járműre hat ellentétes irányú erő, így a szabályzó ezzel fog egyensúlyt tartani. Amennyiben növeljük az erősítést, úgy csökken az állandósult állapotbeli hiba. Elméletben végtelen nagy erősítés nullára csökkenti ezt a hibát, viszont a végtelen erősítés végtelen beavatkozó jelet jelent, ami nem megvalósítható. A gyakorlatban ennél sokkal hamarabb elérjük a korlátokat, hiszen az aktuátorok csak véges erő kifejtésére képesek. Ezt a szabályzó megtervezésénél figyelembe kell venni.

<img src="../_images/control/arj_control_09.png" width="400" height="300" /> <br>
9. Ábra: arányos szabályzó karakterisztikája.

Az állandósult állapotbeli hibát úgy is eliminálhatjuk, ha az erősítés mellett egy olyan szabályzó tagot is hozzáadunk, ami "észre veszi", ha sokáig adott hiba áll fent, és növeli ennek megfelelően a beavatkozó jelet. Minél tovább áll fent a hiba, annál jobban növeljük a beavatkozó jelet. Ez gyakorlatilag a hiba időbeli integráljával arányos bevatkozó tagot jelent. Ezt szokás I tagnak nevezni. Az így kialakuló szabályzót pedig PI szabályzónak nevezni. A szabályzó elrendezését 10. Ábra mutatja, a kimeneti karakterisztikát a 11. Ábra. Láthatjuk, hogy a hiba valóban eltűnt, ugyanakkor a kezdeti tranziensz szakasz is megváltozott. Megjelent a túllendülés, ezzel együtt a célérték körüli oszcilláció, továbbá a beállás is lassabb lett.

<img src="../_images/control/arj_control_10.png" width="300" height="120" /> <br>
10. Ábra: arányos szabályzó, 100-as erősítéssel és integrátor, 10-es erősítéssel.

<img src="../_images/control/arj_control_11.png" width="400" height="300" /> <br>
11. Ábra: arányos szabályzó, 100-as erősítéssel.

A fenti beállási oszcillációt és túllendülést javíthatjuk a paraméterek megváltoztatásával, illetve egy olyan szabályzó tag hozzáadásával, amely a hiba változására reagál. Ez gyorsítja a beállást, és a túllendülést is gyorsabban kompenzálja. Ez gyakorlatilag a hiba változásával arányos tagot jelent, ami egyenértékű a hiba deriváltjának figyelembevételével. Ezt a tagot szokás D tagnak nevezni, a kialakuló szabályzót PID szabályzónak nevezni. Ugyanakkor a D tag erősítését óvatosan kell megválasztani, mert könnyen instabillá teheti a rendszert. A szabályzó felépítését a 12. Ábra mutatja, a karakterisztikáját a 13. Ábra.

<img src="../_images/control/arj_control_12.png" width="300" height="160" /> <br>
12. Ábra: arányos szabályzó, 100-as erősítéssel és integrátor, 10-es erősítéssel, D tag 100-as erősítéssel.

<img src="../_images/control/arj_control_13.png" width="400" height="300" /> <br>
13. Ábra: arányos szabályzó, 100-as erősítéssel, D tag 100-as erősítéssel.

Empirikus úton, figyelembevéve a túllendülés mértékét, a beállási időt és az állandósult állapotbeli hibát, válasszuk a következő paraméter értékeket:
$P=175$
$I=10$
$D=50$
Ezzel a beállás már nagyon szép, az eredményt a 14. Ábra mutatja.
<img src="../_images/control/arj_control_14.png" width="400" height="300" /> <br>
14. Ábra: arányos szabályzó, 170-es erősítéssel, I tag 10-es erősítéssel, D tag 50-es erősítéssel.

A 15. Ábrán látható, milyen hatása van, ha hozzádunk egy 3°-os lejtő által keltett extra gyorsító erőt. A túllendülés nagyobb lesz, hiszen a szabályzót egy olyan modellel állítottuk be, amely sík talajon mozgó autót feltételez.

<img src="../_images/control/arj_control_15.png" width="400" height="300" /> <br>
15. Ábra: lejtő hatása a zárt hurkú szabályzóra.

Ezt kompenzálhatjuk, ha a lejtővel arányos előrecsatolt ágat alkotunk meg. Azonban a lejtő becslése nehéz, általában a jármű mozgása alapján következtethetünk rá, ami így csak késve jelzi a lejtő mértékét. Adjunk hozzá egy 1 s-mal eltolt, 5%-os hibával rendelkező lejtőkompenzációt. A blokkdiagramot a 16. Ábra, az eredményt a 17. Ábra mutatja.

<img src="../_images/control/arj_control_16.png" width="600" height="150" /> <br>
16. Ábra: előrecsatolt ággal kiegészített szabályzó rendszer.

<img src="../_images/control/arj_control_17.png" width="400" height="300" /> <br>
17. Ábra: előrecsatolt ág hatása a szabályzóra.



## 4. Járműszabályzási megoldások
