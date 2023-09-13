---
layout: default
title: MCAP fájlok
parent: ROS 2 haladó 
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



# `MCAP` fájlok szerkesztése

Az `ROS 2` az log adatokhoz az `MCAP` formátumot használja. Ez a formátum **nem** dedikáltan az ROS saját formátuma, hanem egy nyílt forráskódú konténerfájl típus tetszőleges multimodális log-adatokhoz. Támogatja az időbélyegzővel ellátott, előre sorba rendezett adatokat. Így ideális a pub/sub vagy robotikai alkalmazásokban való használatra is, a `ROS 2` is ezért döntött mellette. A következőkben arról lesz szó, hogyan lehetséges a formátum C++,	Go,	Python,	Rust,	Swift vagy TypeScript nyelven történő szerkesztése. Illetve egy python példán keresztül gyakorlati oldalról is szemléltetjük ezt.


# Források

- [docs.ros.org/en/humble/How-To-Guides/Visualizing-ROS-2-Data-With-Foxglove-Studio.html](https://docs.ros.org/en/humble/How-To-Guides/Visualizing-ROS-2-Data-With-Foxglove-Studio.html)
- [mcap.dev](https://mcap.dev/)
- [mcap.dev/docs/python/raw_reader_writer_example](https://mcap.dev/docs/python/raw_reader_writer_example)
