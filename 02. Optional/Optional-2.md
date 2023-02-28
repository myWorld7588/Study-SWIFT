# SWIFT-Optional-2

**IUO(Implicitly Unwrapped Optional) 옵셔널 묵시적(암시적) 추출**

즉 별도의 추출하는 과정을 거치지 않고 자동으로 옵셔널이 해제되는 것

```swift
let name: String!
```

기존 Optional Type 을 선언할 때 처럼 Type 뒤에 ? 를 붙이는 것이 아닌 Type 뒤에  ! 를 붙임

IUO 도 옵셔널 타입임. IUO 도 Optional Type 으로 선언하는 방법 중 하나.  근데 Non-Optional Type 으로 처리되어야 할때 값을 자동으로 추출해줌.  BUT 특정 조건일 때만 강제 추출해줌…

```swift
var num: Int! = 4
print(num) // Optional(4) 
```

원래 우리가 알던 옵셔널 선언은 에러를 뱉어냄.. 왜냐? Optional Type을 Non-Optional Type에 대입하고 있으니까

(Optional Binding을 통해서 넣어야 함)

```swift
var num: Int? = 4
var num2: Int = num // Error!
// 기존 Optional Type 
```

그치만 IUO 는 가능함. 실제로 num2 엔 “4\n” 이 들어가있슴

```swift
var num: Int! = 4
var num2: Int = num
// IUO
print(num2) // "4\n"
```

IUO 를 쓴다고 원하는 떄에 다 자동으로 값이 Unwrapping 되는것이 아님, Optional Type 을 Non-Optional Type 에 대입할때 별도의 추출 과정없이 대입이 가능한것임

iOS를 써봤다면 우리는 매일매일 IUO 옆에 있었슴…. IBOUtlet 을 사용할때 바로 IUO 를 씀.

```swift
@IBOutlet weak var tableView: UITableView!
```

IUO 를 코딩할 일을 거의 없고..  보통 두 가지 경우에 만나게 됨

**1. IBOutlet**

**2. API에서 IUO를 return 한 경우**

그냥 안전하게 Optional Binding을 쓰는 것을 매우 추천, 이론만 알아두고 쓰지말자
