# NOTION 에 더 자세히 ARC 메모리 관리에 대해.

Swift를 사용할 때 메모리 관리가 어떤 식으로 작동하는지에대해 배워보자.

# ARC 면접단골 질문 iOS 개발자라면 절대 몰라선 안되는 중요한 개념

# 1. 참조 (Reference) 타입과 Heap

ARC를 들어가기에 앞서 먼저 참조(Reference)와 힙(Heap)에 대해서 공부해볼 것임 왜 자꾸 힙에 집착하냐 묻는다면 **ARC가 메모리 영역 중 힙 영역을 관리**하는 것이기 떄문.

Swift는 힙에 메모리를 언제 할당한다!?

**인스턴스, 클로저 등등 참조 타입(Reference Type)은 자동으로 힙에 할당 한다!**

그래서 우리가 힙을 직접 건들이지 않았어도 자동으로 할당하여 쓰고 있던 것이다.

힙에 할당된다는 것이 말로하면 잘 이해하기 힘들어서 실제 참조 타입을 선언할 경우 메모리가 어떤 식으로 할당되는지 예제를 통해 봅시다.

```swift
class Cat {
    var name: String?
    var age: Int?
    
    init(name: String?, age: Int?) {
        self.name = name
        self.age = age
    }
}
 
let sundae = Cat(name: "Sundae", age: 1)
```

위 코드처럼 Cat 이란 클래스가 있고 Sundae 이라는 인스턴스를 생성하고 값을 초기화 했다.

포스팅을 위해 예제에선 Sundae가 전역 변수가 됐지만, 보통 우리가 개발할 때 그렇듯, 그냥 어디 클래스에서 생성된 **지역 변수라고 생각 해주셈**!! 그럼 메모리엔 어떻게 저장 되냐면

이렇게 저장 됨

- 스택 영역: sundae (0x1111111)
- 힙 영역: Cat Instance, name: “Sundae”, age: 1

**지역 변수 sodeul은 스택에 할당 되고**

**실제 Cat 인스턴스는 힙에 할당 됨**

스택에 있는 sundae는 힙 영역에 있는 **인스턴스를** **참조**하고 있는 형태이다. 

따라서 sundae **안엔 힙에 할당된 인스턴스의 주소값**이 들어가 있다.

참조(Reference) 이기 때문에 당연히 다음과 같은 작업을 해주면

```swift
let clone = sundae
```

우리는 인스턴스가 복사되지 않는 것은 알고 있다.  

실제 메모리는 아래처럼 **같은 힙 영역의 인스턴스를 가리키고 있을꺼다.**

- 스택 영역: sundae (0x1111111)
- 스택 영역: clone (0x111111)
- 힙 영역: Cat Instance, name: “Sundae”, age: 1

여기까진 어찌보면 Reference에 대한 기본 중에 기본 개념이다.  

그런데 힙이라는 것이 특징 중 하나는

## 사용하고 난 후에는 반드시 메모리 해제를 해줘야 한다.

라는 것이었다.  메모리를 해제하기 위해선 release, free 등등 방법이 있는데

우린 지금껏 인스턴스를 마음대로 할당하고 사용해 왔지만 단 한번도 저런 함수들을 통해 인스턴스를 직접 메모리에서 해제해 준 적이 없다… ??

그럼, 스택에 있는 cat, clone이 함수 들이 종료 시점에 사라지고 나면

힙에 남은 쓰이지도 않는 인스턴스는 누가 메모리 해제 해준다는 말인가?

메모리 누수 Leak 으로 이어진단 말인가?  **아.니.다.!  ARC가 해제 해준다 ㅎㅎ**

</br>

# 2. ARC 란 무엇일까?

Swift uses *Automatic Reference Counting* (ARC) to track and manage your app’s memory usage. In most cases, this means that memory management “just works” in Swift, and you don’t need to think about memory management yourself. **ARC automatically frees up the memory used by class instances when those instances are no longer needed.**

**ARC는 클래스 인스턴스가 더 이상 필요하지 않을 때 메모리를 자동으로 해제한다** 라는 내용이다.

이게 핵심!!! 한마디로 **우린 지금껏 힙에 메모리를 자동 할당하며 사용해 왔지만 손수 메모리를 해제해주지 않아도 됐던 이유는 ARC라는 놈이 메모리를 자동으로 해제해주기 때문이다!**

최종적으로 ARC에 대해 정의를 내리자면, 

**ARC란, 힙에 할당된 인스턴스의 메모리를 알아서 관리해주는 놈이다.**

</br>

# 2-1. GC vs RC

힙 영역의 메모리를 관리하는 방법은 GC와 RC가 있다.  

가장 유명한 자바의 **GC(Garbage Collection)** 과 비교해 보자. (ARC는 RC에 포함됨)

자바의 GC와 스위프트의 RC의 가장 큰 차이점은 **참조를 계산하는 시점**에 있다

</br>

**Garbage Collection**
* 참조 계산 시점: Run Time ▪ 어플 실행 동안 주기적으로 참조를 추적하여 사용하지 않는 instance를 해제함
* 장점: 인스턴스가 해제될 확률이 높음(RC에 비해)
* 단점: 개발자가 참조 해제 시점을 파악할 수 없음▪ RunTime 시점에 계속 추적하는 추가 리소스가필요하여 성능저하 발생될 수 있음

</br>

**Reference Collection**
* 참조 계산 시점: Compile Time 컴파일 시점에 언제 참조되고 해제되는지 결정되어 런타임 때 그대로 실행됨
* 장점: 개발자가 참조 해제 시점을 파악할 수 있음▪ RunTime 시점에 추가 리소스가 발생하지 않음
* 단점: 순환 참조가 발생 시 영구적으로메모리가 해제되지 않을 수 있음

</br>


**일단 참조를 계산하는 시점이 언제인지, 장단점이 무엇인지 정도만 알아둡시다.**

</br>

# 3. ARC의 메모리 관리 방법

Reference Count **(RC)** 를 이용하여 관리한다.

**메모리의 참조 횟수(RC)를 계산하여, 참조 횟수가 0이 되면 더 이상 사용하지 않는 메모리라 생각하여 해제함** 다시 말해 **RC**는 **이 인스턴스를 현재 누가 가리키고 있느냐 없느냐(참조하냐 안하냐)를**

**숫자로 나타낸 것**

만약 참조 횟수가 10이라면 해당 인스턴스가 10군데에서 참조되고 있단 뜻이고, 참조 횟수가 0이라면 아무데서도 참조되지 않으니 필요없다! 메모리 해제해라! 란 뜻

따라서!!! **모 - 든 인스턴스는 자신의 RC 값을 가지고 있다**

(인스턴스가 생성될 때 힙에 같이 저장된다고 함)

그럼 이 RC는 어떤 기준으로, 어떻게 셀까!!??

예제를 통해 보자 확인해보자~!

</br>

# 3-1. 참조 횟수 Count Up (+)

참조 횟수가 +1이 되는 순간은 **인스턴스의 주소값을 변수에 할당할때** 이다. 

쉽게 두가지로 나눠서 설명 ㄱㄱ

## ****① 인스턴스를 새로 생성할 때****

```swift
let sundae = Cat(name: "Sundae", age: 1)
```

아까 살펴봤던 이 코드는 실행되는 시점에 지역 변수 sundae는 스택에 할당되고 실제 인스턴스는 힙에 할당된다 했잖슴? 그리고 sundae엔 힙에 할당된 인스턴스의 주소값이 들어간다 했음

이렇게

- 스택 영역: sundae (0x1111111)
- 힙 영역: Cat Instance, name: “Sundae”, age: 1
- 

**인스턴스를 새로 생성할 때 (새로운 변수에 대입할 때) 해당 인스턴스에 대한 RC가 증가함**


## ****② 기존 인스턴스를 다른 변수에 대입할 때****

```swift
let clone = sundae
```

**기존 인스턴스를 다른 변수에 대입할 때도** 당연히 참조에 의하기 때문에 **RC 값이 증가됨**


clone 이란 변수에 0x111111 이란 인스턴스의 주소값이 대입 되었으니 RC값이 증가된 모습

</br>


# 3-2. 참조 횟수 Count Down (-)

그럼, 참조 횟수는 언제 내려갈까? 크게 4가지 경우로 설명 할수있다.

## ****① 인스턴스를 가리키던 변수가 메모리에서 해제되었을 때****

## ****② nil이 지정되었을 때****

## ****③ 변수에 다른 값을 대입한 경우****

## ****④ 프로퍼티의 경우, 속해 있는 클래스 인스턴스가 메모리에서 해제될 때****

