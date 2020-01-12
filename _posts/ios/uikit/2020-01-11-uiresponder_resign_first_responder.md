---

title: "UIResponder - resignFirstResponder()"

---

`instance Method`    
<b>윈도우</b>에서 해당 객체가 first responder를 포기(양도)할거라고 알리는 메소드.

---

## Declaration

``` swift
func resignFirstResponder() -> Bool
```

## Discussion

Default 값은 true.  
커스텀 responder에서 해당 메소드 override 해서 원하는 값 리턴 가능.  
<br>
override를 했다면 어느 지점에서든 꼭 super method를 호출해야함. 
