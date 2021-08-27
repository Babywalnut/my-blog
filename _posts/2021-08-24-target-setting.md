---
layout: single
title:  "[iOS] Xcode 11에서 iOS 12 이하 버전 생성하기"
date:   2021-08-24 2:54:50 +0900
classes: wide
categories: 
  - iOS
tags: [Deployment target, xcode]
---



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
   
