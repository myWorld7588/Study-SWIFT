# ****트레일링 클로저(Trailing Closure) 란?****:

**함수의 마지막 파라미터가 클로저일 때, 이를 파라미터 값 형식이 아닌 함수 뒤에 붙여 작성하는 문법.** 

**이때, Argument Label은 생략된다**

중요한 것은 **마지막 파라미터가 클로저, Argument Label은 생략** 이 두가지임

클로저는 1급 객체이기 때문에, 함수의 파라미터로 클로저를 전달할 수 있다고 한 것을 유념하고 보자.

</br>
</br>

# 1-1. 파라미터가 클로저 하나인 함수

다음과 같이 클로저 하나만 파라미터로 받는 함수가 있을때 

```swift
func doSomething(closure: () -> ()) {
    closure()
}

// 함수를 호출하려면 이렇게 해야했슴
doSomething(closure: { () -> () in
    print("Hello!")
})

```

이렇게 **클로저가 파라미터의 값 형식**으로 함수 호출 구문 **() 안에 작성되어 있는 것**을 **Inline Closure** 라고 부른다

근데 이 경우 마지막에 괄호도 **})** 이렇게 되어 있어 헷갈리기 쉽고, 코드를 딱 봤을 때 해석도 쉽지 않음..

따라서 이 클로저를 파라미터 값 형식으로 보내는 것이 아닌, **함수의 가장 마지막에 클로저를 꼬리처럼 덧붙여**서 쓸 수 있는데, 

```swift
doSomething () { () -> () in
    print("Hello!")
}
```

이렇게 쓰는 것이 바로 **Trailing Closure**! 

여기서 중요한 핵심은 다음 두 가지 이다.

### **1. 파라미터가 클로저 하나일 경우, 이 클로저는 첫 파라미터이자 마지막 파라미터이므로 트레일링 클로저가 가능**

### **2. "Closure"라는 ArgumentLabel은 트레일링 클로저에선 생략됨**

**파라미터가 클로저 하나일 경우**엔 호출구문인 **()도 생략**할 수도 있다

</br>
</br>

# 1-2. 파라미터가 여러개인 함수
첫번째 파라미터로 success 라는 클로저를 받고, 두번째 파라미터로 fail 이라는 클로저를 받는 함수가 있다.

```swift
func fetchData(success: () -> (), fail: () -> ()) {
    // Action
```

이런 함수가 있을때 Inline Closure 의 경우 호출하는 방법은 이렇다.

```swift
fetchData(success: { () -> () in
    print("Success!")
}, fail: { () -> () in
    print("Fail!")
})
```

그런데 트레일링 클로저의는 마지막 파라미터의 클로저는 함수 뒤에 붙여서 쓸수있다.

```swift
fetchData(success: { () -> () in
    print("Success!")
}) { () -> () in
    print("Fail!")
}
```

따라서 이런 모양으로 사용이 가능하다.  파라미터가 여러개일 경우, 함수 호출 구문 () 을 마음대로 생략해선 안된다.  Success란 파라미터는 파라미터 값 타입으로 넘겨주어야 하니까.

이제 경량 문법을 통해 클로저를 간단하게 바꾸어보자!!!

</br>
</br>


# 2. 클로저의 경량 문법이란?:

### 문법을 최적화 하여 클로저를 단순하게 쓸 수 있게 하는 것.

```swift
func doSomething(closure: (Int, Int, Int) -> Int) {
    closure(1, 2, 3)
}
```

위 처럼 함수가 있다고 가정해보자,  **이 함수는 파라미터로 받은 클로저를 실행** 하는데, 이때 **클로저의 파라미터로 1, 2, 3** 이란 숫자를 넘겨주고 있다.  그렇다면 실제 이 함수를 호출할 때 어떻게 했어야 했냐면.

```swift
doSomething(closure: { (a: Int, b: Int, c: Int) -> Int in
    return a + b+ c
})
```

이렇게 클로저를 FULL 작성했어야 했음. (+Inline Closure 방식). 이걸 경량문법으로 간단히 바꿔보자

</br>
</br>

# 2-1. 파라미터 형식과 리턴 형식을 생략할 수 있다

여기서 파라미터 형식과 리턴 형식은 `(Int, Int, Int) → Int` 들이다.  그래서 아래처럼 생략해서 쓸수있다.

```swift
doSomething(closure: { (a, b, c) in
    return a + b + c
})
```

</br>
</br>

# 2-2. Parameter Name 은 Shorthand Argument Names 로 대체하고 이 경우 Parameter Name 과 in 키워드를 삭제한다

Shorthand Argument Names 가 뭐냐면, Parameter Name 대신 사용 할 수 있는것임..

위에서 Parameter Name 과 in 키워드는 `(a, b, c) in` 이거잖아?  이때 이 `a b c` 라는 Parameter Name 대신에 

a → $0

b → $1

c → $2

이런식으로 `$` 와 `index` 를 이용해서 Parameter 에 순서대로 접근하는것이 바로 Shorthand Argument Names 이다.  따라서 경량 문법 규칙에 의해 위 구문은 이렇게 간단화 될수있슴

```swift
doSomething(closure: {
    return $0 + $1 + $2
})
```

</br>
</br>

# 2-3. 단일 리턴문만 남을 경우, return 도 생략한다

단인리턴문 이란 것은… 이렇게 클로저 내부에 코드가 return 구문 하나만 남은경우를 말함

```swift
doSomething(closure: {
    return $0 + $1 + $2
})
```

이때는 return 이란 키워드도 다음과 같이 생략할수있슴.
