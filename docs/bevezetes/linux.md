---
layout: default
title: Linux
parent: Bevezetés
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


# Linux

A leírásban alapvető Linux ismeretek találhatók. Linuxban (ebben a leírásban értsd  Ubuntu, Raspbian) a legtöbb dolgot lehetséges vagy épp célszerű terminálból intézni.
Ez a tutorial segít a linux terminál alapjainak megismertetésében.

{: .highlight }
Fontos, hogy a megszokott `ctrl`+`v`, `ctrl`+`c` helyett itt a `ctrl`+`shift`+`v`, `ctrl`+`shift`+`c` működik. A `ctr`+`c` pl. egy ROS node (program) befejezésére használható itt. 

# Ajánlott terminálprogramok

Számos program választható a szöveges parancssor elérésére. ROS/ROS2 esetén talán a következők a legjobb választások.

## Windows Terminal

Ahogy a neve is mutaja ez WSL esetén, Windows-on releváns. Előnye, hogy egy helyen használhatunk több Linux disztribúciót akár Windows parancssorral is. `Ctrl-Shift-P` billentyű kombinációkkal majd a Split down, Split left parancsokkal oszhatjuk szét hasonló módon a terminált:

![Alt text](windows_terminal01.png)

## Terminator

Linuxon értelmezett terminál, de telepíteni kell.

``` bash
sudo apt update
sudo apt install terminator
```

Terminator-ban `Ctrl-Shift-O`, `Ctrl-Shift-E` billentyű kombinációkkal oszthatjuk tovább az adott ablakot. `Ctrl-Shift-W` bezárja az aktív ablakot.

![Alt text](terminator01.png)


## VS code terminal

A fejlesztőkörnyezet beépített terminálja, mind Windowson, mind Linuxon működik.

![Alt text](windows_vs_code_terminal01.png)

![](https://code.visualstudio.com/assets/updates/1_54/local-terminal-reconnection.gif)

# Fontosabb terminal parancsok
## Korábbi parancsok


- `Fel nyíl🔼` vagy `Le nyíl🔽` - A közvetlenül ezelőtti parancsokat érhetjük el így.
- `Ctrl+R` billenytűkombinációval korábbi parancsok hívhatók elő, időrenben egyre korábbiak
- `Ctrl+Shift+R` billenytűkombinációval korábbi parancsok hívhatóak elő, de időrenben előre haladva 

A megszokott `ctrl`+`v`, `ctrl`+`c` helyett itt a `ctrl`+`shift`+`v`, `ctrl`+`shift`+`c` működik. A `ctr`+`c` pl. egy ROS node (program) befejezésére használható itt.

![Alt text](linux_recent01.gif)

## Automatikus kiegészítés

- `Tab` billenytűvel az elkezdett parancsok egészíthetőek ki
- `Tab Tab` billenytűkombinációval az összes lehetséges parancsot fogja kilistázni

![](https://sze-info.github.io/arj/erzekeles/terminalintro01.gif)

## Képrnyőtörlés

- `Ctrl+L` billenytűvel törölhetőek a korábbi szövegek, így jobban átlátható lesz a terminal

## Könyvtárak közötti navigáció
- `cd`: adott könyvtárba / mappába történő belépés
  - pl `cd ~/catkin_ws/src`, `cd ../..`
- `ls`: listázás: könyvtárak, fájlok
- `mkdir`: könyvtár készítése
- `pwd`: aktuális munkakönyvtár kiíratása (print working directory)
- `cp`: Ezzel a paranccsal tudunk másolni.(`cp` `/file/helye` `/ahova/másolni/akarod/`, `cp -r` `/a/könyvtár/helye` `/ahova másolni/akarod`)
- `mv`: Ezzel adott fájlt vagy könyvtárat tudunk mozgatni (áthelyezni) vagy átnevezni. (`mv /a/fájl/helye *fájl új neve`, `mv /a/fájl/helye /a/fájl/új/helye`)
- `rm`: Fájlok törlése. (`rm /a/fájl/helye, rm -r /a/fájlok/és/mappák/helye`) Az `rm -r` parancsnál minden törlődni fog a meghatározott helyen.
- `rmdir`: Egy üres könyvtár törlése.
- `chmod`: (change mode) Arra alkalmas, hogy megváltoztassuk az fájlok / mappák hozzáférési jogait. Thetetjük ezt például karakteres kapcsolókkal (r, w, stb.), vagy oktálisan (számjegyekkel).
  - pl `chmod +x my_python_node.py`: írási jog hozzáadása
  - pl `chmod 777 everything.py`: minden jog hozzáadása

### `chmod`

|N|Sum|`rwx`|Permission|
|-|---|-----|----------|
|`7`	|4(r)+ 2(w) + 1(x)|	`rwx`	|read, write and execute|
|`6`	|4(r)+ 2(w)|	`rw-`	|read and write|
|`5`	|4(r)+ 1(x)|	`r-x`	|read and execute|
|`4`	|4(r)|`r--`	|read only|
|`3`	|2(w)+ 1(x)|`-wx`	|write and execute|
|`2`	|2(w)|`-w-`	|write only|
|`1`	|1(x)|`--x`	|execute only|
|`0`	|0   |`---`	|none|

### Könyvtárak

| Hely  |  Magyarázat |
|-------|---|
|`/`    | A könyvtárfa kiindulópontja, gyökér  |
|`/boot`| Rendszerindítás, bootloader |
|`/bin` | A futtatható parancsok, binaries |
|`/sbin`| A rendszergazda parancsai, superuser/system bin   |
|`/lib` | Az induláshoz szükséges osztott rendszerkönyvtárak -libraries- illetve, modulok, meghajtóprogramok  |
|`/dev` | Eszközök, min pl USB (`ttyUSB0`) - devices  |
|`/etc` | Beállítófájlok, helyi indító parancsok, jelszavak, hálózati-beállítók, etc. helye.  |
|`/home`| Itt található minden felhasználó saját könyvtárat. Pl. ha, a `sanyi` felhasználóval vagyunk belépve `/home/sanyi` tartalmazza a fájlainkat. A `/home/sanyi/Desktop` vagy röviden `~/Desktop` pl az asztalunk tartalma. |
|`/mnt` | A felcsatolt (mountolt) perifériák, fájlrendszerek helye, mount.  |
|`/proc`| Process information |
|`/root`| A root user könyvtára  |
|`/tmp` |  Temp |
|`/usr` |  Universal system resources, alkalmazások, rendszereszközök |
|`/var` |  Változó adatok pl.: nyomtatási munkák, emailek |

## Verziókezelés
- `git clone`: git repo klónozása
- `git config --global user.name "Sanyika"`: felhsználónév beállítása
- `git config --global user.email "sanyika@gggmail.com`: email beállítása
- `git init`: lokális repó inicializálása
- `git add <file>`: Fájl hozzáadása
- `git status`: aktuális státusz lekérdezése
- `git commit -m "My beautiful commit"`: Commit, üzenettel
- `git push`: push
- `git pull`: pull
- `git branch <new_branch_name>`: branch készítése
- `git checkout <branch_name>`: új branch
- `git merge <branch_name>`: a jelenlegi branch-be mergeli a branch-t

{: .important-title }
> Tipp
>
> A legtöbb művelet VS code-al elvégezhető terminál nélkül is.

## Szöveges fájlok
- `wget`: webes tartalmak letöltése terminalból
- `cat`: fájl tartalmának kiiratása
- `touch`: szöveges fájl létrehozása
  - pl `touch hello.txt`
- `echo`: kiíratás, vagy fájlba írás (`>>` operátor). Amennyiben nem létezik a fájl, létrehozza (`touch`).
  - pl `echo "hello" >> hello.txt`  
  - pl `echo "n = 5; print('\n'.join(':D ' * i for i in range(1, n + 1)))" >> hello.py` 
  - pl `rostopic list >> hello.txt` 
  - pl `rostopic echo -n1 /scan >> hello.txt` 
- `nano`: szövegszerkesztő: egyszerű, terminál-alapú
- `code`: szövegszerkesztő: GUI, VS code
  - pl `code .` megnyintja az aktuális mappa tartalmát
  - pl `code ~/.bashrc` megnyintja a  `~/.bashrc` tartalmát szerkesztésre
- `catkin`: catkin tools: wrapper a `cmake` és `make` parancsok egyszerűbb használatához


## Telepítés
- `sudo apt install` vagy `sudo apt-get install`: szoftver csomagkezelővel történő telepítés, Advanced Packaging Tool (APT). 
  - pl `sudo apt install tree mc` - tree és mc programok telepítése 
- `sudo`: (Superuser do) Lehetővé teszi, hogy rendszergazdaként vagy más felhasználó nevében hajtsunk végre parancsokat.
- `sudo apt update`: csomagindex frissítése, ha új verziók jönnek ki különböző szoftverekből, ezt a paracsot a telepítés (`apt install`) előtt célszerű kiadni.
- `sudo apt update`: már telepített csomagok frissítése
- `apt list`: listázza asz összes telepített csomagot
  - pl `apt list | grep ros`: leszűri csak az ros 

## Egyéb hasznos
- `Ctrl + a` vagy `home`: A sor elejére dob.
- `Ctrl + e` vagy `end`: A sor végére dob.
- `Ctrl + ◀` / `Ctrl + ▶`: Az előző / következő szóra ugrik. 
- `grep`: (Global \ Regular Expression \ Print) fájlokban illetve parancsok kimenetében keres. 
  - pl `grep 'ROS' ~/.bashrc`: listázza a `bashrc` fájlban az `ROS` szöveget tartalmazó sorokat

Forrás: 
[Ubuntu magyar dokumentációs projekt `CC by-sa 2.5`](http://sugo.ubuntu.hu/community-doc/hardy/universe/basic/terminal_hasznalata.html), [Óbuda University `CC BY-NC-SA 4.0`](https://github.com/ABC-iRobotics/ros_course_materials_hu/blob/main/LICENSE.md)