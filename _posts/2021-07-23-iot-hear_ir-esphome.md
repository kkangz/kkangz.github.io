---
title: '[HomeAssistant] 하트IR ESPHOME 펌웨어로 변경, HA연동하기(1)'
author: kkangz
date: 2021-07-23 22:30:00 +0900
categories: [Dev, Home IoT]
tags: [HomeIoT, 하트IR, HomeAssistant, ESPHome, tuya-convert]
---

### 개요

저는 집에 있는 헤놀로지 NAS를 이용하여 Docker에 Home Assistant 를 운영 중 입니다.

집에 있는 에어컨에 와이파이 모듈이 없어서, 아래와 같이 생긴 하트 모양의 

IR 기기를(Timethinker Tuya IR Controller)

사용해서 거실에 둔 다음, 가정에서 사용하는 대부분의 리모컨 버튼을 

입력하여 자유자재로 원격으로 조종하고자 하는게 목표 입니다.



![](https://user-images.githubusercontent.com/9496842/126789866-667e2ac7-adfc-445f-a2a4-854ffc4b7a62.png)



제품에 보면 Google Assistant, amazon alexa 등을 지원해서 그냥 써도 될 것 같지만,

많은 사람들이 저런 클라우드 서버를 통하지 않고 빠른 동작 처리를 위해 로컬에서 사용하기 위해서

펌웨어를 업데이트 하여 Home Assistant 내 ESPHOME 으로 기기를 연동 합니다. 



내용이 길어서 3부작으로 작성하려고 합니다.

### 목차

1. Home Assistant 내 ESPHOME 에서 Firmware Image (Bin) 만들기 (본 페이지)
2. 라즈베리파이를 이용해서 업데이트 서버로 위장, 하트IR 펌웨어 변경하기





### 준비물

* 라즈베리파이 3 
   * 라즈베리파이가 아니더라도 리눅스가 돌아가는 Wifi 달린 컴퓨터.
   * vmware로도 가능
* 하트 IR
* ESPHome 설치가 가능한 Home Assistant 환경


### ESPHome 설치


먼저 ESPHome이 설치가 안되어 있으신 분들은, 

Home Assistant 의 Supervisor - 애드온 스토어에서 - ESPHome 검색 후 다운로드 합니다.

![](https://user-images.githubusercontent.com/9496842/126802185-6f84729c-b804-4445-b65e-4bf8f0ddc21a.png)

설치 후 실행한 다음, 웹 UI 열기 를 눌러서 ESPHOME 대쉬보드로 들어갑니다.

![](https://user-images.githubusercontent.com/9496842/126802292-2883e221-8a75-4efb-8a61-235b6a974c80.png)


### ESPHome 기기 등록

대쉬보드에서 + 버튼을 눌러서 설정을 추가 해 줄 것인데요, 저는 거실에서 쓸 것이므로 livingroom 으로 지었습니다.
WI-FI SSID하고 비밀번호는 집 WIFI, PASSWORD를 넣어주세요!

![](https://user-images.githubusercontent.com/9496842/126802391-75317607-30ba-4a94-b5cb-a391a3cbef76.png)

ESP Device는 ESP8266을 선택하고 NETX를 눌러 줍니다.

![](https://user-images.githubusercontent.com/9496842/126802433-c08eae45-ca9c-4bb3-8640-0f92a036981d.png)

그럼 설정이 하나 추가 되었는데요, 여기서 ISNTALL 을 누르고, Manual Download를 눌러서 바이너리 파일을 빌드 합니다.
다 끝나면 bin 파일이 하나 저장되는데 이걸 컴퓨터에 저장해 놓습니다.

![](https://user-images.githubusercontent.com/9496842/126804605-8fb0a65d-5a0a-49f9-93a4-37fc653cfbe1.png)

![](https://user-images.githubusercontent.com/9496842/126804647-e95e98b6-b579-4136-a310-2a82c444c7c5.png)

![](https://user-images.githubusercontent.com/9496842/126804697-c778c9fe-30b3-4570-a87d-9925140a2ff9.png)


여기까지 Firmware Image (Bin) 파일 만들기였습니다.
다음 포스팅에서는 tuya-convert 라는 해킹(?) 툴을 이용해서 tasmota라는 바이너리를 올리고, 
tasmota 바이너리 내에 위치한 firmware update기능으로 이 바이너리를 최종적으로 하트 IR 에 플래시 할 예정입니다.

이상입니다!
