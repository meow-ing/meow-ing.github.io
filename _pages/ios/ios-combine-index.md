--- 

title: "Combine"
layout: single
author_profile: false

---

이벤트 처리 연산자를 결합해서 비동기 이벤트 처리를 커스텀마이징(사용자화).

--- 

## Overview

Combine framework는 시간의 흐름에 따라 처리할 수 있는 선언적(?) Swift API를 제공한다.  
이 값들은 많은 종류의 비동기 이벤트들을 표현할 수 있다.  
Combine은 계속 바뀌는 값을 노출하기 위해 _publisher_ 를 선언한다.   
그리고 publisher가 노출하는 값들을 수신하기 위해  _subscriber_ 를 선언한다. 

* Publisher protocol은 계속 값의 시퀀스를 전달할 수 있는 타입을 선언한다.  
publisher는 upstream publisher들로부터 받은 값에 따라 행동하고 upstream publisher들한테 다시 재전송하는 연산자를 가지고 있다. 
* publisher chain의 끝에서, Subscriber는 요소를 받는대로 행동한다.  
publisher는 subscriber로부터 명시적으로 요청을 받았을 때만 값을 송출한다.   
이렇게 하면 subscriber 코드로 연결된 publisher로부터 받는 이벤트 속도를 제어할 수 있다.  
<br>
몇 Foundation type들은 publisher를 통해 모든 가능성(functionality)를 노출한다.(Timer, NotificationCenter, URLSession)  
Combine은 또한 Key-Value Observing을 준수하는 property에 대해 장착된(built-in) publisher를 제공한다. 
<br><br>
여러개의 publisher의 결과 값들을 처리하고, publisher들의 동작을 제어할 수 있다.   
예를 들어, textField publisher의 업데이트를 구독하고 textField의 text를 URL request에 사용할 수 있다. 그리고 응답값을 받고 앱을 업데이트하기 위해 다른 publisher를 사용할 수 있다.  
<br><br>
Combine을 이용해서, 이벤트 처리 코드를 중앙 집중화하고 중첩 클로저 및 convension-based 콜백들을 제거해서 일기 쉽고 유지보수하기 쉬운 코드를 작성할 수 있을 것이다. 

## Topic
### Essentials

1. [Receiving and Handling Events with Combine]({{ site.baseurl }}{% link _posts/ios/combine/2020-01-12-doc_combine_receiving_and_handling_events_with_combine.md %})

### Publishers

