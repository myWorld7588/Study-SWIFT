# COW (Copy-on-Write)

### 컴퓨터 프로그래밍에서 복사 동작을 할 때, 실제 원본이나 복사본이 수정되기 전까지는 복사를 하지 않고 원본 리소스를 공유함.  원본이나 복사본에서 수정이 일어날 경우. 그때 복사하는 작업을 함

**이 작업을 Swift 에선 Collection Type을 복사해서 사용할 때 일어남**

**Array, Dictionary, Set** 이런 **Collection Type** 들

![https://blog.kakaocdn.net/dn/ce68ec/btqF7MvgYYa/CTgnirALaFdCCUNWlPBgCK/img.png](https://blog.kakaocdn.net/dn/ce68ec/btqF7MvgYYa/CTgnirALaFdCCUNWlPBgCK/img.png)

자 위와 같은 `array1`이라는 배열이 있음 메모리상에는

![https://blog.kakaocdn.net/dn/enbBLq/btqNU1quW9K/RzI0l7WlDroBd8wemmFXkk/img.png](https://blog.kakaocdn.net/dn/enbBLq/btqNU1quW9K/RzI0l7WlDroBd8wemmFXkk/img.png)

이렇게 되겠찌?! 자 근데 여기서 `array2`를 만들어 보자

![https://blog.kakaocdn.net/dn/biUt8N/btqGbb8tSHy/L1owCWu8eqtrC0GRVLekM1/img.png](https://blog.kakaocdn.net/dn/biUt8N/btqGbb8tSHy/L1owCWu8eqtrC0GRVLekM1/img.png)

그리고 `array1`의 값을 `array2`에 대입했음

Swift는 **array**가 무슨 형식이다?`@frozen public **struct** Array<Element>`

**구조체 형식**이다 따라서 저렇게 대입한 것은 `array1`에서 `array2`로 **값** **복사**가 일어났다는 것임

자 그럼 원래 대로라면

![https://blog.kakaocdn.net/dn/cXwdyh/btqNWgHdKeR/JwD3jeLyXadPb0TWgNCUjk/img.png](https://blog.kakaocdn.net/dn/cXwdyh/btqNWgHdKeR/JwD3jeLyXadPb0TWgNCUjk/img.png)

이렇게 `array2`가 바로 새로운 메모리를 할당해서 `array1`의 값을 복사해서 들고 있어야 정상임

**근데 COW를 쓰면 뭐다? 바로 복사를 하지 않는다!** COW를 사용하면 Swift에선 자주 안 쓰이는 단어지만 다음처럼 **array2가 array1**을 **참조**하고 있는 형태가 됨

![https://blog.kakaocdn.net/dn/lACwo/btqNVR1Y0ki/xrQEfpQFw9EBDTVBDRYUkK/img.png](https://blog.kakaocdn.net/dn/lACwo/btqNVR1Y0ki/xrQEfpQFw9EBDTVBDRYUkK/img.png)

이런 식으루! 근데 이러다가

**원본(array1)이나 복사본(array2) 둘 중 하나를 수정한다????!!**

![https://blog.kakaocdn.net/dn/cX5laX/btqF9moNVNu/J42XTKGzQrsSkVKgHHDUy0/img.png](https://blog.kakaocdn.net/dn/cX5laX/btqF9moNVNu/J42XTKGzQrsSkVKgHHDUy0/img.png)

요롷게 말이징 그럼 이때!!! 첫 수정이 일어난 이 때!!

**비로소 찐 복사가 일어나서 array2의 값이 메모리에 할당됨**

![https://blog.kakaocdn.net/dn/cXwdyh/btqNWgHdKeR/JwD3jeLyXadPb0TWgNCUjk/img.png](https://blog.kakaocdn.net/dn/cXwdyh/btqNWgHdKeR/JwD3jeLyXadPb0TWgNCUjk/img.png)

이해감? 위 그림이 처음 복사 했을 때 나오는 게 아니라

**첫 수정**이 일어 났을 때 나오는 것임 ㅇㅋ?

이것이 바로 COW 임

실제 swift 결과물을 봐도 알 수 있셈!!

![https://blog.kakaocdn.net/dn/eD4vJc/btqF8v1cluS/jYFNc9AeZA2T1d3KY6zVdk/img.png](https://blog.kakaocdn.net/dn/eD4vJc/btqF8v1cluS/jYFNc9AeZA2T1d3KY6zVdk/img.png)

실제 수정이 일어나야 주소값이 바뀌는 거 보임? 👀

원본이든 복사본이든 수정이 먼저 일어난 놈의 주소값이 바뀌는 것을 알 수 있다!

## **2. 사용 용도**

그럼 왜써..?? 결국 수정 일어날 때 복사해야 하는 거 아냐? 라고 생각할 수도 있지만

**복사를 한다고 해서 꼭 수정을 하는 것은 아님** 만약 10000000개의 element를 가지는 배열 array1이 있을 때, 이를 array2로 복사 했음 근데 COW가 아니라면 메모리가 그만큼 두 배로 늘어났을 거 아님?

근데 내가 원본이나 복사본이나 수정을 안함. 앞으로도 할 일 없음. 이럴 경우엔 COW를 사용했을 때보다 2배의 메모리를 먹고 있는 것임 이렇듯 **실제 수정이 이뤄질 때 복사를 하고 그 전엔 참조를 통해  불필요한 복사 를 줄이자!** 라는 것이 COW의 장점이자 사용 이유임

## **3. 특징? 한계?**

한계라고 하긴 좀 애매한데 맨 처음 COW를 이해하고 든 생각 **그럼 첫 수정 작업을 할 땐 COW 때문에 실행 시간이 더 걸리겠네?** 그래서 시도해봤음

![https://blog.kakaocdn.net/dn/bzLi6S/btqF9lwYNFC/thi4K2G45UzJGo5XzkGkz1/img.png](https://blog.kakaocdn.net/dn/bzLi6S/btqF9lwYNFC/thi4K2G45UzJGo5XzkGkz1/img.png)

**결과적으로 COW 때문에 첫 번째 수정 작업 때 시간 작업이 더 걸림 복사 작업을 이때 해야하니께**

찾아보니까 **“While adding a small overhead to resource-modifying operations”** 약간의 오버헤드를 발생시킨다 함
