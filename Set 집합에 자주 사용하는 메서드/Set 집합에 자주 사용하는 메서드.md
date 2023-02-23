**Set(집합)을 사용할 때 알아두면 편한 메서드들을 공부하고 정리해보자**

# Set 집합이란?

- 정렬되지 않은 컬렉션이다. (따라서 Set을 print할경우, 삽입 순서에 상관없이 뒤죽박죽 나온다)
- 배열과 달리 중복 요소를 허용하지 않는다
- 해시를 통해 값을 저장하기 때문에 배열에 비해 검색 속도가 빠르고, 당연히 저장되는 자료형은 Hashable 프로토콜을 준수하고 있어야한다.
- Swift는 형식에 민감하기 때문에 저장되는 자료형은 모두 동일한 자료형이어야한다.

**Set도 Array, Dictionary 같은 Collection Type이고 때문에 구조체로 Stack에 저장됨.**  

**다만, NSSet 이란 클래스도 지원함 (Objective-C)**

</br>


# 1. Set 생성하기

```swift
<주의> Set은 타입추론으로 생성 불가 (선언이 배열과 동일하기 때문)
var set1 = [1, 5, 3, 2, 1]           // Set이 아닌 배열로 추론됨
 
// 1. 타입 Annotation으로 생성하기
var set2: Set<Int> = []
 
// 2. 생성자로 생성하기
var set3 = Set<Int>()
```

### <주의> Set은 타입추론으로 생성 불가 (선언이 배열과 동일하기 때문)

`var set1 = [1, 5, 3, 2, 1] // Set 이 아닌 배열로 추론됨`

**Set은 배열과 동일하게 []로 생성**하기 때문에, **타입 추론으론 생성할 수 없다!! (무조건 배열로 추론됨)** 또한, 여러 타입의 값을 저장하고 싶을 경우 Any를 선언할 수 있는 배열이나 딕셔너리의 Value와 달리, 앞서 말했듯 **Set은 Hashable 프로토콜을 채택한 자료형만 저장**할 수 있기 때문에 **Any로는 생성 불가**하지만  **Error!**

```swift
var set3: Set<Any> = [] // Error!: Type 'Any' does not conform to protocoal 'Hashable'
```

타입에 민감하지 않은 Objective-C의 **NSSet이란 클래스**를 이용하면 여러 타입을 지정하는 Set을 사용할 수 있다.

```swift
// 3. 여러 타입을 저장하는 Set 생성하기
var set3: NSSet = [1, "s"]
```

</br>

# 2. Set 갯수 확인하기

```swift
var set1: Set<Int> = [1, 2, 5, 0]
 
let count: Int = set1.count            // Set 갯수 확인 : 4
let isEmpty: Bool = set1.isEmpty       // Set 비었는지 확인 : false
```

만약 **Set이 비었는지**를 확인하고 싶다면 **딕셔너리와 마찬가지로**

`set1.count == 0 이 구문보단, set1.isEmpty`를 사용하자!

