---
title: "Arch Linux 홈서버 셋업"
date: 2022-01-22T22:54:03+09:00
draft: false
categories: ["Server"]
tags: ["arch"]
cover:
  image: https://firebasestorage.googleapis.com/v0/b/storage-f1f46.appspot.com/o/homeserver-02.png?alt=media&token=9c5d2559-eace-44a7-96a7-e3fcaeb3e47b
ShowToc: true
TocOpen: true
---


## 서버 히스토리

### AWS

나의 첫 서버는 [AWS](https://aws.amazon.com/)의 프리 티어 서버였다. 싱글 가상 코어, 1기가 램, 매우 적은 용량의 HDD는 서버를 단순히 파일 저장을 위한 클라우드로 사용할 수 밖에 없었다. code-server 를 올리고 nginx로 이것저것 세팅을 하다보면 금새 램은 꽉 차있었고, 패키지 몇 개를 설치하다 보면 빠르게 줄어드는 디스크 가용 용량은 매우 불편했다. 심지어 서버 region이 멀리 떨어져 있는 지역이라 터미널에 글자를 입력하면 1초 뒤에 반응하는 심각한 불편함이 있었다. 이때 돈을 주고서라도 좋은 사양의 서버가 필요하다는 것을 깨닫고 vultr로 넘어갔다.

### Vultr

$5/mo, 싱글 가상 코어, 1기가 램, 50기가의 SSD, 무엇보다 IDC가 서울에 있어 네트워크 속도가 매우 빨랐다. (단, 서울 region 인스턴스는 VAT 10% 별도 부과.) 50GB 라는 용양이 충분하지는 않았지만 나름 만족하면서 1년간 사용했다.

### Oracle

성능은 AWS와 비슷하나 완전 무료로 서울 region 인스턴스를 사용할 수 있다는 장점 덕분에 vultr 외에 추가적인 인스턴스가 필요할 때 유용했다.

## 왜 집에 서버가 필요한가?

- 네트워크 속도가 매우 빠르다
- 하드웨어에 직접적으로 접근 가능
- 비용 절감

집에 있는 10년 된 낡은 삼성 노트북을 홈서버로 사용한다. 윈도우를 설치하여 각종 작업을 하기에는 성능이 많이 부족하지만 리눅스를 사용하기에는 충분한 성능이다. (i5-2450M, 6GB, HDD 500GB)

## 왜 Arch Linux를 사용하는가?

Linux 초보자에게는 절대 Arch Linux를 추천하지는 않는다. Arch Linux는 시스템 깊숙히 유저가 원하는 대로 커스텀 할 수 있고, 그렇기에 미니멀리즘을 추구한 굉장히 가벼운 시스템을 구축할 수 있다. 다만 이것이 초보자에게는 설치가 어렵다는 단점이 될 수 있다. (CLI로만 설치 해야함)

## 네트워크 세팅

USB 부팅을 이용해서 Arch Linux 설치 미디어를 부팅시키고 CLI로 설치, 필수 패키지를 AUR에서 다운받았다. 이후 랜선을 연결하여 `192.168.219.107` 사설 IP를 부여받았다.

### DHCP 고정 할당

공유기 설정에서 집 공인 IP가 유동으로 되어있긴 하나 한 번도 변한 적이 없어서 그대로 두었고, Arch Linux가 설치된 노트북의 사설 IP를 고정시켰다.

![img](https://firebasestorage.googleapis.com/v0/b/storage-f1f46.appspot.com/o/homeserver-01.png?alt=media&token=5d2f9795-9626-47f0-92bc-7a14ab7677d9)

### 도메인 할당

유플러스 공유기에는 불편한 단점이 있는데, 네트워크 밖의 기기는 당연히 공인 IP를 이용하여 홈서버에 접속 하는 것이 맞지만 **네트워크 내부에 있는 기기가 홈서버에 접속할 때는 반드시 사설 IP를 이용해야 한다.** 공인 IP로 접속이 불가능하다. 그래서 나는 공인 IP와 사설 IP 각각 도메인을 할당시켜 사용하고 있다. 해당 IP를 가리키는 A Record 두 개를 만들기만 하면 된다.

### 포트포워딩

![img](https://firebasestorage.googleapis.com/v0/b/storage-f1f46.appspot.com/o/homeserver-02.png?alt=media&token=9c5d2559-eace-44a7-96a7-e3fcaeb3e47b)

## 마치며

추후에는 팬 속도 조절, VM 세팅 등의 고급 세팅을 할 계획이다.
