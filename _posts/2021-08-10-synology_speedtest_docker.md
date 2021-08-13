---
title: 'Synology NAS에 인터넷 속도 측정하여 탤레그램으로 알리기'
author: kkangz
date: 2021-08-10 20:30:00 +0900
categories: [Dev, NAS]
tags: [Synolgoy, Xpenology, NAS, SppedTest, Docker, Telegram]
---
# 개요
Synology NAS의 Docker 에 SpeedTest 이미지를 추가하여 최종적으로는 Telegram 봇을 통해
일별 인터넷 속도를 알려주는 걸 설정 해 봅니다.
얼마전에 유명 IT유튜버 [잇섭님 KT 사건](https://namu.wiki/w/KT%2010%EA%B8%B0%EA%B0%80%20%EC%9D%B8%ED%84%B0%EB%84%B7%20%EC%86%8D%EB%8F%84%20%EC%A0%80%ED%95%98%20%EC%82%AC%EA%B1%B4?from=KT%2010%EA%B8%B0%EA%B0%80%20%EC%9D%B8%ED%84%B0%EB%84%B7%20%EC%86%8D%EB%8F%84%20%EC%A0%80%ED%95%98%20%ED%8F%AD%EB%A1%9C%20%EC%82%AC%EA%B1%B4){:target='_blank'}도 있고 하니 저도 매일 속도테스트를 해 보기로 했습니다. 

## Docker에 SpeedTest 설치
Syology NAS 콘솔 접속 후 Docker - 레지스트리로 들어가서 speedtest 를 검색합니다.   
몇개가 뜨는데, 그 중 henrywhitaker3/speedtest-tracker 를 선택해서 다운받습니다.

![](https://user-images.githubusercontent.com/9496842/128807405-83c7cdb3-b1a5-4a5b-a878-e496ced35798.png)

## 설정파일이 들어갈 경로 만들기
다운하고있을때 SpeedTest 도커의 설정파일이 저장될 아무 폴더를 하나 만들어 줍니다.   
저는 /docker/speedtest 로 만들었습니다.

## SpeedTest 컨테이너 실행
Docker - 이미지로 가서, henrywhiteaker3/speedtest-tracker:latest 를 더블클릭해서 컨테이너를 생성해 줍니다.
먼저 고급설정으로 들어갑니다.

![](https://user-images.githubusercontent.com/9496842/128807883-07fc26fd-bcbd-408d-ac59-216116e51b4e.png)

볼륨 탭에서 아까 만든 폴더를 추가해 줍니다.
마운트 경로에는 /config 라고 적어주고 적용 합니다.

![](https://user-images.githubusercontent.com/9496842/128807970-31162096-bd4e-4a74-8354-f40bf882dc96.png)

그리고 환경 탭으로 가서, 
아래 내용을 넣습니다. 꼭 넣어야 합니다. OOKLA_EULA_GDPR은 무조건 true로 해야 동작을 하고,   
TZ에 Asia/Seoul 을 넣어주어야 이후 텔레그램 알림 시 시간 설정이 정확히 동작 합니다.

> 변수 : OOKLA_EULA_GDPR
> 값 : true
> 변수 : TZ
> 값 : Asia/Seoul

![](https://user-images.githubusercontent.com/9496842/128831356-382180a0-fdbc-4eec-8036-2ec58ef88885.png)

다음, 적용을 누르고 시작 합니다.

![](https://user-images.githubusercontent.com/9496842/128808306-4766984f-1967-44cb-b896-42f041496022.png)

그 뒤 Docker - 컨테이너- henrywhiteaker3/speedtest-tracker:latest 를 더블클릭 하여 WebServer 포트를 알아냅니다.
컨테이너 포트가 80인 로컬 포트를 찾으면 됩니다. (사진 상으로는 49157)

![](https://user-images.githubusercontent.com/9496842/128808867-15b97cab-f2a9-490d-9d81-f37e345b822d.png)


## Server 설정 및 테스트

인터넷 브라우저로 webserver:49157 로 접속 해 봅니다.   
그리고 Settings 메뉴로 들어갑니다.

![](https://user-images.githubusercontent.com/9496842/128809798-c0086261-8af8-4be4-968c-72055b78d34f.png)

Server 항목을 보면, Server ID 를 넣어주는데요,   
중간에 Server에 아래 값을 넣어 즙니다.   
```
> 6527
```

![](https://user-images.githubusercontent.com/9496842/128813946-22ebc1e7-972b-4911-942e-757221af4bfb.png)
---
### 참고 
이 서버 숫자는,아래 페이지에 나온 Korea 서버들 중 일부 입니다.   
원하시는 서버가 있으면 해당 서버 번호를 추가하시면 됩니다. 하나씩 넣어서 테스트 잘 되는 서버 id를 여러개 넣어도 됩니다.(, 로 구분)
https://williamyaps.github.io/wlmjavascript/servercli.html
![](https://user-images.githubusercontent.com/9496842/128810099-be633ddf-2ad1-434d-ab59-afa32df53ac1.png)
---

그리고 메인화면으로 가서 Start your first test! 를 누르고 기다리면 아래와 같이 결과를 볼수 있습니다.

![](https://user-images.githubusercontent.com/9496842/128814113-d7bc4674-55af-4358-8f75-e673c30357fb.png)


# 텔레그램 알림 받기

모바일 텔레그램 앱을 이용해서 봇 토큰을 받고 SpeedTest 서버에 추가하는 과정입니다.

# 텔레그램 봇 토큰 받기

모바일로 텔래그램 앱을 켠 뒤, 채팅창 목록에서 추가 - 사용자 명 BotFather 를 검색하고 추가 합니다.   
그 뒤 /newbot 명령어로 새로운 봇을 생성 한 뒤,   
이름을 설정 해 주고, Bot의 Username을 입력합니다.   
Username은 unique하게 지어야 하기 때문에 다른 사람들이 미리 생성한   
이름은 넣을 수 없으며, 꼭 끝이 소문사 bot 으로 끝나야 합니다.   
마지막으로 생성 후 나온 Token 을 복사해 놓고, 중간에 t.me/ 로   
시작하는 Telegram 방에 접속 해 놓습니다.

![](https://user-images.githubusercontent.com/9496842/128817318-1c5ad8b8-cc16-4d24-8b0a-0b84489a3e02.png)

# 텔레그램 chat id 알아내기

채팅창 목록에서 추가 - 사용자 명 에 IDBot 을 입력 한 뒤 추가 합니다.
그리고 /getid 명령어를 이용해서 숫자로 된 id 를 확인 합니다.

![](https://user-images.githubusercontent.com/9496842/128817614-c5856626-f0bd-4b39-a8f8-445c539d4be7.png)

### Token, ID 넣고 Test

Settings 로 가서  Telegram bot token 에 위에서 얻은 Tokoen 을,   
Telegram chat id 에 위에서 얻은 id를 입력 합니다.   
그리고 Test nofitication 을 눌러보면! 아까 들어간 채팅방에서 알림을 받을 수 있습니다!

![](https://user-images.githubusercontent.com/88093966/129059184-71782044-d3dd-4db2-8033-0456ce5a6af9.png)
![](https://user-images.githubusercontent.com/9496842/128817688-f36d67e7-ec7c-4a8c-af3d-c9418fcb7aa9.png)


Spreadtest overview time - 알림을 받을 시간 입니다. UTC 기준으로 넣어야 되더라구요.
한국이 9 시간 빠릅니다. 그래서 16으로 넣으면 아침 7시에 들어 옵니다.
Threshold alert percentage - 기본적으로 평균보다 15% 속도가 낮으면 추가 알림을 받을 수 있습니다.   
그 외 Download, Upload, Ping 들도 넣을 수 있으니 참고해 주세요 :)   

이렇게 설정하면 매일 아래와 같이 결과를 받아볼 수 있습니다.   
그런데...그런데!! 결과가 이렇게 너무 천차만별 입니다. ㅠㅠㅠ   
저는 1기가 인터넷 쓰는데...    
서버를 바꿔서 테스트를 좀 더 해봐야겠습니다.    

![](https://user-images.githubusercontent.com/88093966/129058630-6a414897-eb65-4d16-8743-7b886e0874f1.png)



이상입니다!