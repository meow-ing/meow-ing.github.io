--- 

title: "Receiving and Handling Events with Combine"

---

asynchronous 소스로 이벤트를 사용자화하고 수신받는다.

---

## Overview

Combine 프레임워크는 이벤트를 처리하기 위한 선언적인 접근법을 제공한다.  
delegate 콜백 또는 종료 블록을 여러개 구현하는 것보다, 주어진 이벤트에 대한 하나의 접근 체인을 생선하는게 더 좋다.   
체인의 각 부분은 Combine연산자를 가지고 있다.  
이 Combine 연산자는 이전 단계에서 전달받은 요소로 구분된 동작을 수행한다. 
<br><br>
TextField에서 입력받은 값으로 리스트를(tableView, collectionView) 필터링 해본다고 생각해보자.  
AppKit에서, 키 입력을 받을 때마다 Combine으로 구독한 Notification을 생산한다.  
Notification을 받으면, 내용과 이벤트 전달 시간을 수정하는 연산자를 사용할 것이고 앱을 업데이틑 하기 위한 최종 결과물을 사용할 것이다. 

## Connect a Publisher to a Subscriber

Combine으로 textField의 Notification을 받아보자.  

1. NotificationCenter의 default instance에 접근.
2. 인스턴스의 publisher(for:object:) 메소드 호출.
* 수신받고 싶은 notification 이름과 object 설정.
* notification 요소를 생성하는 publisher 리턴.

``` swift
let publisher = NotificationCenger.default
                    .publisher(for: NSControl.textDidChangeNotification, object: textField)
```

publisher로부터 요소들을 받기 위해 Subscriber를 사용한다.  
subscriber는 수신받을 타입을 선언하기 위해 [associated type, Input](https://developer.apple.com/documentation/combine/subscriber/3213652-input)을 정의한다. 
publisher 또한 생성할 [associated type Output](https://developer.apple.com/documentation/combine/publisher/3204681-output)을 정의한다.  
publisher와 subscriber는 생성 혹은 수신을 실패 했을 때 알려주기 위해 [Failure](https://developer.apple.com/documentation/combine/publisher/3204680-failure)도 정의한다.  
<b>subscriber가 publisher를 구독하려면 반드시 Output과 Input은 매치해야한다.  Failure또한 동일. </b>

<br><br>

Combine은 기본적으로 2개의 subscriber을 제공해준다. 이 들은 자동으로 연결된 publisher의 output과 failure와 매치된다. 

* sink(receiveCompletion:receiveValue:) 
    * 2개의 closure 사용.
    * receiveCompletion: publisher가 완료되거나 실패했을 때 실행됨.
    * receiveValue: publisher로부터 요소를 받을 때 실행됨. 
* assign(to:on:)
    * property의 key path를 사용해 주어진 object에 바로 모든 요소가 할당된다. (수신 받을 때마다 모델의 property에 변경사항이 바로 적용된다는 뜻)
    
    
``` swift
let subscriber = NotificationCenger.default
                    .publisher(for: NSControl.textDidChangeNotification, object: textField)
                    .sink(receiveCompletion:{ pring($0)},
                        receiveValue:{ pring($0) }) //notification.userInfo 수신
```

Both the sink(receiveCompletion:receiveValue:) and assign(to:on:) subscribers request an unlimited number of elements from their publishers. To control the rate at which you receive elements, create your own subscriber by implementing the Subscriber protocol.


## Change the Output Type with Operators

sink subscriger에서는 receiveValue closure에서 모든 작업을 수행한다.  
수신받은 요소로 많은 작업을 하거나, 호출간의 상태를 유지보수 하는건 부담스러운 작업이 될 수 있다. 
combine 연산자로 이벤트 전달을 사용자화하면 부담을 완화시킬 수 있다. 
<br>

textField의 string 값만 필요하지만 NotificationCenter.Publisher.Output은 많은 데이타를 콜백해준다.  
publisher의 output은 순차적인 요소이기 때문에, combine은 순차적인 수정 연산자를 제공한다.( map( _ :), flatMap(maxPublishers: _ :), reduce( _ : _  :))
<br>

``` swift
let sub = NotificationCenter.default
            .publisher(for: NSControl.textDidChangeNotification, object: filterField)
            .map( { ($0.object as! NSTextField).stringValue } )
            .sink(receiveCompletion: { print ($0) },
                receiveValue: { print ($0) })
```

* map( :) 연산자로 publisher output 타입 변경.
* textField의 string value값 수신.

``` swift
struct MyViewModel {
    var filterString: String
}

var myViewModel = MyViewModel(filterString: "filter")

let sub = NotificationCenter.default
            .publisher(for: NSControl.textDidChangeNotification, object: filterField)
            .map( { ($0.object as! NSTextField).stringValue } )
            .assign(to: \MyViewModel.filterString, on: myViewModel)
```

## Customize Publishers with Operators

연산자로 너가 원하는 동작을 수행하기 위해 publisher를 확장할 수 있다. 확장하지 않으면 직접 코드를 작성해야한다.  
이벤트 처리 체인을 향상시키는 3개의 연산자가 있다. 

1. filter( _ :)
* 특정 길이의 입력을 무시하거나 영숫자가 아닌 문자를 거부하는 등 사용.
2. debounce(for:scheduler:options:)
* 위 필터링 작업이 큰 경우 - 큰 database에서 쿼리 하는 경우 - 유저가 입력을 마칠 때까지 가디릴 수 있다. 
* publisher가 이벤트를 생성하기 전 기다려야하는 시간을 설정할 수 있다. 
* RunLoop class에서 seconds 혹은 milliseconds 시간을 편하게 정의 할 수 있다. 
3. receive(on:options:)
* UI 업데이를위해, 콜백을 메인스레드에서 실행할 수 있게 해준다.
* RunLoop에서 제공하는 Scheduler 인스턴스를 매개변수로 사용해, combine이 main run loop에서 subscriber를 호출하게 한다. 

``` swift
let sub = NotificationCenter.default
            .publisher(for: NSControl.textDidChangeNotification, object: filterField)
            .map( { ($0.object as! NSTextField).stringValue } )
            .filter( { $0.unicodeScalars.allSatisfy({CharacterSet.alphanumerics.contains($0)}) } )
            .debounce(for: .milliseconds(500), scheduler: RunLoop.main)
            .receive(on: RunLoop.main)
            .assign(to:\MyViewModel.filterString, on: myViewModel)
```

## Cancel Publishing when Desired

publisher는 완료되거나 실패할 때까지 계속 요소들을 방출한다.  
만역 publisher를 그만 구독하고 싶을 때, 구독취소를 하면된다.  
sink(receiveCompletion:receiveValue:), assign(to:on:)메소드로 생성된 subscriber는 Cancellable protocol를 따르고 있고, cancel()메소드를 부를 수 있다. 

``` swift
sub?.cancel()
```

만약 커스텀 subscriber를 만들었다면, publisher를 처음에 구독할 때 [Subscription](https://developer.apple.com/documentation/combine/subscription) 객체를 받을 것이다.
subscription 객체를 따로 저장하고, cancel()메소드를 호출하면 된다.  
Cancellable protocol을 따르는 커스텀 subscriber를 만들었다면, subscriber또한 cancel( ) method가 있을 것이다. 
저장한 subscription.cancel()메소드와 한께 subscriber.cancel() 호출해라.
