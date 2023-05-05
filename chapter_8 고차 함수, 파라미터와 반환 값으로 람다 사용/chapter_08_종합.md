# 고차함수

다른 함수를 인자로 받거나 함수를 반환하는 함수

- 코틀린의 함수는 일급객체라 고차함수의 존재가 가능하다.
    - 복습: 일급객체란 무엇인가?

# 인라이닝

함수 호출의 오버헤드를 줄이기 위해 컴파일러가 함수의 본문을 호출 위치에 삽입하는 최적화 기술

(이는 람다를 인자로 받는 함수에서 람다 호출시 오버헤드가 발생하는데 이를 줄일 수 있음)

왜 람다를 인자로 받는 함수 내부에서 람다를 호출하면 오버헤드가 발생하는가?

- 람다가 변수를 포획하면 람다가 생성되는 시점마다 새로운 무명 클래스 객체가 생성된다. → 실행 시점에 무명 클래스 생성에 따른 오버헤드 발생

inline 키워드를 람다를 인자로 받는 함수 앞에 명시해주면, 컴파일러가 컴파일 시점에 인라이닝을 수행한다.

## **예시1**

다음과 같은 예시 함수가 있다고 가정하자

performOperation 함수는 인자로 받은 람다, operation을 단순히 호출한다.

```kotlin
inline fun performOperation(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}
```

람다를 인자로 받기 때문에, operation은 performOperation 호출 시 명시적으로 선언해야한다.

```kotlin
val result = performOperation(5, 3) { x, y -> x + y }
```

위는 operation 함수를 단순히 전달받은 인자 두개를 더하는 연산으로 정의한 케이스다. 결과값은 8이 나온다.

이렇게 코드를 작성했으면, 컴파일러는 inline 키워드를 확인하고 컴파일 시점에 해당 코드를 나름대로 해석 및 조작(?)하여 최적화를 진행한다.

```kotlin
val result = { x: Int, y: Int -> x + y }(5, 3)
```

1차 인라이닝을 수행하면 performOperation에서 operation을 수행하는 부분을 실제 코드로 바꿔치기한다.

위 결과값은 위 람다를 호출하는 것과 동일하다. 다만 호출되는 함수의 depth가 달라진다.

as is : performOperation → operation

to be : operation

위 람다를 한번 더 인라이닝 하여,

```kotlin
val result = 5 + 3
```

최종적으로 이런식으로 컴파일을 수행하여 바이트코드를 생성한다.

## **예시2**

각 리스트를 순회하면서 action을 호출하는 고차함수가 있다고 가정해보자

```kotlin
inline fun <T> List<T>.applyForEach(action: (T) -> Unit) {
    for (element in this) {
        action(element)
    }
}
```

그리고 다음과 같이 개발자가 코드를 작성했다.

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
numbers.applyForEach { number -> println(number) }
```

이때, 인라이닝은 다음과 같이 수행된다.

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
for (element in numbers) {
    { number: Int -> println(number) }(element)
}
```

먼저 applyForEach의 본문을 그대로 코드에 삽입한뒤, 

 

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
for (element in numbers) {
    println(element)
}
```

람다 호출도 제거하여 불필요한 무명 클래스의 생성을 방지한다.


## 인라이닝의 최적화는 의미가 있는가? 에 대한 토론

인라이닝의 목적은 결국 람다 호출시 생성되는 불필요한 익명 객체 생성을 방지하는 것이다.

현대는 하드웨어 자원도 풍부하고, GC의 성능도 좋은 시점에서 객체 몇개의 추가 생성의 의한 오버헤드가 체감적일까?라는 의문이 들었다.

IoT나 하드웨어 자원을 극한으로 효율적으로 관리해야하는 분야에선 kotlin을 사용하지 않기 때문이다. 

결론 : 그래도 인라이닝을 사용해서 최적화를 진행하기 때문에, 없는 것 보단 낫지않을까 라는 결론이 났다.

## 람다와 익명함수의 차이점
<img width="764" alt="스크린샷 2023-05-05 오전 11 46 36" src="https://user-images.githubusercontent.com/85692623/236370606-d0373bf8-c493-40af-aa6d-2707dbadd927.png">


## Non-local return 문

자신을 둘러싸고 있는 블록 보다 더 바깥에 있는 다른 블록을 반환하게 만드는 return문

- 레이블을 사용해서 표현한다.
- 이는 인라인 함수에서만 가능함을 주의

```kotlin
fun processList(list: List<Int>, predicate: (Int) -> Boolean): Boolean {
    list.forEach { element ->
        if (predicate(element)) {
            return@processList true // non-local return
        }
    }
    return false
}

fun main() {
    val numbers = listOf(1, 2, 3, 4, 5)
    val result = processList(numbers) { number -> number > 3 }
    println(result) // 출력: true
}
```

일반적으로 람다 내부에서 return을 하면 람다식을 끝내지만, 레이블을 통해 레이블 자체 함수를 return 하도록 설정할 수 있다.

만약 넌로컬이 불편하고 가독성을 떨어뜨린다고 판단되면, 무명함수를 활용하여 로컬 return을 명시적으로 활용하는 것도 좋은 방법이다.

### 왜 넌로컬 리턴문은 인라인 함수에서만 가능한가?
<img width="764" alt="스크린샷 2023-05-05 오후 12 11 49" src="https://user-images.githubusercontent.com/85692623/236370533-82fbdb9b-50d8-4841-9d5c-e8b29ef79e55.png">

