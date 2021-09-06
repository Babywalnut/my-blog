---
title:  "[iOS] Xcode 11에서 iOS 12 이하 버전 생성하기"
date:   2021-08-24 2:54:50 +0900
categories: 
  - iOS
tags: [Deployment target, xcode]
toc: true
toc_sticky: true
header:
  teaser: /images/background3.jpg
  overlay_image: /images/background3.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
---

이번 포스팅에서는 iOS 프로젝트 생성시 앱의 최소 버전을 12이하로 설정하는 법에 대해 적어보겠습니다.

<span style="color:gray">참고자료 : <br></span><a href ="https://sarunw.com/posts/create-new-ios12-project-in-xcode11/" style="color:darkgreen"><U>Create a new iOS12 project in Xcode11</U></a>

<br>

iOS 12 이후부터는 앱의 생명주기와 UI의 생명주기가 달라지므로 더 낮은 버전을 사용하고 싶을 때는 수정해줘야 하는 부분이 있습니다. 

생명주기에 대한 자세한 정보는  <a href ="https://babywalnut.github.io/my-blog/ios/AppDelegate-SceneDelegate/" style="color:gray"><U>[iOS] AppDelegate & SceneDelegate</U></a>에서 확인하시면 됩니다.


## **Deplyment Target 수정**

현재 앱 타겟의 General 탭에서 Deployment Info 를 수정해줍니다.

![DeployTarget](https://user-images.githubusercontent.com/56648865/130635921-e877ded8-4ce6-40f3-b3e6-c7c53bc99701.png)






## **@available**

프로젝트 첫 생성시 Xcode는 앱의 최소 버전을 13버전 이상으로 셋팅합니다. 

최소 버전을 12 이하로 설정하려면 몇가지를 수정해줘야 합니다.



1. **SceneDelegate.swift**
SceneDelegate는 iOS 13 이후 버전에 서 필요하므로 availability attributes인 @available을 SceneDelegate 클래스에 추가해줍니다.
    ```swift
       @available(iOS 13.0, *)
       class SceneDelegate: UIResponder, UIWindowSceneDelegate {
         ...
       }
    ```
2. **AppDelegate.swift**
AppDelegate에 선언되어 있는 메서드 중 두개의 메서드는 SceneSession을 관리하는 메서드입니다. 
마찬가지로 각각 @available 를 추가해줍니다.
```swift
       // MARK: UISceneSession Lifecycle
       @available(iOS 13.0, *)
       func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
         ...
       }
       
       @available(iOS 13.0, *)
       func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
         ...
       }
```
3. **UIWindow**
마지막으로 SceneDelegate에서 Scene을 관리하기 위하여 선언해주었던 UIWindow 변수를 Window 관리를 위하여 AppDelegate에 선언해줍니다.
```swift
       class AppDelegate: UIResponder, UIApplicationDelegate {
       	var window: UIWindow?
       }
```

<br><br>
## **Before I finish** 
이 글을 쓰는 시점에서는 여전히 많은 앱들이 iOS 12이하 버전을 지원하고 있습니다. 물론 이후에는 더이상 예전의 생명주기를 설정해주지 않을 날도 오겠지만 또 다른 생명주기를 사용할 수도 있기 때문에 조금더 해당 설정을 편리하게 설정할 수 있으면 더 좋지 않을까하는 생각이 들었습니다 😂 

이상 포스팅을 마치겠습니다.🙈