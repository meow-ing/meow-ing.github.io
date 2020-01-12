---

title: "UIResponder"
author_profile: false
categories:
- iOS

---

`Class`<br>
이벤트에 응답하고 처리하기 위한 추상 인터페이스.

---

## Declaration

``` swift
class UIResponder : NSObject
```

## Overview

UIResponder의 인스턴스인 responder 객체들은 UIKit 앱의 이벤트 처리 백본을 구성.  
UIApplication 객체, UIViewController 객체, 모든 UIView 객체(UIWindow 포함)등 중요 객체들 또한 reponder이다.  

<br>
이벤트가 발생하면, 이벤트를 처리하기 위해 UIKit이 앱의 responder에게 이벤트들을 전달한다.  
<br>
터치 이벤트, 모션 이벤트, remote-control 이벤트, press 이벤트등이 있다.  
특정한 이벤트를 다루기 위해서는, responder는 적절한 메소드를 override 해야한다.  
<br>
터치 이벤트를 처리하기 위한 메소드
* touchesBegan(_:width:)
* toutouchesBegan(_:with:)
* touchesMoved(_:with:)
* touchesEnded(_:with:)
* touchesCancelled(_:with:)

터치 이벤트가 발생하면, responder는 터치의 변화를 추적하거나 앱의 인터페이스를 적절하게 업데이트 하기 위해 UIKit이 제공해주는 이벤트 정보를 사용한다.
<br><br>
이벤트 처리 외에 UIKit responder는 처리 되지 않은 이벤트를 다른 부분에게 전달한다.  
지정된 responder가 이벤트를 처리 하지 않으면 `responder 체인`의 다음 responder에게 이벤트를 전달한다.    
UIKit은 사전에 정의된 규칙으로(어떤 객체가 다음 responder가 되어 이벤트를 받을지) 동적으로 responder 체인을 관리한다.  
`UIView -> super UIView -> UIViewController -> UIWindow -> UIApplication -> UIApplicationDelegate` 

<br>
Responder는 UIEvent 처리뿐만 아니라 input view를 통해 커스텀 입력도 허용하고 있다.  
시스템의 키보드가 input view의 한 예.  
유저가 UITextField와 UITextView를 탭했을 때, 해당 viwe는 first responder가 되고 input view를 노출한다.  
커스텀 input view를 생성해서 다른 responder가 활성화 화면 그 커스텀 input view는 노출할 수 있다.  
responder와 커스텀 input view를 연결하려면, responder의 inputView property에 할당해야한다. 

<br>
[참고 링크](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events)

## Topic
### Managing the Responder Chain

1. [var next: UIResponder?]({{ site.baseurl }}{% link _posts/ios/uikit/2020-01-11-uiresponder_next.md %})
2. [var isFirstResponder: Bool]({{ site.baseurl }}{% link _posts/ios/uikit/2020-01-11-uiresponder_ is_first_responder.md %})
3. [var canBecomeFirstResponder: Bool]({{ site.baseurl }}{% link _posts/ios/uikit/2020-01-11-uiresponder_ can_become_first_responder.md %})
4. [func becomeFirstResponder() -> Bool]({{ site.baseurl }}{% link _posts/ios/uikit/2020-01-11-uiresponder_ become_first_responder.md %})
5. [var canResignFirstResponder: Bool]({{ site.baseurl }}{% link _posts/ios/uikit/2020-01-11-uiresponder_can_resign_first_responder.md %})
6. [func resignFirstResponder() -> Bool]({{ site.baseurl }}{% link _posts/ios/uikit/2020-01-11-uiresponder_resign_first_responder.md %})

#### 견해

이해한게 맞다면 아래와 같이 동작할 것 같다.(Resign 과정도 동일)

1. 특정 객체가 first responder가 되고 싶음.
2. 객체의 윈도우에 first responder가 되고 싶다고 요청. : becomeFirstResponder()
3. 객체의 상태를 확인하고 first responder로 지정할지 말지 결정. : canBecomeFirstResponder property
4. 만약 객체가 custom responder이고 becomeFirstResponder() 메소드를 override 했다면, responder chain 관리를 위해 어디서든 <b>꼭 super canBecomFirstResponder()를 호출</b>해야함.

