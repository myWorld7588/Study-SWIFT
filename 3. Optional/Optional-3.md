# SWIFT-Optional-3

삼항연산자같은 느낌..? Ternary Operator 느낌이 남

`Optional Expression ?? Non-Optional Expression`

```swift
let name: String? = "Sundae"
```

기존 옵셔널 바인딩을 이용하면 아래처럼 길~~게 했을꺼임.  근데 Nil-coalescing 을 이용하면 간단하게 만들수있슴

```swift
if let name = name {
    print("hello, \(name)")
} else {
    print("helklo, What is your name?")
}
```

```swift
print("hello, " + (name ?? "What is your name?")) // 이렇게 한줄로 간단하게 끝
```

**주의할 점**

Nil-Coalescing에 사용되는 Optional Type과 Non-Optional Type은

**Optional을 제외하면 동일한 Type이어야 함**

ex)

Optional **String** Type ?? Non-Optional **String** Type **(O)**

Optional **String** Type ?? Non-Optional **Int** Type **(X)**
