---
layout: posts
title:  "[Swift]Swift API GuideLines (part 1)"
date:   2021-03-07 10:52:50 +0900
categories: 스위프트
tags: [Swift API GuideLines ]
---

### **Swift API GuideLines?**

애플은 Swift사용자들에게 일관된 룰에 따라 코드를 작성하게하여 스위프트 생태계에 더 가까이 접근할 수 있는 기회를 제공합니다. 

즉, 가이드라인을 숙지하여 Swift를 사용한다면 다른 Swift유저가 내 코드를 처음 봐도 비교적 그 의미를 쉽게 파악할 수 있으며 스위프트가 추구하는 방향을 이해할 수 있습니다. 

Swift를 계속 사용할 유저라면 필수적으로 익혀야할 명명법 규약에 대한 지침서라고 생각합니다.

<a href="https://swift.org/documentation/api-design-guidelines/">Swift API Guidelines 공식문서</a>

<br>
<br><br>

## **Fundamentals**

* **사용 시점에서의 명료성**<br>

 코드 내에서 메소드와 프로퍼티는 한번만 선언이 되지만 반복적으로 사용됩니다. 따라서 선언을 한 뒤 API내에서 용도에 따라 사용되는 시점에서 그 의미가 모호하지 않게 코드의 의미가 잘 전달되게 작성해야 합니다.

* **명료성은 갈결성보다 중요하다**<br>

간결하게 코드를 작성할 수 있지만 스위프트에서는 단순히 코드를 짧게 쓰는게 목적이 아닙니다. 오히려 코드를 짧게 작성하여 모호한 의미를 가지는 것보다는 길더라도 의미를 파악하기 쉽게 명료하게 쓰는것이 중요합니다.
<br><br><br>


## 명명법(Naming)

* **모호성을 피하고자 필요한 모든 단어를 포함해라.**

  >파라미터 'position'을 받는다고 명시되어있다. 이럴 경우 label값으로 at을 명시하여 remove 함수가 호출되어 사용될 때 x라는 파라미터가 어떻게 사용되는지 쉽게 이해하도록 도울 수 있다.
```swift
extension List {
  public mutating func remove(at position: Index) -> Element
}
employees.remove(at: x)
```

<br>
* **명확한 의도전달을 하기위하여 많은 단어가 필요할 수도 있지만 그 의미가 중첩되는 단어는 생략해라.**

  >Element가 함수명에도 명시되어있고 파라미터타입에 반복되어 명시되어있다. 이럴 경우에는 Element의 의미가 중첩되므로 한쪽을 제거해줘야한다.
    ```swift
  public mutating func removeElement(member: Element) -> Element?
  allViews.removeElement(cancelButton)
  ``` 
  이 예시에서는 호출시 Element를 노출하지 않기 위해 함수명에서 제외해줬지만 상황과 쓰임에 따라 달라질 수 있다.
  ```swift
  public mutating func remove(_ member: Element) -> Element?
  allViews.remove(cancelButton) // clearer
  ```
  때때로 의미가 모호해지는것을 방지하기 위해 반복되는 값이 있을 수 있지만 파라미터의 타입보다는 그 역할을 명시해주는게 더 좋다.
<br>

* **단순히 변수, 타입, 매개변수의 정의된 형태를 명시해주는 것 보다는 영할에 따라 명명해라.**
  >- "Hello"라는 인사를 저장하는 변수의 이름에 타입을 명시해주는것은 비효율적이다.<br>
  >- 프로토콜 또한 "associatedtype"을 명명할때 타입자체를 설명하는 것은 비효율적이다.<br><span style="color:pink">(associatedtype:프로토콜을 정의해줄때 프로토콜에서 정의한 변수의 타입이 특정타입이 될수도 있고 아닐수도 있을경우 associatedtype으로써 정의해준다.)</span>
  >- 파라미터를 사용할때도 파라미터명을 파라미터의 타입자체로 명명하는 것은 비효율적이다.
  ```swift
  var string = "Hello"
  protocol ViewController {
  associatedtype ViewType : View
  }
  class ProductionLine {
  func restock(from widgetFactory: WidgetFactory)
  }
  ```
  >- "greeting"과 같이 변수가 해주는 역할로써 
  >- "ContentView"라는 역할로써 명명한다.<br>
  >- "suppier"라는 역할로써 명명한다.
  ```swift
  var greeting = "Hello"
  protocol ViewController {
  associatedtype ContentView : View
  }
  class ProductionLine {
  func restock(from supplier: WidgetFactory)
  }
  ```

<br>
* **사용 시점에서 파라미터의 이름만으로 명확히 역할을 파악하기 힘들다면 파라미터의 의미를 더 쉽게 파악할 수있도록 함수명과 레이블을 적절히 명명해라.**

  > "graphics"를 위해 뭘 추가하라는 뜻인건 알겠지만 어떤것을 추가하고 graphics가 무엇을 뜻하는지 사용시점에서 의미가 모호하다
  ```swift
  func add(observer: NSObject, for keyPath: String)
  grid.add(self, for: graphics) // vague
  ```
  더욱 명확하게 명시해주기 위해 "addObserver"로 어떤것을 추가 하는지 알 수있도록하고 두번째 인자의 레이블에 "graphics"의 의미를 알기 쉽도록 해준다.
  ```swift
  func addObserver(_ observer: NSObject, forKeyPath path: String)
  grid.addObserver(self, forKeyPath: graphics) // clear
  ``` 

<br><br>
## 전문용어를 적절히 사용하기(Use Terminology Well)

* **더욱 일반적인 단어가 의미를 잘 전달할 수 있다면 애매한 용어는 피해라. “피부(skin)”가 충분히 목적을 전달할 수 있다면 “표피(epidermis)를 사용하면 더 혼란스러워 질 수 있다.**

* **특정 분야의 전문 용어를 명명법으로 사용할때는 그 분야의 전문가의 입장과 초보자의 입장을 충분히 고려해야한다. 전문가가 봤을때 의미가 잘못된 단어를 사용해서 불편하게 해서도 안되고 초보자가 봤을때 이해하기에 너무 어려워서도 안된다.**

* **약어를 되도록 피하고 축약되지 않은 상태로 사용해라.**

* **기존 유저들의 코드와 쓰임을 충분히 관찰한 후 특정 상황에 쓰이는 명명법을 따라라. 다른 명명으로도 의미를 확실히 알 수 있다하더라도 이미 많은 유저들이 따르고 있는 암묵적인 룰을 발견한다면 따르는 것이 좋다.**

<br><br>
## 매개변수

* **읽을 때 더 자연스럽고 영어 문법에 맞도록 매개변수를 명명해라.**

    >매개 변수를 잘 선언하면 아래와 같이 읽기 편하고 의미를 파악하기 쉽다.
    ```swift
    /// `predicate`를 만족하는 `self`의 요소를 포함하는 `Array`를 반환.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
    /// 주어진 요소의 `subRange`를 `newElements`로 치환.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```
    >아래와 같은 명명은 읽기에도 부자연스러우며 문법에도 맞지 않는다.
    ```swift
    /// `includedInResult`를 만족하는 `self`의 요소를 포함하는 `Array를 반환.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
    /// `r`로 표시된 요소의 range를 `with`의 컨텐츠로 치환.
    mutating func replaceRange(_ r: Range, with: [E])
    ```

    <br>
*  **어떠한 메서드나 함수에서 옵셔널과 같이 전달하지 않아도되는 매개변수가 존재할 경우 한번에 사용해라.**

    >이처럼 옵셔널을 사용하여 선언해주면 호출시 필요없는 파라미터는 nil이므로 지정해주지 않아도 괜찮다.
    ```swift
    extension String {
      /// ...description...
      public func compare(
        other: String, options: CompareOptions = [],
        range: Range? = nil, locale: Locale? = nil
      ) -> Ordering
    }
    ```
    아래와 같은 경우 쓸때 없이 여러번 선언해주며 읽기도 힘들다.
    ```swift
    extension String {
        /// ...description 1...
        public func compare(other: String) -> Ordering
        /// ...description 2...
        public func compare(other: String, options: CompareOptions) ->      Ordering
        /// ...description 3...
        public func compare(
           other: String, options: CompareOptions, range: Range) -> Ordering
        /// ...description 4...
        public func compare(
           other: String, options: StringCompareOptions,
           range: Range, locale: Locale) -> Ordering
    }
    ```

    <br>
* **default값을 갖는 파라미터는 인자값을 받아야 하는 파라미터보다 함수나 메서드를 선언해줄 때 뒤쪽에 표기해라**

    default값을 갖지않는 파라미터 즉, 함수나 메서드 호출시 인자를 명시해줘야하는 의미상으로 보통 더 중요하다. 그러면 호출될때 좀 더 안정적인 형태로 사용될 수 있다.