---

title: "UIResponder - canBecomeFirstResponder"
author_profile: false
categories:
- iOS

---

instance property.  
해당 객체가 first responder가 될수 있을지 안될지 Boolean값 리턴.

### Declaration

``` swift
var canBecomeFirstResponder: Bool { get }
```

### Discussion

default값으로 false를 리턴.  
Subclass는 해당 메소드를 꼭 override 해야햠.  

뷰 계층 구조가 활성화 되지 않았으면 해당 메소드를 부르지 않는다.  
결과값이 아직 정의 되지 않았기 때문.