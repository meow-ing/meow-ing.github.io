---

title: "UIResponder - becomeFirstResponder()"
author_profile: false
categories:
- iOS

---

`instance property`   
UIKit에게 <b>윈도우</b>에서 first responder가 되게 요청하는 메소드.

--- 


## Declaration

``` swift
func becomeFirstResponder() -> Bool
```

## Discussion

객체가 first responder가 되기 원할 때 메소드 호출. 하지만 first responder가 되는건 보장 못 함.  
해당 메소드가 호출 되면 UIKit은 현재 first responder에게 resign 할 수 있을지 물어봄.(아마도 거절할테지만..). 
만약에 resign을 하면, UIKit은 해당 객채의 canBecomeFirstResponder 메소드를 호출.  
<br>
해당 객체가 first responder가 되면, first responder에 타겟팅 된 일련의 이벤트들이 해당 객체에 첫번쨰로 전달.  
<br>
뷰 계층이 활성화 되기 전에는 해당 메소드는 유효하지 않음.  
window property(UIView의 property)를 체크해서 뷰가 화면에 떠있는지 안떠있는지 확인할 수 있음.  
(If that property contains a valid window, it is part of an active view hierarchy. If that property is nil, the view is not part of a valid view hierarchy.)
<br>
Highlighting selection과 같은 동작 혹은 상태를 업데이트 하기 위해 custom responder에서 해당 메소드를 override 해도 됨.  
override를 했다면 어느 지점에서든 꼭 super method를 호출해야함. 
