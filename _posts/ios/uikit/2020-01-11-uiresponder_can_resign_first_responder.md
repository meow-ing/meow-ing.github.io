---

title: "UIResponder - canResignFirstResponder"

---

`instance property`   
수신자가 first responder를 포기할지 말지 Boolean값 리턴.

--- 

## Declaration

``` swift
var canResignFirstResponder: Bool { get }
```

## Discussion

Default 값은 true.  
커스텀 responder에서 해당 메소드 override 해서 원하는 값 리턴 가능.  
<br>
예를 들어, textField가 유효하지 않은 값을 포함하고 있다면 false 값 리턴해도 됨.
