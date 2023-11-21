---
layout: default
title: Gyakorlat
parent: Mesterséges intelligencia
---

# Gyakorlat

## Conda Környezet Telepítése

Telepítés:
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
```

Conda solver frissítése:
```bash
conda config --set auto_activate_base false
conda update -n base conda
conda install -n base conda-libmamba-solver
conda config --set solver libmamba
```