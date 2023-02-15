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

Dictionary 같은 경우, Key-Value로 저장되어있고, 정렬되지 않는다.  라는 특징을 가지고있잖슴?

따라서 print 결과 값은 찍을 때마다 달라질것이다.  배열이 아니라 넣은 순서대로 정렬이되지 않기 때문이다.

Dictionary 의 경우, 순회하며 받는 루프 상수가 튜플 (key, value) 타입이다.

첫번째 방법과 같이 튜플 상수를 각각 선언해서 받아도 되고, 혹은 두 번째 방법과 같이 한번에 받아서 점문법을 이용해서도 접근 가능하다.  보통 첫번째 방법처럼 쓴다.

만약, key 또는 value 만 반복 하고 싶다면.

```swift
for key in dict.keys {
    print(key)          // C B A
}

for value in dict.values.sorted() {
    print(value)        // Apple Banana Cherry
}
```

이렇게 해주면 key, value 만 순회할 수 있다.   참고로 정렬 함수인 sorted 함수도 둘다 사용할수있다.

# 1-3. Sets

```swift
let nums: Set<Int> = [1, 2, 3, 4]

for num in nums {
    print(num)        // 3 2 4 1
}
```

Set 은 배열과 비슷하지만, 정렬되지 않고 중복 요소를 저장하지 않는 것 뿐이잖슴?

따라서 for - in 문은 그냥 배열하고 동일하게 사용하면 됨.

다만, Dictionary 와 동일하게, print 결과 값은 찍을 때마다 달라질 것이다.





