iOS 개발자라면 꼭 알아야 하는 중요한 개념인, strong, weak, unowned, 순환참조 에 대해 알아보자

# 1. strong (강한 참조)

이번에 알아볼 것은 강한 참조에 관한 것임. 먼저, 이전 포스팅에서 **ARC**라는 것은 **참조 횟수를** 

**계산하여 0이 되는 시점에 힙에서 자동으로 해제**하는 것이라 했음 그러면서 다음 예로 들었었음

```swift
class Human {
    var name: String?
    var age: Int?
    
    init(name: String?, age: Int?) {
        self.name = name
        self.age = age
    }
}
 
let sodeul = Human(name: "Sodeul", age: 26)
```

---

이렇게 sodeul이라는 변수에 인스턴스를 생성하면

(포스팅을 위해 예제에선 sodeul이 전역 변수가 됐지만,

보통 우리가 개발할 때 그렇듯, 그냥 어디 클래스에서 생성된 **지역 변수라고 생각 해주셈**!!!!)

![https://blog.kakaocdn.net/dn/ca8mix/btqUl93cMVf/Ez0dymHQ03RXWg0bDga9D0/img.png](https://blog.kakaocdn.net/dn/ca8mix/btqUl93cMVf/Ez0dymHQ03RXWg0bDga9D0/img.png)

이런 식으로 인스턴스는 힙 영역에 할당되고, sodeul이란 지역 변수가 Human 인스턴스의 주소값을 할당받기 때문에 Human 인스턴스의 RC가 +1 된다고 했음 ARC를 배웠으니 이는 당연해 보여야 함

근데 이 **당연한 것이 바로 strong, 강한 참조**임!!! **인스턴스의 주소값이 변수에 할당될 때, RC가 증가하면 강한 참조(strong)** 인 것임 우리가 지금껏 자연스럽게 인스턴스를 생성하고 사용하던 것이

모두다 strong이자 강한 참조였던 것임 근데 난 strong으로 선언한 적 없는뎅!?ㅎ_ㅎ  하겠지만

별다른 선언이 없다면 **default 값이 strong** 임 ㅎㅎㅎㅎ

따라서 위에서 sodeul로 생성한 인스턴스는 default인 s**trong**으로 선언되어 '**강한 참조**' 객체인 것.

하지만 이 strong 에도 문제가 있었으니... 그것은 바로.. 순환참조..

# 2. 순환참조

## **2-1. 순환참조란?**

ARC를 소개할 때 ARC의 단점에 대해 뭐라 설명 했었냐면

## 순환 참조가 발생 시 영구적으로 메모리가 해제되지 않을 수 있슴.

라고 설명했다. 이제 이 순환 참조에 대해 알아 봅시다.. !

```swift
class Man {
    var name: String
    var girlfriend: Woman?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Man Deinit!") }
}
 
class Woman {
    var name: String
    var boyfriend: Man?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Woman Deinit!") }
}
```

---

여기 **Man**과 **Woman**이란 클래스가 있음 여기서 중요한 것은

- **Man에게는 Woman 타입의 프로퍼티(girlfriend)가 있고,**
- **Woman에게는 Man 타입의 프로퍼티(boyfriend)가 있단 것임**

남자친구와 여자친구는 존재 할 수도 있고 그냥 없어요일 수 있으니 옵셔널 타입임

쨌든 자 이제 우린 다음과 같이 인스턴스를 생성할 것임

```swift
var chlosu: Man? = .init(name: "철수")
var yeonghee: Woman? = .init(name" "영희")
```

---

이렇게 생성한 경우, 메모리에는 다음과 같이 할당 됨

![https://blog.kakaocdn.net/dn/mg2oe/btqUeV5TXAl/kEEXpep5RukTjObQZkKNW0/img.png](https://blog.kakaocdn.net/dn/mg2oe/btqUeV5TXAl/kEEXpep5RukTjObQZkKNW0/img.png)

자 여기까진 문제가 없어 보임 근데 만약 **철수와 영희가 사귀기로 했음** 💜

**따라서 다음 코드를 작성 하면**

```swift
chelosu?.girlfriend = yeonghee
yeonghee?.boyfriend = chelosu
```

---

**메모리는 다음과 같은 모양이 됨**

![https://blog.kakaocdn.net/dn/mI6zs/btqUl9B21Wy/DJxKw9qgpif4yZVN2Phqf0/img.png](https://blog.kakaocdn.net/dn/mI6zs/btqUl9B21Wy/DJxKw9qgpif4yZVN2Phqf0/img.png)

뭔가 달라진 게 보임!? **Man 인스턴스와 Woman 인스턴스의 RC가 각각 1씩 증가 했음**

이유라 함은 **boyFriend 프로퍼티엔 Man 인스턴스의 주소값을 할당하고,**

**girlFriend 프로퍼티엔 Woman 인스턴스의 주소값을 할당함** 또한 girlFriend, boyFriend 프로퍼티는 **기본 값 strong**이기 때문에 **서로의 RC 값이 1씩 증가**해버린 것임

이렇게 철수 인스턴스의 프로퍼티는 영희를 참조하고, 영희 인스턴스의 프로퍼티는 철수를 참조하는 것 처럼 **두 개의 객체가 서로가 서로를 참조하고 있는 형태를 순환 참조** 라고 함

근데 strong의 문제점이 순환 참조라 했는데, 어떤 문제점이 있을까?

## **2-2. 순환 참조의 문제점**

자 우린 지금까지 철수, 영희 변수의 인스턴스를 만들어서 갖고 놀았음 근데 이제 질려버린 것임 재미없엉 철수와 영희를 kill 하기로 결정 따라서 다음과 같이 nil을 지정했음

```swift
chelosu **= nil**
yeonghee **= nil**
```

---

그러면 저번 포스팅에서 배웠던 대로 **인스턴스를 가리키던 변수에 nil을 대입했으니 RC가 1만큼 감**

**소하고 0일 경우 메모리에서 해제 되어야 함**

따라서 우리가 원하는 시나리오는 철수, 영희의 인스턴스를 다른 변수에 대입한 적도 없고,

철수, 영희를 가리키던 변수에 **nil을 할당** 했으니, **Reference Count가 0**이 되어 **철수와 영희가 가리키**

**던 인스턴스가 힙에서 각각 해제되어야 하는 시나리오**임 

그런데…. 위와 같이 해줄 경우 Man, Woman 클래스의 **deinit 메서드가 호출되지 않음** deinit 함수는 

인스턴스가 메모리에서 해제될 때 실행되는 메서드임 

(만약 진짜 deinit 메서드가 호출되지 않는지 궁금하면 위 예제를 직접 돌려보셈)

그말인 즉, **철수와 영희가 가리키던 인스턴스가 힙에서 사라지지 않고 계속 메모리를 먹고 있단 말임**

먼저, 이유를 알기 위해선 철수와 영희 변수에 nil을 대입했을 시점의 메모리 상태를 보면 됨

![https://blog.kakaocdn.net/dn/cHFpAZ/btqUtWClpkr/OD9V3ZnmWhF4uUqzECY4q0/img.png](https://blog.kakaocdn.net/dn/cHFpAZ/btqUtWClpkr/OD9V3ZnmWhF4uUqzECY4q0/img.png)

자 천천히 자세히 잘 보면…

**먼저 철수와 영희 변수에 nil을 대입한 순간, 각각 가리키던 인스턴스의 RC 값을 1씩 감소 시킴**

그런데 **힙에 있는 Man&Woman 인스턴스들의  RC는 0이 아니고 1임**

왜냐?

**순환참조 때문에 아까 서로의 RC가 1씩 증가 됐기때문이다.**

따라서 RC가 0이 아니기 때문에 해제되지 않고 계속 힙에 남아 있는 것이다.

이렇듯, **strong으로 선언된 변수들이 순환참조 됐을 시** 큰 문제점은 **서로가 서로를 참조하고 있어서** 

**RC가 0이 되지 못한다는 것**임 심지어 **해당 인스턴스를 가리키던 변수(철수, 영희)도 nil이 지정됐기** 

**때문에** 인스턴스에 접근 할 수 있는 방법도 없어 **메모리 해제도 못함** 따라서 어플이 죽기 전까진 

**memory leak이 계속 발생**하는 것임 이렇게 **strong을 사용해서 순환참조에 문제가 생긴 경우**를

**강한 순환 참조** 라고한다. 

이제 strong을 사용할 경우 발생하는 강한 순환 참조의 문제점에 대해 우린 이해했다.

그럼 해결은 어떻게 할까?

# 3. weak, unowned

강한 순환 참조를 해결하기 위해선 weak와 unowned를 사용하면 됨!!

## **3-1. weak (약한 참조) 란?**

- **인스턴스를 참조할 시, RC를 증가시키지 않는다**
- **참조하던 인스턴스가 메모리에서 해제된 경우, 자동으로 nil이 할당되어 메모리가 해제된다**

이 두가지가 핵심임 **프로퍼티를 선언한 이후, 나중에 nil이 할당**된다는 관점으로 보아 

**weak는 무조건 옵셔널 타입의 변수 여야함**

아까 위 강한 순환 참조의 예제를 다시 들어보겠음 강한 순환 참조는 서로가 서로를 강하게 참조하고 있던 것이 문제였으니 **둘 중 한 쪽을 weak로 선언해 주는 것임**

(지금은 **철수와 영희의 수명이 동일하기 때문에 아무나 weak로 선언**해 주지만, 수명이 다를 경우 어떤 쪽을 weak로 선언해야 하는지는 나중에 보겠음)

```swift
class Man {
    var name: String
    **weak** var girlfriend: Woman?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Man Deinit!") }
}
 
class Woman {
    var name: String
    var boyfriend: Man?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Woman Deinit!") }
}
```

---

weak를 선언하는 방법은 간단함 **순환 참조를 일으키는 프로퍼티 앞에 weak**를 붙여주시면 됨

자 그럼 아까 강한 순환 참조를 일으켰던 코드를 다시 실행해 보자

```swift
var chelosu: Man? = .init(name: "철수")
var yeonghee: Woman? = .init(name: "영희")

chelosu?.girlfriend = yeonghee
yeonghee?.boyfriend = chelosu
```

---

자 이렇게 weak로 선언한 경우, **RC 가 증가하지 않는댔음**!!!!!

따라서 메모리는 다음과 같이 그려짐

![https://blog.kakaocdn.net/dn/Nry76/btqT910dwBm/q2VL1kUdALzryINSnnY6tK/img.png](https://blog.kakaocdn.net/dn/Nry76/btqT910dwBm/q2VL1kUdALzryINSnnY6tK/img.png)

**girlfriend의 프로퍼티가 weak이기 때문에 Woman Instance의 주소값을 할당받지만(참조하지만),** 

**Woman Instance의 RC의 값은 건들지 않음** 

따라서 위와 같은 그림이 나오는 것임 자 그럼, 이게 어떻게 순환 참조를 해결해주는지 다시 철수와 

영희를 kill 해보겠음

```swift
chelosu = nil
yeonghee = nil
```

---

순서대로 설명 하겠음

### **① 철수, 영희 변수는 nil이 할당된 순간 각각의 인스턴스에 대한 RC를 1씩 감소한다**

![https://blog.kakaocdn.net/dn/COU1t/btqUhAH8Ro3/gqi2IZOXb3FEjtmjPX9gaK/img.png](https://blog.kakaocdn.net/dn/COU1t/btqUhAH8Ro3/gqi2IZOXb3FEjtmjPX9gaK/img.png)

### **② RC가 0이 된 Woman Instance가 메모리에서 해제된다**

**이때, RC Count Down 방식에 따라 boyfriend의 RC도 -1 감소한다**

![https://blog.kakaocdn.net/dn/P9GjA/btqUpag3Ozu/fJ4IyBRG6NeGg9wJOonnMK/img.png](https://blog.kakaocdn.net/dn/P9GjA/btqUpag3Ozu/fJ4IyBRG6NeGg9wJOonnMK/img.png)

**어떤 인스턴스(Woman Instance)의 프로퍼티(boyfriend)**가

**다른 인스턴스(Man Instance)**를 가리키고 있을 때,

**그 프로퍼티(boyfriend)가 속한 인스턴스(Woman Instance)가 메모리에서 해제**되면

**그 프로퍼티(boyfriend)가 가리키고 있던 인스턴스(Man Instance)의 RC가 -1 감소**한다)

만약 이 부분이 이해가 안 간다면, 이전 포스팅 ARC에 대해 다시 공부

### **③ weak로 선언된 girlfriend가 참조하던 인스턴스가 메모리에서 해제 되었으니, girlfriend의 값이 nil로 할당된다**

(weak의 특징! 가리키던 인스턴스가 메모리에서 해제될 경우 nil이 할당된다!)

![https://blog.kakaocdn.net/dn/8fBhS/btqUk0T1TBu/kwrinbJrwdmY7TixPiWwak/img.png](https://blog.kakaocdn.net/dn/8fBhS/btqUk0T1TBu/kwrinbJrwdmY7TixPiWwak/img.png)

### **④ Man Instance의 RC 값도 0이 되었으니 메모리에서 해제한다**

![https://blog.kakaocdn.net/dn/d7LA6x/btqUmKb9t0p/3Tg20gS70iU6kOkKWYS77K/img.png](https://blog.kakaocdn.net/dn/d7LA6x/btqUmKb9t0p/3Tg20gS70iU6kOkKWYS77K/img.png)

### **⑤ deinit 함수도 정상 작동 한다**

![https://blog.kakaocdn.net/dn/cpMTyC/btqIl9gkC72/dvU8WlsmTZvwh3vvGdvc5k/img.png](https://blog.kakaocdn.net/dn/cpMTyC/btqIl9gkC72/dvU8WlsmTZvwh3vvGdvc5k/img.png)

깨끗.. 편 - 안...이것이 weak이자, 순환 참조를 해결하는 첫 번째 방법이다.

이렇게 **순환 참조이지만 weak로 선언되어 RC 값을 올리지 않는 것**을 **약한 순환 참조** 라고 한다.

> **그럼 철수와 영희 중에 누구든 weak로 선언해도 상관 없나!?**
> 

→ 예제에선 철수와 영희의 수명이 "동일"하기 때문에 아무나 weak로 선언 했지만 강한 순환 참조가 난 경우, **둘 중에 수명이 더 짧은 인스턴스를 가리키는 애를 약한 참조로 선언함**

철수가 먼저 죽는다 -> 영희의 boyfriend가 nil이 될 수 있다 -> 영희의 boyfriend를 weak로 선언한다

영희가 먼저 죽는다 -> 철수의 girlfriend가 nil이 될 수 있다 -> 철수의 girlfriend를 weak로 선언한다

### **3-2. unowned (미소유 참조)**

먼저 unowned와 weak는 공통점이 많다.

- **강한 순환 참조를 해결할 수 있다.**
- **RC 값을 증가시키지 않는다.**

여기까진 weak랑 동일함 근데 차이점이 뭐냐? **unowned**은

**인스턴스를 참조하는 도중에 해당 인스턴스가 메모리에서 사라질 일이 없다고 확신**

하는 것이 핵심임 따라서 **참조하던 인스턴스가 만약 메모리에서 해제된 경우,**

**nil을 할당받지 못하고 해제된 메모리 주소값을 계속 들고 있다.**

너무 위험하지 않음? 약간 옵셔널 강제 해제 연산과도 같은 느낌임 따라서

**unowned으로 선언된 변수가 가리키던 인스턴스가 메모리에서 먼저 해제된 경우, 접근하려 하면 에러를 발생시킴**

```swift
class Man {
    var name: String
    unowned var girlfriend: Woman?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Man Deinit!") }
}
 
class Woman {
    var name: String
    var boyfriend: Man?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Woman Deinit!") }
}
```

---

**철수보다 "영희가 더 오래 산다는 가정"하에**

이번엔 철수 인스턴스의 girlfriend 프로퍼티를 **unowned**로 선언 했음

근데 이렇게 선언한 메모리에 올라가는 방식은 weak와 똑.같.다

![https://blog.kakaocdn.net/dn/cpcoV5/btqUf0eUD97/DHmiFAlu5yJh1WxmsoWAFk/img.png](https://blog.kakaocdn.net/dn/cpcoV5/btqUf0eUD97/DHmiFAlu5yJh1WxmsoWAFk/img.png)

마찬가지로 RC를 건들지 않고 참조됨 이 외에 **메모리 해제하고 하는 등 전체적인 동작이 weak와 똑같음.**  근데!!!! 어떤 제약이 붙냐면 위에서 가정 했듯이,

**unowned가 붙은 철수의 grilfriend가 가리키는 영희(Woman) 인스턴스는,**

**철수(Man) 인스턴스가 메모리에서 해제되기 전까진 절대 절대 먼저 해제되어선 안됨**

(철수가 먼저 죽고 난 후에 영희가 죽어야 함) 이 부분이 weak와 다른 부분임

만약 **영희의 인스턴스가 철수의 인스턴스보다 먼저 메모리에서 해제**되자나!?!?

```swift
yeonghee **= nil**
```

---

**weak의 경우엔 자동으로 girlfriend의 값이 nil로 지정** 되겠지만,

**unowned의 경우 nil을 할당받지 못해 이미 해제된 메모리 주소값을 계속 들고 있음**

![https://blog.kakaocdn.net/dn/cTb8Ud/btqUhBlCtsr/U5KFqmFSXyihAPeQPiiMAk/img.png](https://blog.kakaocdn.net/dn/cTb8Ud/btqUhBlCtsr/U5KFqmFSXyihAPeQPiiMAk/img.png)

이런 식으로...!!! 따라서 **weak** 경우, 철수의 girlfriend 프로퍼티에 접근하면

```swift
cheolsu?.girlfriend         // nil
```

위와 같이 nil이 지정되어 있으나.. 

**unowned**의 경우엔

```swift
cheolsu?girlfriend       // Error!: execution was interrupted, reason: signal SIGABRT
```

**이미 메모리에서 해제된 포인터 값에 접근하려 해서 에러가 발생함** 이것이 바로  

weak와 unowned의 차이점임

**unowned는 에러를 발생시킬 위험이 있어서 웬만해선 weak를 사용하는 것을 권장함**

> **그럼 철수와 영희 중에 누구든 unowned로 선언해도 상관 없나!?**
> 

→ 아무나 할 순 있지만, 강한 순환 참조가 난 경우 **weak와 반대로 둘 중에 수명이 더 긴 인스턴스를 가리키는 애를 미소유 참조로 선언함**

철수가 먼저 죽는다 -> 영희의 수명이 더 길다 -> 수명이 더 긴 영희를 가리키는 철수의 girlfriend를 unowned로 선언한다

영희가 먼저 죽는다 -> 철수의 수명이 더 길다 -> 수명이 더 긴 철수를 가리키는 영희의 boyfriend를 unowned로 선언한다

근데 인생사 철수가 먼저 죽을지 영희가 먼저 죽을지 모르잖음? 철수가 오랜 지병을 앓았지만 영희

가 갑자기 비명횡사 할 수도 있고.. 이런 경우엔 unowned를 사용하는 게 위험하다... unowned의 경우 

내가 가리키는 인스턴스가 **먼저 메모리에서 해제될 일은 일어나지 않는다!!!(나보다 수명이 길거나** 

**같다!!)** 라고 판단되는 경우에 쓰면 되는데.... unowned를 사용하는 경우는 [5. Closure 클로저 3/3](https://www.notion.so/5-Closure-3-3-9c07f2e2bfcb42a6885b411389880a2c) 

에 더 추 가 했으니, 이 포스팅을 참조(만) 하길..... + 아 그리고 원래 unowned는 가리키던 메모리가 

해제돼도 nil을 할당받지 않아서 **Non-Optional Type으로 선언**해야 했지만 **Swift 5.0부턴 Optional** 

**Type으로도 선언 가능**함!!

## **3. 정리**
