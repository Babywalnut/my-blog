---
title:  "[HIG] Collections"
date:   2021-08-30 17:38:50 +0900
categories: HIG
tags: [Collections, CollectionView]
toc: true
toc_sticky: true
header:
  overlay_image: /images/hig.png
---

이번 포스팅에서는 `Human Interface Guidelines > Views > Collections`를 번역해보겠습니다.

<span style="color:gray">참고자료 : <br></span><a href ="https://developer.apple.com/design/human-interface-guidelines/ios/views/collections/" style="color:darkgreen"><U>Collections</U></a>

<br>
## **Collections**

collection은 그림과 같은 컨텐츠들을 일련의 순서를가지고 관리해주며 커스터마이징이 가능한 매우 시각적인 레이아웃으로 보여줍니다.

collection은 꼭 엄격한 선형구조를 띄지 않아도 되기때문에 다양한 사이즈로 항목들을 보여주는데에 유용합니다. 일반적으로 collection은 이미지 컨텐츠를 보여줄때 사용되는 것이 이상적입니다.

구조적으로 하위 항복들과 시각적으로 구분이 필요할 때는 배경이나 다른 꾸밀때 사용되는 View들을 통해서 구분시킬 수 있습니다.

<br>
<p align="center"><img alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/131330681-05e26657-94ab-4522-87ff-55863e540944.png"></p>

<br>
collection은 상호작용과 애니메이션을 지원합니다. 기본적으로 탭해서 컨텐츠를 선택할 수 있고 꾹 눌러서 수정을하고 스와이프해서 스크롤을 할 수 있습니다. 만약 이외에 다른 동작이 필요할 경우에 custom action들을 추가할 수 있습니다. 

collection 내부에서는 아이템이 추가되거나 삭제되거나 다시 정렬될 때 애니메이션을 활용할 수 있으며 직접 만든 커스텀 에니메이션을 활용할 수도 있습니다.


<br>
**기본적인 행이나 그리드 레이아웃으로 충부한 경우인데도 불구하고 틀자체를 바꾸는 새로운 디자인을 사용하는 것을 자제해야합니다.** collection은 사용자의 경험을 강조하기 때문에 관심을 끄는 핵심요소이기보다는 아이템을 고르는 행위자체를 편하게 하는데 목적을 둬야합니다.

만약 collection내에서 아이템을 고르는 행위자체가 어려워진다면 사용자들은 당황하게되고 그들이 원하는 컨텐츠에 도달하기 이전에 흥미를 잃어버릴 것입니다. 적당한 너비의 패딩을 컨텐츠 둘레에 주어서 레이아웃이 깔끔하고 컨텐츠들간에 겹치는 현상을 방지해야 합니다.


<br>
**이미지가 아닌 텍스트 데이터를 다룰 때는 collection이 아닌 table을 사용하는 것을 고려해야합니다.** 일반적으로 스크롤 가능한 리스트로 텍스트 정보를 보여줘야할 때는 table을 사용하는 것이 간결하고 효과적입니다.


<br>
**레이아웃이 동적으로 변할때는 주의해야합니다.** collection의 레이아웃은 언제든 변할 수 있습니다. 만약 사용자가 사용하는 도중에 레이아웃이 변하도록 구현했다면 그 변화가 필요하고 충분히 의미가 있는 변화여야하고 추적하기 쉬워야합니다.

이유가 없는 레이아웃 변화는 앱을 예측하기 어렵고 사용하기 어렵게 만듭니다. 만약 레이아웃이 변화하는 동안 정보가 손상된다면 사람들은 불편함을 느낄 것입니다.

<br><br>
## **Before I finish** 
이번 포스팅을 요약하자면 collection을 사용할 때는 다루고자하는 데이터가 어떤형태인지 또 사용자가 컨텐츠를 고를 때 편리하게 구현하였는지를 고민해야 한다는 내용같습니다. 너무 collection에 과도하게 많은 투자를하여 사용하기 불편하다던가 기존에 있는 collection의 형태에서 너무 벗어나는 것에 주의하며 사용해야 할 것 같습니다👍

다음 포스팅에서는 UICollectionView의 공식문서를 번역해보겠습니다.

이상 포스팅을 마치겠습니다.🙈