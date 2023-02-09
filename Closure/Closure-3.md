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

