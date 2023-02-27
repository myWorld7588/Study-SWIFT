Swift에서 Singleton은 클래스의 인스턴스화를 단일 인스턴스로 제한하고 해당 인스턴스에 대한 전역 액세스 지점을 제공하는 디자인 패턴입니다.

Singleton 패턴은 응용 프로그램에서 특정 클래스의 단일 인스턴스만 있도록 보장해야 할 때 유용합니다. 이는 데이터베이스 연결 또는 설정 관리자와 같이 여러 응용 프로그램 부분에서 공유해야 하는 리소스가 있는 경우에 자주 발생합니다.

Swift에서 Singleton을 만들기 위해서는 일반적으로 클래스의 단일 인스턴스를 보유하는 정적 속성과 클래스가 한 번만 인스턴스화될 수 있도록하는 비공개 이니셜라이저를 정의합니다. 다음은 Swift에서 Singleton의 예입니다.

```swift
csharpCopy code
class MySingleton {
    static let shared = MySingleton()

    private init() {}

    func doSomething() {
        // ...
    }
}

```

이 예에서 **`shared`** 속성은 **`MySingleton`** 클래스의 단일 인스턴스를 보유하는 정적 속성입니다. 비공개 **`init`** 메서드는 클래스가 한 번만 인스턴스화될 수 있도록 보장하고 **`doSomething()`** 메서드는 Singleton 인스턴스에서 작업을 수행하는 데 사용됩니다.

you simply use the **`shared`** property, like this:

```swift
scssCopy code
MySingleton.shared.doSomething()

```

</br>

# 1. Singleton Pattern 이란?

싱글톤 패턴이란, **특정 용도로 객체를 하나만 생성하여, 공용으로 사용하고 싶을 때 사용하는 디자인 유형이다.**

만약 다음과 같은 **User의 정보를 저장하는 클래스**를 만들었음

```swift
class UserInfo {
    var id: String?
    var password: String?
    var name: String?
}
```

---

그리고 **A ViewController**에선 **id**를, **B** **ViewController**에선 **password**를, **C** **ViewController**에선 **name**을 입력 받아

이를 **UserInfo라는 클래스**에 저장해야 한다고 생각해 보

```swift
//A ViewController
let userInfo = UserInfo()
userInfo.id = "sundae"

//B ViewController
let userInfo = UserInfo()
userInfo.password = "123"

//C ViewController
let userInfo = UserInfo()
userInfo.name = "sundae"
```

---

만약, 이런 식으로 A, B, C ViewController에서 **각각 UserInfo 객체를 만들어서 저장**하면,

![https://blog.kakaocdn.net/dn/b7DLbv/btqOYtTGZ4t/2HuCG2pgmg1TcJMkxhIne1/img.png](https://blog.kakaocdn.net/dn/b7DLbv/btqOYtTGZ4t/2HuCG2pgmg1TcJMkxhIne1/img.png)

이런 식으로 **각 Instance의 프로퍼티에만 저장**될 것이고 이는, 우리가 원하는 그림이 아닐 것이란 말임?

**한 Instance에 모든 정보가 저장**되어야 하는데...!! 그럼 방법이 하나 더 있긴 함

**인스턴스는 참조 타입**이기 때문에, **User Info 인스턴스를 한번 생성한 후**, 이 인스턴스를 A->B->C로 **필요할 때마다** **참조로 넘겨**

**줄 수도 있긴 함** 근데 이렇게 해도 되지만, App 어디 클래스든 User Info 인스턴스가 참조되어야 할 때마다

매번 이 인스턴스를 넘겨주기도 귀찮고...... 코드도 지저분해져... 따라서 **이 클래스에 대한 Instance는 최초 생성될 때 딱 한번만 생**

**성해서 전역에 두고, 그 이후로는 이 Instance만 접근 가능하게 하자** 는게 바로 **Singleton Pattern**임!

따라서 싱글톤을 사용하면 다음과 같은 그림이 됨

![https://blog.kakaocdn.net/dn/VmsQc/btqOYt0xgaU/k4fR7SVzSexrukeToKNAKk/img.png](https://blog.kakaocdn.net/dn/VmsQc/btqOYt0xgaU/k4fR7SVzSexrukeToKNAKk/img.png)

이런 식으로 **한 Instance에 어디 클래스에서든 접근 가능하게 하는 것**임!

# **2. Singleton Class 만드는 방법**

Swift에선 매우매우매우매우 간단하다

(Objective-C는 Dispatch_once 막 이런 거 썼었는데 Swift는 세상 간편하다)

</br>


# **2-1. static 프로퍼티로 Instance 생성하기**

```swift
class UserInfo {
    static let shared = UserInfo()

    var id: String?
    var password: String?
    var name: String?
}
```

---

먼저 전역으로 저장될 것이니,**static**을 이용해 **Instance를 저장할 프로퍼티를 하나 생성**해주셈

### **2-2. init 함수 접근제어자를 private로 지정하기**

```swift
class UserInfo {
    static let shared = UserInfo()

    var id: String?
    var password: String?
    var name: String?

    private init() { }
}
```

---

혹시라도 Init 함수를 호출해 **Instance를 또 생생하는 것을 막기 위해**, **`init()` 함수 접근 제어자를 private로 지정**해주면 됨 ㅎㅎ

</br>


# **3. Singleton Class 접근하는 방법**

아까 생성해뒀던 **static 프로퍼티**를 이용하면 됨

```swift
//A ViewController
let userInfo = UserInfo.shared
userInfo.id = "Sundae"

//B ViewController
let userInfo = UserInfo.shared
userInfo.password = "123"

//C ViewController
let userInfo = UserInfo.shared
userInfo.name = "Sundae"
```

---

어느 클래스에서든 **shared란 static 프로퍼티로 접근**하면, 하나의 Instance를 공유하는 것임 :)

</br>


# **4. Singleton의 장단점**

그럼 Singleton의 장점, 단점이 뭔지 알아보자!!!

</br>

# **5. Swift Singleton 개짱**

왜 Swift가 짱인지 알았다

---

**The free function dispatch_once is no longer available in Swift. In Swift, you can use lazily initialized globals or static properties and get the same thread-safety and called-once guarantees as dispatch_once provided.**

---

Objective-C에서 Singleton을 생성할 땐, **dispatch_once**를 이용해 단 한번만 불리게 하는 작업이 있음

왜냐??

Java나 다른 언어에서도 마찬가지지만, **Singleton을 생성하는 것**은 **Multi Threading 환경에서 Thread-Safe하지 않기 때문**임

무슨말이냐면, **여러 쓰레드**가 만약 **동시에 Singleton을 생성**하면, 경우에 따라 **Instance가 2개 3개 생성될 수도 있었음** 따라

서 **Objective-C**에서도, App이 실행되고 단 한번만 실행되게끔 **dispatch_once를 사용해서 다음과 같이 Singleton을 만들었음**

Swift는, 별도의 작업을 해주지 않더라도 static을 사용해 타입 프로퍼티로 인스턴스를 생성하면, **사용 시점에 초기화(lazy)** 되잖

음!? 따라서 **Singleton Instance가 최초 생성되기 전까진 메모리에 올라가지 않고, Dispatch_once도 자동 적용된다고 함!!!!**

따라서 별 코드 없이도 Instance가 여러 개 생성되지 않는, **Thread-Safe한 방법**이 되는 것임

# **6. iOS에선 Singleton을 언제 쓰냐면,**

```swift
let screen = UIScreen.main
let userDefault = UserDefaults.standard
let application = UIApplication.shared
let fileManager = FileManager.default
let notification = NotificationCenter.default
```
