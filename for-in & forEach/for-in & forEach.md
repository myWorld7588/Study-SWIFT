for - in 그리고 forEach 에 대해서 알아보자 사용법은 물론, 차이점에 대해서도 알아보자

# 1. for - in 의 정의:

컬렉션에 저장된 요수 수만큼 반복되며, 저장된 요소가 루프 상수에 하나씩 들어간다.  

Collection Type 에 따라 사용법을 나누어서 설명 ㄱㄱ

</br>
</br>

# 1-1. Array

```swift
let nums: [Int] = [1, 2, 3, 4]
 
for num in nums {
    print(num)        // 1 2 3 4
}
```

</br>

nums 란 배열이 있을때, 위와 같이 해주면 nums의 요소 갯수만큼 반복문이 반복한다.

1번째 반복할땐 num 이란 루프 상수에 nums[0]의 값이 들어가고

2번째 반복할땐 num 이란 루프 상수에 nums[1]의 값이 들어간다.

n번째 반복할땐 num 이란 루프 상수에 num[n-1] 의 값이 들어간다.

이런식으로 배열의 갯수만큼 반복하기 때문에, for - in 반복문을 사용하여 배열의 모든 요소에 num 이란 루프 상수로 접근이 가능하다.  (여기서 num 이란 루프 상수의 이름은 알아서 센스 있게 작성)

만약 배열의 요소를 순회하면서, 현재 반복문의 index 도 알고 싶다면 어떻게 하나?  라고 할경우 다음과 같이 사용할수도있다.

```swift
for (index, num) in nums.enumerated() {
    print("(index: \(index) num: \(num))")                // (index: 0 num: 1) (index: 1 num: 2) (index: 2 num: 3) (index: 3 num: 4)
}
 
for index in nums.indices {
    print("(index: \(index) num: \(nums[index]))")        // (index: 0 num: 1) (index: 1 num: 2) (index: 2 num: 3) (index: 3 num: 4)
}
 
for index in 0..<nums.count {
    print("(index: \(index) num: \(nums[index]))")        // (index: 0 num: 1) (index: 1 num: 2) (index: 2 num: 3) (index: 3 num: 4)
}
```

enumerated 메서드나 indices 를 이용하거나, 직접 반복 횟수를 지정해서 받을 수도있다.

</br>
</br>

# 1-2. Dictionary

```swift
let dict: [String : String] = ["A" : "Apple", "B" : "Banana", "C" : "Cherry"]
  
for (key, value) in dict {
    print("(\(key) : \(value))")                     // (B : Banana) (C : Cherry) (A : Apple)
}
 
for element in dict {
    print("\(element.key) : \(element.value))")      // (B : Banana) (A : Apple) (C : Cherry)
}
```

1

