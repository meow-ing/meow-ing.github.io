---

title: "UIResponder - next"

---

`instance property`    
resopnder 체인의 next responder를 리턴. next responder가 없으면 nil를 리턴.

--- 

## Declaration

``` swift
var next: UIResponder? { get }
```

## Discussion

UIResponder 클래스는 next responder를 자동으로 저장하고나 set하지 않는다.  
그러기 때문에 default값은 nil이다.  
Subclass는 적절한 next responder를 리턴하기 위해 이 메소드를 꼭 override 해야한다.  
<br>
* UIView
	1. 해당 뷰를 관리하는 UIViewController 리턴.
	2. (UIViewController가 없다면) super view 리턴.
* UIViewController
	1. UIViewController view의 super view 리턴.
* UIWindow
	1. application 객체 리턴.
* share UIApplication
	1. 일반적으로 nil 리턴.
	2. application이 UIResponder의 subclass이면 app delegate return.

