# SWIFT-Function-1

# Function 함수 정복하기!

- 일반적으로 `func`이라는 키워드로 생성하는 것은 모-두 함수 Function 이때 `doSomething()`은 함수이다

```swift
func doSomething() {
    print("UhMook")
}
```


Method 메서드 - 클래스 `class`, 구조체 `struct`, 열거형 `enum`속에 포함되어있는 함수를 Method/메서드 하고 한다

만약 `func`로 선언한 함수가 클래스 `class`, 구조체 `struct`, 열거형 `enum`속에 속해 포함되어있다면 `doSomething`을 함수라 하지않고 메서드라고 표현함


```swift
class soonMook {
    func doSomething() {
        print("UhMook")
    }
}
```

- IOS 앱 개발에는 함수를 독단적으로 쓸 일은 거의 없슴. 클래스나 구조체 내에서 사용하는게 대부분.

> 사실 `func` 키워드를 이용해 이름을 붙여주는 함수들도 모두 클로저임!! 클로저는 다음과 같이 두 가지 종류가 있음
> 

![https://blog.kakaocdn.net/dn/7cJQa/btqQhMp9gqo/jT7hCUOFvbe0mUNgivsqW1/img.png](https://blog.kakaocdn.net/dn/7cJQa/btqQhMp9gqo/jT7hCUOFvbe0mUNgivsqW1/img.png)

Named Closure, Unnamed Closure 가 있는데 다음과 같이 선언해왔던 이름이 있는 함수는

# Named Closure

```swift
func doSomething() {
    print("UhMook")
}		
```

---

Named Closure임! 근데, 이를 클로저라 부르지 않고,

그냥 함수라고 부르는 것 뿐임! (클로저란 사실은 변함 없음)

# Unnmaed Closure

다음과 같이 이름을 붙이지 않고 사용하는 함수를 익명함수, 즉 Unnamed Closure라고 부름 보통 Closure라고 하면 Unnamed Closure를 말하긴 함 :)

```swift
let closure = { print("UhMook") }
```

---

따라서, 정리하자면 클로저는 Named Clousre & Unnamed Closure 둘다 포함하지만, 보통 Unnamed Closure를 말한다!

## 요약

`func`이란 키워드로 선언되는 것은 "함수"이자, Named Closure고 이 함수가 클래스, 구조체, 열거형에 속해 있으면 메서드라고 부른다

함수를 선언할때 어떻게 선언하는지 예제.

```swift
func name(parameters) -> ReturnType {
    // action
}
```

1. 기본 함수를 선언하기 위해선 `func`이라는 키워드를 써야함
2. `func`이란 키워드를 썼으면 함수 이름을 써주어야함
3. 원하는 파라미터를 `()` 안에 적는다.  

# 파라미터를 선언하는 방식에는 크게 3 가지가 있슴

### ① Argument Label과 Parameter Name 각각 명시하기

- Argument Label : 함수를 호출할 때 사용하는 이름
- Parameter Name : 함수 내에서 사용할 파라미터의 이름

(ArgumentLabel ParameterName: Type) 이런 형식으로 사용하는데 예제 ㄱㄱ

```swift
func sayHello(to name: String) {
    print("Hello, \(name)")
}

sayHello(to: "Sundae") // Hello, Sundae
```

ArgumentLabel 은 `to` 에 해당하고, ParameterName 은 `name` 에 해당함

`sayHello(to: “UhMook”) // Hello, UhMook`

Parameter Name은 name에 해당하고, 다음과 같이 받은 파라미터를 함수 내부에서 사용할 때 사용


### 자.. 이제 Print 함수의 어원을 알아봅시다.  이렇게 Argument Label을 _ (under bar)로 표시.. Argument Label을 생략하고 싶다면
### _ 를 사용해서 생략할 수 있슴 이것을 Wildcard Pattern이라고 함

Function
```swift
print(_:separator:terminator:)
```
Writes the textual representations of the given itmes in to the standard output.

Declaration
```swift
func print(
    _ items: Any...,
    separator: String = "",
    terminator: String = "\n"
)

```
다음과 같이 Wildcard Pattern을 이용해 Argument Label을 생략한 함수는,

```swift
func sayHello(_ name: String) {
    print("Hello, \(name)")
}

sayHello("UhMook")
sayHello("Sundae")
```
실제 호출할 때 Argument Label을 생략하고 `sayHello("UhMook")` 이런식으로 호출


### ② Argument Label & Parameter Name을 한번에 명시하기

`(name: Type, name: Type)` 이렇게 하나만 선언할 경우 `name`은 Argument Label 이자, Parameter Name 이 된다

```swift
func sayHello(name: String) {
    print("Hello, \(name)")
}
```

name은 Parameter Name으로 쓰여서 함수 내부에서도 name이란 이름으로 파라미터를 사용하고, 실제 호출할 때 Argument Label 또한 name 으로 사용함

```swift
sayHello(name: "UhMook")
```

### ③ 파라미터를 받지 않을 경우

파라미터를 받지 않을 경우, 다음과 같이 `()` 파라미터가 없음을 꼭 명시해야함 

(Objective C 는 명시 안해도 가능)

```swift
fun sayHello() { }
```

### ③-① 리턴타입

Swift에서 Return Type은 -> 라는 키워드랑 함께 써야함->를 쓰고 그 뒤에 Return하고자 하는 Type을 명시해주면 됨

```swift
func sayHello(name: String) -> String {
    return name
}
```

만약 Return Type 이 없을 경우 이렇게 생략 가능

```swift
func sayHello(name: String) {}
```

### ④ SWIFT 에서의 함수호출 방법

`functionName(parameters)` 이렇게 호출하면 되는데 위에서 설명했듯이 함수 호출시 Parameter 에는 Argument Label 이 들어감

```swift
sayHello("Sundae")         // Argument Label 생략
sayHello(name: "Sundae")   // Argument Label 명시
```
