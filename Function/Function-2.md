# SWIFT-Function-2

# 파라미터로 전달되는 Value 타입의 값은 복사된 상수 값이다
1. Value 타입의 값은 복사돼서 전달된다
2. 이때 복사된 값은 상수 `let` 이다

먼저, Value 타입은 Int, String, Struct, Enum, Tuple 같은 자료형들을 말하는 것. 다음과 같이 name이라는 파라미터를 받는 함수를 만듬

```swift
func sayHello(name: String) { }
var name: String = "Sundae"
sayHello(name: name)
```

“Sundae” 라는 값을 가진 name 이란 변수의 값을 함수의 파라미터로 넘김
sayHello 라는 함수가 시행될때 name 이란 파라미터의 값인 “Sundae” 를 복사해서 파라미터로 가짐
따라서 sayHello 함수가 실행되는 도중에 호출한 곳에서 name이란 변수를 바꿔도, 함수 내부엔 전혀 영향을 주지 않음


```swift
func sayHello(name: String) {
    DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
        print("Check #2 \(name)"")
    }
}
 
var name: String = "Sundae"
sayHello(name: name)
name = "UhMook"
print("Check #1 \(name)")

// Check #1 UhMook

// Check #2 Sundae
```

함수 실행 이후 name의 값이 변경되든 말든 상관 없는 것, 또한 파라미터로 전달될 때 복사된 값은 상수 라고 했기 때문에 함수 내부에서 파라미터로 받은 값을 변경해서 사용할 수 없음!!!
```swift
func sayHello(name: String) {
    name = "UhMook" // Error! : can not aasign to value: 'name' is a 'let' constant
}
```

# 2. 파라미터로 전달되는 Reference 타입의 값은 참조된다
먼저, Reference 타입은 class, function, closure 같은것들을 말하는 것.  

말 그대로, 파라미터로 참조타입을 전달할 경우에는

이땐 복사된 상수 값이 아닌, 전달되는 값의 주소값을 들고 옴!!

만약 다음과 같이 함수 내부에서 전달된 인스턴스의 프로퍼티를 변경할 경우

```swift
class Cat {
    var name: String
    var age: Int
    init(name: String, age: Int) {
	self.name = name
	self.age = age
    }
}

func changeName(cat: Cat) {
    cat.name = "UhMook"
}
 
let Sundae = Cat.init(name: "Sundae", age: 1)
changeName(cat: Sundae)
print(cat.Sundae)
```

실제 호출된 곳 인스턴스의 cat 의 프로퍼티 값이 UhMook  으로 바뀜! 왜냐? instance 는 Reference 타입이고, 그렇기 때문에 참조에 의한 전달이 되었기 때문임 (복사해서 전달 X)


# 3. In-Out Parameters : Value 타입의 값을 참조로 전달하는방법
입출력 파라미터 라고 함.. Value 타입의 값을 Reference 타입의 값처럼 "참조"로 전달하고 싶을 때 사용하는 파라미터.

선언은 다음과 같이 함

```swift
(name: inout Type)
```

① 함수 파라미터 선언할 때 inout 키워드 작성하기 (입출력 파라미터로 쓰고 싶다면, 파라미터 type 쓰기 전에 inout 키워드를 쓰면 됨)

```swift
func sayHello(name: inout String) { }
```

② 함수를 호출할 때 Argument 앞에 & 붙여주기

입출력 파라미터의 경우, 참조로 전달하기 때문에 함수 호출할 때 Argument 앞에 &를 붙여주어야 함

```swift
sayHello(name: &name)
func sayHello(name: inout String) {
    name = "Sundae"
}
 
var name: String = "UhMook"
sayHello(name: &name)
print(name)

// Sundae
```

name의 값이 sayHello에 의해서 변경된 값으로 나온다 :) + 참고로 inout 파라미터는 다음에 공부할 파라미터 기본값 설정, 가변 파라미터 등을 지원하지 않음!

# 4. 파라미터에 기본 값 설정하기

파라미터는 기본값을 가질수있다. (name: Type = 기본값) 파라미터 옆에 = (등호) 를 넣어서 기본값을 지정함

파라미터에 기본 값을 설정한 경우, 호출 시 파라미터를 생략해도 된다

```swift
func sayHello(name: String = "Sundae") {
    print("Hello, \(name)")
}

sayHello()                  //"Hello, Sundae"
sayHello(name: "UhMook")    //"Hello, UhMook"
```

> 
파라미터를 지정하지 않으면 기본 값 으로 들어가고
파라미터를 지정해주면 지정한 값 으로 들어감
여기서 중요한것은 기본 값이 있는 파라미터는 생략이 가능하다
> 

# 5. 가변 파라미터

하나의 파라미터가 여러 개의 아규먼트를 받을 때 사용한다 가변 파라미터의 타입은 배열이다 라는게 “정의”

(name: Type…) 이렇게 Type 뒤에 … 을 붙인다.  아래와 같이 가변 파라미터를 가지는 함수를 만들었음

```swift
func printSum(of nums: Int...) { } // 이럴경우 nums 라는 가변 파라미터는 nums: [Int] 배열이 됨
```

```swift
// printSum 호출시
printSum(of: 1, 2, 3, 4) // ,(comma)를 이용해서 여러 개의 아규먼트를 다음과 같이 나열하는거
```

>
가변 파라미터는 ,(comma) 를 이용해 여러 Argument 를 받기 때문에
가변 파라미터 바로 뒤에 있는 Parameter 는 무조건 Argument Label 을 가져야 함
주의할 점임!!!!
>

```swift
func printSum(of numbs: Int..., _ initValue: Int) {} // Error!: A parameter following a variadic parameter 

```

만약 Argument Label을 Wildcard Pattern으로 생략해 버리면, 컴파일러 입장에서 어디까지 ,(comma)가 가변 파라미터의 값인지 알 수 없기 때문에 다음에 오는 파라미터는 꼭 Argument Label을 가져야 함 (아래처럼)

```swift
func printSum(of nums: Int..., initValue: Int) { }
printSum(of: 1, 2, 3, 4, initValue: 100)
```

또.. 주의할 점이라면 가변 파라미터는 기본값을 가질수 없고 하나의 파라미터에서 밖에 사용을 못함.  두개 가변 파라미터를 가지면 에러남


```swift
func printSum(of nums: Int..., nums2: Int...) {  // Error! : Only a single variadic parameter '...' is permitted
```


# 6. Nested Function (중첩함수)

Swift 에선 함수 안에 함수를 선언할 수 있다.  다른 함수 안에 포함되어 있는 함수를 Nested Function 중첩함수라 한다

```swift
func outer() {
    print("outer")
    
    func inner() {
        print("inner")
    }
}
```

outer 함수 내에서 inner 함수를 실행 할 순 있지만

outer 함수 외부에선 inner 함수를 실행할 수 없음!!!
