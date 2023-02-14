배열(Array)을 사용할때 알아두면 편한 메서드를 정리해보자.

## 먼저! Swift 에는 총 2개의 배열 (Array) 이 존재한다.

하나는 우리가 평소에 사용하고있는 Array 라는 것이고

다른 하나는 Foundation 에서 제공하는 NSArray 라는 것이다.  (Object-C 문법)

### *Array*

**타입:** 구조체 타임

**저장위치:** Stack (스택)

**요소 (Element) 자료형:** 모두 동일한 자료형만 저장 가능하다. (Int형배열은 Int형만 저장가능하고, String형 배열에는 String 형만 저장 가능하다)

**COW(Copy-On-Write): Yes**

### *NSArray*

**타입:** 클래스 타입

**저장위치:** Heap (힙)

**요소 (Element) 자료형:** 인스턴스라면 타입 상관 없이 저장 가능 (Int, Double 같은 구조체 타입은 저장 불가, NSNumber 같은 인스턴스로는 저장가능.  인스턴스이기만 한다면 NSString NSSnumber 등 타입에 상관없이 하나의 배열에 모두 저장가능)

**COW(Copy-On-Write): No**

</br>
</br>

# 1. 배열 생성하기

```swift
// 1. 타입 추론으로 생성하기
var array1 = [1, 2, 3]
var array2 = [] // Error!: 타입 추론으론 빈 배열 생성 불가
 
 
// 2. 타입 Annotation으로 생성하기
var array3: [Int] = [1, 2, 3]
var array4: [Int] = [] // 빈 배열 생성
 
 
// 3. 생성자로 생성하기
var array5 = Array<Int>()
var array6 = [Int]()
var array7 = [Int](repeating: 0, count: 10) //생성과 동시에 10개 Element 생성 및 0으로 초기화
```

보통 간편한 1번이나 2번을 방법을 많이 사용함.  2번의 Type Annotation 에 익숙해질필요가있다.

Swift 는 Type 에 굉장히 민간한 언어이다.  선언과 동시에 Type 을 꼭 명시하거나 추론할 수 있게 해주어야 하고, 해당 Type 으로 된 값만 배열에 저장할수 있다.

따라서 배열 1개에 여러타입을 저장하고싶다면, Type 을 Any로 선언하거나, Objective-C의 클래스인 NSArray 를 사용하면 된다.

```swift
// 4. 여러 타입을 저장하는 배열 생성하기
var array8: [Any] = [1, "Sundae", 15.2]
var array9: NSArray = [1, "Sundae", 15.2]
```

NSArray는 Class 형식이라 요소는 무조건 “인스턴스” 로 구성되어야 한다.  때문에 array8 에 삽입한 1, 15.2 라는 숫자는 Int, Double 형이 아니라, NSNumber 라는 인스턴스로 저장된다.

왜냐하면 Int, Double 형은 구조체 타입이기 때문에 인스턴스가 아니기 때문이다.  이럴 경우 Swift 가 Array 를 구조체로 만든 이유 (Stack 의 장점) 이 사라진다.

만약 Any 를 사용한다고 하면 런타임시 타입을 알 수 있기 때문에 컴파일 시 실행 오류를 잡기도 힘들다. 너무 추상적인 타입이라 절대 남발하지말길.. **웬만해선 Array 만 써도 거의 다 되긴함 ㅎ**

</br>
</br>

# 2. 배열 갯수 확인하기

```swift
var array1 = [1, 2, 3]

let count: Int = array1.count      // 배열 갯수 확인 : 3
let isEmpty: Bool = array1.isEmpty // 배열 비었는지 확인 : false
```

만약 배열이 비었는지를 확인하고 싶다면

`array1.count == 0` 이 구문보단 `array1.isEmpty` 를 사용하자

</br>
</br>


# 3. 배열 요소에 접근하기

```swift
var array1 = [1, 2, 3]

// 1. Subscript로 접근하기
array1[0]        // 1
array1[1]        // 2
 
// 2. 범위로 접근하기
array1[0...1]    // [1, 2]
 
// 3. 속성으로 접근하기
array1.first     // Optional(1)
array1.last      // Optional(3)
```

**Subscript**라는 것은 우리가 흔히 배열에 **대괄호**([])로 접근하는 것을 말한다. 

근데 Subscript나 범위로 접근하는 것은 **위험성**이 있다.

위 주석 결과값에서도 알 수 있듯이 **반환형이 Non-Optional Type 이다.**

따라서 만약 **해당 index에 해당하는 값이 없으면 그것은 에러**로 빠짐

따라서 만약 **첫번째, 마지막 요소에 접근한다면** Subscript보다 **first**, **last**란 속성을 사용하는 것이 더 안전하다. 이것들은 리턴 타입이 **Optional Type이라서 만약 값이 없으면 에러가 아닌 nil을 리턴한다.**

</br>
</br>

# 4. 배열에 요소 추가하기

```swift
// 1. append : 끝에 추가
var array1 = [1, 2, 3]
array1.append(4) // [1, 2, 3, 4]
array1.append(contentsOf: [5, 6, 7]) // [1, 2, 3, 4, 5, 6, 7]
 
// 2. inset : 중간에 추가
var array2 = [1, 2, 3]
array2.insert(0, at: 0) // [0, 1, 2, 3]
array2.insert(contentsOf: [10, 100], at: 2) // [0, 1, 10, 100, 2, 3 ]
```

배열에 요소 추가는 `append`와 `insert`로 할수 있다.  `contentsOf`파라미터를 사용하면 배열에 배열을 붙일 수도 있다. 한가지 유의할 점은, 자료구조 할때 귀에 딱지 앉게 말했지만,

배열의 경우, `insert` 시에 오버헤드가 발생

![https://blog.kakaocdn.net/dn/obT6y/btqSXjtpAw8/4zGsVjnQwEbdVoiC9jrwck/img.gif](https://blog.kakaocdn.net/dn/obT6y/btqSXjtpAw8/4zGsVjnQwEbdVoiC9jrwck/img.gif)

![https://blog.kakaocdn.net/dn/cPYNzI/btqS4LIIsmk/qdiON3eIstXJfhdNYZxZbk/img.gif](https://blog.kakaocdn.net/dn/cPYNzI/btqS4LIIsmk/qdiON3eIstXJfhdNYZxZbk/img.gif)

배열의 경우 index 로 접근하기 때문에 insert 를 해주면 insert 를 하는 위치부터 배열을 재배치 해야 하기 때문에 오버 헤드가 발생하는 것이다.

때문에 꼭 필요한 경우를 제외하고는 `append` 를 쓰도록 하자

</br>
</br>

# 5. 배열 요소 변경하기

배열 안에 저장된 요소 (Element) 의 값을 변경하고 싶을때 쓰는것 

```swift
// 1. Subscript 로 변경하기
var array1 = [1, 2, 3]
array1[0] = 10                  // [10, 2, 3]
array1[0...2] = [10, 20, 30]    // [10, 20, 30]
array1[0...2] = [0]             // [0]
array1[0..<1] = []              // []

// 2. replaceSubrange 로 바꾸기 (범위 변경 시)
var array2 = [1, 2, 3]
array2.replaceSubrange(0...2, with: [10, 20, 30])    // [10, 20, 30]
array2.replaceSubrange(0...2, with: [0])             // [0]
array2.replaceSubrange(0..<1. with: [])              // []
```

</br>
</br>

# 6. 배열 요소 삭제하기

```swift
// 1. 일반적인 삭제하기
var array1 = [1, 2, 3, 4, 5, 6, 7, 8, 9]
 
array1.remove(at: 2)             // [1, 2, 4, 5, 6, 7, 8, 9] 
array1.removeFirst()             // [2, 4, 5, 6, 7, 8, 9]   
array1.removeFirst(2)            // [5, 6, 7, 8, 9]
array1.removeLast()              // [5, 6, 7, 8]
array1.popLast()                 // [5, 6, 7] 
array1.removeLast(2)             // [5]
array1.removeAll()               // [] 
 
// 2. 특정 범위 삭제하기
var array2 = [1, 2, 3, 4, 5, 6, 7, 8, 9]
 
array2.removeSubrange(1...3)     // [1, 5, 6, 7, 8, 9] 
array2[0..<2] = []               // [6, 7, 8, 9]
```
</br>
</br>

| 함수 이름 | 용도 | 리턴 타입 |
| --- | --- | --- |
| remove(at:) | 파라미터로 받은 index에 해당하는 값 삭제 | 삭제된 값 리턴Non-Optional Type |
| removeFirst | 첫 번째 요소 삭제 | 삭제된 값 리턴Non-Optional Type |
| removeFirst(_:) | 첫 번째 요소부터 파라미터로 받은 갯수 만큼 삭제 | X |
| removeLast | 마지막 요소 삭제 | 삭제된 값 리턴Non-Optional Type |
| popLast | 마지막 요소 삭제 | 삭제된 값 리턴Optional Type |
| removeLast(_:) | 마지막 요소부터 파라미터로 받은 갯수 만큼 삭제 | X |
| removeAll | 전체 요소 삭제 | X |
| removeSubrange(_:) | 파라미터로 받은 범위만큼 index 요소 삭제 | X |
| [n...m] = [] | Subscript 문법으로 n ~ m까지 index 요소 삭제 | X |

</br>
**removeLast**는 리턴 값이 **Non-Optional Type**이라, **마지막 값이 없을 경우 에러**를 내지만, **popLast**는 리턴 값이 **Optional Type**이기 때문에 **마지막 값이 없을 경우 nil을 돌려주고 에러를 내지 않음!**

따라서 **removeLast 보단 조금 더 안전한 popLast를 쓰도록하자!**

참고로 remove 역시 **마지막 요소를 삭제하는 것이 아니면 insert와 같은 이유로 오버헤드가 발생**함
</br>
</br>

# 7. 배열 비교하기

```swift
var array1 = [1, 2, 3]
var array2 = [1, 2, 3]
var array3 = [1, 2, 3, 4, 5,]
 
array1 == array2                    //true
array1.elementsEqual(array3)        //false
```

모든 요소 (Element) 가 같아야만 True 이다.

</br>
</br>


