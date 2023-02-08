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


```swift
doSomething(closure: {
    $0 + $1 + $2
})
```

만약 단일 리턴문이 아닐경우 아래처럼 에러가 나버린다.
```swift
doSomething(closure: {
    print("\($0), \($1), \($2)")
    $0 + $1 + $2 // Error! Missing return in a closure expected to return 'int';
})
```
</br>
</br>

# 2-4. 클로저 파라미터가 마지막 파라미터면, 트레일링 클로저로 작성한다.


위에서 **트레일링 클로저**를 배웠다, 따라서 마지막 파라미터인 클로저를 다음과 같이

```swift
doSomething() {
    $0 + $1 + $2
}
```

이렇게 트레일링 클로저로 작성이 가능하단 것을 알수있다, 또한 파라미터가 하나인 경우 ()도 생략 가능하다고 배웠슴

</br>
# 2-5. ()에 값이 아무 것도 없다면 생략한다

이것이 최종화된 클로저의 경량 문법이다
```swift
doSomething {
    $0 + $1 + $2
}

```

</br>
</br>

# 3.  @autoclosure 란?

### **파라미터로 전달된 일반 구문 & 함수를 클로저로 래핑(Wrapping) 하는 것**

autoclosure는 **파라미터 함수 타입 정의 바로 앞** 에다가 붙여야 한다.

```swift
func doSomething(closure: @autoclosure () -> ()) {
}
```

이렇게 했을 경우, 이제 closure란 파라미터는 **실제 클로저를 전달받지 않지만, 클로저처럼 사용이 가능**

다만, 클로저와 다른 점은 실제 클로저를 전달하는 것이 아니기 때문에 파라미터로 값을 넘기는 것 처럼 **()**를 통해 **구문을 넘겨줄 수**가 있음

```swift
doSomething(closure: 1 > 2)
```
// 여기서 1 > 2 는 클로저가 아닌 일반 구문이지만, 실제 함수 내에서는

</br>

```swift
func doSomething(closure: @autoclosure () -> ()) {
    closure()
}
```
이렇게 일반 **구문을 클로저처럼 사용**할 수 있음! 왜냐? **클로저로 래핑**한 것이니까  근데, 주의점은

![https://blog.kakaocdn.net/dn/bVyl5T/btqQLku6PrF/eVQvtff5GEH91yZLuzG9v1/img.png](https://blog.kakaocdn.net/dn/bVyl5T/btqQLku6PrF/eVQvtff5GEH91yZLuzG9v1/img.png)

autoclosure를 사용할 경우, **파라미터가 반드시 없어야 함**...!!!! 리턴 타입은 상관 없음!

</br>

# 3-1. Autoclosure 특징: 지연된 실행

원래, **일반 구문은 작성되자마자 실행**되어야 하는 것이 맞음

근데 autoclosure로 작성하면, 함수 내에서 클로저를 실행할 때까지 구문이 실행되지 않음

왜냐? 함수가 실행될 시점에 구문을  클로저로 만들어주니까..

따라서, autoclosure의 특징은

**원래 바로 실행되어야 하는 구문이 지연되어 실행**한다는 특징이 있음

뭐..... 잘쓰면 좋다는데….. 아직 와닿진 않네 ㅇ_ㅇ

</br>
</br>

# 4. @escaping

지금까지 짜왔던 다음과 같은 클로저는

```swift
func doSomething(closure: () -> ()) {
}
```

모오두 non-escaping Closure  임.  무슨 말이냐면, **함수 내부에서 직접 실행하기 위해서만 사용한다.  따라서 파라미터로 받은 클로저를 변수나 상수에 대입할 수 없고, 중첩 함수에서 클로저를 사용할 경우,  중첩함수를 리턴할 수 없다.  함수의 실행 흐름을 탈출하지 않아, 함수가 종료되기 전에 무조건 실행 되어야 한다.**


실제로 상수에 클로저를 대입해보자.
```swift
func doSomething(closure: () -> ()) {
    let y: () -> () = closure // Error!: Using non-escaping parameter 'closure' in a context expecting an @escaping closure
}
```

**non-escaping parameter**라고 에러가 뜸 또한 함수의 흐름을 탈출하지 않는다는 말은, **함수가 종료되고 나서 클로저가 실행될 수 없다는 말**임!

```swift
func doSomething(closure: () -> ()) {
    print("function start")
    
    DispatchQueue.main.asyncAfter(deadline: .now() + 10 { // Error!: Escaping closure capture non-escaping paramter
    closure()
    }
    print("function end")
}
```

따라서, 10초 뒤 클로져를 실행하는 구문을 넣으면, 함수가 끝나고 클로저가 실행되기 때문에 에러가 남.  또한, 만약 중첩함수 내부에서 매게변수로 받은 클로저를 사용할 경우

```swift
func outer(closrue: () -> ()) -> () -> () {
    func inner() {
        closure()
    }
    return inner // Error!: Escaping local function captures non-escaping parameter 'closure'
}
```

중첩함수를 리턴할수 없슴.  이 모든 에러의 원인은 non-escaping closure 의 주변 값 capture 방식에있다.  (중첩함수가 클로저를 return할 경우 capture 문제 등)

어쨌든… 이렇게 함수 실행을 벗어나서 함수가 끝난 후에도 클로저를 실행하거나, 

중첩함수에서 실행 후 중첩 함수를 리턴하고 싶거나, 

변수 상수에 대입하고 싶은 경우!! 이때 사용하는 것이 **@escaping** 키워드이다.

```swift
func doSomething(closure: @escaping () -> ()) {
}
```

이렇게 클로저 파라미터 타입 앞에 @escaping 을 붙여주면 되고,  변수나 상수에 파라미터로 받은 클로저를 대입할 수 있고

```swift
func doSomething(closure: @escaping () -> ()) {
    let y: () -> () = closure
}
```

그리고 함수가 종료된 후에도 클로저가 실행 될 수 있다.

```swift
func doSomething(closure: @escaping () -> ()) {
    print("function start")
    
    DispatchQueue.main.asyncAfter(deadline: .now() + 10 { // Error!: Escaping closure capture non-escaping paramter
    closure()
    }
    print("function end")
}

doSomething { print("closure") }

// function start
// function end
// closure
```

함수가 종료된 후에도 클로저가 실행됨

**근데 이 escaping 클로저를 사용할 경우 주의해야할 점이 있는데,  메모리 관리와 관련된 부분이다.**

예를 들어, 만약 함수가 종료된 후 클로저를 실행하는데, 이때 클로저가 함수 내부 값을 사용함

그럼 이때 함수는 이미 종료 되었는데, **클로저는 함수 내부 값을 어떻게 사용할까**?

이런 메모리 관련 부분 ㅎㅎㅎ!!!! ARC 배우면서 더 알아보자

