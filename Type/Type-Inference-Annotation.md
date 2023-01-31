# SWIFT-Type-Inference

```swift
// ex)
let name // Error! Type annotation missing in pattern
var age //  Error! Type annotation missing in pattern
```

지금까지 C나 Objective-C나 뭐 기타 좀 오래된 언어들의 선언 방식은 다음과 같이



```objectivec
const NSString *name;
NSInteger age;
```


자료형 `NSString`, `NSInteger` 를 선언함으로써 컴파일러는 name 과 age 에 가각의 자료형에 맞는 메모리를 할당했을것


Swift 도 마찬가지로 컴파일러가 알수있게 `var변수, let상수` 에 선언할 Type 을 명시해줘야함. 


### **명시하는 방법엔 2가지가 있슴. 

## 타입 추론(Type Inference) vs 타입 어노테이션(Type Annotation)**

# 1. 타입추론 Type Inference

- 변수나 상수를 선언할 때 타입을 명시해 주지 않고 그냥 값만 넣어 초기화가 가능 `let name = “UhMook” // 선언과 동시에 초기화`
- **타입추론 기능 - 말그대로 추론 `“UhMook”` 이란 타입을 보고 `String` 으로 추론한값을 타입으로 지정해버림**
- 타입추론은 변수나 상수를 초기화할 때 입력된 값을 분석하여 변수에 적절한 타입을 컴파일러가 스스로 추론하는 기능
- 초기값을 “Hello”가 입력되었다면 이 변수는 String 타입이라고 추론
- 초기값을 1987가 입력되었다면 정수로 초기화 되었으니 Int타입이라고 추론
- 따라서 `type(of: name)` 으로 확인해보면 `String.Type` 이란건 알수있다

### 실제로 자주 쓰는 타입 추론에도 문제 점이 몇 가지 있다

**ⓛ 원하는 타입으로 추론되지 않는 경우**

만약 17.0 이라는 값을 `num` 이라는 상수에 넣고싶음. 그리고 이때 자료형은 Float 으로 지정하고싶음.

```swift
let num = 17.0
type(of: num) // Double.Type
```

이렇게 Double 형으로 잡혀버림.  같은예로 Character & String 이 있슴. Swift 에선 Character 나 String 값이나 모두 “” 를 이용해서 대입함.

만약 name 이라는 값에 “A” 라는 Character 값을 넣고 싶지만

```swift
let name = "A"
type(of: num) // String.Type
```

이렇게 String 형으로 잡혀버림. 컴파일러가 초기값을 확인하고 자료형을 유추할때 Float 이나 Double 이나 같이 애매해 버릴 경우 자료형을 더 큰 범위의 자료형으로 지정해버림

**② 초기값이 없는 경우**

타입 추론은 무조건 선언과 동시에 초기화를 진행하여 컴파일러가 초기값을 보고 자료형을 유추하는 것
만약 초기값이 없는 경우 타입을 유추할 수 없기 때문에 문제가 됨 만약 타입 추론을 쓰다가 이처럼 문제점이 발생했을 때는 Type Annotation 을 사용해야 함

# 2. 타입 어노테이션 Type Annotation

```swift
let name: String
var age: Int
```

이렇게 직접 자료형을 지정해주는 것. 이때 자료형을 지정할땐 `let 변수명: 자료형` 이런식으로 : 을 쓰고 뒤에 자료형을 명시함

당연히 타입을 알려줬으니 초기값이 없어도됨
