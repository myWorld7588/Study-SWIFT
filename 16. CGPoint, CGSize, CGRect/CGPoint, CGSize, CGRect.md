## **1. View의 위치와 크기를 결정하는 방법**

iOS에서 View를 그리기 위해선 어떤게 필요할까? 단순하게 View 하나를 놓고 본다면

먼저, **어디에 그릴지 위치**가 필요함

![https://blog.kakaocdn.net/dn/tu18Z/btqLxuWRm3D/VBkP3iKHrkPeXCOE1bMUIk/img.png](https://blog.kakaocdn.net/dn/tu18Z/btqLxuWRm3D/VBkP3iKHrkPeXCOE1bMUIk/img.png)

이처럼 View의 **시작 위치**를 알기위한  **x, y 좌표**가 필요하고,

이 좌표는 **iOS 뷰 기준점인 왼쪽 꼭대기(0, 0)으로부터 시작**함

또, 한 가지 더 필요함 Label 같은 유동적인 크기가 아니라면

**시작지점부터 어느 크기만큼 그릴 건지 width, height가 필요**함

![https://blog.kakaocdn.net/dn/bAeLQt/btqLwMcpAhg/VhVlgRKZVaBG9aFGUGZliK/img.png](https://blog.kakaocdn.net/dn/bAeLQt/btqLwMcpAhg/VhVlgRKZVaBG9aFGUGZliK/img.png)

자, 이렇게 기초적으로 View를 그릴 때 필요한 것들에 대해 알아 봤음

결론 적으로, **x 좌표, y 좌표, width, height** 이 네가지가 필요함

### 이제부터 설명할 CG 3총사가 **View를 그릴 때 필수**적으로 알아야하는 것들임

## **2. (x, y) 좌표를 설정할 수 있는 CGPoint**

자, CGPoit라는 구조체 생김새에 대해 먼저 보겠음

![https://blog.kakaocdn.net/dn/1iWtP/btqLwMjdDFp/ZBPHkZmcPvslkz07sZUfyk/img.png](https://blog.kakaocdn.net/dn/1iWtP/btqLwMjdDFp/ZBPHkZmcPvslkz07sZUfyk/img.png)

여기서 중요한 것은 CGPoint는 **x, y 라는 Float 변수**를 가지고 있단 것임

따라서 **View의 위치를 나타낼 땐 이 CGPoint**를 이용함 CGPoint가 꼭 View의 위치를 나타낼 때만 쓰는 것은 아님 뭐 x, y를 나타내야 할 땐 언제든 CGPoint를 쓸 수 있음

![https://blog.kakaocdn.net/dn/MGQZT/btqLAyRyYJ1/lbUbmqqyO6k2b8XYjkilwK/img.png](https://blog.kakaocdn.net/dn/MGQZT/btqLAyRyYJ1/lbUbmqqyO6k2b8XYjkilwK/img.png)

실제 사용은 이런 식으로 함!!! 이제 CGPoint를 언제 쓰는지 이해감?

## **3. (width, height) 사이즈를 설정할 수 있는 CGSize**

자, CGSize란 구조체에 대해서도 보자

![https://blog.kakaocdn.net/dn/cIXCAs/btqLC1ZM6lK/hDWLNP8ZcmTRyk8EAmCdTK/img.png](https://blog.kakaocdn.net/dn/cIXCAs/btqLC1ZM6lK/hDWLNP8ZcmTRyk8EAmCdTK/img.png)

**CGSize**는 **width, height**를 가지고 있음 따라서 view의 size를 설정할 때는 이 CGSize를 사용 함!

![https://blog.kakaocdn.net/dn/d4vlnv/btqLzY32Sel/v7Wi330KK9oyS3exzkyiQK/img.png](https://blog.kakaocdn.net/dn/d4vlnv/btqLzY32Sel/v7Wi330KK9oyS3exzkyiQK/img.png)

자 근데 여기서 질문 View를 구성할 땐 **x, y, width, height가 필요**하고 이들은 **CGPoint, CGSize를 이용해서 구현**한다 했는데.. 실제 UIView를 init할 땐 frame의 파라미터로 **CGPoint도, CGSize도** 

**아닌 CGRect**라는 놈이 들어감... 이 놈은 뭘까!?!!?!

![https://blog.kakaocdn.net/dn/bk50Jj/btqLC1rWWVD/YkTeL0r0bfmBuUeIXXLzYK/img.png](https://blog.kakaocdn.net/dn/bk50Jj/btqLC1rWWVD/YkTeL0r0bfmBuUeIXXLzYK/img.png)

## **3. CGSize와 CGPoint를 품은 CGRect**

제목 말 그대로임 CGRect 구조체를 보면

![https://blog.kakaocdn.net/dn/mNAgt/btqLyKLXwJt/pA4Nm5tXCbcYCZfXs93J90/img.png](https://blog.kakaocdn.net/dn/mNAgt/btqLyKLXwJt/pA4Nm5tXCbcYCZfXs93J90/img.png)

자 보이심!? **CGPoint 타입의 변수 origin, CGSize 타입의 변수 size** 가 각각 존재함 따라서 View를 나타낼 때 **origin이란 것은 x, y 좌표를, size라는 것은 width, height를** 나타낼 때 쓰는구나! 하면 됨

실제 View의 frame에 접근할 때에는 

![https://blog.kakaocdn.net/dn/caVgHw/btqLC1lcuWW/EEAFG4QDKc7BflnEbXXv8K/img.png](https://blog.kakaocdn.net/dn/caVgHw/btqLC1lcuWW/EEAFG4QDKc7BflnEbXXv8K/img.png)

이런 식으루 **CGPoint, CGSize를 포함한 CGRect를 통해 한번에 정의**할 수 있음

근데 위 문법을 더 간단하게 이렇게 표현하여 사용함!!!!

![https://blog.kakaocdn.net/dn/bAr82I/btqLyLRV2tS/KLQDF1UmztNx29FLSmgho1/img.png](https://blog.kakaocdn.net/dn/bAr82I/btqLyLRV2tS/KLQDF1UmztNx29FLSmgho1/img.png)

실제 뷰를 정의 할 때도 이렇게 CGRect를 사용해서 정의하면

![https://blog.kakaocdn.net/dn/cDyfj4/btqLC1S2zO0/HqqLG3ROQdIguK9uRZ5NC1/img.png](https://blog.kakaocdn.net/dn/cDyfj4/btqLC1S2zO0/HqqLG3ROQdIguK9uRZ5NC1/img.png)

내가 지정한 x, y 좌표에 내가 지정한 width, height로 그려진다
