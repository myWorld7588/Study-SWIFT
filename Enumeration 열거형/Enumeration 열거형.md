# 1. 열거형이란?

**같은 주제로 연관된 데이터들을 멤버로 구성하여 나타내는 자료형**

**이미 "정해놓은 입력 값"만 선택해서 받고 싶을 때 사용**하는 것이 **열거형이다.**

열거형을 사용할 경우, 코드 가독성도 좋아지고, 오타낼 일도 줄여주니 안정성도 향상됨!

또한 Heap에 저장되는 String과 달리, **Enum은 값 형식으로 Stack에 저장**되어

성능면에서도 향상된다.

</br>

# 2. 열거형 정의하기

## 2-1. 원시값이 없는 열거형

그냥 다음과 같이 **열거형 이름만 쓰고 선언해주면, 원시값이 없는 열거형임…!**

```swift
enum Position {
    case top
    case mid
    case jug
    case adc
    case sup
} // 이렇게 case를 일일이 다 써서 쭉 나열해도 되고,

enum Position {
    case top, mid, jug, adc, sup
} // 이런 방식으로 하나의 case에 ,(comma)를 이용해서 나열해서 써도 됨!!!!
```

이렇게 작성해 주면 **원시값이 없는 열거형 이다!**

</br>

열거형으로 타입이 지정된 경우, 아래처럼 **.(점문법)을 이용해** **내가 선언한 case에 한해서만 접근**할 수 있다!!

따라서 오타의 가능성도 현저히 줄어들고, 코드의 가독성도 매우 높아짐

```swift
var user1: Position = .top
var user2: Position = .sup
var user3: Position = . // .(점문법) 자동완성
```

</br>

# 2-2. 원시값이 있는 열거형

위에서 case를 지정할 때, 우린 아무런 값도 대입하지 않았다!! 근데 사실 이 **case에 원시값을 지정**해줄 수도 있는데, 이를 **Raw Value**라고 함. 이때 Raw Value가 될 수 있는 자료형은 총 3가지가 있다!!!

- Number Type
- Character Type
- String Type

위와 같은 원시 값을 가지고 싶다면, **enum 선언 시 이름 옆에 Type을 꼭꼭 명시**해주어야 한다.





