---
layout: default
title: ROS melodic
parent: Telepítés
---

# ROS melodic

A következő lépések végrehajtásával az ROS Melodic változatát fogjuk telepíteni. 

{: .warning }
> Az ROS melodic 2023 júniusa óta **nem támogatott** (EOL - End of Life disztibúció).

## Forrás megadása

Adjuk hozzá a packages.ros.org helyet a telepítési források listájához:

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

## Kulcs megadása

Állítsuk be az ROS Melodic telepítéséhez szükséges kulcsot:

```
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

## Telepítés

Frissítsük a Debian csomagokat:

```
sudo apt update
```
A következő parancs a tényleges telepítést fogja indítani:
```
sudo apt install ros-melodic-desktop-full
``` 
A telepítést követően tegyük a basrc-be az ROS környezeti változóját:

```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

Az ROS Melodic telepítését bemutató videó [itt](https://youtu.be/e-VjpK5mYOI) tekinthető meg:

<iframe width="560" height="315" src="https://www.youtube.com/embed/e-VjpK5mYOI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Megjegyzés

További, *hivatalos* információk a telepítéssel kapcsolatban elérhetőek itt: [wiki.ros.org/melodic/Installation/Ubuntu](http://wiki.ros.org/melodic/Installation/Ubuntu)