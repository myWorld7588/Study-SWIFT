딕셔너리를 사용할때 알아두면 편리한 메서드를 정리하고 알아보자.



# 딕셔너리란?

- Key : Value 가 함께 저장되는 자료구조이다. (예를들어 영한 사전이라고 하면 영어 단어(Key)를 입력하면 한글로 풀이된 뜻(Value) 가 나오는 것이다)
- 절령되지 않은 컬렉션이다 ( 따라서 딕셔너리를 print 할 경우, 삽입 순서에 상관 없이 뒤죽박죽 나온다)
- 값은 중복 가능하지만, 키는 중복되면 안된다
- Swift는 형식에 민감하기 때문에, 모든 Key의 자료형은 같아야하며, 마찬가지로 모든 Value 의 자료형도 같아야한다.

**딕셔너리도 배결과 같은 Collection Type**이고, 때문에 **구조체로 Stack에 저장**된다.  다만 **NSDictionary란 클래스도 지원**함 (Objective-C)

</br>

# 1. 딕셔너리 생성하기

```swift
// 1. 타입 추론으로 생성하기
var dict1 = ["height": 165, "age" : 100]
var dict2 = [:]                                         // error! 타입 추론으론 빈 딕셔너리 생성 불가
 
 
// 2. 타입 Annotation으로 생성하기
var dict3: [String: Int] = [:]                          // 빈 딕셔너리 생성
var dict4: [String: Int] = ["height": 165, "age" : 100]
 
 
// 3. 생성자로 생성하기
var dict5 = Dictionary<String, Int>()                   // :이 아닌 ,로 명시
var dict6 = [String: Int]()
```

보통 간편한 1번이나 2번의 방법을 많이 사용함, Type Annotation 은 중요하니까 2번을 많이 쓰도록 노력해보자.

Swift는 뭐라구?? Swift는 Type 에 정말, 굉장히, 어마무시하게 민감한 언어이다 !!!

**선언과 동시에 Type을 꼭 명시**하거나 **추론할 수 있게** 해주어야 하고, **해당 Type 으로 된 값만 딕셔너리에 저장**할 수 있다!!

따라서 만약, 

- **딕셔너리 한개에 여러 타입의 Value 를 저장**하고 싶다??
- 혹은 나는 **Runtime 시점에서 타입을 알 수 있는데**??

라고 할수 있잖슴?  그러면 **Key** 값의 **Type**을 **Any**로 선언하거나, Objective-C의 클래스인 **NSDictionary** 를 사용하면 된다.


```swift
// 4. 여러 타입을 저장하는 딕셔너리 생성하기
var dict7: [String: Any] = ["name": "Sodeul", "age": 100]
var dict8: NSDictionary = ["name": "Sodeul", "age": 100]
```

Value 같은 경우, 배열 때와 마찬가지로 Any라는 자료형을 쓰면 된다.  근데 Key 값도 여러 타입으로 받고 싶은데?? 해서 Any 로 선언할 경우. “Type ‘Any’ does not conform to protocol ‘Hashable’" 에러를 볼수있다.

즉, **Any**가 **Hashable** 프로토콜을 준수하지 않는다는 뜻이다.

- **딕셔너리는 Key 값을 해시한 것을 토대로 데이터를 저장**하기 때문에 딕셔너리의 Key 값은 아무나 올 수 없다.
- **무조건 Hashable 이란 프로토콜을 준수하는 자료형만 Key 값**을 가져올수 있다.
- 따라서, Any 도 올수 없으며, 만약 커스텀 자료형을 만든다면, Hashable 프로토콜을 준수해야만 딕셔너리의 Key 값으로 사용할수있다.

Array(배열) 에서도 말했지만, Any 를 남발하는 것은 정적 바인딩 언어인 Swift를 동적 바인딩 시키는 것이기에, 타입에 민감한 Swift 를 소중히 지켜주자

</br>

# 2. 딕셔너리 갯수 확인하기

```swift
var dict1 = ["height": 165, "age" : 100]
 
let count: Int = dict1.count            // 딕셔너리 갯수 확인 : 3
let isEmpty: Bool = dict1.isEmpty       // 딕셔너리 비었는지 확인 : false
```

만약 딕셔너리가 비었는지를 확인하고 싶다면

```swift
dict1.count == 0 // 이 구문보다는 
dict1.isEmpty // isEmpty 를 사용하자.
```

</br>

# 3. 딕셔너리 요소에 접근하기

```swift
var dict1 = ["height": 165, "age" : 100]
 
// 1. 반환 값 - Optional Type
let height = dict1["height"]                     // Optional(165)
let weight = dict1["weight"]                     // nil
 
// 2. 반환 값 - Non Optional Type
let height = dict1["height", default: 150]       // 165
let weight = dict1["weight", default: 200]       // 200
```

딕셔너리의 경우, **Subscript로 요소에 접근**하면 **기본반환값이 Optional Type** 이기 때문에 (해당 Key 값이 없을때를 대비)  따라서, 만약 Optional Type이 싫다고 한다면, Default 값을 직접 명시하면됨.

그러면 반환 값이 Non-Optional Type

</br>

# 4. 딕셔너리에 요소 추가하기

```swift
var dict1 = ["height": 165, "age" : 100]
 
// 1. Subscript로 추가하기
dict1["weight"] = 100       // 해당 Key가 없다면, 추가 (insert)
dict1["height"] = 200       // 해당 Key가 있다면, Value 덮어쓰기 (update)
 
// 2. updateValue(:forKey)
dict1.updateValue(100, forKey: "weight")   // 해당 Key가 없다면, 추가하고 nil 리턴 (insert)
dict1.updateValue(200, forKey: "height")   // 해당 Key가 있다면, Value 덮어쓰고 덮어쓰기 전 값 리턴 (update)
```

Subscript의 경우, Insert 인지 Update  인지 알 수 없지만

`updateValue`의경우, 리턴 값을 통해 Insert 인지, Update 인지를 알 수 있다. 

**(참고로 Insert 와 Update를 상황에 따라 해주는 것을 Upsert라고 함)**

**upsert is a database operation that will update an existing row if a specified value already exists in a table, and insert a new row if the specified value doesn't already exist**

**upsert는 지정된 값이 테이블에 이미 존재하는 경우 기존 행을 업데이트하고 지정된 값이 아직 존재하지 않으면 새 행을 삽입하는 데이터베이스 작업이다.**

Subscript의 경우, insert인지 update인지 알 수 없지만,

updateValue의 경우, 리턴 값을 통해 insert인지 update인지를 알 수 이씀!

(참고로 Insert와 upsert를 상황에 따라 해주는 것을 upsert라고 함)

</br>

# 5. 딕셔너리에 요소 삭제하기

```swift
var dict1 = ["height": 165, "age" : 100]
 
// 1. Subscript로 삭제하기 (nil 대입하기)
dict1["weight"] = nil      // 해당 Key가 없어도 에러 안남
dict1["height"] = nil      // 해당 Key가 있다면, 해당 Key-Value 삭제
 
// 2. removeValue(forKey:)
dict1.removeValue(forKey: "weight")  // 해당 Key가 없다면, nil 반환
dict1.removeValue(forKey: "age")     // 해당 Key가 있다면, 해당 Key-Value 삭제 후 삭제된 Value 반환 : Optional(100)
 
// 3. removeAll() : 전체 삭제하기
dict1.removeAll()
```

</br>

# 6. Key, Value 나열하기

```swift
var dict1 = ["height": 165, "age" : 100]
 
// 1. Key 모두 나열하기
dict1.keys                         // "height, "age"
dict1.keys.sorted()                // "age", "height
 
// 2. Value 모두 나열하기
dict1.values                       // 165, 100
dict1.values.sorted()              // 100, 165
```

정렬도 가능하다 (Key, Values 는 찍을때마다 다르게 나올테니 당황하지마시길

(딕셔너리는 정렬되지않는 Collection Type)

</br>

# 7. 딕셔너리 비교하기

```swift
var dict1 = ["height": 165, "age" : 100]
var dict2 = ["height": 165, "age" : 100]
var dict3 = ["Height": 165, "Age" : 100]
var dict4 = ["name": "sodeul", "address" : "Suwon"]
 
dict1 == dict2              // true
dict1 == dict3              // false (대소문자 다름)
dict1 == dict4              // false (모든 Key-Vlaue 다름)
```

비교 연산자로 비교할 수 있으나, 모든 Key & Value가 정확히 모두 일치해야만 true 가 됨.

대소문자도 비교하기 때문에, dict1 과 dict3는 다른 딕셔너리이다.

</br>

# 8. 딕셔너리 요소 검색하기

```swift
var dict1 = ["height": 165, "age" : 100]
 
let condition: ((String, Int)) -> Bool = {
    $0.0.contains("h")
}
 
// 1. contains(where:)  : 해당 클로저를 만족하는 요소가 하나라도 있을 경우 true
dict1.contains(where: condition)  // true
 
// 2. first(where:)     : 해당 클로저를 만족하는 첫 번쨰 요소 튜플로 리턴 (딕셔너리는 순서가 없기 때문에, 호출할 때마다 값이 바뀔 수 있음)
dict1.first(where: condition)  // Optional((key: "height", value: 165))
 
// 3. filter            : 해당 클로저를 만족하는 요소만 모아서 새 딕셔너리로 리턴
dict1.filter(condition)  // ["height": 165]
```

### *** 중요 ***

### 딕셔너리 요소를 검색할때는 클로저를 이용해서 검색하게 되는데,  이때 클로저의 파라미터 타입은 내가 지정한 딕셔너리의 타입을 갖는 튜플 이어야 함!  반환 타입은 무족건 Bool 이어야함..

**만약 [String:String] 타입의 딕셔너리라면**, **((String,String)) → Bool** 

**클로저를 이렇게 작성해야함**

















