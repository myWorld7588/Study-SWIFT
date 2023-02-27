## **1. Codable이 도대체 뭘까**

Codable이 그래서 도대체 뭔지 정의부터 얘기하고 가자면 **Swift4부터 추가된 프로토콜로,**

**JSON 데이터를 간편하고 쉽게 Encoding / Decoding 할 수 있게 해준다**

뭐 그런 내용임..! 한 마디로 서버와 데이터를 주고 받을 때 보통 JSON으로 주고 받잖음?

이 JSON 데이터를 다룰 때 보통 직접 다룰 수도 있고,

SwiftyJSON과 같은 라이브러리를 통해서 다룰 수도 있잖음??

근데 이 Codable을 이용하면, 직접 다루거나 라이브러리를 사용하는 것보다 훨씬!! 간편하게?

JSON 데이터를 다룰 수 있음 ! 그러면 Codable의 사전적 정의를 살펴봤으니,

실제 코드로는 어떻게 정의되어 있는지 보자.

![https://blog.kakaocdn.net/dn/cd5Lbp/btqOH8O793R/ShTtajem0k4u3CkArIt3o1/img.png](https://blog.kakaocdn.net/dn/cd5Lbp/btqOH8O793R/ShTtajem0k4u3CkArIt3o1/img.png)

Codable은 **Decodable**과 **Encodable**로 이루어져 있다고 typealias로 정의되어 있음

그럼 **Decodable**과 **Encodable**이 뭐냐

![https://blog.kakaocdn.net/dn/uDFeV/btqOvei7kSi/1Z4ePV9EWYNfBLMixSW6c0/img.png](https://blog.kakaocdn.net/dn/uDFeV/btqOvei7kSi/1Z4ePV9EWYNfBLMixSW6c0/img.png)

뭔진 모르겠지만 일단 `protocol`이군 👀 따라서 Codable이란, **Encodable & Decodable 프로토콜을 준수하는 프로토콜이다** 정도가 되겠음 **Struct**, **Class**, **Enum** 모두 **Codable을 채택**할 수 있음!!!

그럼 Codable을 이용해서 도대체 어떻게 쉽게 Encoding / Decoding 하는지 살펴보자.

## **2. Codable을 이용한 Encoding**

먼저, **Encoding**을 먼저 다뤄보겠음 사실 앱을 개발하다 보면 JSON 데이터를 Decoding 하는 일은 많

지만 Encoding 하는 일은 별로 없을 거란 말이지? 

먼저, Encoding이 무엇이냐 정의부터 내리면 **내가 원하는 struct, class, enum 등등의 인스턴스를** 

**JSON 형태의 Data로 만들어주는 것** 예제로, Encoding할 데이터를 Struct로 하나 만들고 sundae 라는 

구조체 변수도 생성해보겠음

```swift
struct Cat: Codable {
    var name: String
    var age: Int
}

let sundae: Cat = .init(name: "Sundae", age: 1)
```

---

자, 여기서 중요한 것은, Codable을 이용해 JSON으로 Encoding 하고 싶을 경우,

반드시 **Codable이라는 Protocol을 준수**하고 있어야함

자, 그럼 undae라는 구조체 변수를 Encoding 해보겠음 Codable을 이용하면 얼마나 간단하냐면

```swift
 **let** data **=** **try**? JSONEncoder().encode(sundae)
```

---

이렇게 한 줄로 끝남..! 실제 data를 String으로 변환시켜 출력해보면

```swift
{"name": "sundae", "age": 1}
```

데이터가 잘 만들어져 있음!!!! `JSONEncoder`클래스에다가 encode 메서드를 호출해주는데,

이때 codable을 준수하는 구조체 타입인 sodeul을 넣어주면 JSON 데이터를 알아서 뚝딱 만들어줌.

이 encode 함수를 좀 더 자세히 보자면

```swift
open func encode<T>(_ value: T) throws -> Data where T : Encodable
```

**Generic**으로 선언되어 있는데,이 T는 **Encodable**이라는 **프로토콜을 준수**하고 있어야만 이 **encode라**

**는 메서드를 사용**할 수 있음 우리는 **Codable(Encodable + Decodable)**을 사용했기 때문에 encode라

는 메서드를 사용할 수 있음! 또한 **throws**라고 되어있는 것 처럼, Encoding 중 에러가 발생할 수 있기 

때문에 반드시 **try**와 같이 써주어야 함!!! 만약, Codable 프로토콜을 채택하지 않고 사용하려고 한다

면, encode 메서드는 Cat 구조체가 Encodable을 준수하도록 요구한다 라는 메세지가 뜨면서 안됨

쨌든 Codable을 이용하면 이렇게나 간단하고 단 한 줄 만으로 JSON 데이터를 만들 수 있따.

**질리도록 사용하는 Decoding을 다뤄보겠음**

# **3. Codable을 이용한 Decoding**

이번엔 **Decoding**을 다뤄보겠음!! 실제로 우리가 가장 많이 사용하는 것은 이 Decoding이기 때문

자, 먼저 **Encoding**은 내가 원하는 struct, class, enum 등등의 인스턴스를 JSON 형태의 Data로 만들어

주는 것이랬음 그럼 **Decoding**은 머냐거? **JSON 형태의 Data를 struct, class, enum 등의 인스턴스에**

**자동으로 파싱** 해주는 것임 이해가 안 갈 수 있으니, 예제로 보자 :)

자 또 다음과 같이 Codable을 준수하는 구조체가 있음

```swift
struct Cat: Codable {
    var name: String
    var age: Int
}
```

---

이번엔, 만약 **서버에서 name과 age가 담긴 JSON 데이터를 준다**고 생각해보셈

지금은 서버와 작업하는 것이 아니니 다음과 같이  data라는 변수를

```swift
let data = """ {
    "name" : "sundae",
    "age"  : 1
}

""".data(using: .utf8)!
```

---

서버가 준 JSON 데이터라 생각하고 고고 - 그럼 나는 이제 이 data를 Human이란 구조체 변수에 Decoding을 하고 싶다면

```swift
**let** sundae **=** **try**? JSONDecoder().decode(Cat.**self**, from: data)
```

---

자, 이렇게 써주면 또 단 한 줄 만으로 파싱이 됨. 결과값을 보면

```swift
sundae?.name     // "sundae"
sundae?.age      // 1
```

sundae이란 객체에 JSON Data 값이 아주 잘 파싱되어 있음!!!! 먼저, 어떻게 파싱되는지는 보기 전에 왜 함수가 저런 모양인지 **decode 함수 원형**부터 살펴보면,

![https://blog.kakaocdn.net/dn/m3IbW/btqOuGNICzc/EMDAmjsL3yvlPAJEdQejW1/img.png](https://blog.kakaocdn.net/dn/m3IbW/btqOuGNICzc/EMDAmjsL3yvlPAJEdQejW1/img.png)

Encode와 마찬가지로 **Generic T**는 **Decodable을 준수**하고 있어야 하고, Decoding 중에 실패할 수 있기 때문에, 반드시 **try**랑 함께 써주어야 함 또한 **type** 파라미터는 **T.Type**을 받기 때문에, 우리가 파싱

하고자 하는 클래스 **Cat 의 type**을 써줘야 해서 **Cat.self**를 써준 것임 자, 그럼 어떤 방식으로 파싱이 되는지를 확인해보면. Cat 타입의 구조체를 하나 만들고, **JSON Data의 Key 값과 동일한 이름의 구조체 변수에 value에 값을 파싱함** 그리고 **파싱된 구조체를 리턴**함 좀 더 쉽게 설명하자면,

![https://blog.kakaocdn.net/dn/NhLQZ/btqOH9AywVs/JXqxxkhcjFRskNVR7tMgg1/img.png](https://blog.kakaocdn.net/dn/NhLQZ/btqOH9AywVs/JXqxxkhcjFRskNVR7tMgg1/img.png)

이런 식으로

JSON Data의 "**Key**" 값(name, age)가 **구조체의 변수 이름**(name, age)와 동일하면, 그 **변수의 값**

(name, age)에 **Value("sundae", 1)을 파싱**하는 것임 따라서 **JSON Data의 Key 값은 Codable을 따르는** 

**타입(Cat 구조체)의 멤버 이름과 1대1 매칭 되어야만 문제 없이 사용할 수 있다**

그럼.. 여기서 궁금증이 생김 그럼 서버에서 주는 Key 값에 무조건 맞춰서 변수명을 사용해야 하나..?

Decodable의 특징으로 설명하겠음

# **3-1. CodingKey**

만약 서버에선 **Name** 이라는 Key를 사용한다고 침

```swift
struct Cat: Codable {
    var name: String
}

let data = """ {
    "Name" : "sundae",
}
""".data(using: .utf8)!
```

근데 변수명은 대문자로 시작하지 않으니까.. 우리가 **name**이라고 이름을 바꿔줘버리면!

**1대1 매칭이 되지 않기 때문에 Decoding Fail** 로 간주되어, nil로 떨어짐

```swift
let sundae = try? JSONDecoder().decode(Cat/self, from: data) // **Error!: nil**
```

---

따 라 서 만약 Key 값에 대응하는 이름을 바꿔주고 싶다면, **CodingKey** 라는 프로토콜을 이용해서 

Key의 이름이 바뀌었다는 걸 명시해줘야 함 어디에 명시하냐??

CodingKey을 준수하는 타입에 enum으로 다음과 같이 명시해줌

```swift
struct Cat: Codable {
    var name: String
}
    enum CodingKeys: String, CodingKey {
        case name = "Name"
    }
}
```

---

이런 식으로,

**Name이라는 키를 name이란 이름으로 받겠다고 정의**해줘야 함 (귀찮)

# **3-2. 특정 Key-Value가 없이 오는 경우**

만약 서버에서 데이터를 던져주는데, 어느 순간 특정 Key-Value가 누락되어서 올 수도 있잖음?

```swift
struct Cat: Codable {
    var name: String
}

let data = """ {

}
""".data(using: .utf8)!
```

---

이렇게 data에 name이라는 **Key-Value가 누락**되어 아무 것도 안와버린 것임 그러면 Codable은 이를 

어떻게 처리하냐?

```swift
let sundae = try? JSONDecoder().decode(Cat.self, from: data)    **// Error!: nil**
```

에러ㅋ 진짜 예민한 친구다. 따라서 이럴 경우를 대비해서 2가지 처리를 할 수 있는데,

### **① Key가 없을 경우를 대비해 직접 Decoding 함수를 작성한다**

먼말이냐면, JSON을 Decoding 할 때 **init(from decoder: Decoder)** 이 함수가 호출되는데, 우리가 이 

함수를 직접 건드려서 **야 키 없으면 이 값으로 대체하자!!!** 하고, 우리가 기본 값을 셋팅해준다는 것임

어떻게 사용하는진 코드로 보여주겠음!!!

```swift
struct Human: Codable {
    var name: String

    init(from decoder: Decoder) throws {
        let values = try decoder.container(keyedBy: CodingKeys.self)
        name = try? values.decode(String.self, forKey: .name)) ?? ""
    }
}
```

---

이렇게 정의해주면  name이라는 Key가 없이 오더라도, 

Decoding Fail이 아닌 **우리가 미리 정의해둔** **"" 값이 name**에 들어감

### **② 변수를 옵셔널로 선언하자**

사실....... 맘편하게 Optional로 선언하면

```swift
struct Human: Codable {
    var name: String?
}
```

---

문제 해결 :)  ... 하지만 나중에 Optional Binding 하기가 귀찮겠찌

# **3-3. Value 값이 null인 경우**

마지막으로 다뤄보겠음 **JSON Data에선, Value 값이 null 일 수가 있는데,**

```swift
let data = """
{
    "name" : null
}
""".data(using: .utf8)!
```

---

Codable은 Value 값이 null일 경우

```swift
let sundae = try? JSONDecoder().decode(Cat.self, from: data)     // nil 
```

또.. 에러;; 

툭 건들면 에러여; 쨌든 이럴 땐 단순하게 **변수를 Optional로 선언**하면 됨

```swift
struct Human: Codable {
    var name: String?
}
```

---

