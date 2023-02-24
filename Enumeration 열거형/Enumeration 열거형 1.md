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

</br>

# 2-3. Number Type을 가지는 열거형

```swift
enum Position: Int {
    case top    // 0
    case mid    // 1
    case jug    // 2
    case adc    // 3
    case sup    // 4
} // 이렇게 Int라는 타입을 enum 선언 시 이름 옆에 명시해주면 
  // 가장 먼저 선언된 case부터 0부터 1씩 증가된 값이 들어감!!

```

만약 내가 **RawValue를 직접 지정** 해주고 싶다면 물론 가능하다

```swift
enum Position: Int {
    case top = 0     // 0
    case mid = 10    // 10
    case jug         // 11
    case adc = 24    // 24
    case sup         // 25
} // 대신 Raw Value가 없는 case는, 바로 이전 case의 Raw Value에서 +1한 값으로 셋팅됨!!
```

### Int형이 아닌 자료형으로 했을 경우에, 모든 case에 대해 값을 지정해주는 것이 아니면 다음과 같은 에러가 발생함.

그 이유는, Number Type의 Raw Value는 **만약 값이 없으면**, **바로 이전 case의 Raw Value의 값에서 1이란 정수값**을 더한 값을 가진다. 그런데 바로 이전 Raw Value값인 adc가 **실수 4.0** 이다. 

sup의 Raw Value를 컴파일러가 지정해야 하는데, **바로 이전 case인 adc의 Raw Value가 정수값이 아닌**

**실수값이기 때문에** 못더해!!!!!! 하고 에러를 뱉는 것이다.


따라서 Int형이 아닌 Number 자료형을 사용할 경우

```swift
enum Position: Double {
    case top = 1.0    // 1.0
    case mid = 2.0    // 2.0
    case jug = 3.0    // 3.0
    case adc = 4      // 4
    case sup          // 5
}
```

**Raw Value를 생략하고 싶다면, 바로 이전 case의 Raw Value를 정수 값** 으로 지정해 줘야함.

</br>

# 2-4. Character Type을 가지는 열거형

```swift
enum Position: Character {
    case top = "t"
    case mid = "m"
    case jug = "j"
    case adc = "a"
    case sup = "s"
}
```

### **주의할 점!** Character Type으로 열거형을 선언할 경우 **모-든 case에 대한 Raw Value를 직접 선언**해주어야 함

**만약 하나라도 빵꾸나면 바로 에러가난다.** 아까 Double에서 에러난 것과 같은 맥락임이다. 컴파일러는 **지정되지 않은 Raw Value를 이전 case Raw Value를 보고 + 1** 해야 하는데, **이전 case의 Raw Value가 "T"로 정수형이 아니라 + 1  할수없어! 하고 에러 뱉는 것이다.**

</br>

# 2-5. String Type을 가지는 열거형

String은 Character와 달리 **Raw Value를 지정하지 않으면, case 이름과 동일한 Raw Value가 자동완성.**

간단하고 편하구만

```swift
enum Position: String {
    case top              // top          
    case mid              // mid
    case jug = "iphone"   // iphone
    case adc              // adc
    case sup              // sup
}
```

그럼, 원시 값이 있는 열거형의 경우는 Raw Value에 어떻게 접근할까?

이름 그대로 **rawValue**란 속성을 이용해 접근하면 됨

```swift
var user1: Position = .adc    // adc
var user2: Position = .jug    // jug
user1                         // adc
user1.rawValue                // "adc"
user2                         // jug
user2.rawValue                // "iphone"
```

만약, Raw Value가 있는 열거형의 경우,

**Raw Value를 통해서도 열거형을 생성**할 수 있는데!!! 이땐 다음과 같은 생성자를 이용하면 됨

```swift
var user4 = PositionWithRawValue.init(rawValue: "sup")     // sup
var user5 = PositionWithRawValue.init(rawValue: "supp")    // nil
```

근데, 만약 없는 Raw Value 값을 대입할 수 있으니,

이때 반환되는 열거형은 **옵셔널 타입**임! (없는 Raw Value일 경우, nil 리턴)

