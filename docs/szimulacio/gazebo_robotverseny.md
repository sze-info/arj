---
layout: default
title: Gazebo robotverseny
parent: Szimuláció
---

# Gazebo robotverseny

A leírás egy középiskolásoknak szóló [roboversenyre](https://robotverseny.github.io/) készült, de egyetemi környezetben is használható. A támogatott operációs rendszer Ubuntu 18.04, az ROS verzió pedig melodic.


A szükésges csomagok így telepíthetőek:

```
sudo apt-get -y install ros-melodic-ros-control ros-melodic-gazebo-ros-control ros-melodic-ros-controllers ros-melodic-navigation qt4-default ros-melodic-ackermann-msgs ros-melodic-serial ros-melodic-teb-local-planner* ros-melodic-tf-conversions zip unzip ros-melodic-jsk-rviz-plugins python3-catkin-tools
```

Készítsünk egy külön workspace-t ('sim_ws'), hogy később könnyen törölhessük, ha már nem kell.

```
cd ~
mkdir -p sim_ws/src
cd ~/sim_ws/src
git clone https://github.com/robotverseny/racecar_gazebo
git clone https://github.com/robotverseny/megoldas
cd ~/sim_ws
catkin build
```

Adjuk meg bashrc-ben a szimulátorhoz szükséges modellek elérési útvonalát.

```
echo "export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:~/sim_ws/src/racecar_gazebo/f1tenth/virtual/dependencies/racecar_gazebo/models" >> ~/.bashrc
source ~/.bashrc

```

Hogy ne kelljen minden terminalban megadnunk a workspace-t, tegyük azt is a bashrc-be. Ha ezt nem szerenénk, elég mindig kiadni a `source ~/sim_ws/devel/setup.bash` parancsot.

```
echo "source ~/sim_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
A csomagok telepítését és a workspace létrehozását bemutató videó itt érhető el: [youtu.be/cXABl5jbmVc](https://youtu.be/cXABl5jbmVc)

<iframe width="560" height="315" src="https://www.youtube.com/embed/cXABl5jbmVc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Később, ha a verseny után már nem szükséges, a `bashrc`-ből törölhető ez a sor, nyissuk meg vs code-ból: `code ~/.bashrc`, majd a fájl utolsó soraiból töröljük a korábban hozzáadottat. 