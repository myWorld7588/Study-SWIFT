Closure 는 프로그래밍할때 진짜 많이 쓰이니까 제대로 배우고 알아두자

[Function 함수정복 1/3](https://www.notion.so/Function-1-3-2bab07edc47b4fceb5d443bc062e509f) 에 있는 간단한 클로져 내용은 아래와 같다.  콜로저란 개념에 대해 간단하게 다시 짚고가자.

> 사실 **`func` 키워드를 이용해 이름을 붙여주는 함수들도 모두 클로저**임!! 클로저는 다음과 같이 두 가지 종류가 있음
> 

![https://blog.kakaocdn.net/dn/7cJQa/btqQhMp9gqo/jT7hCUOFvbe0mUNgivsqW1/img.png](https://blog.kakaocdn.net/dn/7cJQa/btqQhMp9gqo/jT7hCUOFvbe0mUNgivsqW1/img.png)

**Named Closure**, **Unnamed Closure** 가 있는데 다음과 같이 선언해왔던 **이름이 있는 함수**는

# Named Closure

```swift
func doSomething() {
    print("UhMook")
}		
```

---

**Named Closure**임! 근데, 이를 **클로저라 부르지 않고,**

그냥 **함수**라고 부르는 것 뿐임! (클로저란 사실은 변함 없음)

# Unnmaed Closure

다음과 같이 **이름을 붙이지 않고 사용하는 함수**를 **익명함수**, 즉 **Unnamed Closure**라고 부름 보통 **Closure라고 하면 Unnamed Closure**를 말하긴 함 :)

```swift
let closure = { print("UhMook") }
```

---

따라서, 정리하자면 **클로저는 Named Clousre & Unnamed Closure 둘다 포함하지만, 보통 Unnamed Closure를 말한다!**

# **리뷰끝 ———————————————————————**

클로저도 익명이긴 하지만 함수이기 때문에 **1급객체 함수**의 특성을 고대로 다 가지고 있슴.  때문에 클로저 또한 **자료형**을 가지고있다.

# 1. Closure 표현식

```swift
{(Parameters) -> Return Type in
    실행구문
}
```

표현식은 이렇게 쓰는데, 익명 함수인 만큼 `func` 이란 키워드를 쓰지않는다.  그리고 클로저는 다음과 같이 **Closure Head** 그리고 Closure Body 로 이루어져있는데 이 둘을 바로 구분지어주는게 바로 `in` 이라는 키워드다.


실제로 어떤식으로 사용하는지 봐보면서 이해를 해보자~

# 1-1. Parameter 와 Return Type 이 둘다 없는 클로저

자.. 클로저는 뭐다? 익명이긴 하지만 함수이다…! 따라서 1급 객체이기 때문에 다음과 같이 상수에 클로저를 대입할수있다.  특히 Parameter 와 Return Type 이 둘다 없는 경우는 다음과 같이 사용한다.

```swift
let closure = {() -> () in
    print("Closure")
}
```

함수처럼 Return Type 을 생각해도되고,, 심지어 Return Type 이 있어도 생략가능하고.. 함수에선 안되는 Parameter 조차 생략이 가능하다…  이건 **클로저 경량 문법에서 배울것**이니.  지금은 교과서 적인 표현부터 알고 넘어가자.

# 1-2. Parameter 와 Return Type 이 있는 클로저

```swift
let closure = { (name: String) -> String in
    return "Hello, \(name)"
}
```

주의해야할점!! Function 때 배운대로 Parameter 의 “name” 은 단독으로 쓰였으니 Argument Label 이자, Parameter Name 이라고 생각할 수 있지만.  

## **클로저에서는 Argument Label 을 사용하지 않음**

따라서, name 은 Argument Label 이 아니고 오직 Parameter Name 이다.  따라서 Closure 를 호출시

```swift
closure("UhMook")
closure(name: "UhMook") // Error! 에러남
```

이렇게 Argument Label 을 사용하지않음. 사용하면 에러남.

# 2. 1급 책제로서 클로저

# 3. 클로저 실행하기

# 3-1. 클로저가 대입된 변수나 상수로 호출하기

# 3-2. 클로저를 직접 실행하기
