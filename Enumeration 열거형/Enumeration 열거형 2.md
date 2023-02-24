
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

