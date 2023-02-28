
# **1. JSON이란..?**

JSON은 뭐의 약자냐면 **J**ava**S**cript **O**bject **N**otation인데,  원래는자바스크립트 언어에서

**객체 속성**을 표현하기 위한 방법으로 사용하기 시작한 **데이터 구조**임!!

(**간결하고 쉽게 데이터를 나타내는 방법 중 하나)**

### JSON에 사용되는 **데이터 구조**는 다음과 같이 크게 **2가지 종류**임!!!

# **1-1. JSON 객체**

JSON 객체는 쉽게 말해 **`"Key" : Value`** 로 이루어진 데이터들의 집함임!

**Dictionary** 생각나지 않음? 같은 성격임! 자 이 **JSON 객체**는 말이지

**중괄호** **{** **로 시작**해서, **중괄호 }** **로 끝나고**,

```json
{    // **중괄호** **{** **로 시작**해서
}    // **중괄호 }** **로 끝나고**
```

---

이 **중괄호 사이**에 **`Key-Value`가 쌍**을 이루어 이렇게 들어 가는 것임. 이때, **`Key-Value`**는 **`: (colon)`**을 이용해서 다음과 같이 표현함!

```json
{
"programLanguage": "Swift"
}
```

---

이런 식으로.  근데 만약 **Key-Value를 더 추가**하고 싶다면, **,(comma)**를 이용해서 추가해주면 됨!!

```json
{
"programLanguage": "Swift",
"experienceLevel": "Entry"
"year": "1"
}
```

---

이것이 바로 JSON 객체임! 앞서 잠깐 언급했지만, JSON 객체는 **Swift의 Dictionary 성격**을 가지고 있

기 때문에, Swift에서 이 JSON 객체를 다룰 땐 **Dictionary 계열의 자료형**을 사용함!!!

근데 Dictionary가 뭐다? 배열과 달리 **index가 필요 없이 Key에 해당하는 Value만 제대로 매칭된다.**

따라서, JSON 객체를 생성하고 만약 출력 했는데, 내가 생성한 Key-Value  쌍이 순서대로 되어있지 

않더라도 문제가 되지 않으니 걱정할 필요없다.

JSON 객체에선 **순서가 상관 없다!!** 자, 근데 이 Key-Value 쌍을 다룰 때 알아 둘 것이 있다.

### **Key-Value의 타입**

자, 방금 우리가 생성한 **Key-Value의 타입**은

```json
"programingLanguage": "Swift" // 문자열: 문자열
```

**문자열 : 문자열** 타입이었잖음?? 근데 이 **Key-Value의 타입은 아무 타입이나 막 쓰면 안 되고**,

**미리 지정된 타입**만 쓸 수 있음. 그 지정된 타입이 무엇이냐면,

![https://blog.kakaocdn.net/dn/boZJzk/btqO0gnq43c/I0SmTMLwBJqXFjMb5q6I4k/img.png](https://blog.kakaocdn.net/dn/boZJzk/btqO0gnq43c/I0SmTMLwBJqXFjMb5q6I4k/img.png)

이런 것들임 이건 내가 정한 게 아니라 지켜야되는 **타입 조건**임!!!

**Key의 타입은 무조건 문자열(String)**만 가능하고, Value 타입은 꽤나 다양해 보임

NULL 값도 가능하고 근데 타입 중에 **Value 타입으로 JSON 객체 타입**이 된단 게 이해 안 갈 수도 있기

에, 우리가 지금 배우고 있는 이 **JSON 객체** 또한 **Value 값에 다음과 같이 포함**될 수 있음!!

```json
{
    "programLanguage" : "Swift",
    "experienceLevel" : 1,
    "otherLanguage" : {
        "programLanguage" : "Python",
        "experienceLevel" : 0
    }
}
```

이런 식으로 사용되는 게 JSON 객체임!

# **1-2. JSON 배열**

JSON 객체에서 Swift에 대한 '**프로그래밍 언어**'에 대한 정보를 담기엔 더할 나위 없이 좋았음.

근데 만약 내가 '**여러 언어의 정보**'를 데이터로 만들고 싶다면, 어떻게 해야할까???

이때 사용하는 것이 바로 **JSON 배열**임! 우리가 평소 사용하던 배열과 똑같다 생각하면 됨!!

자 이 JSON 배열은 말이지,,

**대괄호 [ 로 시작**해서, **대괄호 ]** **로 끝나고**, 대괄호 사이에 **{원하는 아이템을 넣어주면 됨}**

```json
[{"programLanguage": "Swift", "experienceLevel": "1"}]
```

---

만약 2개의 언어 데이터를 다루고 싶다? 하면 JSON 객체와 똑같이 **,(comma)**를 이용해서 나열.

```json
[{"programLanguage": "Swift", "experienceLevel": "1"},
{"programLanguage": "Python", "experienceLevel": "0"}]
```

이때, **배열 안**에 들어가는 **아이템의 타입**은 매우 다양한데

![https://blog.kakaocdn.net/dn/cBPO8U/btqOYuM2b5e/rdK6nxbs8VHKV4Q26A3ZT1/img.png](https://blog.kakaocdn.net/dn/cBPO8U/btqOYuM2b5e/rdK6nxbs8VHKV4Q26A3ZT1/img.png)

뭐 이런 것들이 가능함, **배열 아이템으로 또 JSON 배열**이 들어갈 수도 있다. 대충 예제로 보여주겠음.

```json
// 정수
[1, 3, 5, 7, 9]
 
// 문자열
["a", "b", "c", "d", "e"]
 
//하위에 JSON 배열이 포함된 경우
[
    ["a", "b", "c", "d", "e"],
    ["A", "B", "C", "D", "E"],
    ["가", "나", "다", "라", "마"]
]
 
//하위에 JSON 객체를 나열한 경우
[
    {"programLanguage" : "Swift"},
    {"programLanguage" : "Python"}
]
```

이런 식으로 사용할 수 있는게 JSON 배열임!!

물론, JSON 배열을 다룰 때 **Swift는 Array 계열의 자료형**을 사용함!

배열인 만큼, Index가 중요하기 때문에, JSON Object처럼 **순서가 뒤바뀔 일이 없음**!

원하는 Text 형태의 데이터를 쉽고 간편하게!! 만들 수 있는 게 바로 JSON인 것이다..!
