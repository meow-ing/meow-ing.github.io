--- 

title: "Publisher"

---

`protocol`  
순차적으로 값의 시퀀스를 전달할 수 있는 타입 선언.

---

## Declaration

``` swift
protocol Publisher
```

## Overview

publisher는 여러개의 subscriber instance에 요소를 전달한다.  
subscriber의 Input, Failure 타입은 publisher의 Output과 Failure와 일치해야한다.
publisher는 subscrebier를 허용하기 위해 receive(subscriber:) 메소드를 구현한다.  
그 후에, publisher는 아래의 subscriber의 메소드를 호출 할 수 있다. 

* receive(subscription:)
    * 구독이 성공 됐음을 알려주고, Subscription instance를 리턴.
    * subscription은 publisher에게 요소를 요청하거나, 구독을 취소할 때 사용한다. 
* receive(_:)
    * publisher는 subscriber에게 하나의 요소를 전달한다.
* receive(completion:)
    * 성공 혹은 실패고 publishing이 끝났다고 subscriber에게 알린다.

정상적으로 동작하려면 모든 publisher는 downstream subscriber를 위해 이 조건을 준수해야한다.  
<br><br>
publisher extension은 이벤트 처리 체인을 정교하게 만드는 많은 종류의 연산자를 정의한다.  
각 연산자는 Publisher protocol을 구현하는 형식으로 반환한다.  
이 타입 대부분은  Publishers enum의 extension으로 존재한다.  

예를 들어, map( _ :) 연산자는 Publishers.Map 인스턴스를 반환한다. 

## Creating Your Own Publishers

Publisher protocol로 커스텀 publisher를 만드는 것보다, Combine 프레임워크에서 제공해주는 타입을 이용해서 publisher를 사용하는걸 권장한다.  
* [PassthroughSubject](https://developer.apple.com/documentation/combine/passthroughsubject)과 같이 [Subject](https://developer.apple.com/documentation/combine/subject)를 상속받아서 사용. send( _ :) 메소드를 호출해서 요청에 따라 값을 publish.
* [CurrentValueSubject](https://developer.apple.com/documentation/combine/currentvaluesubject)는 subject의 기본 값을 업데이트 될 때마다 사용.
* property에다가 @Published 기입. property 값이 변경될 때마다 이벤트를 송출하는 publisher 획득 가능. [Published](https://developer.apple.com/documentation/combine/published) 참고.

## 지극히 주관적으로 이해하기

### 구독하기

1. 요구사항에 따른 publisher들을 생성.
* 각 publisher는 요구사항에 맞게 연산함.
* 요구사항에 맞게 publisher들을 나열함. >> 요구사항에 따라 publisher가 많을 수도 있고 적을 수도 있다.
* 나열된 순서대로 publisher가 수행될 예정. > 이것을 publihser chain이라고 함.
* upstream publihser : 바로 내 위에서 수행되는 publisher.
* downstrem publihser : 나 다음으로 수행될 publisher.
2. sink 또는 assign 메소드로 publisher를 구독할 subscriber 생성.
* sink와 assign은 publisher 메소드. subscriber를 리턴한다.
* publisher chain의 끝은 subscriber가 되야한다.

### 이벤트 처리

1. 이벤트 발생
2. publisher chain 실행.
* publisher chain에 따라 나열된 순으로 차례대로 publisher 실행.
* publishr는 upstream publisher부터 받은 값으로 계산을 하고 새로운 Ouput과 함께 publisher(downstrem publisher)를 리턴한다.
3. publisher chain의 끝에서 subscriber에게 최종 결과 값 전달.


## Topic

### Declaring Publisher Topography

* publisher는 꼭 Output과 Failure 타입을 정의해야한다.

1. associatedtype Output
2. associatedtype Failure

``` swift
//NotificationCenter.Publisher 의 예

extension NotificationCenter {

/// A publisher that emits elements when broadcasting notifications.
public struct Publisher : Publisher {

    /// The kind of values published by this publisher.
    public typealias Output = Notification

    /// The kind of errors this publisher might publish.
    ///
    /// Use `Never` if this `Publisher` does not publish errors.
    public typealias Failure = Never
    }
}
```

### Mapping Elements

