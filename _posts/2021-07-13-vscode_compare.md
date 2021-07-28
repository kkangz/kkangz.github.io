---
title: '[VSCode] VSCode에서 파일/소스 비교하기(Source Compare)'
author: kkangz
date: 2021-07-13 17:30:00 +0900
categories: [Development Tips]
tags: [VSCode, Compare, 소스비교, 파일비교]
---

VSCode 에서 간단 파일 비교를 위한 팁 입니다.
먼저 Compare 창 진입은 VSCode 내에 command로 하는데, 방법은 아래와 단축키로 Show all command 창에서 명령을 내립니다. 

> [Windows] Ctrl + Shift + p 

> [Mac]  Command + Shift + P or F1 (with fn key)

위 단축키를 누르면 상단에 command 입력창이 뜨는데, 요기에 compare 만 치시면 아래와 같이 3개 명령이 나옵니다.

![](http://user-images.githubusercontent.com/9496842/125379024-bd137500-e3ca-11eb-8b00-1701c054493d.png)

그리고 각 편한 방법대로 비교하시면 됩니다!


1. Compare Active File With.. (현재 열려 있는 파일과, 다른 파일간에 비교하기)
다른 파일을 선택하면 이렇게 파일 비교를 해 준다.
![](http://user-images.githubusercontent.com/9496842/125445955-10fca98e-6050-43dc-ba54-a3e7d1505ec2.png)


2. Compare Active File with Clipboard. (현재 열려있는 창과, 클립보드 내용과 비교하기)
현재 Clipboard 에 복사되어있는 코드랑 비교할때 사용.
![](http://user-images.githubusercontent.com/9496842/125446032-08813f90-f1f3-4eea-98cd-2ed23d3d86e5.png)


3. Compare Active File with saved. 현재 수정된 코드와 저장전 코드와 비교하기
작업중인 문서가 저장하기 전 이라면, 저장 전과 현재 코드를 비교 해줌.
![](http://user-images.githubusercontent.com/9496842/125446097-f458f737-71a6-4870-bda0-fb4c07dd9a21.png)


이상입니다!
