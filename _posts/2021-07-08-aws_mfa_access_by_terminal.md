---
title: '[AWS] EC2/기타 Terminal에서 AccessDeniedException 에러 해결하기(MFA)'
author: kkangz
date: 2021-07-08 17:30:00 +0900
categories: [Dev, AWS]
tags: [알고리즘, DFS, DFS알고리즘, 코딩테스트]
---

웹 콘솔에서는 잘 되는데, Terminal 에서 각종 AWS 리소스에 접근 하려고 하면 본 포스팅을 주의깊게 봐주세요!

그리고 본인 계정이 [MFA(Multi-Factor Authentication)](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html){:target='_blank'}를 통한 OTP 인증을 안하고 있으면 지나가 주셔도 좋습니다.

본 포스팅은 Terminal 에서 MFA 인증 필요한 상황에서 AccessDeniedException 을 해결하기 위한 포스팅 입니다!


```
botocore.exceptions.ClientError: An error occurred (AccessDeniedException)
when calling the GetItem operation: User: arn:aws:iam::xxxxxxxx
is not authorized to perform: dynamodb:GetItem on resource:
arn:aws:dxxxxxxxxxx:table/xxxxxxxxx with an explicit deny
```

여기서 에러는 Explicit Deny. 즉 명싱적으로 거부하는 권한(Policy)이 있다는 뜻 입니다.

저같은 경우는 아래와 같은 Deny 룰이 그룹권한에 포함 되어 있었습니다.

```json
{
            "Sid": "DenyAllExceptListedIfNoMFA",
            "Effect": "Deny",
            "NotAction": [
                "iam:CreateVirtualMFADevice",
                "iam:EnableMFADevice",
                "iam:GetUser",
                "iam:ListMFADevices",
                "iam:ListVirtualMFADevices",
                "iam:ResyncMFADevice",
                "iam:ChangePassword",
                "iam:GetAccountPasswordPolicy",
                "sts:GetSessionToken"
            ],
            "Resource": "*",
            "Condition": {
                "BoolIfExists": {
                    "aws:MultiFactorAuthPresent": "false"
                }
            }
        }
```

Web console에서는 MFA로 접속하기 때문에 정상적으로 다 접근이 가능하지만, Terminal에서는 MFA인증이 안되어 AccessDeniedException이 발생하는 거였습니다!

Terminal에서도 MFA인증과정이 필요 합니다.
먼저 내 MFA 디바이스 SerialNumber를 아래 명령어로 찾고, 

> aws iam list-virtual-mfa-devices

본인 계정에 나오는 "SerialNumber" 값을 기록 해 둔 다음, 

![](http://user-images.githubusercontent.com/9496842/124892036-76153080-e014-11eb-9dcc-512dd0838a00.png)


아래 명령어로 access key id, secret access key, session token을 구합니다. (Expiration 시간까지 세션 유지 됨.)

> aws sts get-session-token --serial-number \<SERIAL_NUMBER\> --token-code XXXXXX (XXXXX는 Authy등 OTP 번호)

![](http://user-images.githubusercontent.com/9496842/124892462-de641200-e014-11eb-8816-87b84abd3518.png)

그 다음 환경변수 AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN 를 설정 해 주면 정상적으로 됩니다!!!

* MAC/Linux
> export AWS_ACCESS_KEY_ID="ASIA5EN2LGZPLPEO7J2U"<br>
> export AWS_SECRET_ACCESS_KEY="rMil+kXhPfHwdyjpjb1tRhst0l59Mejc7idYkoQR"<br>
> export AWS_SESSION_TOKEN="IQoJb3JpZ2luX2VjEPD//////////wEaCXVzLWVhc3QtMiJHMEUCI.... snip... "

* Windows
> setx AWS_ACCESS_KEY_ID ASIA5EN2LGZPLPEO7J2U<br>
> setx AWS_SECRET_ACCESS_KEY rMil+kXhPfHwdyjpjb1tRhst0l59Mejc7idYkoQR<br>
> setx AWS_DEFAULT_REGION "IQoJb3JpZ2luX2VjEPD//////////wEaCXVzLWVhc3QtMiJHMEUCI.... snip... "


다시 해제하고 하려면 unset 으로 각 환경변수를 해제한 뒤 다시 시도하세요 :)


