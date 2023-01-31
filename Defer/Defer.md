**Defer: 미루다 연기하다 이런 뜻.. defer 는 보통 함수안에서 작성되는 클로저인데, 작성된 위치와 상관없이 함수 종료 직전에 실행되는 구문.** 자기의 실행을 함수의 **맨 마지막으로 미룬다** 뭐 이런느낌임

****2-1. defer를 읽기 전에 함수가 종료되면 defer는 실행되지 않는다****

```swift
func testDefer() {
    print("Check #1")
    
    return;
    defer { print("defer #1") }
    
    print("Check #2")
}
 
testDefer()

// Check #1
```

****2-2. 하나의 함수에서 여러 번 defer를 호출 가능하며, 실행 순서는 가장 마지막에 실행된 defer부터 역순이다****

```swift
func testDefer() {
    defer { print("defer #1") }
    defer { print("defer #2") }
    defer { print("defer #3") }
}
 
testDefer()

// defer #3
// defer #2
// defer #1
```

****2-3. defer는 중첩해서도 사용 가능하며, 실행 순서는 가장 바깥쪽 defer부터 실행된다 *** 이때는 **가장 바깥 쪽에 있는 defer가** **가장 먼저 실행**되고, **가장 안쪽에 있는 defer가** **가장 마지막에 실행** 됨 *

```swift
func testDefer() {
    defer {
        defer {
            defer {
                print("defer #3")
            }
            print("defer #2")
        }
        print("defer #1")
    }
}

// defer #1
// defer #2
// defer #3
```

# 3. 언제사용할까?

Defer 가 읽힌 이후론 함수의 종료 직전에 실행된다는걸 알았슴. 그럼 Defer 는 언제 사용할까?

## 함수를 종료하기 직전에 정리해야 하는 변수나 상수를 처리하는 용도로 사용됨

예를 들어, NSLock 을 이용해 상호배제 [**Mutual exclusion (상호 배제) equivalent to @synchronized in Obj-C]** 

상호배제를 걸때 함수가 종료되기 전에 Lock 이 걸린 경우, 이 Lock 을 풀어주어야 하는데 데드락이 걸리지않는다.

그렇지만 함수가 종료되는 시점마다 Lock 을 풀어주는 코드를 추가하는 것보다 Defer 를 사용해서 알아서 종료되기 전에 Lock 을 풀어줄수있다.

```swift
let myLock: NSLock = .init()

func fetchData() {
    myLock.lock()
    
    defer { myLock.unlock() }
    
    if data == nil { return }
    
    //이후 작업
}
```

SQLite 를 사용할때 Defer 를 사용해서 DB 를 닫아주는 용도로도 사용함
