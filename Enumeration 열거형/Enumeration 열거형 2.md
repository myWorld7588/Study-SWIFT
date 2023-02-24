
# 1. Associated Values(연관값)

Enumeration 열거형 1/2 에서 Raw Value라는 것에 대해 배움. **(열거형에 원시값을 지정해주는 것)**

Apple제품에 대해 열거형을 만들어보자.

```swift
enum AppleProduct: String {
    case iPad = "5, 526GB"
    case iPhone = "13, 128GB"
    case macBook = "Pro, 256GB"
}
```

위에 코드처럼 Raw Value 를 통해 case별로 원하는 값을 지정해주도 되지만,  Apple 제품이 모두 같은 이름과 스펙일리 없잖음?? 그럼 이렇게 Raw Value를 사용할 경우

### 모든 case가 동일한 형식 (위 코드에선 String) 으로 Raw Value를 가져야 하고, case별 값은 미리 지정된 한 가지 값만 가질 수 있다.

라는 단점을 지니고있다.  따라서 이를 보완해서 사용할 수 있는 것이 바로 

### Associated Value 즉, 연관 값 이라는 것이다.

</br>

# 1-1. 연관값을 가지는 열거형 선언 방법

```swift
enum TypeName {
    case caseName(Type)
	case caseName(Type, Type, ...)
}
```

**이처럼 case 옆에 튜플 형태로 원하는 Type을 명시하면 된다**.  이때 Tuple은 Named Tuple도 되고, Unnamed Tuple 도 가능하다.

**따라서, 아까 Raw Value로 한계가 있던 애플제품 예제를 연관 값으로 바꾸면**

```swift
enum AppleProduct {
	case iPad = (model: String)
	case iPhone = (model: String, storage: Int)
	case macBook = (model: String, storage: Int, size: Int)
} // 이런 식으로 튜플을 활용해서 원하는 연관값을 받을 수 있게 선언가능
```

</br>

# *사용방법*

# 1-2. 연관값을 가지는 열거형 생성 방법

### 열거형 생성 시 연관값을 함께 전달한다

열거형 생성 시 내가 만들어둔 case가 연관값이 함께 뜬다. (자동완성)

마지막으로 이렇게 열거형에 직접 값을 지정해서 사용할 수 있다

```swift
let product: AppleProduct = .iPhone(model: "13", storage: 528)
```

</br>

# 1-3. 연관값을 가지는 열거형의 Switch 매칭

```swift
switch product {
    case .ipad("5s"): break                        // 연관값이 5s면 매칭
    case .ipad: break                              // 연관값 무시
    case .iPhone("X", _): break                    // 연관값 생략 가능
    case .iPhone(let model, var storage): break    // 연관값 상수(변수) 바인딩
    case let .macBook(model, stroage, size): break // 모든 연관값을 let으로 바인딩 시 let을 맨 앞으로 뺄 수 있슴  
}

```

switch 매칭 시킬 때, 이런 식으로 다양하게 연관값을 매칭시켜서 사용할 수 있슴. <설명은 주석에>

</br>

# 1-4. 연관값을 가지는 열거형의 if 사용법

```swift
if case let .iPhone("8", storage) = product {  // product의 첫 번째 연관값이 "8" 이면 매칭
    print(storage)
}

if case .iPhone(_, 64) = product { // product의 첫 번째 연관 값은 상관없고, 두 번째 연관값이 64면 매칭
    print("iPhone 64GB")
}

```

이런 식으로도, if 를 통해서도 연관값을 매칭시킬수 있다.

