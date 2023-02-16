forEach 에 대해서 알아보자 사용법은 물론, 차이점에 대해서도 알아보자.
Fast Enumeration, 즉 for-in과 forEach에 대해 알아보자.


# 2. forEach 의 정의:

반복 실행하려는 코드를 파라미터로 받고, 저장된 요소는 클로저 상수로 전달된다.

forEach는 해당된 Collection 의 요소 갯수만큼 반복해주는 함수다.  하지만 반복할때 할 작업은 클로저로 작성해서 파라미터로 넘겨야한다.  Collection 에 저장된 요소는 클로저 반복 실행할때마다 클로저 상수로 넘겨준다.  이론은 보면 어려우니 코드로 보자!

</br>

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

</br>


# 2-2. Dictionary

for - in 과 똑같으나 클로저로 넘겨준단 점만 다르니 참고하자.

```swift
let dict: [String : String] = ["A" : "Apple", "B" : "Banana", "C" : "Cherry"]
 
dict.forEach {
    print("(\($0.key) : \($0.value))")  // (B : Banana) (C : Cherry) (A : Apple)
}
 
dict.forEach { (key, value) in
    print("(\(key) : \(value))")        // (C : Cherry) (A : Apple) (B : Banana) 
}
 
dict.keys.forEach {
    print($0)       // B C A
}
 
dict.values.sorted().forEach {
    print($0)       // Apple Banana Cherry
}
```

</br>

# 2-3. Set
Set 은 배열과 같다

```swift
let nums: Set<Int> = [1, 2, 3, 4]
 
nums.forEach {
    print($0)               // 2 3 1 4
}
```

Set은 Array 와 같이 보면 된다.

Set 은 배열과 비슷하지만, 정렬되지 않고 중복 요소를 저장하지 않는 것 뿐이잖슴?

따라서 forEach 문은 그냥 배열하고 동일하게 사용하면 됨.

다만, Dictionary 와 동일하게, print 결과 값은 찍을 때마다 달라질 것이다.

</br>

# 3. 그래서.. for-in 과 forEach의 차이점이먼데..

for-in 과 forEach의 사용법은 알아봤으니까,, 이제 차이점을 알봅시다.

for-in 문은 우리가 직접 구현하는 “반복문” 이었다.  하지만




