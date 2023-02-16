forEach 에 대해 알아봅시다.

# 2. forEach 의 정의:

반복 실행하려는 코드를 파라미터로 받고, 저장된 요소는 클로저 상수로 전달된다.

forEach는 해당된 Collection 의 요소 갯수만큼 반복해주는 함수다.  하지만 반복할때 할 작업은 클로저로 작성해서 파라미터로 넘겨야한다.  Collection 에 저장된 요소는 클로저 반복 실행할때마다 클로저 상수로 넘겨준다.  이론은 보면 어려우니 코드로 보자!

# 2-1. Array

```swift
let nums: [Int] = [1, 2, 3, 4]

nums.forEach {
    print($0)  // 1 2 3 4
}
```

이렇게 사용하면 된다.  이럴 경우 내가 전달할 print 함수를 찍는 클로저를 nums 요소의 갯수 (4) 만큼 반복한다.  만약 for-in 처럼 반복 index 를 알고 싶다면

```swift
nums.enumerated().forEach {
    print("(index: \($0) num: \($1))")           // index: 0 num: 1) (index: 1 num: 2) (index: 2 num: 3) (index: 3 num: 4) 
}

nums.indices.forEach {
    print("(index: \($0) num: \(nums[$0]))")     // (index: 0 num: 1) (index: 1 num: 2) (index: 2 num: 3) (index: 3 num: 4)
}

```

for-in 과 마찬가지로 enumerated 메서드나 indices 를 사용하면됨.
