# **1. 튜플이란?**

자 튜플이라 함은 무엇이냐

### *(expr1, exp2, ... )* 이렇게 괄호 () 안에 표현식이 원하는 만큼 들어가있는 것임

```swift
let a = (12, 34)
```

자 이런 식으로 괄호 안에 원하는 타입을 나열해서 쓰는 것이 튜플임

타입을 다음과 같이 막 섞어섞어 써도 됨

```swift
let sundae = ("sundae", 1, 17.24)
```

이렇게 ‘sundae’ 에 대한 값들을 **Int, String, Double 자료형을 마음대로 섞어서 튜플로** 나타낼 수 있음 이러면 sundae 의 자료형은

```swift
type(of: sundae)    // (String, Int, Double).Type
```

### **내가 지정한 타입대로 됨, 근데 튜플도 몇 가지 규칙이 있음**

## **1-1. 튜플에 저장된 값에 접근 하려면 .(dot) 문법을 사용한다**

```swift
let sundae = ("sundae", 1, 17.24)
```

자 이 튜플의 값에 접근하기 위해선 **.(dot)**을 이용해서 **index**로 접근 가능함

```swift
sundae.0    // "sundae"
sundae.1    // 26
sundae.2    // 17.24
```

## **1-2. let으로 선언하면 Immutable Tuple, var로 선언하면 Mutable Tuple**

자 아까 튜플에서 멤버 값을 바꿔 보겠음

```swift
sundae.0 = "uhmook" // Error! cannot assisn o property:
```

그럼 에러가 남 당연함 sundae는 **let으로 선언된 상수**이기 때문에 당연히 튜플 값도 상수로 저장됨 (Immutable Tuple) var로 선언하면

```swift
var sundae = ("sundae", 1, 17.24)
sundae.0 = "uhmook" // 수정 가능 >< (Mutable Tuple)
```

## **1-3. 튜플을 선언한 후엔 자료형 및 멤버의 갯수는 수정할 수 없다**

먼가 튜플이 아니라 **어느 자료형에도 해당하는 말**인데 괜히 튜플이라 하니까 헷갈릴까봐 정리해둠

자 아까 선언했던 튜플을 보겠음

```swift
let sundae = ("sundae", 1, 17.24)
type(of: sundae)    // (String, Int, Double).Type

// 이 자료형은 String, Int, Double임. 근데 1번째 인덱스에 Int 값이 아닌 String 값을 대입하려
// 하면 에러가 남. Int Type 인데 왜 String 을 대입하냐며 에러나는 거임.

sundae.1 = **"one" // Error!: Cannot assign value of type 'String' to type 'Int'**
```

어쨌든 튜플로 선언한 것도 하나의 자료형이기 때문에 **선언 이후에 자료형 및 멤버의 갯수를 바꿀 수 없음**

.

# **2. Named Tuple**

### *(name: expr1, name2: exp2, ... )* 멤버 앞에 이런 식으로 붙여주면 됨)

```swift
// Named Tuple
var sundae = (name: "sundae", age: 1, weight: 17.24)

// 접근
sundae.name     // "sundae"
sundae.age      // 1
sundae.weight   // 17.24
```

index가 아닌 **이름으로 접근 가능해짐!!** 가독성이 훨씬 높아졌음. 아주 편리한 자료형임.

# **3. Tuple Decomposition**

이번엔 튜플을 분해 하는 방법에 대해 알아보겠음.  자 다시 다음과 같이 튜플이 있을 때

```swift
// Named Tuple
var sundae = (name: "sundae", age: 1, weight: 17.24)

**// sundae라는 튜플의 멤버를 각가의 상수에 저장하고 싶다면?**
let name = sundae.name
let age = sundae.age
let weight = sundae.weight

```

근데 이렇게 하는 건 튜플 멤버의 갯수가 많아지면 그만큼 지저분해지고

코드도 더러워지고 별로. 따라서 **Decomposition 문법**을 쓰면 다음과 같이 쉽게 분해할 수 있음

```swift
let (name, age, weight) = sundae
```

**차례대로 상수 name, age, height 3개를 생성하고 튜플 멤버 값을 순서대로 저장하는 것임**

바로 위의 name, age, hieght를 직접 선언하여 대입해줬던 코드와 결과적으로 같은 코드이다.

**하지만 매우매우 간단해짐, 근데 간단한 만큼 주의할 점은 있음**

# **3-1. 튜플의 갯수와 지정할 상수의 갯수는 동일해야 한다**

먼말이냐면 튜플 sundae의 멤버 값이 3개잖음??? 그럼 무조건 

```swift
var sundae = (name: "sundae", age: 1, weight: 17.24)
let (name, age, weight) = sundae // 이렇게 값이 3개
```

이 값도 3개가 와야 함 **만약 3개 보다 부족하거나 많다면 에러남.**

그럼 만약 나는 name, age라는 두 개의 상수만 생성하고 튜플 값을 넣고 싶으면 어떡함!???

쓸데없는 height라는 상수도 만들어서 바인딩 해야됨 ? ㅠ_ㅠ

아니~~~ 그땐 wildcard pattern을 이용하면 됨!!!!

# **3-2. 상수를 생략하고 싶다면 wildcard pattern을 이용한다**

wildcard pattern이 머냐면 가끔 표현식을 사용하지 않거나 생략할 때 **_ (언더바)** 많이 쓰잖음?

머 제일 대표적으로 for in 구문 쓸 때 쓰는 거

```swift
for _ in array { } // 막 이런거
```

이게 바로 wildcard pattern 임 쨌든 튜플도 마찬가지로 **wildcard pattern 을 이용해서 필요 없는 멤버는 생략**하고 바인딩 할 수 있음

```swift
let (name, age, _) = sundae // 이런식으로
```

요런식으루 wildcard pattern을 이용하면 된다

# **4. Tuple Matching**

튜플이 빛을 발할때는 바로 **Switch 문** 을 사용할 때이다. 

왜냐면  **Switch 문은 Tuple Matching을 지원하기때문이다.**

# **4-1. Tuple Matching을 지원하는 Switch 문**

자 다음과 같은 튜플이 있을 때 다음과 같이 Switch 문의 조건을 줄 수 있음

```swift
// Tuple
let resolution = (3840.0, 2160.0)

// switch 조건문
switch resolution {
    case (3840, 2160):
        print("4K")
    default:
        break
}
```

case 조건을 위처럼 Tuple 자료형으로 설정하는 것임 따라서 resolution은 자기와 같은 튜플 조건문을 가진 case 문을 실행하게 되는 것임

# **4-2. Interval Matching도 가능하다**

```swift
// Interval Matching도 가능함

switch resolution {
    case (3840...4096, 2160):
        print("4K")
    default:
        break
}
```

# **4-3. wildcard pattern을 이용해 원하는 멤버 값에 대한 조건을 걸 수 있다**

```swift
switch resolution {
    case (_, 1080):
        print("1080p)
    case (3840...4096, 2160):
        print("4K")
    default:
        break
}
```

`_를` 이용해 위에처럼 원하는 튜플의 멤버값에 대한 조건을 걸 수 있음

# **4-4. value binding을 활용할 수 있다**

value binding은 아까 튜플 Decomposition 문법같이

```swift
switch resolution {
    case let(w, h) where w / h == 16.0 / 9.0:
        print("16:9")
    case (_, 1080):
        print("1080p)
    case (3840...4096, 2160):
        print("4K")
    default:
        break
}
```

**튜플 resolution의 멤버 값은 상수 w, h에 바인딩 되고,** 이 바인딩 된 상수 w, h 값을 이용해 조건문을 만듬.
