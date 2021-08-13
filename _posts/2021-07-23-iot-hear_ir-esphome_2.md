---
title: '[HomeAssistant] 하트IR ESPHOME 펌웨어로 변경, HA연동하기 (2)'
author: kkangz
date: 2021-07-23 22:35:00 +0900
categories: [Dev, Home IoT]
tags: [HomeIoT, 하트IR, HomeAssistant, ESPHome, tuya-convert]
---

본 포스팅에서는 하트IR을 통해 에어컨을 제어하기 위해 앞 포스팅에서 

생성한 ESPHome 바이너리를 하트IR에 올리기 까지 내용을 기술 합니다.


본 포스팅은 시리즈로 게시됩니다. 

### 목차

1. Home Assistant 내 ESPHOME 에서 Firmware Image (Bin) 만들기
2. 라즈베리파이를 이용해서 업데이트 서버로 위장, 하트IR 펌웨어 변경하기 (본 페이지)


### tuya-convert 이용해서 펌웨어 변경(하트IR 펌웨어 -> tasmota)

대략적인 순서는 하트IR의 펌웨어를 tuya-convert 이용해서 Tasmota Firmware로 올리고,

Tamota Firmware에서 다시 ESPHome Firmware(앞 포스팅에서 만든) 로 올리는게 순서 입니다.

라즈베리파이나 WiFi가 가능한 리눅스 컴퓨터의 터미널을 실행 후, 
아래와 같이 차례로 입력 합니다.

```console
> sudo su - root
> mkdir tuya_convert
> cd tuya_convert
> git clone https://github.com/ct-Open-Source/tuya-convert .
> ./install_prereq.sh
> ./start_flash.sh
```
전체적인 과정은 아래와 같습니다.

```console
pi@raspberrypi:~ $ sudo su - root
root@raspberrypi:~# mkdir tuya_convert
root@raspberrypi:~# cd tuya_convert/
root@raspberrypi:~/tuya_convert# git clone https://github.com/ct-Open-Source/tuya-convert .
Cloning into '.'...
remote: Enumerating objects: 1387, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1387 (delta 0), reused 0 (delta 0), pack-reused 1386
Receiving objects: 100% (1387/1387), 3.88 MiB | 2.05 MiB/s, done.
Resolving deltas: 100% (857/857), done.
root@raspberrypi:~/tuya_convert# ./install_prereq.sh
Hit:1 http://archive.raspberrypi.org/debian buster InRelease
Get:2 http://raspbian.raspberrypi.org/raspbian buster InRelease [15.0 kB]
Get:3 http://raspbian.raspberrypi.org/raspbian buster/main armhf Packages [13.0 MB]
Fetched 13.0 MB in 12s (1,098 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree
Reading state information... Done
..생략..
Ready to start upgrade
```

여기까지 하고, ./start_flash.sh 를 수행하면 각종 경고문구 무시하시고..   
(하트IR의 펌웨어를 변경하기 때문에 조심하라는..경고 입니다.ㅠㅠ 임의로 장비의 펌웨어를 변경하는 행위이니 당연히 위험은 소비자의 몫.)

yes 입력 후 엔터.

주의사항 : 컴퓨터에서 ssh로 라즈베리파이나 리눅스로 접속하면안됩니다.ㅠ   
 이 과정을 거치면 라즈베리파이의 wifi연결을 끊고 Wifi AP 를 새로 만들기 때문에 연결이 끊깁니다..^^;;

![](https://user-images.githubusercontent.com/9496842/126811947-09c74f99-851b-4447-af95-6f771cc553d3.png)

여기까지 나오면 ENTER 누르지 말고 기다리세요.
\
내용 읽어보시면 아시겠지만 스마트폰으로 vtrust-flash Wifi에 접속 합니다.\
\
그리고 WiFi 접속 후 Connect 페이지 까지 폰에서 확인 합니다.

![](https://user-images.githubusercontent.com/9496842/126822669-76e1b8dd-c5e5-47cc-b1ce-508b6a7fd402.png)

그 뒤, 하트모양 IR을 전원 연결 하고 리셋버튼(버튼이 하나밖에 없어요^^)를 
\
5~6초간 누른 뒤 녹색 LED가 깜빡 깜빡 할때까지 기다립니다. 
\
깜빡 거리기 시작하면 ENTER를 눌러주세요!

그럼 아래와같이 Scan을 시작합니다.. 
저는 바로 찾아서 Firmware backup을 진행하던데, 안되면 여러번 시도해야 한다고 합니다.

기존 펌웨어 백업 중..
(백업된 펌웨어는 tuya_convert/backups에 저장됩니다. 하지만 전 쓸 일이 없을 것 같습니다.ㅎㅎ)
![](https://user-images.githubusercontent.com/9496842/126812043-25cc2a1e-4323-410f-bb5f-6918c92ca337.png)

백업이 완료되면 flash 옵션이 나옵니다. \
2) flash tasmota.bin 선택해야 하니 2 누르고 Enter 그리고 y 를 입력 주세요.

![](https://user-images.githubusercontent.com/9496842/126812076-846a11b1-d160-4e1d-93dc-393f3a7f7d1d.png)


플래시가 끝나면 아래와 같이 나옵니다.    
tasmota-XXXX 를 연결해서 남은 설정을 하라고 하네요.
다른 기기도 flash할거니? 묻는 화면엔 n 을 누르고 빠져 나옵니다.

이제 라즈베리파이or리눅스 컴퓨터로 하는 작업은 모두 끝났습니다.

![](https://user-images.githubusercontent.com/9496842/126812131-b28a1869-846f-4a3e-9472-534cf9729812.png)

이제 다시 휴대폰으로 Wifi를 검색해 봅니다.   
tasmota-XXXXX 를 찾아서 연결한 뒤, AP1 SSID() 항목에 와이파이 이름이랑 비밀번호를 입력합니다.    
그리고 Save 눌러주세요.

![](https://user-images.githubusercontent.com/9496842/126822847-014f8aae-6a40-40db-8f51-011d6e475ecb.png)

이제 하트IR기기가 해당 와이파이로 접속을 할 겁니다..   
이제 본인 집의 공유기 설정으로 가서 tasmota-XXXXXXX 로 연결되어있는 기기가 있는지 확인 합니다.   
아래 화면은 공유기마다 설정이 달라서.. 무선 공유기 설정화면에서 찾아보셔야 합니다!   
그리고 화면에 나온 ip를 같은 네트워크에 연결된 컴퓨터 브라우저로 접속 합니다.

![](https://user-images.githubusercontent.com/9496842/126808483-045fff82-0dfa-49bf-abcc-7cfc3d37d07b.png)


짠~ Tamosta 바이너리가 하트IR에 올라갔습니다.   
Tasmota로 둔갑한 하트IR에서 보여주는 화면 입니다.   
이제 이걸 다시 앞 포스팅에서 만든 ESPHome binary로 교체해야 합니다.   
Firmware Upgrade 메뉴로 들어가 주세요.   

![](https://user-images.githubusercontent.com/9496842/126808564-25df1288-2fa1-4d5b-a45f-2de8249e90d2.png)

아래 Upgrade by file upload로 펌웨어 변경을 할 예정입니다.   
앞 포스팅에서 만든 Binary 파일을 선택 후 Start Upgrade 를 눌러주세요!

![](https://user-images.githubusercontent.com/9496842/126808790-10b1d73c-938b-4b21-9e86-2e9ae1e14b5c.png)

자 이제 정상적으로 업로드가 완료 되었습니다.

![](https://user-images.githubusercontent.com/9496842/126808825-32530a2e-8705-4024-8a9e-6b413f9e5c13.png)

이제 ESPHome 대쉬보드로 돌아가보면, 아까 만든 설정이 ONLINE 으로 바뀐 걸 볼 수 있습니다.

![](https://user-images.githubusercontent.com/9496842/126808931-e6496a34-5f34-4e0c-9643-00b6429f3ead.png)

이후에 EDIT로 YAML 파일 설정 하단에 아래 라인을 넣은 뒤 INSTALL - Wireless 하고 기다리면,   
하트IR이 불이 켜지고 하트IR에 리모컨을 향하고 버튼을 눌러 보시면 LOG에서 관련 정보가 뜰겁니다!   
이제 이 값들로 잘 설정을..ㅎ   

```xml
esphome:
  name: livingroom
  platform: ESP8266
  board: esp01_1m

# Enable logging
logger:

# Enable Home Assistant API
api:

ota:
  password: "5e191a0ed751137a3b274c51eddd8a76"

wifi:
  ssid: "xxxxx"
  password: "xxxxxxxxx"

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Livingroom Fallback Hotspot"
    password: "MsA2r2Le1i2r"

captive_portal:

# 여기서부터 아래에 추가해주세요^^
web_server:
  port: 80
  
status_led:
  pin: GPIO4

sensor:
  - platform: uptime
    name: "IR Uptime"

  - platform: wifi_signal
    name: "IR WiFi signal"
    update_interval: 60s

binary_sensor:
  - platform: status
    name: "IR Status"

  - platform: gpio
    pin: GPIO13
    id: physical_button

text_sensor:
  - platform: version
    name: "IR ESPHome version"

remote_transmitter:
  pin:
    number: GPIO14
  carrier_duty_percent: 50%

remote_receiver:
  pin: GPIO5
  dump: all

switch:
  - platform: template
    name: Turn on TV
    turn_on_action:
      - remote_transmitter.transmit_sony:
          data: 0x00000750
          repeat:
            times: 5
            wait_time: 45ms
    id: tv_on
```


![](https://user-images.githubusercontent.com/9496842/126859982-7fd006a1-380a-4760-a953-f8ed13c0cd99.png)

![](https://user-images.githubusercontent.com/9496842/126859977-5446f8ab-c442-419f-954b-6b0efc6abcce.png)

![](https://user-images.githubusercontent.com/9496842/126859979-77c37ef7-3ab0-47d3-ab6a-37024a1f869a.png)


이상입니다!
