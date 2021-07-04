---
layout: post
title:  "[iOS] UIStackView"
date:   2021-07-04 2:52:50 +0900
categories: iOS
tags: [UI, StackView, UIView]
---

이번 포스팅은 여러개의 View들을 행으로 정렬하거나 열로써 정렬하여 효과적으로 관리하는 Interface인 

UIStackView에 대해서 포스팅해보겠습니다.

<span style="color:gray">참고자료 : <br></span><a href ="https://developer.apple.com/documentation/uikit/uistackview" style="color:darkgreen"><U>UIStackView 공식문서</U></a>

<br>
## **UIStackView**

Stack view는 디바이스에 맞게 스크린 사이즈나 UI 내에서 사용할 수 있는 공간의 변화에 따라

유동적으로 user interface를 맞춰주는 Auto Layout을 따르게 됩니다..

Stack view가 layout을 관리하는 view들은 모두 Stack view의 `arrangedSubviews`프로퍼티에 저장됩니다.<br>
(이기능에 대해서는 아래에서 더 자세히 다루겠습니다.)

해당 프로퍼티에 저장된 view들은 Stack view의 `axis`에 따르고 `arrangedSubviews`에 저장된 순서에 따라 정렬됩니다.

각 view의 정확한 layout은 Stack view의 `axis`,`distribution`,`alignment`,`spacing`와 같은 Stack view의 프로퍼티에 따라 달라집니다.


![uistack_hero_2x_04e50947-5aa0-4403-825b-26ba4c1662bd](https://user-images.githubusercontent.com/56648865/124387296-eb34ed00-dd18-11eb-80fe-5855cadf3ca4.png){: width="600"}{: .center}

<br>


<span style="color:red">**Stack view의 size와 position만 지정해주면 Stack view가 안에 있는 요소 view의 layout과 size를 관리한다.**<span>

<br>
## **Stack View와 Auto Layout**

Stack View는 내부에 포함하고 있는 view들의 size와 position을 잡기 위해 Auto Layout을 사용합니다.

Stack View는 첫번째 view부터 마지막 view까지 axis를 따라 정렬합니다.

**horizontal** 즉, 축이 가로로 놓여있는 Stack에서는 첫번째 나열되어 있는 view는 Stack의 leading edge에 붙어서 놓이고

마지막 view는 trailing edge에 붙어서 가로로 정렬됩니다.

**vertical**, 축이 세로로 놓여있는 Stack에서는 Stack의 top edge와 bottom edge를 기준으로 세로로 정렬됩니다.

만약 `isLayoutMarginsRelativeArrangement`프로퍼티가 true일 경우 Stack view는 내부에 정렬된 view들을

edge가 대신에 margin영역에 붙여서 정렬합니다.

<br>
Stack view 내부의 view들을 resize하여 내부에 채워넣는 Stack view의 Enum 프로퍼티인 `Distribution`를 사용할 때

`fillEqually` case를 제외한 모든 경우에 각각 내부에 view들의 사이즈를 계산할 때 `intrinsicContentSize`를 사용합니다.
(intrinsicContentSize는 각 view들이 가지고 있는 고유 layout의 size입니다.)

`UIStackView.Distribution.fillEqually`는 `arrangedSubviews`에 포함된 view들의 사이즈를 axis를 기준으로

동일한 크기로 맞춰 Stack View를 채워줍니다.

<br>
Stack view 내부의 view들을 정렬하는 프로퍼티인 `Alignment`를 사용할 때 

`fill` case를 제외하고는 각 view들의 축의 수직방향의 size를 계산할 떄 `intrinsicContentSize`를 사용합니다.

`UIStackView.Alignment.fill`은 내부의 모든 view들을 축의 수직방향크기를 꽉 채우도록 resize시킵니다.

이 경우 Stack view는 가능한 한 내부의 view 중 축의 수직방향으로 가장 큰 view에 맞춰 모든 view를 늘립니다.

![cd8978aa-c754-456a-b2b3-d5485952fc9d](https://user-images.githubusercontent.com/56648865/124392547-3824bd80-dd31-11eb-9c4e-7f9f252d0bd4.png){: width="600"}{: .center}

<br><br>
## **Stack View자체의 Positioning과 Sizing**

Stack view 내부의 view들의 layout을 설정할 때 Auto Layout을 사용하지 않더라도 

Stack view자체의 layout을 설정할 떄 Auto Layout을 사용할 수 있습니다. 

다시 말해 Stack view의 위치를 정할때 최소 인접한 두 edge를 고정시키면 됩니다.

추가적인 설정 없이 Stack view 내부의 view사이즈를 기반으로 시스템은 Stack view 자체의 크기를 결정하게 됩니다.

*  Stack view의 축 방향으로의 `fitting size` 즉, vertical이면 세로 horizontal이면 가로 방향은 내부에 포함된 view들의 size와 사이의 공간의 합과 같습니다.

*  Stack view 축의 수직 방향의 `fitting size`는 Stack view에 포함된 view들 중 가장 큰 view의 size와 같습니다.

*  Stack view의 `isLayoutMarginsRelativeArrangement`프로퍼티가 `true`라면 Stack view의 사이즈는 위에 설명한 size에 margin을 포함한 size가 됩니다.

<br>
Stackview의 너비와 높이를 직접 지정해주면 내부 view들의 layout과 size를 사용가능한 공간에 맞추게 됩니다.

view들의 정확한 layout은 Stack view의 프로퍼티를 어떻게 지정해주냐에 따라 달라집니다. 

Stack view내부에 공간이 부족하거나 남을경우 내부 view들을 어떻게 다루는 지는 <a href ="https://developer.apple.com/documentation/uikit/uistackview/distribution" style="color:darkgreen"><U>UIStackView.Distribution</U></a> 과

 <a href ="https://developer.apple.com/documentation/uikit/uistackview/alignment" style="color:darkgreen"><U>UIStackView.Alignment</U></a> 에서 자세히 확인할 수 있습니다.

<br>
top, bottom, center Y position 이외에도 first baseline 이나 last baseline으로도 Stack view의

position을 결정할 수 있습니다. 

Stack view의 `fitting size`처럼 baseline도 Stackview가 포함하는 view들에 의해서 결정됩니다.

*  horizontal Stack view는 `forFirstBaselineLayout`와 `forLastBaselineLayout`의 반환값으로 가장 높은 view를 반환합니다.<br><br>만약 가장 높은 view 역시 Stack view라면 안에 존재하는 Stack viewdml `forFirstBaselineLayout`와 `forLastBaselineLayout`의 반환값을 반환합니다.

*  vertical Stack view는 처음에 정렬되어 있는 view를 `FirstBaselineLayout`으로 반환하고 마지막에 정렬되어 있는 view를 `LastBaselineLayout`으로 반환합니다.<br><br>이 또한 Stack view라면 위와 똑같은 방식으로 내부 Stack view의 해당 값을 반환합니다.

<br>
<span style="color:red">**Baseline alignment는 view 자체가 가지고 있는 `intrinsic height`에 의해 height 이 결정되는 view에만 적용이 됩니다. 만약 view가 다른 요소에 의하여 늘어나고 줄어들게 된다면 baseline은 잘못된 위치에 나타나게 될 것입니다.**<span>
