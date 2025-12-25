---
title: "Rocky Linux 9에서 Python 3.14 설치"
date: 2025-12-25T12:11:00+09:00
draft: false
categories: ["Coding"]
tags: ["server", "linux", "python", "rocky"]
ShowToc: true
TocOpen: true
---

```sh
sudo dnf update -y
sudo dnf install tar curl gcc openssl-devel bzip2-devel libffi-devel zlib-devel wget make findutils -y
sudo dnf groupinstall "Development Tools" -y
cd /opt
sudo wget https://www.python.org/ftp/python/3.14.0/Python-3.14.0.tar.xz
sudo tar -xf Python-3.14.0.tar.xz
cd Python-3.14.0
sudo ./configure --enable-optimizations
sudo make -j $(nproc)
sudo make altinstall
python3.14 --version
```
