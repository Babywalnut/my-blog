---
layout: single
title:  "[iOS] AppDelegate & SceneDelegate"
date:   2021-06-15 2:52:50 +0900
categories: iOS
tags: [iOS, AppDelegate, SceneDelegate, lifeCycle, 생명주기]
toc: true
toc_sticky: true
header:
  overlay_image: /images/background3.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
---

이번 포스팅에서는 AppDelegate 와 iOS13, ios 11 이후 등장한 SceneDelegate에 대해서 다뤄보겠습니다

<span style="color:gray">참고자료 : <br></span><a href ="https://medium.com/@kalyan.parise/understanding-scene-delegate-app-delegate-7503d48c5445" style="color:darkgreen"><U>Understanding Scene Delegate & App Delegate</U></a><br>
<a href ="https://learnappmaking.com/scene-delegate-app-delegate-xcode-11-ios-13/" style="color:darkgreen"><U>Scene Delegate vs. App Delegate Explained</U></a><br>
<a href ="https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle" style="color:darkgreen"><U>Managing Your App's Life Cycle</U></a>


<br>
## **iOS 13, Xcode 11 AppDelegate, SceneDelegate**
<br>
Xcode 11 이전 버전에서 iOS 어플레이케이션 프로젝트를 생성하면 AppDelegate.swift, ViewController.swfit 그리고 Storyboard 파일이 생성되었습니다.

하지만 Xcode 11 이후 버전부터는 한가지 파일이 더 추가되었는데 바로 SceneDelegate.swift입니다.


AppDelegate와 SceneDelegate의 역할과 용도에 대해 헷갈리는 부분이 많아 해당 내용에 대해 정리해보겠습니다.

<img width="756" alt="스크린샷 2021-06-15 오후 6 03 01" src="https://user-images.githubusercontent.com/56648865/122025025-f379c680-ce03-11eb-9b9b-bf7d8ad65e5e.png">

iOS 13 이전 버전에서는 AppDelegate만을 사용하여 앱의 생명주기와 UI의 생명주기에 관련된 로직들을 모두 다뤘습니다. 

<img width="871" alt="스크린샷 2021-06-15 오후 6 04 17" src="https://user-images.githubusercontent.com/56648865/122025194-1e641a80-ce04-11eb-934d-4a695a683689.png">

iOS 13 이후 버전부터는 AppDelegate가 혼자서 맡던 역할이 AppDelegate와 SceneDelegate에게 나눠졌고 

이로인하여 iPad OS 에서는 multi-window를 구현할 수 있게 되었습니다.

```
iOS 13 이후부터는 AppDelegate는 앱의 생명주기를 관리하고 SceneDelegatesms 앱에서 스크린에 표시되어지는 UI의 생명주기를 관리합니다.
```

<br><br>
## **AppDelegate와 SceneDelegate의 역할**

<br>
#### **AppDelegate**

iOS 13에서도 여전히 AppDelegate는 앱의 시작점을 맡고있습니다. AppDelegate는 앱의 생명주기를 관리할때 호출됩니다.

이외에도 

* root view controller 설정

* 앱의 초기설정 셋팅

* 노티피케이션 푸쉬 관리

등이 가능합니다.
<br><br>

AppDelegate.swift의 default형식에는 3개의 메서드가 정의되어 있고 매우 중요한 역할을 하고 있습니다.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool

func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration

func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>)
```
<br>

* **첫번째 메서드**는 구현중인 앱이 처음 실행되면서 앱의 셋업이 완료되었을때만 호출됩니다. <br><br>
iOS 13 이전 버전에서는 UIWindow 객체를 구성하고 해당 UIWindow에 뷰컨트롤러를 할당하여 <br><br>
화면에 보여지게하기 위해서도 사용해왔습니다.<br><br>
iOS 13 이후 버전부터는 앱이 Scene을 가지고있다면 해당 메서드는 더이상 해당 이벤트에 관여하지않고 SceneDelegate가 관리하게 됩니다.<br><br>
![스크린샷 2021-06-17 오후 11 26 44](https://user-images.githubusercontent.com/56648865/122416251-8ad94800-cfc3-11eb-8ec4-1561f148cb0e.png)<br><br>
위와 같이 메서드가 반환하는 true는 해당 메서드에 명시되어 있는 조건을 만족하면 앱이 실행될 수 있다는 것을 뜻합니다.<br><br>
하지만 때에 따라 false를 통해 앱을 실행시켜주지 않을 수도 있습니다.

<br>
* **두번째 메서드**는 사용자나 시스템이 새로운 Scene을 생성했을때 호출됩니다. <br><br>
해당 메서드로 Scene이 만들어졌을 때 전달 받는 scene session은  Scene에 관련된 모든 정보를 추적합니다. <br><br>
즉, 해당 Scene에 대한 어떤 SceneDelegate를 사용하는지, 어떤 Storyboard를 사용하는지, 필요에따라 어떤 Scene subclass를 사용하는지 등 Scene의 여러 Configuration을 설정합니다.<br><br>
<img width="1276" alt="스크린샷 2021-06-20 오후 12 54 25" src="https://user-images.githubusercontent.com/56648865/122661566-a82a3400-d1c6-11eb-863f-06ca427289dc.png"><br><br>
Configuration을 설정하는 것을 info.plist를 통하여 정적으로 설정할 수도 있고 해당 메서드내에 동적으로 코드를 구현할 수도 있습니다.<br><br>
만약 info.plist에 구현했다면 위의 코드와 같이 인자로 받아오는 값을 return값에 넣어주면 간단하게 설정할 수 있습니다.

<br>
* **세번째 메서드**는 유저가 app switcher를 통하여 Scene을 닫을 때 호출됩니다.<br><br>
해당 메서드와 단지 Scene과의 연결을 끊고 Scene과 관련된 메모리를 해제시키지만 user data나 state를 영구적으로 제거하지않는 `sceneDidDisconnet(_:)`메서드와는 확실히 구분해야합니다.<br><br>
`sceneDidDisconnet(_:)`메서드는 Scene을 disconnect하고 나중에 reconnect시킬 수 있습니다..<br><br>
하지만 discard메서드는 영구적으로 Scene의 user data 나 State를 완전히 제거합니다.(저장되지않은 텍스트편집기 앱의 초안(draft)같은)<br><br>
여기서 특이한 점은 앱이 `Not running`상태에서 discard메서드가 호출되면 앱의 마지막 상태를 추적해놨다가 나중에 다시 실행시킬 때 마지막 상태를 불러오게됩니다.<br>
(해당 기능은 stateRestoration과 관련되어있는데 추후 설명하겠습니다.)

<br>


<br><br>
#### **SceneDelegate**

iOS 13 이후 버전부터 이전에 AppDelegate가 가지고있던 권한의 일부가 이관되었습니다.

UIWindow에 관련된 권한인데 SceneDelegate의 UIScene을 뜻합니다.

앱은 하나 이상의 scene을 가질 수 있고 그 scene은 앱의 인터페이스와 컨텐츠를 다룹니다.

따라서 SceneDelegate는 디스플레이에 보여지는 UI와 데이터에 대한 권한을 가지고 있습니다.
<br><br>

SceneDelegate.swift의 default형식에는 6개의 메서드가 정의되어 있습니다.

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)

func sceneDidDisconnect(_ scene: UIScene)

func sceneDidBecomeActive(_ scene: UIScene)

func sceneWillResignActive(_ scene: UIScene)

func sceneWillEnterForeground(_ scene: UIScene)

func sceneDidEnterBackground(_ scene: UIScene)
```
<br>

* **첫번째 메서드**는 UISceneSession 생명주기에서 첫번째로 호출되는 메서드입니다. <br><br>
이 메서드는 새로운 UIWindow를 만들고 root view controller를 설정해주고 디스플레이에 보여질 key window로 설정합니다.<br><br>

<br>
* **두번째 메서드**는 앱이 실행될때 즉, 처음으로 앱이 active상태에 돌입할때나 backgound에서 foreground상태로 넘어올 때 호출됩니다.<br><br>

<br>
* **세번째 메서드**는 WillEnterForeground 메서드 바로 뒤에 호출되며 Scene이 설정되고 화면에 표시할 준비를합니다.<br><br>

<br>
* **네번째, 다섯번째 메서드**는 앱이 background에 올라갔을때 호출됩니다.<br><br>

<br>
* **마지막 메서드**는 scene이 앱으로부터 disconnect될 때 호출됩니다.(재연결 가능)<br><br>
해당 메서드는 위에 설명한 AppDelegate의 세번째 메서드 설명을 보면 더 이해할 수 있습니다.


<br><br>
**정리하자면.. IOS 13버전 이전에는 AppDelegate에서 앱과 UI 생명주기를 모두 관리했지만**
**13버전 이후에는 UI의 생명주기는 SceneDelegate로 이관되었고 생명주기 또한 변경되었습니다.**
**window개념이  scene으로 대체되었고 AppDelegate에서는 Scene을 추적하는 Scene Session에 관련된 메서드가 추가되었습니다.**

<br><br>
**포스팅을 하는 시점에서 개발하는 앱마다 SceneDelegate를 활용할수도 있고 안할수도 있습니다.<br> iOS 13버전을 최소버전으로 사용하라면 SceneDelegate를 활용할것이고 아니라면 이전의 AppDelegate만 활용할 것입니다.<br>때문에 AppDelegate만 활용하고 싶은 경우에는 SceneDelgate.swift파일을 삭제하고 이관된 메서드를 다시 AppDelegate에 구현하면 됩니다.<br>그렇게되면 생명주기 역시 이전의 생명주기를 따르게 됩니다.**

