---
title: 'Ubuntu Samba 설정하여 MacOS에서 공유폴더 마운트 하기'
author: kkangz
date: 2021-08-09 20:30:00 +0900
categories: [Development Tips]
tags: [Samba, Ubuntu, MacOS]
---
# 개요
Ubuntu에서 설정한 Samba Service를 이용하여 MacOS 에서 마운트 하는 방법 입니다.   
먼저 Ubuntu 에서 Samba 설정을 해 줍니다.

## Samba 설치
아래 명령어를 Ubuntu 에서 순차 입력하여 Samba를 설치 합니다.
```console
sudo apt-get -y update
sudo apt-get -y intall samba
```

또한 samba의 계정에 맞는 path 등을 설정 합니다.   
저는 home directory를 samba로 공유하도록 하겠습니다.   
먼저 아래 명령어로 samba 설정파일을 수정 합니다.

```console
sudo vi /etc/samba/smb.conf
```

이후 맨 아래쪽에 아래 라인을들을 추가 합니다.
계정명이 kkangz 인 경우 입니다.   

```
[kkangz]
    comment = kkangz HOME
    path = /home/kkangz
    public = no
    writable = yes
    browseable = yes
    valid users = kkangz
```
그리고 아래 명령어로 Samba Password 설정 및 유저 추가를 합니다.설정합니다.

```console
> sudo smbpasswd -a kkangz
New SMB password:
Retype new SMB password:
Added user kkangz.
```

저장 후 아래 명령어로 Samba를 재시작 합니다!   
```console
sudo service smbd restart
```

## MacOS에서 네트워크 연결 설정

이제 MacOS 로 옵니다.   
MacOS에서는 바탕화면에서 Command+K 를 누르거나,   
Finder - 이동 - 서버에 연결 메뉴 클릭해서   
아래 "서버에 연결" 메뉴로 진입 합니다.

![](https://user-images.githubusercontent.com/9496842/128672909-4a9bbdfd-162d-4e24-a601-00a8292d6094.png)

주소에는 smb://\<UBUNTU PC의 IP주소\> 를 넣어 줍니다. 

![](https://user-images.githubusercontent.com/9496842/128672942-0c4a06cb-75c9-4a53-8b8b-3aa6a375dc26.png)

이름, 암호에는 Samba에 설정한 name과 password를 넣어 주고 볼륨을 선택하면 정상적으로 연결 됩니다!

![](https://user-images.githubusercontent.com/9496842/128672965-68d1542b-2d15-4ef9-b786-e64a3c67d5f1.png)

![](https://user-images.githubusercontent.com/9496842/128673017-61c793b3-d83c-4f2e-9666-a2041839c7e6.png)

![](https://user-images.githubusercontent.com/9496842/128673209-87e3b1dc-091b-4847-832b-1511e3de8a95.png)

![](https://user-images.githubusercontent.com/9496842/128673243-097bcb37-e16f-4ef2-acf5-eede10114320.png)


## 그 외 Samba 명령어들

### Samba에 등록된 유저 확인
```console
sudo pdbedit -L
```

### Samba에 등록된 유저 확인(자세히 보기)
```console
sudo pdbedit -L -v
```

### Samba에 등록된 유저 삭제
```console
sudo smbpasswd -x username
```


이상입니다!