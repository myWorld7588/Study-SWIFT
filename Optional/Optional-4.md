# SWIFT-Optional-4

# Optional Chaining: 옵셔널 체이닝 이란??

### 옵셔널을 연쇄 적으로 사용하는것!

```swift
person.contacts?.address    // 이같이 .(dot) 을 통해 내부 프로퍼티나 메서드에 연속적으로 접근할때
person?.contacts?.address   // 옵셔널 값이 하나라도 껴있으면 그것은 옵셔널 체이닝이다.
```

### < Sample / 예제 / 구조체 >

```swift
struct Contracts {
	var email: String
	var address: [String : String]
}

struct Cat {
	var name: String
	var contact: Contacts
	init(name: String, email: String, address: String) {
		self.name = name
		contacts = Contacts(email: email, address ["home" : address])
	}
}

var sunMook: Cat? = cat(name: "SundaeUhmook", email: "sunmook@gmail.com", addresss: "myWorld")
```

현재 `sunMook` 은 옵셔널 타입이기 때문에 `?` 로 접근해야함 `sunMook?.contacts.email`

옵셔널 포현식이 멤버에 접근할때, 표현식이 `nil` 일수 있으니 `?` 를 써주는것

만약 `sunMook` 이 `nil` 일 경우엔 어떻게 될까? `nil` 이 반환됨

만약 `sunMook, Contacts, Email` 중 단 하나의 표현식이라도 nil 이라면 결과값은 `nil` 임

# 2-1 옵셔널 체인의 특징

- 옵셔널 체이닝의 결과값의 타입은 마지막 표현식 의 옵셔널 타입이다

```swift
let email = sunMook?.contacts.email
```


그런데 반환값 email 을 확인해보면

`Type(of: email)   // optional<String>.type`

마지막 표현식 email 타입에 옵셔널을 씌운 optional String 을 확인할수있다.



> email 은 옵셔널이 아닌 일반 String인데 왜 반환값은 옵셔널 String 일까..?  내가 원하는 email 에 닿기 전에 `sunMook` 이 `nil` 이면 그냥 `nil` 을 뱉어 버리기 때문, 이런 가능성 때문에 옵셔널 체이닝의 반환값은 무조건 옵셔널 타입임
> 

# 2-2 옵셔널 체이닝의 마지막 표현식은 옵셔널 이라도 ? 를 생략한다

```swift
struct Contacts {
	var email: String?
	var address: [String : String]
}
```

> email 의 변수를 옵셔널 `String` 으로 바꾸었다.. 그럼 옵셔널 이니까 접근할때 옵셔널 체이닝에의해 `?` 를 써야할것같지만. 아.니.다! 왜냐하면 옵셔널 체이닝의 마지막 표현식은 옵셔널이든 아니든 `?` 를 생략한다
> 

`let email = sunmook?.contacts.email? // Error`

`let email = sumook?.contacts.email // 이렇게 “?” 를 지우면 에러안남`

만약 email 이란 변수의 속성에 접근한다면?  `sunmook?.contacts.email?.count` 이렇게 써주면됨.

왜냐면 email 은 더이상 마지막 표현식이 아니기 때문

# 2-3 옵셔널 체이닝의 표현식중 하나라도 nil 이라면, 이어지는 표현식은 평가하지않고 nil 을 리턴한다

```swift
sunMook = nil
let emil = sunMook?.contacts.email
let email = sunMook?.
```
