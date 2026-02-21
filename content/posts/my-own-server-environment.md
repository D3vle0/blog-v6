---
title: "홈서버 및 Oracle 서버 세팅"
date: 2026-02-20T20:55:00+09:00
draft: false
cover:
  image: img/my-own-server-environment/1.jpg
  caption: ""
categories: ["Server"]
tags: ["server", "linux", "rocky", "debian"]
ShowToc: true
TocOpen: true
---

홈서버와 Oracle cloud 서버를 초기화 하고 다시 세팅할 때 내 환경에 맞춰 쉽게 세팅할 수 있도록 기록한다. 한 줄 씩 실행하면 된다.

## 홈서버 (Debian 13)

### Debian 13 설치 시 보안 설정

- lvm + luks2 설정하기
- root 로그인 금지

```sh
# 사용자 관리자 권한 주기
sudo -i
usermod -aG sudo <사용자>
exit
# ssh port 변경
sudo vi /etc/ssh/sshd_config
# NEW_PORT에 원하는 포트 입력
NEW_PORT=2222; sudo sed -i "s/^#\?Port [0-9]*/Port $NEW_PORT/" /etc/ssh/sshd_config && sudo systemctl restart sshd
# 노트북 덮개 닫아도 절전되지 않게
sudo sed -i 's/^#\?HandleLidSwitch=.*/HandleLidSwitch=ignore/' /etc/systemd/logind.conf && sudo systemctl restart systemd-logind
```

### 초기 패키지 설치

```sh
# 업데이트
sudo apt update -y
sudo apt upgrade -y

# 패키지 설치
sudo apt install nala -y
sudo nala install zsh git curl wget htop -y

# Python 설치
sudo nala install python3 python3-pip python3-venv build-essential -y
python3 --version

# Node.js 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.4/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
source ~/.bashrc
\. "$HOME/.nvm/nvm.sh"
nvm install node
node -v; npm -v

# 터미널 설정
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.powerlevel10k
echo 'source ~/.powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
source ~/.zshrc
sed -i 's/POWERLEVEL9K_CONTEXT_{REMOTE,REMOTE_SUDO}_TEMPLATE=.*/POWERLEVEL9K_CONTEXT_{REMOTE,REMOTE_SUDO}_TEMPLATE="%n"/' ~/.p10k.zsh && sed -i 's/POWERLEVEL9K_TIME_FORMAT=.*/POWERLEVEL9K_TIME_FORMAT="%D{%H:%M}"/' ~/.p10k.zsh && source ~/.p10k.zsh

# Docker 설치
sudo nala remove $(dpkg --get-selections docker.io docker-compose docker-doc podman-docker containerd runc | cut -f1)
sudo nala update
sudo nala install ca-certificates
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
printf "Types: deb\nURIs: https://download.docker.com/linux/debian\nSuites: $(. /etc/os-release && echo "$VERSION_CODENAME")\nComponents: stable\nSigned-By: /etc/apt/keyrings/docker.asc\n" | sudo tee /etc/apt/sources.list.d/docker.sources > /dev/null
sudo nala update
sudo nala install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker $USER && newgrp docker
```

<!--
서비스 배포, 방화벽 설정, ssh 로그인 설정 쓸 것
오라클 rocky linux 9도 똑같이

특히 서비스 배포 관련 코드는 hugo-protector로 암호화
-->