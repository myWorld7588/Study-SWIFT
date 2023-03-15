# 1. 프로퍼티 란?

**지금껏 클래스, 구조체에서 선언하던 상수/변수가 프로퍼티 아닌감?**  뭐 맞긴 맞음!!! 

근데 **Swift에선 이 프로퍼티가 총 3가지 형태로 존재**함

- **Stored Property : 저장 프로퍼티**
- **Computed Property : 연산 프로퍼티**
- **Type Property : 타입 프로퍼티**

이 프로퍼티라는 것이 값을 **Class, Struct, Enum과 연결**한다고 하는데,  어떤 프로퍼티는 위 세가지 중에 어떤 때에 사용할 수 있는지에 대해서도 공부해보자.

# 2. 저장 프로퍼티 (Stored Property)란?

### **클래스와 구조체에서만 사용할 수 있고, 값을 저장하기 위해 선언되는 상수/변수**

```swift
class Human {
    let name: String = "unknown"
    var age: Int = 0
}
 
struct Person {
    let name: String = "unknown"
    var age: Int = 0
}
```

`Human`이란 클래스와 `Person`이란 구조체에 저장된 **name(상수), age(변수) 모두 저장 프로퍼티**임!! 

이제 저장 프로퍼티의 클래스와 구조체의 차이점? 에 대해 알아보자.

# **2-1. Class 클래스 인스턴스를 let/var으로 선언한다는 것은**

우리가 만약 sodeul이란 옵셔널 "**let** (**상수)**"로 **Human class 클래스 인스턴스**를 만들었음

```swift
**let** sodeul: Human? **=** .init()
```

---

위와 같이 만듬. 이때 선언 위치는 **지역변수**라고 가정 하겠음!!! 그러고 나서 내가 이 `sodeul`이란 인스턴

스를 통해 **저장 프로퍼티인 `name`과 `age`를 각각 변경** 해보겠음!!!

![https://blog.kakaocdn.net/dn/byiWB1/btqYmH2YpiR/BGi4aFHuZMModl2oeWTcF1/img.png](https://blog.kakaocdn.net/dn/byiWB1/btqYmH2YpiR/BGi4aFHuZMModl2oeWTcF1/img.png)

자, 그럼 **`name`이란 값 변경은 에러가 나고, `age`란 값 변경은 문제가 없는걸 확인할수있다.**

**엥? name은 상수니 초기화 이후로 변경을 못해서 에러가 나고,**

**age는 변수니 초기화 이후에 변경이 가능하니 당연한 거 아니예여??** 라고 하겠져?

그럼 나는 이렇게 의문을 던질 것임

**sodeul이란 인스턴스를 let, 즉 "상수"로 선언 했는데!!**

**어떻게 age란 저장 프로퍼티가 var라 해도 변경할 수 있을까요!!??**

메모리에 대해 좀 아시는 분이라면 왜 저런 질문을 던지나 의하할테고,

모르시는 분이면 혼란이 올 수 있음!!!

![https://blog.kakaocdn.net/dn/ckRZno/btqYnnJ0ppz/6zPkWfQQUh1xwK039DktfK/img.png](https://blog.kakaocdn.net/dn/ckRZno/btqYnnJ0ppz/6zPkWfQQUh1xwK039DktfK/img.png)

**클래스**는 **참조타입**이기 때문에, 메모리에 이렇게 저장됨 !!!!

**지역 상수 sodeul은 스택에 할당 되고 실제 Human 인스턴스는 힙에 할당 됨**

**스택에 있는 sodeul은 힙 영역에 있는 인스턴스를 참조하고 있는 형태임**

따라서 **sodeul 안엔 힙에 할당된 인스턴스의 주소값**이 들어가 있음

자 근데, 내가 sodeul이란 **클래스 인스턴스를 생성할 때 let으로 한단 것**은

![https://blog.kakaocdn.net/dn/UgnDa/btqYgFSEYMu/PD7noSjkRzyA3yhrR94Dp0/img.png](https://blog.kakaocdn.net/dn/UgnDa/btqYgFSEYMu/PD7noSjkRzyA3yhrR94Dp0/img.png)

**실제 힙 영역에 저장된 저장 프로퍼티 name, age와는 상관 없이**

**스택 영역에 저장된 sodeul 안의 주소값이 상수**로 설정되는 것임!!!!

상수니까 값을 바꿀 수 없어서 자물쇠 건 것으로 표현해 봤음!

따라서,

**클래스의 경우, 인스턴스 생성 당시 let으로 선언하든 var로 선언하든**

**클래스의 저장 프로퍼티에 접근하는 것엔 아무 영향을 주지 않음**

그럼 무엇에 영향을 줄까?

**sodeul이란 스택 상수는 0x11111111이란 인스턴스의 값을 가지고 있음**

**그럼 이제 이 sodeul이란 상수 안의 값을 변경하는 것에 영향을 줌!**

상수니까 값이 변경될 수 없으니

sodeul이란 상수가 **옵셔널 타입**이지만 **nil을 할당할 수도 없고**,

![https://blog.kakaocdn.net/dn/dyM0RH/btqYkbcdsox/zGe5MmCfdsydgYgainThr0/img.png](https://blog.kakaocdn.net/dn/dyM0RH/btqYkbcdsox/zGe5MmCfdsydgYgainThr0/img.png)

sodeul이란 상수에 **다른 Human Instance를 대입할 수도 없음**

(다른 인스턴스의 주소값을 가질 수 없으니!!)

![https://blog.kakaocdn.net/dn/mETRj/btqYgGjNW3t/xRHBc0PXKhFQx8ZWAsD48K/img.png](https://blog.kakaocdn.net/dn/mETRj/btqYgGjNW3t/xRHBc0PXKhFQx8ZWAsD48K/img.png)

아, 당연히 다음과 같이 **클래스** **인스턴스를 생성할 때 var로 한다면**

**var** sodeul: Human? **=** .init()

---

**(옵셔널 타입이면) nil도 할당 가능하고,**

![https://blog.kakaocdn.net/dn/mgGqX/btqYnncb0HE/Ry1AuJpUFfIhmlyD5ecTuK/img.png](https://blog.kakaocdn.net/dn/mgGqX/btqYnncb0HE/Ry1AuJpUFfIhmlyD5ecTuK/img.png)

**다른 Human Instance를 대입할 수도 있음!!!**

(name은 상수니 인스턴스 선언과 별개로 변경 불가능함~.~)

이것이 Class에서 인스턴스를 let 혹은 var로 선언하는 것의 차이임!!!

### **2-2. 구조체 인스턴스를 let/var으로 선언한다는 것은**

자, 우리가 만약 sodeul이란 옵셔널 "**상수**"로 **Person 구조체 인스턴스**를 만들었음

(아 Swift에선 클래스, 구조체 모두 인스턴스라 합니다!!:))

**let** sodeul: Person? **=** .init()

---

위와 같이!!!!!

그러고 나서 내가 이 sodeul이란 인스턴스를 통해

**저장 프로퍼티인 name과 age를 각각 변경** 해보겠음!!!

![https://blog.kakaocdn.net/dn/uHuGk/btqYo0gtEqO/tmhGduAx1LJl7CFdNx8cKK/img.png](https://blog.kakaocdn.net/dn/uHuGk/btqYo0gtEqO/tmhGduAx1LJl7CFdNx8cKK/img.png)

자, 이번엔 **name이란 상수도, age란 변수도 모두 값 변경을 하지 못한다며 에러가 발생**함

**엥? name은 상수니 초기화 이후로 변경을 못해서 에러가 나고,**

**age는 변수니 초기화 이후에 변경할 수 있는데 왜 에러가 나나염??????**

라고 의문을 품을 수 있음!! (안 품는다면 메모리 이해도가 높으시군요!)

일단 sodeul이란 구조체 인스턴스를 할당할 경우

![https://blog.kakaocdn.net/dn/qQF3a/btqYkaRYMks/u6HhJbHwodnlZOzUOHfMAk/img.png](https://blog.kakaocdn.net/dn/qQF3a/btqYkaRYMks/u6HhJbHwodnlZOzUOHfMAk/img.png)

**구조체**는 **값타입**이기 때문에, 메모리에 이렇게 저장됨!!!!

구조체의 저장 프로퍼티들도 모두 stack에 같이 올라간다는 것임

(클래스처럼 힙에 할당된 인스턴스를 참조하는 것이 아니기 때문에)

따라서, sodeul이란 **구조체 인스턴스를 생성할 때 let으로 한단 것**은

![https://blog.kakaocdn.net/dn/ccoYy6/btqYiBClHmE/b2GoVd1lk6Kzo4Dm4iRfyk/img.png](https://blog.kakaocdn.net/dn/ccoYy6/btqYiBClHmE/b2GoVd1lk6Kzo4Dm4iRfyk/img.png)

**name이 상수 저장 프로퍼티고, age가 변수 프로퍼티고 나발이고**

**구조체 인스턴스를 let으로 선언한 순간 구조체의 모~든 멤버를 변경할 수 없는 것임**

![https://blog.kakaocdn.net/dn/bAiImb/btqYmIgIYyc/9bwbw069J4VTHpZJKTsyc0/img.png](https://blog.kakaocdn.net/dn/bAiImb/btqYmIgIYyc/9bwbw069J4VTHpZJKTsyc0/img.png)

당연히 nil을 할당하는 것도 안 되겠지!! :)

다른 구조체 인스턴스를 할당받는 것 또한 안 되고!!!

당연히 **구조체 인스턴스 또한 var로 선언**하면

```swift
**var** sodeul: Person? **=** .init()
```

---

nil도 할당받을 수 있고, 다른 구조체 인스턴스를 할당받을 수 있고

![https://blog.kakaocdn.net/dn/cDdy4f/btqYiDfQ6kX/T1OrwOXBXjCWhzLMjXRFRk/img.png](https://blog.kakaocdn.net/dn/cDdy4f/btqYiDfQ6kX/T1OrwOXBXjCWhzLMjXRFRk/img.png)

또 var로 선언된 저장 프로퍼티의 값도 변경할 수 있음 :)

(name은 상수니 인스턴스 선언과 별개로 변경 불가능함~.~)

이것이 구조체에서 인스턴스를 let 혹은 var로 선언하는 것의 차이임!!!

이해가 됐기를!!!

## **3. 지연 저장 프로퍼티(Lazy Stored Property)**

**프로퍼티가 호출되기 전까지는 선언만 될 뿐 초기화되지 않고 있다가,**

**프로퍼티가 호출되는 순간에 초기화 되는 저장 프로퍼티**

호엥.. 먼말이냐고여?

만약 다음과 같은 Contacts를 저장 프로퍼티로 가지는 Human이란 클래스가 있음

```swift
class Contacts {
    var email: String = ""
    var address: String = ""
    init() {
        print("Contacts Init🐙")
    }
}

class Human {
    var name: String = "Unkonwn"
    var contacts: Contacts =.init()
}       
```

---

자, 이제 내가 Human이란 클래스 인스턴스를 만들고 초기화 했음

```swift
**let** sodeul: Human **=** .init()
```

---

여러분, 원래 클래스건 구조체건 **인스턴스를 생성할 경우**

**초기화 구문(initializer)이 불리는 순간 모~~든 프로퍼티가 초기화** 되어야 함!!!!

(기본값을 가지든, 생성자를 통해 초기화 하든!!!)

따라서 우리가 sodeul이란 인스턴스를 만들고 init을 호출한 순간,

**Human 인스턴스 내에 있는 모~~든 프로퍼티들이 초기화** 되며, **contacts란 프로퍼티도 초기화** 됨!!

따라서 다음과 같이 Contacts 클래스의 초기화 구문이 실행됨

![https://blog.kakaocdn.net/dn/bLN6gR/btqYkaLaukR/eu6Qnk1eoVvRbpayyFXHlK/img.png](https://blog.kakaocdn.net/dn/bLN6gR/btqYkaLaukR/eu6Qnk1eoVvRbpayyFXHlK/img.png)

이게 정상임!!!

근데 만약 내가 **contacts란 저장 프로퍼티 앞에** **lazy**란 키워드를 붙였음

```swift
class Contacts {
    var email: String = ""
    var address: String = ""
    init() {
        print("Contacts Init🐙")
    }
}

class Human {
    var name: String = "Unkonwn"
    lazy var contacts: Contacts =.init()
}       
```

---

이렇게!!! 그러고 다시

```swift
**let** sodeul: Human **=** .init()
```

---

이렇게 인스턴스를 생성하고 초기화 했음

그러면 말이지, 저 **Contacts Init 🐙이란 오징어 대가리가 더이상 나타나지 않음**

왜냐?? **lazy**! **지연되거든!**

선언만 됐을 뿐 contacts란 변수 자체가 초기화 되지 않는 것임

이제, 다음과 같이 내가 **contacts란 변수에 처음으로 접근**하고자 하면

```swift
sodeul.contacts.address **=** "none"
```

---

그럼 이제, **이때 contacts란 변수가 초기화** 되면서,

![https://blog.kakaocdn.net/dn/bLN6gR/btqYkaLaukR/eu6Qnk1eoVvRbpayyFXHlK/img.png](https://blog.kakaocdn.net/dn/bLN6gR/btqYkaLaukR/eu6Qnk1eoVvRbpayyFXHlK/img.png)

문어 대가리가 등장하는 것임!!!!!! 오케이!?????

### **3-1. 지연 저장 프로퍼티의 특징**

**인스턴스가 초기화와 상관 없이, 처음 사용될 때 개별적으로  초기화 된다**

이건 위에서 봤으니 패쓰

**따라서 항상 "변수"로 선언 되어야 한다**

![https://blog.kakaocdn.net/dn/dif9A9/btqYfWG72Mv/PCOmr1pYsZAt74tV4BgNK0/img.png](https://blog.kakaocdn.net/dn/dif9A9/btqYfWG72Mv/PCOmr1pYsZAt74tV4BgNK0/img.png)

머선 말이냐면, let으로 선언될 경우

우리가 필요한 시점에 초기화를 진행할 수 없기 때문임

(이미 메모리에 올라는 가 있지만, 필요한 시점에 내가 원하는 값으로 초기화 되어야 하니)

### **3-2. 지연 저장 프로퍼티는 어떨 때 쓸까?**

자, 만약 내가 미쳐갖고 Contacts란 클래스에 100,000개의 Element를 갖는

email이란 저장 프로퍼티를 선언 했음!

```swift
class Contacts {
    var email: [String] = .init(repeating: "", count: 100,000)
    var address: String = ""
    init() {
        print("Contacts Init 🐙")
    }
}

class User {
    let name: String = "unknown"
    lazy var contacts: Contacts = .init()
}
```

---

근데 Contacts에 대한 정보가 있는 유저도 있고, 없는 유저도 있을 거 아님???

만약 5,000명의 유저가 Contacts 정보를 제공하고,

5,000명의 유저가 Contacts 정보를 제공하지 않는다고 생각해보셈

**lazy로 선언하지 않는다면, Contacts란 인스턴스가**

**유저 수만큼 10,000개 생겨 버림**

(Human 인스턴스가 초기화 됨과 동시에 생성되니)

근데, **lazy로 선언할 경우 Contacts란 인스턴스가**

**contacts란 프로퍼티에 접근하는 유저 수인 5,000개 만큼만 생기고,**

**접근하지 않는 유저 수인 5,000개는 초기화 되지 않아 생기지 않을 것임**!!

따라서,

이럴 경우 lazy를 사용하면 메모리가 반으로 절약된다는 말!

예제가 좀 띠용스럽긴 한데..;;쨌든 이 lazy를 잘 사용할 경우

**성능도 향상되고, 메모리 낭비도 줄일 수 있다**

라고 함네다..........:)

## **4. 열거형에선 못 쓸까?**

![https://blog.kakaocdn.net/dn/wLfMe/btqYmHvnba6/VXYBG3q8AiSxcrFhCnwtQK/img.png](https://blog.kakaocdn.net/dn/wLfMe/btqYmHvnba6/VXYBG3q8AiSxcrFhCnwtQK/img.png)

**어림도 없지 피식.**

클래스 인스턴스와 구조체 인스턴스를 let/var로 선언할 경우 어떤 점이 다른지에 대해 실제 면접 질문으로 받았던 적이 있다!! 그러니 꼭 완벽히 숙지하시길!!!!
