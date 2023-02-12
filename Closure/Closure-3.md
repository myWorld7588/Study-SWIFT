메모리 / ARC

# 1. 클로저에서 값을 캡쳐한다는 것이란.

클로저를 사용하다보면, 값 캡쳐라는 단어를 참 많이 볼 수 있다. 근데 이 캡쳐가 도대체 뭔데??? 이를 알기 위해서 클로저의 기본 개념을 보면

***Closure란 내부 함수와 내부 함수에 영향을 미치는 주변 환경을 모두 포함한 객체이다.***

다음과 같은 코드가 있을 때
```swift
func doSomething() {
    var message = "Hi I am Sundae"
    // 클로저 범위 시작
    
    var num = 10
    let closure = { print(num) }
    // 클로저 범위 끝
    
    print(message)
}
```

closure란 익명함수는, 클로저 내부에서 **외부 변수인 num이라는 변수를 사용(print)** 하기 때문에 **num의 값을 클로저 내부적으로 저장**하고 있는데,
이것을 클로저에 의해 **num의 값이 캡쳐 되었다** 라고 표현함 **message**란 변수는 클로저 내부에서 **사용하지 않기 때문에 클로저에 의해 값이 캡쳐되지 않는다!**

 
이번엔 클로저의 값 캡쳐 방식에 대해 알아야 함!!!!

# 1-1. 클로저의 값 캡쳐 

Closure는 값을 캡쳐할 때 Value/Reference 타입에 관계 없이 Reference Capture 한다

음 이게 무슨 말이냐면, 아까 num이란 변수를 **클로저 내부적으로 저장**한다고 했잖음?

근데 **num은 Int 타입의 구조체 형식**이고, 이는 곧 **Value 타입**이기 때문에, **값을 복사해서** 들고 **저장**해야 되는 것이 일반적임!!!! 그러나, 클로저는 **Value/Reference 타입에 관계없이** 캡쳐하는 값들을 **참조**함!! 이것을 **Reference Capture**라고 함

예제로 보면. 다음과 같이

```swift
func doSomething() {
    var num: Int = 0
    print("num check #1 = \(num)")
    
    let closure = {
        print("num check #3 = \(num)")
    }
    
    num = 20
    print("num check #2 = \(num)")
    closure()
}
```

먼저, **closure는 num이라는 외부 변수를 클로저 내부에서 사용**하기 때문에 **num을 캡쳐**할 것이다!

근데 이때, 어떻게 캡쳐한다? **Reference Capture** 즉, **num이란 변수를 참조**함!

따라서, closure를 실행하기 전에 num이란 값을 외부에서 변경하면,

```swift
// num check #1 = 0
// num check #2 = 20
// num check #3 = 20
```

클로저 내부에서 사용하는 num의 값 또한 변경 된다,  

혹은, 클로저 내부에서 num의 값을 다음과 같이 바꾸면 클로저 외부에 있는 num의 값도 변경이 된다

```swift
func doSomething() {
    var num: Int = 0
    print("num check #1 = \(num)")
    
    let closure = {
        num = 20
        print("num check #3 = \(num)")
    }
    
    closure()
    print("num check #2 = \(num)")
}

// num check #1 = 0
// num check #3 = 20
// num check #2 = 20
```

이렇게 변경된다.. 이렇듯, Closure는 값의 타입이 Value건 Reference건 모두 **Reference Capture**를 한다는 사실… 그럼 만약 **Value Type으로 Capture**를 하고 싶으면 어떻게 할까??

</br>
</br>

# 2. 클로저의 캡쳐 리스트 (Capture Lists)

```swift
let closure = { [num, num2] in
```

**클로저의 시작인 { 의 바로 옆에  []를 이용해 캡쳐할 멤버를 나열한다 이때 in 키워드도 꼭 함께 작성**


# 2-1. Value Type 의 값을 복사해서 Capture 할순 없나?

가능하다, 지금 배우고있는 Capture Lists 라는것을 이용하면 가능하다.
Value Type 의 경우, Value Capture 하고 싶은 변수를 리스트로 명시해주는것!

```swift
func doSomething() {
    var num: Int = 0
    print("num check #1 = \(num)")
    
    let closure = { [num] in
        print("num check #3 = \(num)")
    }
    
    num = 20
    print("num check #2 = \(num)")
    closure()
}
```
위처럼 예제를 보면 closure 를 실행하기 전에 외부 변수 `num` 의 값을 20으로 변경했지만

```swift
// num check #1 = 0
// num check #2 = 20
// num check #3 = 0
```

이렇게 클로저의 `num` 에는 영향을 주지않는다.  하지만, 한 가지 더 유의해야 할 점은 Value Type 으로 캡쳐했을 경우, Closure 를 선언할 당시의 `num` 의 값을 Const Value Type 으로 캡쳐한다.
여기서 중요한 것은 **Const Value Type**, 즉 **상수**로 캡쳐된다는 것이다. 따라서 다음과 같이 closure 내부에서 Value Capture 된 값을 변경할 수 없다.

```swift
let closure = { [num, num2] in
    num = 2 // Error!: can not assign to value: 'num' is an immutable capture
```

정리하자면, 클로저는 기본적으로 Value Type의 값도 Reference Capture를 하지만, 클로져 캡쳐 리스트를 이용하면 Const Value Type으로 캡쳐가 가능 하다.

# 2-2. Reference Type 의 값을 복사해서 Capture 할순없나?

위에서 Value Type의 값을 클로저 "캡쳐 리스트"를 통해서 Value Capture 하는 것 까진 확인하였다.

그럼 **Reference Type의 값도 Capture Lists에 작성하면, Value Capture가 될까?**

```swift
class Cat {
    var name: String = "Sundae"
}
 
var cat1: Cat = .init()
 
let closure = { [cat1] in
    print(cat1.name)
}
 
cat1.name = "Unknown"
closure()
```

위 처럼 코드가 있을 때, cat1이라는 인스턴스는 Reference Type임 근데 내가 **클로저 캡쳐 리스트를 통해 cat1을 캡쳐 했으니까, cat1은 복사되어 캡쳐 됐을까?** 결과 값을 보면 알 수 있음

```swift
Unknown
```

아니요!! (단호) **캡쳐 리스트를 작성한다고 해도, Reference Type은 Reference Capture**를 함

그럼 **Reference Type은 클로저 캡쳐 리스트를 작성할 필요가 없겠군!??** 싶겠지만, 이건 클로저와 ARC를 보면 언제 쓰는지 이해할 수 있다

</br>
</br>

# 3. Closure & ARC

클로저 캡쳐 리스트를 좀 더 자세히 알아보기위해, 클로저 와 ARC 에 대해 알아보자

# ARC 란?: **인스턴스의 Reference Count를 자동으로 계산하여 메모리를 관리하는 방법.**

조금더 세세하게 알아보기위해 **클로저와 인스턴스간의 관계**에 대해 배워보도록 하자.

다음과 같이 내가 Cat 이란 클래스를 만들고, name 을 얻을 수 있는 Lazy 프로퍼티를 **클로저를 통해 초기화** 했음

***(이때, Lazy로 선언하지 않으면 에러가 발생)***

```swift
class Cat {
    var name = ""
    lazy var getName: () -> String = {
        return self.name
    }
    
    init(name: String) {
        self.name = name
    }
 
    deinit {
        print("Cat Deinit!")
    }
}
```

그리고 나선 다음과 같이 sundae 라는 **인스턴스**를 만들고, **클로저로 작성**되어 있는 **getName이란 지연 저장 프로퍼티를 호출**했음 

```swift
var sundae: Cat? = .init(name: "Kim-Sundae")
print(sundae!.getName())
```

그리고 나서 더이상 sundae 라는 인스턴스가 필요 없어서

```swift
sundae = nil
```

이렇게 인스턴스에 nil 을 할당했다, 그럼 인스턴스에 nil이 할당 되었고, 나는 이 인스턴스를 다른 변수에 대입한 적 없고, 따라서 **인스턴스의 ARC가 0이 되어 deinit이 호출**되어야 할 것 아님??

```swift
// Kim-Sundae 출력
```

근데, 이렇게 Kim-Sundae 가 출력된다 **deinit 함수는 불리지 않음…** 왜냐?? 비밀은 클로저에 있음

</br>

# 3-1. 클로의 강한 순환 참조

먼저 클로저는 참조 타입으로, **Heap**에 살고 있다! 따라서, 내가 생성한 Cat 이란 **인스턴스**는,

```swift
print(sundae!.getName())
```

---

**getName을 호출하는 순간** **getName이란 클로저가 Heap에 할당되며, 이 클로저를 참조**할 것이다.

(지연 저장 프로퍼티니 인스턴스 생성 직후가 아닌, 호출되는 순간에 메모리에 올라감)

근데, getName이란 클로저를 보면

```swift
class Cat {
    lazy var getName: () -> String = {
        return self.name
    }
}
```

이렇게 **self**를 통해 Cat **이란 인스턴스의 프로퍼티**에 접근하고 있다!! **클로저는 Reference 값을 캡쳐할 때 기본적으로 "strong"으로 캡쳐**를 함 따라서, 이때 Cat **이란 인스턴스의 Reference Count가 증가**해 버림..!!!

이해감?? Cat **인스턴스는 클로저를 참조**하고, **클로저는 Cat 인스턴스(의 변수)를 참조**하기 때문에 서로가 서로를 참조하고 있어서 둘 다 **메모리에서 해제되지 않는** **강한 순환 참조**가 발생해 버린 것이다.

그럼 어떻게 할까? ARC에서 배웠지만, 강한 순환참조는 **weak, unowned**를 통해 해결할 수 있다고 했음!

</br>

# **3-2. 클로저의 강한 순환 참조 해결법**

클로저에서 해결하려면 앞서 공부한 **weak & unowned**에 우리가 Reference Type일 땐 필요 없다 느꼈던 **캡쳐 리스트**를 이용해야 한다. 

## **weak & unowned + Capture Lists**

이 두가지를 이용해서 강한 순환 참조를 해결하는 것이다. 클로저가 프로퍼티에 접근할 때 **self를 참조하면서 문제가 발생** 했잖슴? 따라서 **self에 대한 참조를 Closure Capture Lists를 이용해 weak, unowned로 캡쳐**해버리는 것.

```swift
class Cat {
    lazy var getName: () -> String? = { [weak self] in
        return self?.name
    }
}
```

```swift
class Cat {
    lazy var getName: () -> String = { [unowned self] in
        return self.name
    }
}
```

이런 식으로 weak, unowned로  Reference Capture를 해버리는 것이다. 이렇게 클로저 리스트를 통해 강한 순환 참조를 해결해 줄 수 있다…  그러면

```swift
// Kim-Sundae
// Cat Deinit!
```

**deinit 이 정상 실행이된다** !! 

근데, **weak**의 경우 nil을 할당받을 가능성이 있기에 Optional-Type으로 **self에 대한 Optional Binding**을 해주어야 하지만, **unowned**의 경우엔 Non-Optional Type으로 **self에 대한 Optional Binding 없이 사용**할 수 있음!!!

(물론 Swift 5.0부턴 unowned도 Optional Type이 되지만, 캡쳐 리스트로 동작할 땐 Non-Optional Type으로 동작 하는듯..!?)

ARC를 공부할 때 **unowned를 도대체 언제 사용는가??** 란 의문이 들었었는데..

위와 같이 **클로저를 Lazy Initialization로 선언해서 강한 순환 참조가 일어난 경우**엔,

인스턴스가 존재해야만 (접근하여) 초기화를 시킬 수 있고, 따라서 이때 **self는 값이 있다고 가정**하기 때문에,,****

이 경우엔 **unowend 를 사용하는 것이 가능하고,**

또 **unowned를 사용할 경우 Optional Binding을 하지 않아도 돼서 코드도 깔끔해 짐**!!

+ 라고 생각 했으나... 만약 **Lazy로 선언된 클로저가 self를 unowned로 캡쳐**하고, 시점 차이로 인해 **해당 인스턴스에 nil이 할당 된 후에도 lazy property 작업이 실행되어야 하는 상황이 생길 경우,** 이때는 **unowned self capture에 문제**가 있어 보입니다!! 따라서..... weak 권장요 ㅎ_ㅎ…

</br>
</br>

# 4. SWIFT 에서 클로저는 여러개

## Named Closure: 전역함수(Global Function), 중첩함수(Nested Function)

## Unnamed Closure: 익명함수(Unnamed Function)

클로저는 **전역 함수, 중첩 함수, 익명 함수** 이 세가지를 모두 아우르는 것이다! 중요해! (물론 일반적으론 익명 함수를 칭하긴 하지만!!)
근데 우린 바로 위에서 Unnamed Closure, 즉 **익명함수**일 때만 값 캡쳐 방식을 살펴봤단 말임? 따라서 **Named Closure**일 때 **값 캡쳐하는 방식**을 살펴볼 것이다.



