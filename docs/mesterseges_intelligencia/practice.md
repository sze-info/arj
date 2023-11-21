---
layout: default
title: Gyakorlat
parent: Mesterséges intelligencia
---

# Gyakorlat

## Gyakrolati anyag letöltése
A gyakorlati anyag frissítéséhez adjuk ki a következő parancsokat:

```bash
cd ~/ros2_ws/src/arj_packages
git checkout -- .
git pull
```

## Conda környezet telepítése
Az anaconda (miniconda) egy izolált virtuális környezetet biztosít, ahol az éppen aktuális munkához szükséges verziószámú Python csomagokat tudjuk telepíteni.

```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
```

A solver hangolja össze a verziókat az előre definiált környezethez (`environment.yml`) szükséges csomagok között. A `libmamba-solver` az alapértelmezetthez solverhez képest egy gyorsabb hangolást tesz lehetővé.

```bash
conda config --set auto_activate_base false
conda update -n base conda
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```

Miután a Conda települt, létrehozzuk a saját virtuális környezetünket:
```bash
cd ~/ros2_ws/src/arj_packages/arj_ai
conda env create -f environment.yml
```
## Gyakorlat megnyitása

Az anyag a következőképpen nyitható meg:
```bash
conda activate gyakorlat
cd ~/ros2_ws/src/arj_packages/arj_ai 
code .
```