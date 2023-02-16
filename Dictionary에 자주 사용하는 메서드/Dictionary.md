딕셔너리를 사용할때 알아두면 편리한 메서드를 정리하고 알아보자.



# 딕셔너리란?

- Key : Value 가 함께 저장되는 자료구조이다. (예를들어 영한 사전이라고 하면 영어 단어(Key)를 입력하면 한글로 풀이된 뜻(Value) 가 나오는 것이다)
- 절령되지 않은 컬렉션이다 ( 따라서 딕셔너리를 print 할 경우, 삽입 순서에 상관 없이 뒤죽박죽 나온다)
- 값은 중복 가능하지만, 키는 중복되면 안된다
- Swift는 형식에 민감하기 때문에, 모든 Key의 자료형은 같아야하며, 마찬가지로 모든 Value 의 자료형도 같아야한다.

**딕셔너리도 배결과 같은 Collection Type**이고, 때문에 **구조체로 Stack에 저장**된다.  다만 **NSDictionary란 클래스도 지원**함 (Objective-C)

</br>

# 1. 딕셔너리 생성하기

```swift
// 1. 타입 추론으로 생성하기
var dict1 = ["height": 165, "age" : 100]
var dict2 = [:]                                         // error! 타입 추론으론 빈 딕셔너리 생성 불가
 
 
// 2. 타입 Annotation으로 생성하기
var dict3: [String: Int] = [:]                          // 빈 딕셔너리 생성
var dict4: [String: Int] = ["height": 165, "age" : 100]
 
 
// 3. 생성자로 생성하기
var dict5 = Dictionary<String, Int>()                   // :이 아닌 ,로 명시
var dict6 = [String: Int]()
```

보통 간편한 1번이나 2번의 방법을 많이 사용함, Type Annotation 은 중요하니까 2번을 많이 쓰도록 노력해보자.

Swift는 뭐라구?? Swift는 Type 에 정말, 굉장히, 어마무시하게 민감한 언어이다 !!!

**선언과 동시에 Type을 꼭 명시**하거나 **추론할 수 있게** 해주어야 하고, **해당 Type 으로 된 값만 딕셔너리에 저장**할 수 있다!!

따라서 만약, 

- **딕셔너리 한개에 여러 타입의 Value 를 저장**하고 싶다??
- 혹은 나는 **Runtime 시점에서 타입을 알 수 있는데**??

라고 할수 있잖슴?  그러면 **Key** 값의 **Type**을 **Any**로 선언하거나, Objective-C의 클래스인 **NSDictionary** 를 사용하면 된다.


```swift
// 4. 여러 타입을 저장하는 딕셔너리 생성하기
var dict7: [String: Any] = ["name": "Sodeul", "age": 100]
var dict8: NSDictionary = ["name": "Sodeul", "age": 100]
```

Value 같은 경우, 배열 때와 마찬가지로 Any라는 자료형을 쓰면 된다.  근데 Key 값도 여러 타입으로 받고 싶은데?? 해서 Any 로 선언할 경우. “Type ‘Any’ does not conform to protocol ‘Hashable’" 에러를 볼수있다.

즉, **Any**가 **Hashable** 프로토콜을 준수하지 않는다는 뜻이다.





