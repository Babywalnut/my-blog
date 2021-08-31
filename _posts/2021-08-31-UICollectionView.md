---
title:  "[iOS] UICollectionView"
date:   2021-08-31 17:38:50 +0900
categories: iOS
tags: [UICollectionView, CollectionView]
toc: true
toc_sticky: true
header:
  overlay_image: /images/background3.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
---

이전 포스팅에서는 HIG의 Collections를 통해 Collection의 올바른 사용예시와 주의해야할 점에 대해서 보았습니다. <a href ="https://developer.apple.com/documentation/uikit/uicollectionview" style="color:gray"><U>Collections(이전 포스팅)</U></a>

이번 포스팅에서는 애플 공식문서의 UICollectionView를 직접 번역해보겠습니다.

<span style="color:gray">참고자료 : <br></span><a href ="https://developer.apple.com/documentation/uikit/uicollectionview" style="color:darkgreen"><U>UICollectionView</U></a>

# **UICollectionView**

UICollectionView는 정렬된 컬렉션형태의 아이템들을 관리하고 커스터마이징이 가능한 레이아웃으로 보여주는 역할을 하는 객체입니다.


<br>
## **Delcaration**

```swift
@MainActor class UICollectionView : UIScrollView
```
UIScrollView 추상 클래스를 상속받고 있습니다. 여기서 `@MainActor` **Swift 5.5**에서 새로 도입된 어트리뷰트로 해당 어트리뷰트를 적용해주면 이 작업은 항상 메인스레드에서 동작하게 됩니다. 때문에 iOS에서 UI관련 동작은 항상 메인스레드에서 동작해야 하므로 **swift 5.5**부터는 해당 어트리뷰트를 통해 적용시켜줄 수 있습니다.


<br>
## **Overview**

collection view를 유저인터페이스로써 적용시킬 때 collection view와 관련된 데이터를 다루는 것이 해당 앱의 주요기능이 될 것입니다.

collection view는 `dataSource`프로퍼티에 저장되어 있는 datasource 객체로부터 데이터를 받습니다. datasource를 사용할 때 collection view의 동작과 데이터관리에 특화된 `UICollectionViewDiffableDataSource`를 사용 할 수 있습니다. 이외에 `UICollectionViewDataSource`를 통하여 커스텀 datasource를 만들어서 사용할 수도 있습니다.

<br>
collection view의 데이터들은 묶어서 보여줄 수 있는 각각의 item으로 이루어져 있습니다. 하나의 item은 보여줄 수 있는 가장 작은 단위입니다. 예를들어 사진앱에서 하나의 아이템은 사진앱의 하나의 사진이됩니다. collection view는 datasource가 제공하는 `UICollectionViewCell`클래스의 인스턴스인 `cell`을 이용하여 item들을 보여줍니다.

<br>
**Figure 1. flow layout을 사용한 collection view.**
<p align="center"><img alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/131495585-b33e56a7-8e96-495f-b13e-e690779e7c4d.png"></p>
<br>

각각의 cell뿐만 아니라 다른 타입의 view를 사용해서도 데이터를 표현할 수 있습니다. 예를 들어 위 그림의 "Characters"나 "Items"처럼 section header나 section footer로써 cell과는 분리된 영역에 원하는 정보를 추가적으로 보여지게 만들 수 있습니다. 이런 추가적인 view은 선택사항이고 원할시 해당 view들의 배치를 관리해주는 collection view의 layout 객체를 이용하여 추가해줄 수 있습니다.

<br>
게다가 UICollectionView를 인터페이스에다가 추가할시에 collection view의 메서드를 사용해서 datasource객체에 저장되어있는 순서대로 아이템을 시각적으로 보여지게 할 수 있습니다. `UICollectionViewDiffableDataSource`객체를 사용하면 이 과정을 자동으로 해줄 수 있습니다. 만약 커스텀으로 datasource를 만들었다면 UICollectionView의 메서드인 `insert`, `delete`, `rearrange`를 사용하여 cell들을 정렬해줄 수 있습니다.

<br>
또한 `delegate`객체를 활용하여 collection view 객체의 선택된 아이템들을 관리할 수 있습니다.


<br>
## **Layout**

`layout` 오브젝트는 collection view의 컨텐츠들을 시각적으로 정렬시키는 역할을 합니다. `UICollectionViewLayout`의 subclass인 layout 객체는 collection view 내부의 모든 cell과 부가적인 view들을 어떻게 모으고 배열시킬지를 정해줍니다. 

비록 layout이 모든 요소들의 위치를 정의해주지만 해당 작업과 관련된 view들에게 직접 적용시켜주지는 않고 정의된 위치정보들을 **collection view가** 화면에 보여지도록 관련 view들에게 적용시켜줍니다. 왜냐하면 cell과 부가적인 view들이 생성될 때 collection view와 datasource객체간에 관계를 가지고 있기 때문입니다. layout 객체는 item data대신에 시각적인 정보를 제공하는 것 외에는 다른 datasource와 다를게 없습니다.

<br>
일반적으로 collection view를 정의해줄 때 layout 객체를 정의해주지만 collection view의 layout을 직접 수정해줄 수도 있다. layout 객체는 `collectionViewLayout`프로퍼티 안에 저장되어있다. 별다른 애니메이션이 없는 경우 해당 프로퍼티를 통해서 다른 직접 layout을 수정해줄 수도 있다. 만약 에니메이션을 사용할 경우에는 `setCollectionViewLayout(_:animated:completion:)`을 대신 사용하면 된다.

<br>
gesture recognizer나 touch event와 같은 상호작용을 통한 변화를 주고싶을 때는 `startInteractiveTransition(to:completion:)`메서드를 사용하여 layout을 수정해주면 된다. 해당 메서드는 변화를 추적하는 gesture recognizer나 event-handling코드를 통해 작동하는 매개역할을 하는 layout 객체를 부여받는다.

event-handling코드가 변화 즉 event 행위가 끝났다고 판단하면 `finishInteractiveTransition()`이나 `cancelInteractiveTransition()`메서드를 호출하여 위에서 사용하던 매개하던 layout을 삭제하고 예정된 target layout객체를 부여한다.


<br>
## **Cells and Supplementary Views**

collection view의 datasource 객체는 item에 해당하는 컨텐츠와 이 컨텐츠를 보여주는 view를 모두 제공합니다. collection view가 컨텐츠들을 불러오면 collection view는 datasource에게 각각의 컨텐츠 item이 보여질 view를 제공해달라고 요청합니다. collection view는 datasource에 재활용된다고 분류된 view들을 `queue`나 `list`형태로써 해당 객체들을 계속 들고 있습니다. 코드에 명시된 view들을 새로 만드는 대신에 **기존 view를 queue에서 빼서 사용하게 됩니다.**

<br>
queue에서 view를 빼오는 메서드는 두가지가 있습니다. 어떤 타입의 view를 가져올지에 따라 사용되는 메서드가 정해집니다.

- collectionView의 아이템에 해당하는 cell을 불러올 때는 `dequeueReusableCell(withReuseIdentifier:for:)`을 사용합니다.
    
- 위에서 언급했던 layout 객체를 통해 부가적인 view들을 요청할 때는 `dequeueReusableSupplementaryView(ofKind:withReuseIdentifier:for:)`을 사용합니다.

<br>
위의 두 메서드를 호출하기 전에 현재 존재하지 않는 불러올 view들을 어떤 방식으로 생성할 건지 collection vew에게 알려줘야합니다. 이를 위해 클래스 형태나 nib 파일 형태로 collection view에 등록해야만 합니다. 예를들어 cell들을 등록할 때 `register(_:forCellWithReuseIdentifier:)`메서드를 사용하여 클래스를 등록하거나 ` register(_:forCellWithReuseIdentifier:)`를 사용하여 nib 파일을 등록합니다. 등록 과정의 일부분으로써 view를 재사용할지 안할지를 reuse identifier을 식별해줍니다. 이 값은 나중에 dequeue과정에서도 사용되는 String타입의 파라미터입니다.