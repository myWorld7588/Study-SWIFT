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


