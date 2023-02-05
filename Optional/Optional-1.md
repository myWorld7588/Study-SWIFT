# SWIFT-Optional-1

# Optional: 옵셔널 ?

Optional 은 nil 을 사용할수있는 Type 과 없는 Type 을 구분하기 위함이며

`nil` 을 사용할수 있는 Type을 Optional Type 이라 부른다

`nil` 이라는 값을 가질수 있다면 Optional Type 이고 Optional Type 을 선얼할때엔 Type 옆에 `?`(물음표)를 붙인다

> nil 이란 “값이없음” 을 뜻함, nil 은 Objective-C 에도 있는 개념이지만 옵씨와는 개념이 조금 다름                                                                          Objective-C 에서는 빈 포인터를 가리킬 때 nil 을 사용하지만 (다른 언어에서는 NULL)                                                                                    스위프트에선 직접 포인터를 접근할 일도 없을 뿐더러 오로지 값이 없을을 나타낼 뿐
> 

그럼 언제 nil 을 사용하냐면, “*너 방금 오류났어 근데 이 정도로 앱을 중단시키긴 좀 그러니까 프로그램의 안정성을 위해 오료는 안낼께. 대신 nil 을 돌려줄테니까 너 오류인건 알고있어라*!!!! ” 이럴때 사용함.. **때문에 Optional 을 Swift 언어의 안정성에 해당한다고 함**.

찾는 값이 업다고 Crash 를 발생시키는 것이 아닌 nil 을 발생시킴.

```swift
let cat = ["name" : "Uhmook", "gender" : "male"] // 딕셔너리가 있을때 name 이란 키에 접근해보면
let name = cat["name"] // "Uhmook" 이렇게 잘 출력됨.  그런데 존재하지않는 Key 값에 접근해보면
let alias = cat["alias"] // 실제로 alias 라는 Key 값이 없기 때문에 오류임
```

하지만 그렇다고 너가 입력한 키가 없어! 하고 컴파일 에러를 내버리기엔 너무 섭섭해.. 그렇다고 리턴을 안할수도없고…

오류 났다고 알려줘야 하는데 0 이나 공백 (””) 을 리턴 해줄수도 없슴… 왜냐면??  0 , (“”) 이것들도 결국 다 값 이기 때문이지…

(0도 숫자 literal 이고, 실제 value 값이 “” 일 수도 잇으니까)  아무튼 이게 바로 nil 임!

```swift
let alias = cat["Uhmook"] // nil 너가 원하는 값없다!! 하고 오류를 반환하지 않는 대신 nil 을 반환해주는 것임 
```

# ****3.  Non-Optional Type vs Optional Type****

nil 은 특수한 값 이기 때문에, 일반적으로 쓰이는 자료형은 nil 값을 담지 못함. 

nil 을 저장할 수 있는건 오로지 Optional 로 선언된 자료형만 nil 을 저장 할수있슴

**nil 을 사용할 수 있는 타입과 사용할 수 없는 타입에 대해 공부할것임.**

# Non-Optional Type:

- 평소에 쓰던 변수는 모두 Non-Optional

```swift
var name : String // 평소에 쓰던 변수는 모두 Non-Optional
name = “Sundae”  // 평소에 쓰던 변수는 모두 Non-Optional
```

- 무조건 값이 필요함

```swift
var name : String
print(name) // Error! Varviable 'name' used before being initialized
```

`name` 이라는 변수를 만드는것까지는 ㅇㅋ.. 근데 선언만 하고 값을 대입하지 않았슴.  그런 상태에서 `name` 에 접근하려하면 `name` 에 지정된 값이 없기때문에 에러가 발생함

그럼 만약 `name` 에다가 값이 없다 `nil` 을 지정하면 어떻게 될까?

```swift
var name : String
name = nil // Error! 'nil' cannot be assined to type 'String'
```

에러뜸.  앞에서 말했듯 nil 을 저장할 수 있는건 오직 Optional Type 만 가능하기 때문임

# Optional Type:

> Optional Type 은 기존 자료형을 Optional 이란 포장지로 한번 포장했기에 Optional Type 값을 쓰기위해선 포장을 까고 써야함. **That’s call Optional Unwrapping**
> 

### Type Annotation 을 이용하여 Optional 선언

- Type Annotation 을 이용하여 Optional 을 선언하기 위해선 다음과 같이 자료형(Type)의 뒤에 `?` 를 붙여주면됨

`var name : String?` 

`name = nil` // ? (Optional) 로 선언된 name 엔 `nill` 값을 저장할수있슴

### Type Inference 를 이용하여 Optional 선언

- Type Inference 를 이용한 방법. 선언과 동시에 초기화를 해주면 되는데… **초기화 되는 값이 무조건 Optional 자료형 이여야함.**

`let a: Int? = nil`

`let b = a` // 이미 Optional Type 값인 a 를 b 에다가 선언과 동시에 초기화 해주면 b도 Optional Type 로 선언됨

`let b = a // nil`

타입 추론에 대해 알고 있다면 당연함. 타입 추론은 초기값의 자료형을 보고 타입을 지정하기 때문 
초기값 a의 자료형이 Optional이면 당연히 b의 자료형도 Optional로 추론됨
이처럼 이미 옵셔널 타입으로 선언된 변수나 상수를 이용해서 초기화 하면, 옵셔널 타입의 변수나 상수를 만들 수 있음

## String 과 Optional<String> 의 비교

```swift
var alias : String // Non-Optional
var name : String? // Optional
print(type(of: alias)) // String
print(type(of: name)) // Optional<String>
```

Non-Optional Type의 경우 **String Type** 이지만, Optional Type의 경우 **Optional<String> Type** 임.

String 을 Optional 로 한번 감쌌다고 생각해보셈.  이렇게 감싸진걸 Optional String Type 이라고 부름. 

**(일반 String Type 과는 엄연히 다른 타입임)**

Optional Type 은 기존 Optional 이란 포장지로 한번 포장했기때문에 옵셔널 타입의 값을 사용할땐 옵셔널 이란 포장지를 까고 써야함.

그것이 바로 **Optional Unwrapping**

# Optional Unwrapping:

Optional<Int>Type 의 Optional 자료형을 사용하기 위해선 Optional 이란 포장지를 까주고 써야함. 만약 Unwrap 하지 않고 사용하게 될 경우 이렇게 됨.

```swift
let a : Int?
a = 2
var myInfo = "우리 집 고양이는 \(a)마리 입니다" // "우리 집 고양이는 Optional(2)마리 입니다"
```

이렇게 우리집 고양이는 무려 Optional(2) 마리임 ㅋㅋ 이처럼 Optional 포장지에 싸여있기 때문에 값을 사용하기 위해선 Optional 포장지를 까는 작업을 우리는 해줘야함.  이작업을 우리는 바로 Optional Unwrapping 이라고 함.

**Optional Type  ——— Optional Unwrapping ———>  Non Optional 로 변활할수있슴**

# Optional Unwrapping에 꼭 알아둬야 할 것

> **Optional Unwrapping 을 위해선 절대 MUST!! Unwrap 하고자 하는 `변수(상수), var(let)` 가 절대 `nil` 이면 안됨**
> 

> `**nil` 은 값이 아님!! `nil` 은 값이 없슴이지 `nil` 이 어떠한 값 이라고 생각하면 절대 안됨!! 따라서 만약 값이 없으면 nil 이지 Optional<nil> 이 아니람 말씀..!!  Optional 은 값을 감싸는 것이지 값이 아닌 nil 을 감싸지 않음.**
> 

a 엔 nil 을 지정했고, b엔 4라는 숫자 literal 를 지정했슴. 이 둘의 결과는

```swift
let a: Int? = nil
	print(”a==\(a)”) // a == nil (옵셔널이 아님!)

let b: Int? = 4
	print(”b==\(b)”) //  b == Optional(4)
```

**절대 Optional(nil) 이 아님 Optional(4) 를 반환함**

nil 은 값이 없기때문에 Optional 이란 포장지도 없고 따라서 Optional 이란 포장지를 까는 Unwrapping 도 하면 안됨.  만약 nil 이 지정된 변수(상수) 를 Optional Unwrapping 하려고 하면 error 남Unwrapping 추출 하는 방법은 여러가지임

# Forced Unwrapping: 강제추출 100% 확신이 있을때 사용할것 (!)느낌표

Forced Unwrapping 은 Optional 값이 nil 이건말건 강제로 벗겨버림.

그런데 아까 Unwrap 하고 하는 `변수(상수) var(let)` 가 `nil` 이면 절대 해체하면 안된다고 했었는데??? 

ㅇㅇ 맞음 일단 설명 고고

```swift
var myInfo = “나는\(a!) 살 입니다” // 나는 4살입니다. 
```

이렇게 Optional(4) 가 해체된 값을 얻을수있슴

> 실제 프로그래밍에서 대부분 사용되는 Optional Unwrapping 방법은 Optional Binding 이란것을 많이씀Forced Unwrapping 은 실제 프로그래밍에서 잘안씀. 이런것도 있구나! 하고 넘어가자
> 

# Optional Binding 이 더중요!

**옵셔널 타입의 값을 사용할 땐**

**옵셔널을 해제해서 사용해야 하고**

**주의할 점은 nil이 지정된 경우엔 절대 해제하면 안 됨**

Optional Unwrapping 방법 중엔

강제 추출(Forced Unwrapping) 방법이 있지만

이는 nil이 들어 있어도 강제로 해제해버리는 특성 때문에

진짜 얘 값 100프로 있다고 확신할 수 있는 경우를 제외하곤

웬만해선 사용하면 안 된다 했음

어떤 방법을 써야하나? 바로바로 Optional Binding 

# Optional Binding:

Optional 을 Unwrapping 하는 방법중하나. 안전하게 Unwrap 할수 있는 방법임. Optional Binding. How? by using Syntax.

어떻게? 다음 세 가지 Syntax를 통해서!!

```swift
if let name: Type = OptionalExpresssion { }
while let name: Type = OptionalExpresssion { }
guard let name: Type = OptionalExpresssion else { }
```

엥 이게 모임 할  수 있음 근데 대충 위 방식들의 공통점은

**1. 옵셔널 표현식을 먼저 평가한다.**

**2. 값이 있는 경우(nil이 아닌 경우) 정의된 상수(name)에 옵셔널이 해제된 값을 저장하고 true를 반환한다.**

**3. 값이 없는 경우(nil)인 경우 false를 반환한다**

**4. 타입 추론이 되므로 Type Annotation은 대부분 생략한다**

** 예제를 통해 하나씩 알아봅시다.  참고로 while 을 이용한 방법은 거의 사용되지 않기 때문에 while 은 제외하고 알아볼거임

### 첫번째 방법. If let

첫번째 방법은 if let 을 이용한 방법이다,  위에서 syntax 는 확인했으니 예제를 보자

```swift
let optionalNum: Int? = 4
```

위처럼 옵셔널 타입의 `optionalNum` 이라는 상수가 있슴, if let 을 이용하여 Optional Binding 을 하면 다음과 같이 사용 가능함

**사용은 이런식으로함, 일단 이해하기 쉬우라고 주석 달아놓음.**


```swift
if let nonOptionalNum = optionalNum {
    // the 'optionalNum' is not nil
    print(nonOptionalNum)
} else {
    // The 'optionalNum' is nill
    print(optionalNum)
}
// optionalNum = 4
```
	

**먼저 위 구문의 결과 값은 4.  4라는 옵셔널이 해제된 값이 나옴, 만약 `optionalNum` 에 `nil` 이 지정 됐다면?**

```swift
let optionalNum: Int? = nil // 결과 값은 nil
```

# 매커니즘은 다음과같다

# 1. ****표현식이 nil인지 아닌지 판별하자****


# 2. ****표현식이 nil이 아닌 경우****


**optionalNum의 값이 nil이 아닌 경우, optionalNum을 Unwrapping 한 값을 nonOptionalNum에 대입함**

이제 우린 Unwrapping된 값인 nonOptionalNum이란 상수를 사용하면 됨 근데 여기서 주의할 점이 몇 가지 있음

### **❗️ 첫 번째 주의할 점**

**`nonOptionalNum` 이라고 if let을 통해 선언된 상수의 scope는 if 구문임 무슨 말인가하면,** 

```swift
if let nonOptionalNum = optionalNum {
    print(nonOptionalNum)
}

nonOptionalNum // Error! 이런 식으로 if문 밖에서 nonOptionalNum 이라는 상수에 접근할 수 없슴
```


### **❗️ 두 번째 주의할 점**

**nonOptionalNum은 optionalNum을 Unwrapping 한 값을 대입한 것 뿐 optionalNum의 값은 여전히 Optional Type이다**

```swift
let optionalNum: Int? = 4

if let nonOptionalNum = optionalNum {
		print(nonOptionalNum) // Optional(4)
		print(optionalNum) // 4
}
```

optionalNum을 Unwrapping 한 값이 nonOptionalNum일 뿐, optionalNum은 여전히 Optional Type의 값임

# 3. ****표현식이 nil인 경우****
	
nil인 경우엔 if 구문을 거치지 않고 else 구문으로 빠짐

### **Optional Binding sample using if - let statement**

```swift
var someValue: Int?
var someAnotherValue: Int! = 0

if let temp = someValue {
	print("it has some value\(Temp)"))
} else {
	print("doesn't contain value") // doesn't contain value
}

if let temp = someAnotherValue {
	print(”It has some value\(temp”)) // It has some value 0
} else {
	print(”doesn’t contain value”)
```

### `if - let` Statement also automatically  unwraps the value and place the unwrapped value in `temp` constant

# Guard Let:

`Guard` 문은 특성상 함수(메서드) 에서만 쓰이며 `Guard` 구문을 만족하지 못하면 `else` 문으로 빠져서 함수의 실행을 종료시킴 `return`

```swift
let optionalNum : Int? = 4
Guard let nonOptionalNum = optionalNum else {
	return
}
print(nonOptionalNum) // 4    **Unwrapped 된 값**
```

표현식이 `nil` 이 **아닌경우** `else` 문으로 빠지지 않고 원래 `Guard`를 실행시키던 scope 로 돌아옴. 그럼 `nonOptionalNum` 이라는 Optional 이 해제된 값을 `Guard` 구문에서 사용가능

**표현식이 `nil` 인경우** `else` 문에서 빠져서 `return` 때림
