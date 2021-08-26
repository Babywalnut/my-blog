---
layout: posts
title:  "[iOS] Quartz 2D"
date:   2021-08-13 2:54:50 +0900
categories: iOS
tags: [Core Graphics, quartz]
---

이번 포스팅에서는 iOS의 Core Graphics의 기반이 되는 Drawing engine인 Quartz 2D에 대해서 살펴보겠습니다.

<span style="color:gray">참고자료 : <br></span><a href ="https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_overview/dq_overview.html#//apple_ref/doc/uid/TP30001066-CH202-TPXREF101" style="color:darkgreen"><U>Quartz 2D 공식문서</U></a>


<br><br>
## **Quartz 2D란?**

iOS나 Mac OSX 어플리케이션 환경에서 접근할 수 있는 수 있는 `Drawing Engine`으로써 2차원 그래픽을 그릴 때 사용됩니다.

Quartz 2D API를 통해서 좌표를 기준으로 그래픽 그리기, 투명도 조절, 그림자, 명암, 레이어의 투명도 등의 그래픽 특징을 조절할 수 있습니다. 

Quartz 2D가 사용될 때는 언제든 Grahpic 하드웨어의 도움을 받게됩니다.

iOS 프레임워크의 `Core Animation`, `UIKit`에서도 Quartz 2D를 이용해서 이미지를 생성할 수 있습니다.

<br><br>
## **Page**

Quartz 2D는 image를 그리는데에 `painter's model`을 사용합니다. 

`painter's model`에서 성공적으로 완성된 그리기 작업은 하나의 paint레이어를 **`Page`**라 불리는 출력용 canvas에 적용시킵니다.

page 위의 paint는 다른 그리기 작업을 시행하여 그 위에 다른 paint를 overlaying하는 방식으로 수정할 수 있습니다.

한번 그려진 paint는 이처럼 paint를 overlaying하는 방식 이외에는 수정할 방법이 없습니다.

<br>
아래 그림은 두가지 다른 형태의 그림을 그릴 때 순서의 중요성에 대해 알 수 있는 그림입니다.

첫번째 경우는 먼저 그린 그림의 테두리를 제외한 부분이 두번째 그림에 가려 모두 가려져 있습니다.

순서를 반대로한 두번째 경우에서는 파란색 그림도 전부 잘 보이게 나타납니다.

이처럼 `painter's model`을 사용할 때 순서는 매우 중요합니다.

<br>
<p align="center"><img width="400" alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/129348675-c99d2323-2311-4238-b074-359f0fb09428.gif"></p>

paint가 그려지는 page는 실제 종이가 될 수도 있고 PDF와 같은 가상의 종이가 될 수도 있고 bitmap 이미지가 될 수도 있습니다. 

우리가 어떤 `graphics context`를 사용하냐에 따라 page의 형태는 달라질 수 있습니다.

<br><br>
## **Drawing Destinations: The Graphics Context**

`graphics context`는 데이터를 캡슐화하는 불투명한 데이터 타입입니다.

Quartz를 통하여 그려진 모든 이미지는 output device 즉 PDF, bitmap, window display에 사용되기 위하여 그려집니다. 

`graphics context`에는 위에서 말한 기기에 따른 page의 타입정보와 여러 파라미터가 저장됩니다.

즉, Quartz를 통해 그려진 모든 객체는 `graphics context`에 포함됩니다.


<br>
우리가 어떤 device를 통해 출력할지에 따라 `graphics context`에 저장되는 고유 특징이 달라집니다. 

한마디로 `graphics context`를 결정하는 것은 `drawing destination`을 결정하는 것과 같은 뜻일 겁니다.

다시 말해 어떤 이미지가 Quartz를 통해 그려지는 과정은 똑같지만 어떤 `grahpics context`를 Quartz에게 제공하냐에 따라 각각 다른 device로 출력이 되는 것입니다.

아래의 그림을 보면 조금 더 이해가 될 것입니다.

<br>
<p align="center"><img width="400" alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/129354049-8e946607-3795-4165-bf03-e991b16f8557.gif"></p>

위의 그림처럼 drawing destination에 따라 여러가지 형태의 `graphics context`로 나타내질 수 있습니다.

graphics context에는 bitmap graphics context, PDF graphics context, window graphics context등 여러가지 종류가 있습니다.

그 중 iOS에서는 View graphics context를 사용합니다.

<br><br>
## **Quartz 2D Coordinate Systems**

Quartz는 그래픽을 생성할 때 고유의 `coordinate system` 즉 좌표계를 통해 위치와 사이즈를 나타냅니다.

이 좌표계는 실수 좌표로 이루어져 있습니다.

<br>
<p align="center"><img width="300" alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/129443904-6710a6a7-6904-4c33-b630-3bd24dcd7be7.gif"></p>


그래픽을 출력하게 되는 기기마다 display의 크기가 다르기 때문에 그래픽의 좌표를 device단에서 설정하게되면 기기마다 다르게 설정해줘야하는 번거로움이 있습니다.

때문에 Quartz는 그래픽을 device와 독립된 **user space**에 좌표계를 생성하고 각 기기의 **device space** 좌표계로 `current transformation matrix` 나 `CTM`을 통하여 맵핑을 시킵니다.

`current transformation matrix`라는 일종의 좌표를 이동(평행이동, 회전 등)시킬 수 있는 행렬을 좌표에 곱하여 기존의 좌표를 이동시킵니다.

다라서 그려질 좌표는 인식하되 실질적으로 그려지기 전에 기존의 Quartz좌표계에 보이는 이미지를 그리는 것이 아닌 각 device에 그려질 이미지로 변환하여 그려지게 됩니다.

UIKit도 고유의 좌표계를 가지기 때문에 마찬가지로 Quartz의 좌표계에 그려질 그래픽을 UIkit좌표계에 맞게 변형시킨 drawing contexts인 UIView를 활용하게 됩니다.

<br>
<p align="center"><img width="700" alt="e3663268-5db4-42c9-a7f0-2114920a9f1f" src="https://user-images.githubusercontent.com/56648865/129444074-0fe5ecce-e81f-4a61-8020-344fad522724.jpeg"></p>


위의 그래프와 같이 UIKit을 통하여 그려질 때는 Quartz좌표계에서 y 축의 양의 방향이 반대로 되어 좌표를 인식하게 됩니다.

**이외에도 문서에는 Quartz의 graphics contxt에 따라 사용되는 Opaque Data Types, Quartz의 메모리 관리 방식, Quartz를 통해 완성된 객체를 수정할 수 있는 Graphics States에 관한 설명이 있습니다.**
