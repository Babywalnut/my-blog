---
layout: post
title:  "[iOS] URLSession"
date:   2021-05-31 10:52:50 +0900
categories: 클로져
tags: [URLSession, network]
---

## **URLSession이란?**

iOS에서 네트워크를 통한 데이터 통신을 도와주는 객체이다.

```swift
class URLSession : NSObject
```
URLSession 클래스와 관련된 클래스들은 우리가 설정한 URL로부터 데이터를 다운로드하고 업로드 시킬 수 있는 API를 제공합니다. 

또한 앱을 직접 사용하지않고 background상태에 있는동안 데이터를 다운로드 받고 suspended상태일때도 API를 사용할 수 있습니다. 

우리의 앱은 한 개 이상의 URLSession을 만드는데 각각은 데이터 통신에 관련된 작업에 쓰입니다. 

예를 들면, 만약 우리가 웹 브라우저를 만들고 있다면 각각의 탭이나 윈도우에 Session을 각각 만들게 될 것입니다. 

혹은 하나의 세션은 브라우저가 직접 상호작용하는데에 또 다른 하나는 백그라운드상에서 다운로드를 위해 쓰일 것입니다. 

각각의 앱은 Session내에서 Task를 나열할 것이고, 각각의 세션은 특정 URL에대한 요청을 나타낼 것입니다.

<br><br>
## **URL Session들의 타입**

하나의 주어진 URL Session내의 Task들은 단일 호스트내에서 동시에 최대로 연결할수 있는 연결의 개수나 연결이 cellular네트워크를 사용할 수 있는지 여부등에 대한 내용을 담고있는 같은 session configuration 오브젝트를 공유하게 됩니다.

URLSession을 통해 부가기능 없이 기본 요청을 보낼떄 사용할 때는 configuration 오브젝트를 가지고 있지 않는 싱글턴으로 구현된 Session을 사용합니다.

이런 기본 Session은 비교적 우리가 직접 만든 Session에 비해 입맛에 따라 커스터마이징하기 힘듭니다.

하지만 만약 제한적인 요청을 보낼 경우에는 좋은 선택이될 것입니다. 

다른 싱글턴 타입을 인스턴스화 시킬때와 마찬가지로 shared 메서드를 통하여 기본 Session에 접근할 수 있습니다.

<br>

**기본세션 이외의 다른 세션을 만들때는 아래의 항목 중 하나의 configuration을 따라야합니다.**

* default Session은 위에서 말한 shared Session과 비슷하게 동작하지만 우리가 직접 설계할 수 있습니다. 
delegate를 추가하여 데이터를 점진적으로 얻을 수도 있습니다. 

* Ephemeral Session들은 shared Session과 비슷하지만 캐시와 쿠키 또는 인증서등을 데이터에 저장하지 않습니다.

* Background Session들은 앱이 동작중이지 않을때 background에서 업로드와 다운로드를 할 수 있게 해줍니다.

각각 configuration을 구성하는 자세한 방법을 확인하려면 URLSession Configuration클래스내의 Creating a Session Configuration Object를 참고하면 확인할 수 있습니다.

<br><br>
## **URL Session Task의 타입**

구현한 Session내에서 우리는 서버로 데이터를 업로드할 수도 있고 데이터를 서버로부터 파일형식으로 디스크에 다운받을 수도 있고 NSData 오브젝트 형태로 메모리에 저장할 수도 있습니다. 

<br>
**이에 대해 URLSession API는 아래의 4가지의 Task를 제공합니다.**

* DataTask는 NSData 오브젝트를 통해 데이터를 주고받습니다. 서버에 짧고 여러번 주고받는 요청일 경우 사용합니다.

* UploadTask는 DataTask와 비슷하지만 앱이 동작하지 않을때도 background에서 업로드할 수 있는 기능을 지원합니다. 또한 파일형식의 업로드도 지원합니다.

* DownloadTask는 파일형식의 데이터를 다운로드합니다. 또한 앱이 실행되지 않을때 background 다운로드와 업로드도 지원합니다.

* WebSocketTask는 TCP, TLS통신에서 WebSocket프로콜을 사용하여 메세지를 주고받습니다.

<br><br>
## **Session Delegate 사용**

Session내에서 Task들은 delegate 오브젝트또한 공유합니다. 

<br>
**우리는 이 delegate를 통해 아래와 같은 이벤트가 발생하였을때 정보를 생성하거나 얻습니다.**

* 인증이 실패했을 경우

* 서버로부터 데이터를 받았을 경우

* 데이터가 캐싱할 수 있는 상태일 경우 

만약 delegate의 기능이 필요하지 않을경우 delegate인자에 nil값을 전달하여 세션을 생성하면 됩니다.. 

**```Session오브젝트는 delegate와 앱이 종료되거나 Session을 무효시키기 전까지는 강한 참조를 이루고 있습니다. 만약 Session을 무효화시키지 않으면 우리의 앱은 앱이 제거될때까지 메모리를 낭비할 것입니다.```**

<br><br>
## **비동기성과 URL Session**

다른 네트워킹 API들과 마찬가지로 URLSession또한 강한 비동기성을 띈다. 

**URLSession은 아래의 두가지 중 구현 방식에 따라 한가지의 방법을 선택하여 앱에 데이터를 반환합니다.**

* completion handler를 호출하여 데이터의 이동을 수행하거나 에러를 반환한다.

* Session의 delegate에서 메서드를 호출하여 데이터가 도착했거나 이동이 완료되었는지 확인한다.