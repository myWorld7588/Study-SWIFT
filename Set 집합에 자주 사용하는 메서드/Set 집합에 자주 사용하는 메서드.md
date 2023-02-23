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

</br>

# 3. Set 요소 확인하기

```swift
var set1: Set<Int> = [1, 2, 5, 0]
 
set1.contains(1)                    // true
set1.contains(10)                   // false
```

Array 배열과 동일하게 쓰면됨.  근데 배열과 다른 점은 

- 배열의 contains를 쓸경우 순회 탐색을 한다. (0번 index부터 마지막 index까지 요소가 있나 순차적으로 탐색)
- Set은 딕셔너리와 같이 Hashable 로 구현되어 있다
- Hash 충돌이 일어나지 않는함, 시간 복잡도가 상수로 O(1) 만큼만 걸린다.  (배열 Array에 비해 매우 빠르다)

</br>

# 4. Set에 값 추가하기

```swift
var set1: Set<Int> = [1, 2, 5, 0]
 
// 1. insert : 값을 추가하고, 추가된 결과를 튜플로 리턴 (중복이면 false, 추가된 값)
set1.insert(1)                // (false, 1)
set1.insert(10)               // (true, 10)
 
// 2. update : 값이 존재하지 않으면 추가 후 nil 리턴, 존재할 경우 덮어쓰기 후 덮어쓰기 전 값 리턴
set1.update(with: 1)          // Optioanl(1)
set1.update(with: 120)        // nil
```

보통 insert와 update를 합쳐서 upsert 작업이라고 함.

위 두 함수는 모두 Set에 upsert 작업을 하지만, **결과 값의 차이**가 있다

</br>

# 5. Set 요소 삭제하기

```swift
var set1: Set<Int> = [1, 2, 5, 0]
 
// 1. remove() : 한 가지 요소 삭제할 때 사용, 삭제 후 삭제한 값 return (없는 요소 삭제 시 nil 리턴)
set1.remove(1)              // Optional(1)
set1.remove(10)             // nil
 
// 2. removeAll() : 전체 요소 삭제
set1.removeAll()
```

</br>

# 6.  Set 비교하기

### 6-1. ==(!=) 연산자로 비교하기

```swift
var set1: Set<Int> = [1, 2, 5, 0]
var set2: Set<Int> = [0, 2, 1, 5]
var set3: Set<Int> = [1, 3, 11, 20]
 
set1 == set2            // true
set1 == set3           // false
```

set은 **정렬되지 않은 Collection** 이기 때문에,순서에 상관 없이 **모든 요소가 같으면 비교 연산자가 true**가 된다.




