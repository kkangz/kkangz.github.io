---
title: '[AWS] Amplify의 SSL인증서를 이용한 DNSZi CNAME 연결방법(https 적용)'
author: kkangz
date: 2021-07-29 20:30:00 +0900
categories: [AWS]
tags: [AWS, Amplify, AWS Certificate Manager, SSL, HTTPS]
---
### 개요
AWS Amplify 를 사용하여 간단한 Web Application 을 만든 이후,   

AWS Certificate Manager 를 통해 SSL, HTTPS 설정하는 방법 입니다.   

제가 가지고 있는 domain 은 네임서버(dns)를 [DNSZi.com](https://dnszi.com) 에서 관리하고 있기 때문에,   
이를 바탕으로 설명하겠습니다.   

또한 저는 제 Amplify 웹을 제가 소유한 도메인의 서브도메인 react.mydomain.com 으로 구성 하려고 합니다.   

먼저 Amplify 를 이용해 간단한 웹 앱을 만들면,   

아래와 같이 초기 설정 단계에서 “Add a custom domain with a free SSL certificate” 이 나옵니다.   

저는 설정을 해 놨기 때문에 체크 되어있지만, 처음엔 진행해야 하는 항목으로 출력이 됩니다.

![](https://user-images.githubusercontent.com/9496842/127426108-e5e6e594-986e-427c-9875-af740e153345.png)

먼저 Amplify 설정 Console 에서 Domain 을 추가해 줍니다. Domain Management - Create   
  
도메인 주소에는 mydomain.com 과같이 root domain 을 입력해 주세요.

![](https://user-images.githubusercontent.com/9496842/127426584-33cf54c9-ba6f-41bf-a69f-c043e680c714.png)

이후 Manage subdomain 메뉴로 들어가서 아래와같이 사용할 subdomain 설정을 해줍니다.    

저의 경우에는 react 로 입력했습니다.   

![](https://user-images.githubusercontent.com/9496842/127426898-7126c490-1acc-4b42-8fa6-48dc8218d6d9.png)


이후에 SSL configuration 설정을 확인해 보면 아래와 같이 뜨는데요,   

![](https://user-images.githubusercontent.com/9496842/127427175-20467132-4fe0-4626-ab81-2356d89180d9.png)

DNSzi - CNAME 관리에서 아래와 같이 추가해 줍니다.   
윗 사진이랑 값 비교해보세요. 좌측이 왼쪽 CNAME 값, 우측이 목적지 도메인 입니다.(마지막에 . 은 지우세요)   

![](https://user-images.githubusercontent.com/9496842/127429828-e567f1be-ba38-450f-9de5-e8d36c452735.png)

몇시간 뒤에 다시 Amplify Console 확인해 보면 xxxxxxxxxxx.cloudfront.net URL 이 생겨있습니다. 아래와 같이요.   

본인의 subdomain 주소와 Cloudfront.net 주소를 마찬가지로 DNSzi 에 추가해 줍니다. 아래 2번 항목처럼요.   

![](https://user-images.githubusercontent.com/9496842/127430007-ad45ed53-2d92-4313-b8c1-b01c46a52ee6.png)

설정이 완료되었습니다! 조금 기다려보시면 SSL Configuration pass 되고   
Domain 설정에 Available 로 변경되는걸 확인하실 수 있을 겁니다.


웹페이지 로딩 화면   

![](https://user-images.githubusercontent.com/9496842/127430247-4853e317-1d7d-4a75-9326-78bbfd2846ba.png)


이상입니다!