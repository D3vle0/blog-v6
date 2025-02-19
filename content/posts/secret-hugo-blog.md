---
title: "딥 웹에 비밀 블로그 만드는 방법 (Hugo) 🔒"
date: 2022-05-23
draft: false
categories: ["Blog"]
tags: ["tor", "hugo"]
ShowToc: true
TocOpen: true
---

## 사전 준비

온라인 상에 존재하지만, 자신만 볼 수 있는 개인적인 메모장을 만드는 방법에는 여러 방법이 있다. 네이버 블로그나 티스토리의 글 보호 기능을 활용하여 글을 열람할 때 비밀번호를 입력하도록 할 수 있는데, 이 글에서는 그런 포털 사이트에서 제공하는 블로그 서비스를 활용하지 않는 방법을 기반으로 설명하겠다. 그 이유는 포털 사이트에서 제공하는 블로그 서비스인 만큼 가입 시 실명으로 가입해야 하기 때문에 본인의 신원이 드러나기 때문이다.

## 보안 0단계

보안 0단계 블로그는 다음과 같은 보안적인 특성을 가진다. (체크 표시가 많을수록 보안이 뛰어남)

|보안 체크리스트|상태|
|---|---|
|표면 웹에 공개되어 있지 않은가?|❌|
|검색엔진에서 검색이 불가능한가?|❌|
|작성자가 누구인지 알 수 없는가?|❌|
|글이 암호화되어 있는가?|❌|

나를 어필해야 하고, 보안에 대해 신경을 쓸 필요가 없다면 포털 사이트 블로그 서비스를 활용해도 좋고, hugo 정적 사이트 생성기를 활용하자.

## 보안 1단계

보안 1단계 블로그는 다음과 같은 보안적인 특성을 가지고 있다.

|보안 체크리스트|상태|
|---|---|
|표면 웹에 공개되어 있지 않은가?|❌|
|검색엔진에서 검색이 불가능한가?|❌|
|작성자가 누구인지 알 수 없는가?|✅|
|글이 암호화되어 있는가?|❌|

보안 0단계와 동일한 방식으로 블로그를 구축하되 자신이 누구인지 밝히지 말라. SNS 계정 링크라던지 본인을 특정할 수 있는 그 어떠한 것도 언급, 기록하지 말아야 한다. 또한 disqus 등 댓글 플러그인도 막아야 한다. disqus를 통해 사이트 관리자가 누구인지 특정할 수 있기 때문이다.

## 보안 2단계

보안 2단계 블로그는 다음과 같은 보안적인 특성을 가지고 있다.

|보안 체크리스트|상태|
|---|---|
|표면 웹에 공개되어 있지 않은가?|❌|
|검색엔진에서 검색이 불가능한가?|✅|
|작성자가 누구인지 알 수 없는가?|✅|
|글이 암호화되어 있는가?|❌|

먼저 위치를 특정할 수 있는 홈서버 말고 oracle cloud에서 무료 리눅스 서버 하나를 구한다. [참고](https://blog.ny64.kr/posts/create-your-own-lifetime-free-server-using-oracle-cloud/) hugo 블로그를 하나 만들고, 서버에서 아래 명령을 입력한다.

```bash
nohup hugo server --bind=0.0.0.0 --baseURL=http://<서버 아이피> --port=<원하는 포트> &
```

사이트에 접속할 수 있도록 localhost가 아닌 0.0.0.0으로 bind를 해주어야 하기에 bind 옵션도 잊지 말고 입력한다. 포트는 3000, 5000, 8080이 아닌 자주 사용되지 않는 포트를 추천한다. 이렇게 하면 검색엔진에서 `서버IP:포트` 를 검색하거나 자신이 작성한 글 내용을 검색해봐도 나오지 않는다.

hugo 블로그를 종료하려면 아래 명령을 입력한다.

```bash
kill -9 `ps -ef | grep hugo | awk '{print $2}'`
```

## 보안 3단계

보안 3단계 블로그는 다음과 같은 보안적인 특성을 가지고 있다.

|보안 체크리스트|상태|
|---|---|
|표면 웹에 공개되어 있지 않은가?|❌|
|검색엔진에서 검색이 불가능한가?|✅|
|작성자가 누구인지 알 수 없는가?|✅|
|글이 암호화되어 있는가?|✅|

블로그에 작성한 글들을 자신만의 알고리즘을 활용하여 암호화시키는 방법이다. gpg를 활용해서 글을 암호화시킨 채로 블로그에 올린다면 **오직 나만이 해독할 수 있는 글**이 될 것이다. 만약 언젠가는 다른 사람이 봐주었으면 하는 글이 있다면 개인 키 (private key) 가 필요한 비대칭키 암호화 알고리즘이 아닌 대칭키 암호화 알고리즘을 활용하는 것도 좋다.

## 보안 4단계

보안 4단계 블로그는 다음과 같은 보안적인 특성을 가지고 있다.

|보안 체크리스트|상태|
|---|---|
|표면 웹에 공개되어 있지 않은가?|✅|
|검색엔진에서 검색이 불가능한가?|✅|
|작성자가 누구인지 알 수 없는가?|✅|
|글이 암호화되어 있는가?|✅|

tor 네트워크에 블로그를 올린다.

```bash
sudo apt install tor
sudo nano /etc/tor/torrc
```

`/etc/tor/torrc` 에서 아래의 내용에 해당하는 곳 주석을 지우고 다음과 같이 작성한다.

```text
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort <포트> 127.0.0.1:<포트>
```

여기서 포트는 hugo가 작동하고 있는 포트다.

```bash
sudo service tor restart
sudo cat /var/lib/tor/hidden_service/hostname
```

`.onion` 형태의 도메인이 나오는데, tor 브라우저에서 해당 주소 뒤에 포트를 붙여 접속한다. 그런데 지금 상태에서는 tor 밖에서 기존 서버 IP 주소를 통해서도 접속할 수 있는 문제가 있다. 이를 해결하기 위해 hugo preview 서버를 내리고, bind 주소를 localhost로 변경, baseurl을 onion 도메인으로 바꿔주면 된다.

```bash
kill -9 `ps -ef | grep hugo | awk '{print $2}'`
nohup hugo server --bind=127.0.0.1 --baseURL=http://<onion 주소> --port=<포트> &
```

> 25.02.09 내용 추가  
> 만약 `hugo server` 가 아닌 `hugo` 명령으로 빌드한 정적 웹사이트를 80 포트에 서빙하고 싶다면 다음과 같이 설정하면 된다.
> ```txt
> # /etc/nginx/sites-enabled/default
>
> server {
>     listen 127.0.0.1:80;
>     server_name <onion 주소>;
> 
>     # 정적 파일 경로 설정
>     root <hugo 블로그 경로>/public;
>     index index.html;
> 
>     location / {
>         try_files $uri $uri/ =404;
>     }
> 
>     # 캐싱 비활성화 (Tor 특성상 권장)
>     add_header Cache-Control "no-store, no-cache, must-revalidate";
>     etag off;
> 
>     # 접근 로그 비활성화 (보안 강화)
>     access_log off;
>     error_log /var/log/nginx/tor-error.log;
> }
> ```
>
> ```txt
> # /etc/tor/torrc
> 
> HiddenServiceDir /var/lib/tor/hidden_service/
> HiddenServicePort 80 127.0.0.1:80
> ```