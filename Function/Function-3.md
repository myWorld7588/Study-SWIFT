# SWIFT-Function-3

# 1. 함수 표기법

종종 Apple Documentation 을 검색하다보면 막 아래처럼 표기되어있는게 함수표기법.

`init(frame:)        insertSubview(_:aboveSubview:)`

> **변수(상수)에 함수를 대입**하기 위해선 이 **함수 표기법으로 대입을 해야 정확히 대입**할 수 있음!!!
> 

> 함수 표기법에선 오로지 **파라미터** 만 표현함! **리턴 타입은 있든 없든 함수 표기법엔 상관없음**
> 

### 1-1. 파라미터가 있는 함수 표기법

`functionName(ArgumentLabel:ArgumentLabel:)` // 

**()괄호 안에 Argument Label**을 써주고 옆에 :를 붙여줌 예로 보면,

```swift
func sayHello(name: String )   { }  // sayHello(name:)
func sayHello(_ name: String ) { }  // sayHello(_:)
func sayHello(to name: String) { }  // sayHello(to:)
```

만약 _(Wildcard Pattern)으로 Argument Label을 생략했다 하면, ArgumentLabel 자리에 _를 써줘야함

파마리터가 여러개라면 이렇게~!

```swift
func sayHello(name: String, age: Int) { }  // sayHello(name:age:)
```

### 1-2. ****파라미터가 없는 함수 표기법****

`func sayHello() { } // sayHello`

`func sayHelloToUhMook() { }   // sayHelloToUhMook`

**sayHello()**

**sayHelloToSodeul()**

이렇게 표기해야하는 거 아닌가??할 수도 있지만, **()를 붙이는 것은 파라미터가 없는 함수를 호출**하는 것이다!!!

# 2. 함수 타입

Swift 에서 함수는 자료형을 가지고있다. 함수가 웬 자료형이냐 싶겠지만, **함수 또한 자료형을 가지고 있고** 이를 **함수 타입**이라고 함!!

### **(Parameter Type) → Return Type**

함수타입은 위와 같이 표현하는데, 여기서 중요한 것은 함수를 선언할때 Return Type 이 없다면 생략하고 썼지만 

함수 타입에선 Parameter Type 이건 Return Type 이건 아무것도 생략 해선 암됨.

### 2-1. 파라미터가 없는 함수 타입

```swift
func sayHello() {} // 이 함수의 타입은 () -> () 걸 알수있슴. 파라미터도 없고, 리턴타입도 없기때문
```

### 2-2. 파라미터가 있는 함수 타입

```swift
func SayHello(_ name: String) {} // 이 함수의 타입은 (String) -> () 인 것을 알수있슴
```

### **함수 타입에선 Argument Label이건, Parameter Name이고 뭐고 다  필요 없음** 단지, 해당 **Parameter의 타입**만 중요함!!! 따라서 만약 **Parameter가 다음과 같이 여러 개**라면,

이번엔 **Return Type**도 추가해보겠슴

```swift
func sayHello(_ name: String, _ age: Int, alias: String) -> String {} // 함수의 타입은 (String,Int,String) -> String
```

# 3. 일급 객체 함수 (**First Class Citizen Function)**

Swift는 객체지향 언어인 동시에 **함수형 언어이다.** 함수형 언어란 **함수가 1급 객체로 취급** 되어 개짱인 것

**일급 객체의 조건**은  **객체가 런타임에도 생성**되어야 하고, **파라미터로 객체를 전달**할 수 있고, **반환값으로 객체를 사용**할 수 있고 등등 있는데, 그중 함수가 일급 객체가 되기 위해선 다음과 같은 조건들을 만족해야함

### 3-1. 변수나 상수에 함수를 대입할 수 있어야 한다

일급 객체 함수가 되기 위해선, **변수나 상수에 함수를 대입**할 수 있어야 하는데,

먼저, **대입**한다는 개념은 **함수를 실행해서 결과값을 대입한다는 것이 아님**!

**함수 자체**를 **변수나 상수에 대입**해서, **해당 변수나 상수가 함수 자체처럼 쓰이는 것**을 말함!

변수나 상수를 대입하는 방법이 총 3가지가 있다

### ****① 함수 이름으로 대입하기****

```swift
func sayHello(_ name: String) {}
let y = sayHello // 이런식으로 함수이름으로 대입가능
```

하지만 이렇게 함수 이름으로 대입할 경우의 문제점은 오버로딩 된 경우, 함수가 중복으로 여러번 쓰일경우 어떤 함수를 말하는것인지 알수없기에 컴파일에러가 뜸

```swift
func sayHello(_ name: String) {}
func sayHello(_ name: String, _ age: Int) {} 
left y = sayHello // Error!
```

### ****② 타입 어노테이션을 통해 함수 타입을 명시해주기****

```swift
func sayHello(_ name: String) {}
func sayHello(_ name: String, _ age: Int) {}
let y: (String) -> () = sayHello
```

위에서 배운 **함수 타입**을 이용해서 **Type Annotation**으로 명시해주는 것.  하지만 함수 타입을 명시해주는 것또한 **문제점**이 있는데,

```swift
func sayHello(name: String) {}
func sayHello(_ name: String) {}
let y: (String) -> () = sayHello //error!
```

파라미터 타입 & 리턴 타입이 동일한 함수가 존재할 경우 함수 타입 또한 동일하기 때문에 컴파일러는 에러를 뱉음

### ****③ 함수 표기법을 통해 대입하기****

상기에서 함수 표기법을 왜 배워야하는지에 대해 말할때 잠깐 설명했듯이 

```swift
func sayHello(name: String) {}
func sayHello(_ name: String) {}
let y = sayHello(name:)
```

이렇게 sayHello 의 함수 표기법을 이용해서 대입하면 문제가 해결 

( + Parameter Type, Return Type이 없는 함수가 오버로딩된 경우 함수 표기법이 아닌 Type Annotation으로 타입을 명시해주어야 함)

함수 대입하는 방법에 대해선 다 배웠음! 근데 헷갈리면 안되는 것이 우린 y **라는 상수에 함수를 대입**한 것이지,

**sayHello라는 실행 결과를 y 에 대입한 것이 아님!** 따라서, y **라는 상수에 저장된 것은 sayHello라는 함수**이기 때문에 이 y **를 통해서 우리가 저장한 함수를 실행**시킬 수 있고, 실행할 때는 다음과 같이 실행함

```swift
y("Sundae")
```

또 중요한 것!

**함수를 저장한 상수 y 를 통해 함수를 호출**할 땐 **Argument Label을 사용하지 않음**

왜냐? sayHello라는 함수 자체를 호출할 땐 당연히 Argument Label이 필요하지만,

**sayHello를 y 라는 상수에 넣는 순간 함수 타입만 가져가지**, **Argument Label까지 가져가진 않음**

만약, 함수를 저장한 상수(변수)를 호출할 때 Argument Label을 사용하면 에러뜸

### **③-②  함수의 반환 타입으로 함수를 사용할 수 있다**

```swift
func outer() -> () -> () {
    func inner() {
        print("inner!")
    }
    
    return inner
}
```

위에서 보면 **outer란 함수가 inner라는 함수를 반환**하구 있다. 참고로 **리턴할 함수 표기는** 함수 대입할 때와 똑같이 **함수 이름을 쓰든, 표기법을 쓰든, Annotation을 쓰든** 하면 됨

**() -> () -> ()**       ???? 이부분이 이해가 안 갈거..함수를 반환하려면, **"반환 타입"에 반환하려는 "함수 타입"을 작성** 해야 한다는 사실을 알아두어야 함 따라서 위 표현식의 의미는 아래와 같이 이해하면됨

() -> () -> () 

**첫 번째 괄호는** outer 함수의 **파라미터**고, **첫 번째 () -> 뒤에 오는 것은 리턴하고자 하는 함수(inner)의 타입**임

따라서, 만약 다음과 같이

```swift
let f = outer()
f() // outer 함수를 받아서 실행해보면 inner! 가 실햄됨
```

### **③-③  함수의 파라미터로 함수를 사용할 수 있다**

보통 **파라미터로 함수를 전달하는 것**을 어디서 많이 사용하냐면, **콜백함수** 에서 사용하는 것임.

예를 들어, 데이터를 fetch 하는 함수를 실행했을 경우, 너 데이터 다 가져왔으면 이 함수 실행시켜줘!

하고, 함수를 파라미터로 전달해 주는 것임

보통 **클로저**를 통해 작성함 

함수 클로저, 둘 다 봐보겠음

```swift
func doSomething(_ callback: () -> ()) {
    callback()
}
```

이렇게 함수를 파라미터로 받을 경우, **"파라미터의 타입"을 "함수 타입"으로 작성해주어야함** 

** 참고. **함수를 파라미터로 받은 경우**, 파라미터로 받은 함수를 "**실행**" 해야 하므로, **()** 를 통해서 **호출 해야함***

# **(두 가지로 파라미터에 함수를 넘겨줄 수 있슴)**

### 1. **함수(Named Closure)로 넘겨주기***

```swift
func success() {
    print("Success!")
}
 
doSomething(success)
```

### 2. **클로저(Unnamed Closure)로 넘겨주기**

```swift
doSomething {
    print("Success!")
}
```
