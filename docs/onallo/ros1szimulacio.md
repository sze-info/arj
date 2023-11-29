---
layout: default
title: ROS1 szimuláció
parent: Önálló feladatok
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



# ROS szimuláció

Két Gazebo alapú szimulációt fogunk megismerni. A Gazebo az ROS-által leginkább támogatott szimulátor, de érdemes megemlíteni az [SVL](https://github.com/lgsvl/simulator)-t, ebből saját verziónk is van a [Nissan](https://github.com/szenergy/nissanleaf-lgsvl)-ra optimalizáva, a [Carla](https://github.com/carla-simulator)-t vagy a [CoppeliaSim](https://www.coppeliarobotics.com/)-et. A két szimuláció:
- F1/10
- Prius

Első esetben szimuláció alapjául a következő tutorial szolgál: https://github.com/linklab-uva/f1tenth_gtc_tutorial. Itt azonban nem csak szimuláció, hanem valós jármű is van, sokkal több témát érintve. Mi most ebből csak a szimulátort használjuk. A szimulátorban egy kis méretű (F1 jármű tizede) robotjárművet fogunk navigálni.

**Vigyázat**: ROS 1-es feladat.

[![Static Badge](https://img.shields.io/badge/ROS_1-Melodic-ef4638)](https://docs.ros.org/en/humble/) 


A gyakorlatról készült [videó](https://www.youtube.com/watch?v=wdRD2X2hpKI) itt tekinthtó meg:

<iframe width="560" height="315" src="https://www.youtube.com/embed/wdRD2X2hpKI?rel=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Telepítés F1/10


A szükésges csomagok (mint a TEB local planner, gazebo szimulátor bizonyos csomagjai) így telepíthetőek:
```
sudo apt-get -y install ros-melodic-ros-control ros-melodic-gazebo-ros-control ros-melodic-ros-controllers ros-melodic-navigation qt4-default ros-melodic-ackermann-msgs ros-melodic-serial ros-melodic-teb-local-planner*
```

Készítsünk egy külön workspace-t, hogy később könnyen törölhessük, ha már nem kell. A `git clone` parancs utáni `.` direkt van, így plusz könyvtár nélül klónoz. A többi parancs ismerős az előző gyakorlatokról.

```
mkdir sim_ws
cd sim_ws/
git clone https://github.com/linklab-uva/f1tenth_gtc_tutorial .
catkin init
catkin build
```

Hogy ne kelljen minden terminalban megadnunk a workspace-t, tegyük a bashrc-be. Ha ezt nem szerenénk, elég mindig kiadni a `source ~/sim_ws/devel/setup.bash` parancsot.

```
echo "source ~/sim_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Később a `bashrc`-ből törölhető ez a sor, nyissuk meg vs code-ból: `code ~/.bashrc`.

### Egy egyszerűbb példa

Külön terminalban `roscore` után nyissuk meg a szimulátort (az első indítás gyakran *lassú*, de aztán relatív gyors lesz):

```
roslaunch racecar_gazebo racecar.launch
```
Szintén külön terminalban nyissunk meg egy egyszerű alkalmazást, amely alapértelmezetten a `/cmd_vel` parancsot publikálja, így irányítva az autót.

```
rosrun rqt_robot_steering rqt_robot_steering
```

Szintén külön terminalban néézük meg `rqt_graph` segítségével, hogyan kommunkiálnak egymással az ROS node-ok.

```
rosrun rqt_graph rqt_graph
```

Több nézet is beállítható, de valami hasonlót fogunk látni:

![](https://raw.githubusercontent.com/horverno/ros-gyakorlatok/master/4-szimulacio/others/rosgraph01.svg)

### Egy összetettebb példa

Nyissuk meg a szimulátort:

```
roslaunch racecar_gazebo racecar.launch
```

A következő a navigation stack indítása. Ez a következő részeket tartalmazza:

- AMCL (Adaptive Monte Carlo Localization)
- global planner based on global costmap
- TEB local planner based on local costmap
- robot controller

```
roslaunch platform navigation.launch
```

A következő parancs indítja az rviz-t is. Itt a 2D nav goal-ra kattintva a térkép bármely részére elnavigál a robot (TEB local planner-t használva).

```
roslaunch console navigation.launch
```

## Telepítés Prius

```
cd ~/sim_ws/src/
git clone https://github.com/osrf/car_demo
catkin build car_demo
source ~/.bashrc
```


### Egy egyszerűbb példa Prius

Mivel gyakran nincs joystick (game_pad) a közelben írjuk át a `joy` node helyett hasonlóan az előzőhöz `rqt_robot_steering` működésűre a Prius demo-t-

Nyissuk meg VS code-ban a package-t.
```
roscd car_demo
code .
```
`Joy` node helyett a `demo.launch`-ba a következők kerüljenek. 

``` xml
<node pkg="car_demo" type="robot_steering_translator.py" name="robot_steering_translator1" output="screen"/>
<node pkg="rqt_robot_steering" type="rqt_robot_steering" name="rqt_robot_st0" />
```

Készítsük el a `robot_steering_translator.py` node-ot, ami `/cmd_vel`-ből a `/prius` topicba küld üzeneteket.

``` python 
#!/usr/bin/env python
import rospy
from prius_msgs.msg import Control
from geometry_msgs.msg import Twist

class Translator:
    def __init__(self):
        self.sub = rospy.Subscriber("cmd_vel", Twist, self.callback)
        self.pub = rospy.Publisher("prius", Control, queue_size=1)
        self.last_published_time = rospy.get_rostime()
        self.last_published = None
        self.timer = rospy.Timer(rospy.Duration(1./20.), self.timer_callback)
        
    def timer_callback(self, event):
        if self.last_published and self.last_published_time < rospy.get_rostime() + rospy.Duration(1.0/20.):
            self.callback(self.last_published)

    def callback(self, message):
        command = Control()
        command.header.stamp = rospy.Time.now()
        if message.linear.x > 0.2:
            command.throttle = message.linear.x
            command.brake = 0.0
        elif message.linear.x < -0.1:
            command.throttle = 0.0
            command.brake = -1 * message.linear.x
        else:
            command.throttle = 0.0
            command.brake = 0.0            
        command.steer = message.angular.z
        #rospy.loginfo("throttle and brake: %.1f %.1f" %(command.throttle, command.brake))
        self.last_published = message
        self.pub.publish(command)

if __name__ == '__main__':
    rospy.init_node('robot_steering_translator')
    rospy.loginfo("robot_steering_translator started")
    t = Translator()
    rospy.spin()
```
Ne felejtsük a `sudo chmod +x nodes/robot_steering_translator.py` parancsot se.

Külön terminalban `roscore` után nyissuk meg a szimulátort (az első indítás gyakran *lassú*, de aztán relatív gyors lesz):

```
roslaunch car_demo demo.launch 
```

Vizsgáljuk meg a topicokat [plotjuggler](https://github.com/facontidavide/PlotJuggler) segítségével. 