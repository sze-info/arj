---
layout: default
title: Érzékelés
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


# Érzékelés

Az érzékelés nyers adatok beolvasását jelenti, fontos, hogy még nem jelent magas szintű adatfeldolgozást. Szenzorai lehtnek kamerák, mikrofonok, LIDAR-ok stb.

{: .note }
Magyar nyelven könnyű összekeverni az érzékelés (sensing) és az észlelés (perception) foglamakat. Az érzékelés egyszerű driver szintű nyers adatok előállításával foglakozik.


## Kamera

*Jellemző gyártók:* Allied Vision, Basler, Stereolabs, Orbbec, Intel

*Jellemző interfész:* GigE, USB3

![cam](camera01.png)

## LIDAR

*Jellemző gyártók:* Velodyne, Ouster, Livox, SICK, Hokuy, Pioneer, Luminar, Hesai, Robosense, Ibeo, Innoviz, Quanenergy, Cepton, Blickfeld, Aeva

*Jellemző interfész:* GigE

LIDAR gyártókat, dataseteket, algoritmusokat tartlamazó gyűjtemény: [github.com/szenergy/awesome-lidar](https://github.com/szenergy/awesome-lidar).

![lidar](lidar01.png)

<iframe width="560" height="315" src="https://www.youtube.com/embed/1IWXO0vvmO8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Radar
*Jellemző gyártók:* Aptiv, Bosch, Continental,  Denso

*Jellemző interfész:* CAN bus


## IMU
*Jellemző gyártók:* Lord MicroStrain, Bosch, XSens

*Jellemző interfész:* Serial, Ethernet, USB, CAN bus

![imu](imu01.png)
## GNSS (GPS)

A [GNSS](https://en.wikipedia.org/wiki/Satellite_navigation) (global navigation satellite system) globális szatelit-alapú navigációs rendszert jelent, köznapi szóhasználatban ezt szokás GPS-nek nevezni. Ha pontosak szeretnénk lenni, akkor a GPS csupán az első ilyen technológia ezen kívül létezik még GLONASS, BeiDou, Galileo és QZSS rendszer is, ezek üzemeltetése különböző államokhoz / szövetségekhez kötődik.

*Jellemző gyártók:* SwiftNavigation, VectorNav, Ublox, NovaTel

*Jellemző interfész:* GigE, CAN bus

![gnss](gps01.png)

## CAN bus

- Sebesség adat lekérdezése, refencia jel
- Kormányszög adat lekérdezése, refencia jel


[Szenzorok ROS-ben](https://docs.google.com/presentation/d/e/2PACX-1vQbXSe4cb-aYgWNNiUF1PHJBZrwl0keWantbFjTe94zm1A9cVGqmWKC4lHCSUr4y7vfq1PrJ2mP8XqP/pub?start=false&loop=false&delayms=3000) _(online google prezentáció magyarul)_

