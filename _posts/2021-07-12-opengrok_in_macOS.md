---
title: '[Docker] Docker로 OpenGrok 설치해서 소스분석을 쉽게하기'
author: kkangz
date: 2021-07-12 17:30:00 +0900
categories: [Dev, Tips]
tags: [OpenGrok, 소스분석툴]
---

Docker는 설치 되어 있다는 가정하에 기고 합니다.
사실 직접 설치를 하려고 했으나 관리주체가 oracle로 바뀌면서 정보도 별로 없고 공식 가이드 참고해서 해도 잘 안되어서 Docker로 진행

Docker Hub 주소 : [https://hub.docker.com/r/opengrok/docker/](https://hub.docker.com/r/opengrok/docker/){:target='_blank'}

인증이 필요하거나, 큰 사이즈의 프로젝트이거나 하면 사용 하기엔 적합하지 않다. (그럴땐 서버에 다이렉트로 OpenGrok 설치)
Opengrok 도커 이미지에는 Tomcat 도 내장되어있어 간편하게 설정 후 웹으로 바로 접속이 가능하다!

* Docker 설치
> docker pull opengrok/docker

* Docker 실행
> docker run -d -v \</나의/소스/경로/\>:/opengrok/src -p 8080:8080 opengrok/docker:latest

주의사항 : 소스경로는 실제 소스파일들이 있는, 최대한 깊은 경로로 지정해 주셔야 합니다! (한단계 위로 하니 Indexing이 안되더라구요.)


![](http://user-images.githubusercontent.com/9496842/125232228-22a62980-e317-11eb-9bca-0810d717de0b.png)



이후 [http://localhost:8080/](http://localhost:8080/){:target='_blank'} 접속!!
처음 접속하면 indexing 중으로 새로고침하다보면 각 폴더들이 차례대로 추가되는 것을 볼 수 있다.

매우 잘 나온다... 

혹시 서버 등에 모든포트 열려있으면 외부에서도 접속 가능할 것 같으니 조심하자.



