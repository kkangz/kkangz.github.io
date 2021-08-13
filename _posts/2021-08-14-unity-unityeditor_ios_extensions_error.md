---
title: '[Unity]] Unable to resolve reference UnityEditor.iOS.Extensions.Xcode'
author: kkangz
date: 2021-08-13 20:30:00 +0900
categories: [Dev, Unity]
tags: [Unity, UnityEditor.iOS.Extensions.Xcode]
---

Unity 에서 Firebase Crashlytics를 추가하는데 이런 에러가 자꾸 난다.   
Firebase Plugin에서 iOS 관련 DLL이 있어서 그런 것으로 보인다.

```
Unable to resolve reference 'UnityEditor.iOS.Extensions.Xcode'. Is the assembly missing or incompatible with the current platform?
```
```
Assembly 'Assets/ExternalDependencyManager/Editor/Google.IOSResolver_v1.2.166.dll' will not be loaded due to errors:
Unable to resolve reference 'UnityEditor.iOS.Extensions.Xcode'. Is the assembly missing or incompatible with the current platform?
Reference validation can be disabled in the Plugin Inspector.

Assembly 'Assets/Firebase/Editor/Firebase.Editor.dll' will not be loaded due to errors:
Unable to resolve reference 'UnityEditor.iOS.Extensions.Xcode'. Is the assembly missing or incompatible with the current platform?
Reference validation can be disabled in the Plugin Inspector.


Assembly 'Assets/Firebase/Editor/Firebase.Crashlytics.Editor.dll' will not be loaded due to errors:
Unable to resolve reference 'UnityEditor.iOS.Extensions.Xcode'. Is the assembly missing or incompatible with the current platform?
Reference validation can be disabled in the Plugin Inspector.
```

![](https://user-images.githubusercontent.com/88815970/129387102-aef4c1df-2048-4279-a8ef-8bc583228e54.png)


이 문제는 간단히 Unity Hub에서 iOS 모듈을 추가해 주면 된다.

![](https://user-images.githubusercontent.com/88815970/129387154-e617179f-dbfa-4df3-b24e-bf3b04c4059f.png)

![](https://user-images.githubusercontent.com/88815970/129387130-49b1c3ac-bb69-4085-9c25-e072e26ce7d0.png)


이상입니다!
