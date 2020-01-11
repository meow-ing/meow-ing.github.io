--- 

title: "Dictionary"
author_profile: false
categories:
    - iOS

---
구조체.  
key-value로 쌍을 이루는 collection

### Declaration

``` swift
@frozen struct Dictionary<Key, Value> where Key : Hashable
```

* [Hashable]({{ site.baseurl }}{% link _posts/ios/swift/2020-01-11-hashable.md %})

### Overview

Dictionary는 해시 테이블로, entry에 빠르게 접근할 수 있다.  
테이블의 각 entry는 key값으로 식별되며, <b>key값은 string 혹은 number와 같이 hashable 타입이어야 한다.</b>  
key값으로 key에 해당하는 value를 찾을 수 있다.  value는 모든 object(any object)가 될 수 있다.    
<br>다른 언어에서는 hashes 혹은 관련 array가 Dictionary와 비슷한 데이타 타입일 수도 있다.

``` swift
let dict = ["key" : "value", 
           "a" : "value a", 
           "b" : "value b"]
            
var emptyDict: [Int: String] = [:]
```

### Getting and Setting Dictionary Values

``` swift
print(dict["key"])
//Prints "value"

dict["c"] = "value c" //set data
dict["a"] = nil //set data nil where key is a
```

## Creating a Dictionary

1. init(grouping: S, by: (S.Element) -> Key)

``` swift
init<S>(grouping values: S, by keyForValue: (S.Element) throws -> Key) rethrows where Value == [S.Element], S : Sequence
```

* values : 그룹핑할 데이타. 
* keyForValue : closure. values의 각 element의 key값을 리턴. 

``` swift
let students = ["Kofi", "Abena", "Efua", "Kweku", "Akosua"]
let studentsByLetter = Dictionary(grouping: students, by: { $0.first! })
// ["E": ["Efua"], "K": ["Kofi", "Kweku"], "A": ["Abena", "Akosua"]]
```
