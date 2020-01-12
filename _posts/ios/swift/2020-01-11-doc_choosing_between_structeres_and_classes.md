---

title: "Choosing Between Structures and Classes(작성중)"
author_profile: false
categories:
- iOS
- ing

---

Decide how to store data and model behavior.

---

## Overview

* Struct를 기본으로 사용하라.
* Objective-c와 사용해야한다면Class 사용 권장.
* 모델링하 데이타의 identity를 제어해야한다면 Class 사용 권장. 
* 동작이 필요하면 Protocol과 함께 Struct를 사용. (Use structures along with protocols to adopt behavior by sharing implementations.)

### Choose Structures by Default

Swift의 Struct에는 다른 언어에서 Class에서만 제공되는 기능을 사용할 수 있다.
* property 저장.
* property 계산.
* method 포함. 
* 동작을 얻기 위해 protocol 채택.

standard library, Foundation에서 구조체를 사용하고 있다.  
많이 사용하고 있는 number, string, array, dictionary가 그 예이다.  
<br>
struct를 사용하면 앱의 전체 상태를 고려할 필요없이 코드의 일부를 쉽게 이해할 수 있다.  
Because structures are value types—unlike classes—local changes to a structure aren't visible to the rest of your app unless you intentionally communicate those changes as part of the flow of your app.As a result, you can look at a section of code and be more confident that changes to instances in that section will be made explicitly, rather than being made invisibly from a tangentially related function call.

### Use Classes When You Need Objective-C Interoperability


