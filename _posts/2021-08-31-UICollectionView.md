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

UICollectionView는 정렬된 아이템들을 관리하고 커스터마이징이 가능한 레이아웃으로 보여주는 역할을 하는 객체입니다.


<br>
## **Delcaration**

```swift
@MainActor class UICollectionView : UIScrollView
```
UIScrollView 추상 클래스를 상속받고 있습니다. 여기서 `@MainActor`는 **Swift 5.5**에서 새로 도입된 어트리뷰트로 해당 어트리뷰트를 적용해주면 이 작업은 항상 메인스레드에서 동작하게 됩니다. 때문에 iOS에서 UI관련 동작은 항상 메인스레드에서 동작해야 하므로 **swift 5.5**부터는 해당 어트리뷰트를 통해 적용시켜줄 수 있습니다.


<br>
## **Overview**

collection view를 유저인터페이스로써 적용시킬 때 collection view와 관련된 데이터를 다루는 것이 해당 앱의 주요기능이 될 것입니다.

collection view는 `dataSource`프로퍼티에 저장되어 있는 datasource 객체로부터 데이터를 받습니다. datasource를 사용할 때 collection view의 동작과 데이터관리에 특화된 `UICollectionViewDiffableDataSource`를 사용 할 수 있습니다. 이외에 `UICollectionViewDataSource`를 통하여 커스텀 datasource를 만들어서 사용할 수도 있습니다.

<br>
collection view의 데이터들은 묶어서 보여줄 수 있는 각각의 item으로 이루어져 있습니다. 하나의 item은 보여줄 수 있는 가장 작은 단위입니다. 예를들어 사진앱에서 하나의 아이템은 사진앱의 하나의 사진이 됩니다. collection view는 datasource가 제공하는 `UICollectionViewCell`클래스의 인스턴스인 `cell`을 이용하여 item들을 보여줍니다.

<br>
**Figure 1. flow layout을 사용한 collection view.**
<p align="center"><img alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/131495585-b33e56a7-8e96-495f-b13e-e690779e7c4d.png"></p>
<br>

각각의 cell뿐만 아니라 다른 타입의 view를 사용해서도 데이터를 표현할 수 있습니다. 예를 들어 위 그림의 "Characters"나 "Items"처럼 section header나 section footer로써 cell과는 분리된 영역에 원하는 정보를 추가적으로 보여지게 만들 수 있습니다. 이런 추가적인 view은 선택사항이고 원할시 해당 view들의 배치를 관리해주는 collection view의 layout 객체를 이용하여 추가해줄 수 있습니다.

<br>
게다가 UICollectionView를 인터페이스에다가 추가할 시에 collection view의 메서드를 사용해서 datasource객체에 저장되어있는 순서대로 아이템을 시각적으로 보여지게 할 수 있습니다. `UICollectionViewDiffableDataSource`객체를 사용하면 이 과정을 자동으로 해줄 수 있습니다. 만약 커스텀으로 datasource를 만들었다면 UICollectionView의 메서드인 `insert`, `delete`, `rearrange`를 사용하여 cell들을 정렬해줄 수 있습니다.

<br>
또한 `delegate`객체를 활용하여 collection view 객체의 선택된 아이템들을 관리할 수 있습니다.


<br>
## **Layout**

`layout` 객체는 collection view의 컨텐츠들을 시각적으로 위치시키는 역할을 합니다. `UICollectionViewLayout`의 subclass인 layout 객체는 collection view 내부의 모든 cell과 부가적인 view들을 어떻게 모으고 배열시킬지를 정해줍니다. 

비록 layout이 모든 요소들의 위치를 정의해주지만 해당 작업과 관련된 view들에게 직접 적용시켜주지는 않고 정의된 위치정보들을 **collection view가** 화면에 보여지도록 관련 view들에게 적용시켜줍니다. 이유는 cell과 부가적인 view들이 생성될 때 collection view와 datasource객체간에 관계를 가지고 있기 때문입니다. 즉, datsource와 layout의 역할은 비슷하지만 datasource는 collection view에게 item data를 제공하고 layout 객체는 시각적인 정보를 제공한다고 생각하시면 됩니다.

<br>
일반적으로 collection view를 정의해줄 때 layout 객체를 정의해주지만 collection view의 변화에 따라 layout을 변경해줄 수도 있습니다. layout 객체는 `collectionViewLayout`프로퍼티 를 통해 설정해줍니다. 별다른 애니메이션이 없는 경우 해당 프로퍼티를 통해서 즉각적으로 layout을 수정해줄 수도 있습니다. 만약 에니메이션을 사용할 경우에는 `setCollectionViewLayout(_:animated:completion:)`을 대신 사용하면 됩니다.

<br>
gesture recognizer나 touch event와 같은 상호작용을 통한 변화를 주고싶을 때는 `startInteractiveTransition(to:completion:)`메서드를 사용하여 layout을 변경해주면 됩니다. 해당 메서드는 gesture recognizer나 event-handling코드를 통해 변화를 추적하는 역할을 하는 layout 객체를 부여받습니다.

event-handling코드가 변화 즉 event 행위가 끝났다고 판단하면 `finishInteractiveTransition()`이나 `cancelInteractiveTransition()`메서드를 호출하여 위에서 사용하던 매개하던 layout을 삭제하고 예정된 target layout객체를 부여합니다.


<br>
## **Cells and Supplementary Views**

collection view의 datasource 객체는 item에 해당하는 컨텐츠와 이 컨텐츠를 보여주는 view를 모두 제공합니다. collection view가 컨텐츠들을 불러오면 collection view는 datasource에게 각각의 컨텐츠들이 보여질 view를 제공해달라고 요청합니다. collection view는 datasource에 재활용된다고 분류된 view들을 `queue`나 `list`형태로써 해당 객체들을 계속 들고 있습니다. 코드에 명시된 view들을 새로 만드는 대신에 **기존 view를 queue로부터 빼서 사용하게 됩니다.**

<br>
queue에서 view를 빼오는 메서드는 두가지가 있습니다. 어떤 타입의 view를 가져올지에 따라 사용되는 메서드가 정해집니다.

- collectionView의 아이템에 해당하는 cell을 불러올 때는 `dequeueReusableCell(withReuseIdentifier:for:)`을 사용합니다.
    
- 위에서 언급했던 layout 객체를 통해 부가적인 view들을 요청할 때는 `dequeueReusableSupplementaryView(ofKind:withReuseIdentifier:for:)`을 사용합니다.

<br>
위의 두 메서드를 호출하기 전에 현재 존재하지 않는 불러올 view들을 어떤 방식으로 생성할 건지 collection vew에게 알려줘야합니다. 이를 위해 생성한 view들을 클래스 형태나 nib 파일 형태로 collection view에 등록해야만 합니다. 예를들어 cell들을 등록할 때 `register(_:forCellWithReuseIdentifier:)`메서드를 사용하여 클래스를 등록하거나 `register(_:forCellWithReuseIdentifier:)`를 사용하여 nib 파일을 등록합니다. 등록 과정의 일부분으로써 view를 재사용할지 안할지 reuse identifier를 통해 식별해줍니다. 이 identifier는 나중에 dequeue과정에서도 사용되는 String타입의 파라미터입니다.

<br>
원하는 view를 datasource 메서드에서 올바르게 dequeue시켰다면 해당 view의 컨텐츠를 설정하고 collection view가 사용할 수 있도록 반환해주어야 합니다. 위에서 언급하였듯이 layout객체로부터 layout정보를 받았다면 collection view는 해당 view를 화면에 보이도록 적용시킬 것입니다.


<br>
## **Data Prefetching**

collection view는 좀 더 즉각적인 일처리를 위하여 두가지 방법의 Prefetching을 제공합니다.

- **Cell prefetching은 해당 cell이 실질적으로 필요한 시점 이전에 cell을 준비해놓는 것입니다.** 예를 들어 grid layout에서 하나의 row에 들어가는 cell들이 필요할 때와같이 많은 수의 cell을 collection view가 동시에 요구할 때 해당 cell들이 실제로 화면에 보여지기 이전에 요청됩니다. 그러므로 스크롤을 하면서 Cell들이 부드럽게 나타나고 사라지는 동작은 여러 레이아웃들이 유기적으로 동작하며 나타나게 됩니다. `Cell prefetching`은 다른 처리를 해주지 않아도 기본적으로 적용이 되어있습니다.

- **Data prefetching은 cell이 실제로 데이터를 필요로하기 전에 collection view의 요구사항을 미리 전달받으며 이루어지는 메커니즘을 가지고 있습니다.** 이러한 점은 네트워크 요청과 같이 가볍지 않은 데이터 전달 프로세스를 사용하여 Cell의 데이터가 전달될 때 유용합니다. `UICollectionViewDataSourcePrefetching` 프로토콜을 따르는 객체를 `prefetchDataSource`프로퍼티로 선언하여 cell들의 데이터가 언제 prefetch지 notification을 받을 수 있도록 해야합니다.


<br>
## **Reordering Items Interactively**

**Collection view는 사용자와의 상호작용을 통해 collection view 내의 아이템들을 이동시킬 수 있습니다.** 일반적으로, collection view내의 아이템 순서는 datasource의 순서에 의해 결정이 됩니다. 만약 사용자가 이를 재정렬할 수 있도록 하려면 gesture recognizer를 설정해주어 사용자와의 상호작용을 추적해 collection view 내의 아이템들이 재정렬되도록 만들 수 있습니다.

<br>
먼저 사용자에 반응하여 아이템들을 재정렬하려면 collection view의 `beginInteractiveMovementForItem(at:)`메서드를 호출합니다. geture recognizer가 터치 이벤트를 추적하는 동안 터치된 위치의 변화를 전달하기 위해 `updateInteractiveMovementTargetPosition(_:)`메서드를 호출합니다. gesture에 대한 추적이 끝났다면 `endInteractiveMovement()`메서드나 `cancelInteractiveMovement()`메서드를 호출하여 상호작용을 종료시키고 collection view를 갱신해줍니다.

<br>
위의 상호작용이 진행되는 동안 변화된 아이템의 정렬을 적용시키기 위해 collection view는 즉각적으로 기존 layout을 무효시킵니다. 상호작용 과정에서 따로 특정한 설정을 해주지 않으면 기본 애니메이션으로 아이템이 재정렬됩니다. 원할시 layout 애니메이션을 커스터마이징해 줄 수도 있습니다. 위의 과정이 끝났을 떄 collection view는 아이템들의 새로운 위치를 기반으로 datasource객체를 갱신해줍니다.

<br>
`UICollectionViewController`클래스는 기본적으로 gesture recognizer를 제공하며 해당 클래스에서 관리해주는 collection view를 재정렬해 줄 수 있습니다. 기본 gesture recognizer를 적용시키기 위해서는 `installsStandardGestureForInteractiveMovement` 프로퍼티의 값을 `true`로 주면 됩니다.


<br>
## **Interface Builder Attributes**

- **Items**<br>프로토타입 cell의 개수입니다. Items는 스토리보드상에 나타낼 프로토타입 cell의 개수를 결정하는 프로퍼티입니다. colletion view는 각각의 다른 타입의 컨텐츠를 표시하거나 같은 컨텐츠를 다른 방식으로 표현할 때 최소한 하나의 cell에서 많게는 여러개의 cell들을 가지고 있어야 합니다. 

<br>
- **Layout**<br>어떤 layout객체를 사용할 것인지 결정합니다. Layout을 통해 `UICollectionViewFlowLayout`을 사용할지 아니면 커스텀하여 생성한 layout객체를 사용할지를 결정하게 됩니다.<br><br>flow layout을 선택했다면 collection view의 스크롤될 방향을 선택하고 header와 footer view를 포함시킬지 결정해야 합니다. header 와 footer view를 활성화시키면 스토리보드에는 header와 footer view로 설정해줄 수 있는 재사용 가능한 view가 추가됩니다. 해당 view들은 코드로도 만들어줄 수 있습니다.<br><br>
만약 커스텀 layout을 사용하기로 결정했다면 해당 레이아웃이 `UICollectionViewLayout`을 상속받도록 해야합니다.

<br>
Flow layout을 선택되었을 때, collection view의 Size inspector에는 flow layout metrics를 설정해주는 attribute들이 추가됩니다. 해당 attribute들을 통하여 cell의 사이즈를 조절하고 header와 footer의 사이즈를 조절하고 cell의 간격, cell들이 포함되어 있는 section의 margin을 조절할 수 있습니다. 이에 대한 자세한 설명은 `UICollectionViewFlowLayout`공식문서를 보시면 됩니다.


<br><br>
## **Before I finish** 

이번 포스팅을 간략하게 요약하자면 <br>
- UICollectionView는 datasource가 제공하는 cell형식으로 이루어진 Item들을 보여주는 역할을 하는 객체입니다. 
- collection view는 datasource를 통해 필요한 데이터와 view를 전달받고 layout을 통하여 화면상에 어떻게 배치될지에 대한 정보를 전달받습니다.
- 사용자에 의한 어떠한 gesture나 이벤트에 따라 layout을 즉각적으로 변경해줄 수 있습니다.
- collection view에서 사용될 view중 재활용될 수 있는 view들은 일종의 queue형태로 꺼내어 쓸 수 있습니다.
- collection view는 cell과 데이터를 필요한 시점에 이전에 미리 전달받아 데이터 지연에 대처합니다. 
- collection view의 Item은 사용자에 의해 순서가 변경될 수 있고 collection view가 갱신되고 갱신된 순서가 datasource에 반영됩니다.
- collection view의 Interface Builder Attribute로는 Items, Layout두가지가 있습니다.

위와 같이 실질적으로 UICollectionView가 어떻게 구성되어 어떠한 원리로 동작하는지에 대하여 알아봤습니다. 다음 포스팅에서는 실제로 collection view를 구현하는 내용에 대하여 포스팅해보겠습니다.

이상 포스팅을 마치겠습니다.🙈