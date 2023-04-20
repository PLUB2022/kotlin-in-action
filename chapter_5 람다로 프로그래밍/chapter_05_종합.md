# 5장 종합

# Ch_05. 람다로 프로그래밍

## joinToString

- 보통 어떨 때 쓸까? 
  - ex) 월, 화, 수, 목, 금 <- 이런 식으로 표현할 때 쓴다.

<br>

##  람다식을 변수에 저장할 때 타입 명시
- 람다식을 변수에 저장할 때 파라미터 타입을 명시할 것인지, 아니면 컴파일러가 추론할 수 있도록 할 것인지는 개발자의 선택이다.

- 람다의 파라미터가 하나뿐이고 그 타입을 컴파일러가 추론할 수 있는 경우 it 을 바로 쓸 수 있다.

- 람다를 변수에 저장할때는 파라미터의 타입을 추론할 문맥이 존재하지 않으면 파라미터 타입을 명시해야 한다.
```kotlin
val myLambda = { x -> x * x }

// Int -> Int 타입의 함수를 저장한다는 것을 추론할 수 있다. 따라서 이 코드는 파라미터 타입을 명시하지 않아도 된다.

val myLambda: (Int) -> Int = { x -> x * x }
// (Int) -> Int로 명시되어 있기 때문에, 컴파일러가 x가 Int 타입임을 추론할 수 있다.

```


## 람다가 포획(Capture)한 변수
코틀린에서는 자바와 달리 람다에서 람다 밖 함수에 있는 파이널이 아닌 변수에 접근할 수 있고, 그 변수를 저장할 수 있다. 

포획된 변수는 람다식이 선언된 위치에서 유효한 범위(scope)를 가지며, 람다식이 호출될 때까지 그 값이 유지된다
-> 이러한 동작 방식을 클로저(closure)라고 한다.

```kotlin
fun main() {
    var count = 0
    val incrementCount = { count++ }
    
    incrementCount()
    incrementCount()
    incrementCount()
    
    println("Count: $count")  // Output: Count: 3
}
```

incrementCount 람다식은 count 변수를 포획하여 값을 증가시키고 있다.



포획된 변수는 해당 람다식이 선언된 위치에서 유효한 범위를 가지므로, 해당 변수가 람다식이 호출될 때까지 유지되어야 한다.

```kotlin
fun createIncrementCount(): () -> Unit {
    var count = 0
    val incrementCount = { count++ }
    return incrementCount
}

fun main() {
    val incrementCount = createIncrementCount()
    
    incrementCount()
    incrementCount()
    incrementCount()
    
    var count = 10
    println("Count: $count")  // Output: Count: 10
    
    incrementCount()
    println("Count: $count")  // Output: Count: 10 (not 11)
}
```

createIncrementCount 함수가 호출될 때마다 새로운 count 변수가 생성된다.

이렇게 생성된 count 변수는 incrementCount 람다식이 포획한 변수와는 다른 변수이며, 값이 유지되지 않는다.


## 고차 함수

함수는 다른 함수를 인자로 받거나 함수를 반환하는 함수를 말한다. 코틀린에서는 람다 식을 사용하여 고차 함수를 구현할 수 있다.

예를 들어, 다음과 같이 인자로 함수를 받는 고차 함수를 구현할 수 있다.
```kotlin
fun calculate(x: Int, y: Int, operation: (Int, Int) -> Int): Int {
    return operation(x, y)
}
```
위 함수는 x와 y라는 두 개의 정수 인자와 operation이라는 함수 인자를 받는다. 
operation은 두 개의 정수를 받아서 정수를 반환하는 함수이고, calculate 함수는 operation 함수를 호출하여 x와 y를 연산한 결과를 반환한다.

이제 calculate 함수를 사용하여 다음과 같은 덧셈, 뺄셈, 곱셈 함수를 호출할 수 있다.

```kotlin
val sum = calculate(10, 5) { x, y -> x + y } // 15
val subtract = calculate(10, 5) { x, y -> x - y } // 5
val multiply = calculate(10, 5) { x, y -> x * y } // 50
```
위 코드에서 { x, y -> x + y }, { x, y -> x - y }, { x, y -> x * y }는 모두 람다 식으로, operation 인자로 전달된다. 각각 덧셈, 뺄셈, 곱셈을 수행하며 결과를 반환한다.

<br>

## 컴비네이션 패턴(Combinator Pattern)

함수형 프로그래밍에서 자주 사용되는 기법 중 하나로, 두 개 이상의 함수를 조합하여 새로운 함수를 만드는 것을 말한다.
이 패턴은 고차 함수를 사용하여 코드의 재사용성을 높이고 가독성을 높일 수 있는 장점이 있다.


컴비네이션 패턴은 보통 두 가지 유형의 함수 조합 기법으로 구성된다. 
- 함수 합성(Function Composition) 기법
- 함수 순서 결정(Function Ordering) 기법


코틀린에서는 함수 합성을 위해 `compose` 함수를 사용할 수 있다. 
`compose` 함수는 두 개의 함수를 인자로 받아 첫 번째 함수를 먼저 적용한 후 두 번째 함수를 적용하는 새로운 함수를 반환한다. 예를 들어, 다음과 같이 두 개의 함수를 합성하여 새로운 함수를 만들 수 있다.

```kotlin
fun square(x: Int) = x * x
fun addTwo(x: Int) = x + 2

val squareAndAddTwo = ::square.compose(::addTwo)
println(squareAndAddTwo(3)) // 25
```
위 코드에서 square 함수와 addTwo 함수를 합성하여 squareAndAddTwo 함수를 만든다.

squareAndAddTwo 함수는 addTwo 함수를 먼저 적용한 후 square 함수를 적용한다. 
따라서 squareAndAddTwo(3)은 square(addTwo(3))과 같으며, 결과는 25가 된다.

코틀린에서는 함수 순서 결정을 위해 andThen 함수를 사용할 수 있다. 
andThen 함수는 compose 함수와 동일한 기능을 수행하지만, 첫 번째 함수와 두 번째 함수의 순서를 바꿔서 적용한다.

```kotlin
val addTwoAndSquare = ::addTwo.andThen(::square)
println(addTwoAndSquare(3)) // 25
```

위 코드에서 addTwo 함수와 square 함수를 합성하여 addTwoAndSquare 함수를 만든다. 
addTwoAndSquare 함수는 square 함수를 먼저 적용한 후 addTwo 함수를 적용한다. 
따라서 addTwoAndSquare(3)은 addTwo(square(3))과 같으며, 결과는 11이 된다.


## all, any, count, find : 컬렉션에 술어 적용

서버에서 1:N 으로 매핑되어 있을 경우 특정 값을 찾을 때 any 나 all을 쓰기도 한다.
어떤 하나라도 조건을 만족하는지 확인하려면, any 함수를 사용하면 된다.
any 함수는 컬렉션의 요소 중 하나 이상이 조건을 만족하면 true를 반환하며, 만족하는 요소가 없으면 false를 반환한다.

```kotlin
val names = listOf("John", "Mary", "Bob")
val hasJohn = names.any { it == "John" }
```

만약 1:N 매핑에서 모든 값이 조건을 만족하는지 확인하려면, all 함수를 사용한다.
all 함수는 컬렉션의 모든 요소가 조건을 만족하면 true를 반환하며, 조건을 만족하지 않는 요소가 하나라도 있으면 false를 반환한다.

```kotlin
val names = listOf("John", "Mary", "Bob")
val allThreeChars = names.all { it.length == 3 }
```

## 함수를 적재적소에 사용하라 : count와 size

count 함수는 주어진 조건을 만족하는 요소의 개수만을 계산하기 때문에, 불필요한 요소까지 순회하지 않아도 되기 때문에 size 함수보다 약간 더 효율적이다

하지만, 컬렉션이 작거나 요소의 개수가 매우 작은 경우에는 차이가 미미하며, 
컬렉션의 크기가 커질수록 count 함수가 더 효율적일 수 있다.

따라서, 컬렉션의 크기를 반환하는 경우에는 size 함수를 사용하는 것이 좋고, 특정 조건을 만족하는 요소의 개수를 계산하는 경우에는 count 함수를 사용하는 것이 좋다. 
그러나 대부분의 경우에는 성능 차이가 미미하기 때문에, 두 함수 중 어떤 것을 사용해도 큰 문제는 없다.


## flatMap

flatMap 함수는 먼저 인자로 주어진 람다를 컬렉션의 모든 객체에 적용하고 람다를 적용한 결과 얻어지는 여러 리스트들을 한 곳에 모은다.

```kotlin
val list = listOf("Kotlin", "is", "awesome")
val flatMapResult = list.flatMap { it.toList() }
println(flatMapResult) // [K, o, t, l, i, n, i, s, a, w, e, s, o, m, e]
```

위 코드에서 flatMap 함수는 각 문자열을 문자 리스트로 변환하여 결과 리스트에 추가하고 결과적으로, 모든 문자 리스트가 하나의 리스트로 합쳐진 결과를 반환한다

```kotlin
val list = listOf(listOf(1, 2, 3), listOf(4, 5, 6), listOf(7, 8, 9))
val flatMapResult = list.flatMap { it }
println(flatMapResult) // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
위 코드에서 flatMap 함수는 list 리스트 안의 리스트들을 하나의 리스트로 합친다
결과적으로, 중첩된 리스트의 모든 요소들이 평탄화된 하나의 리스트가 반환된다



# 지연 계산 컬렉션 연산
코틀린에서 컬렉션과 시퀀스는 모두 데이터를 저장하고 처리하는 데 사용된다.

하지만, 컬렉션은 데이터를 메모리에 저장하고, 모든 데이터를 메모리에 올린 후에 처리를 수행한다. 
반면에 시퀀스는 데이터를 필요할 때마다 처리하고, 다음 데이터를 처리하기 위해 메모리에 저장하지 않는다.


이러한 처리 방식의 차이로 인해, 시퀀스는 대규모 데이터를 다룰 때 더욱 효율적이다.
시퀀스를 사용하면 처리 중에 필요한 데이터만 메모리에 올리기 때문에 메모리 사용량이 줄어들고, 처리 속도가 빨라진다.

```kotlin
val list = (1..1000000).toList()
val sequence = (1..1000000).asSequence()
```

위 코드에서 list는 1부터 1,000,000까지의 정수를 담은 리스트이고, sequence는 같은 정수들을 담은 시퀀스이다.
이제 각각의 컬렉션과 시퀀스에서 filter와 map 함수를 사용하여 같은 결과를 만든다면,

```kotlin
val listResult = list.filter { it % 2 == 0 }.map { it * 2 }
val sequenceResult = sequence.filter { it % 2 == 0 }.map { it * 2 }.toList()
```

위 코드에서 filter 함수를 사용하여 짝수를 필터링하고, map 함수를 사용하여 각각의 요소를 2배로 만들고, toList 함수를 사용하여 시퀀스를 다시 리스트로 변환한다.

```kotlin
val listStartTime = System.currentTimeMillis()
val listResult = list.filter { it % 2 == 0 }.map { it * 2 }
val listEndTime = System.currentTimeMillis()
val listElapsedTime = listEndTime - listStartTime

val sequenceStartTime = System.currentTimeMillis()
val sequenceResult = sequence.filter { it % 2 == 0 }.map { it * 2 }.toList()
val sequenceEndTime = System.currentTimeMillis()
val sequenceElapsedTime = sequenceEndTime - sequenceStartTime
```


이 둘을 비교하기 위해 위 코드로 각각의 컬렉션과 시퀀스에서 filter와 map 함수를 사용하는 데 걸리는 시간을 측정해보면

```kotlin
List Execution Time: 4991 ms
Sequence Execution Time: 2807 ms
```

다음과 같은 결과가 나온다. 

시퀀스가 처리 중에 필요한 요소만 처리하므로, 불필요한 데이터를 메모리에 올리지 않아 메모리 사용량이 적어지고, 이로 인해 처리 속도가 빨라지기 때문이다
또한, 시퀀스는 지연(lazy) 계산 방식을 사용하기 때문에, 필요한 시점에만 계산이 이루어지므로, 중간 처리 결과를 저장하는 임시 컬렉션을 생성할 필요도 없다.


그러나 시퀀스는 컬렉션보다 처리 속도가 빠르지만, 처리 결과를 다시 사용해야 하는 경우에는 컬렉션으로 변환해야 한다. 이는 시퀀스가 지연(lazy) 계산 방식을 사용하기 때문에, 한 번 처리된 데이터를 메모리에 저장하지 않고, 다시 계산해야 하기 때문이다.

따라서, 시퀀스는 대용량 데이터를 처리하는 데 효율적이지만, 중간 결과를 저장할 필요가 없는 단순한 연산의 경우에는 컬렉션보다 더욱 효율적이다. 반면에 중간 결과를 저장하고 재사용해야 하는 경우에는 컬렉션을 사용하는 것이 더욱 효율적이다


## 컬렉션과 시퀀스에 map filter 동작 순서

![55](https://user-images.githubusercontent.com/55054505/233276797-4ef55580-6841-4170-a178-6856c5d44369.jpg)


컬렉션에서 map과 filter를 사용하면, 모든 원소에 대해 순차적으로 연산을 수행한다. 
따라서 컬렉션의 크기가 크면 연산이 오래 걸릴 수 있다. 또한 중간 결과를 모두 저장하기 때문에 메모리 사용량도 크게 증가한다.

반면 시퀀스에서 map과 filter를 사용하면, 연산 결과가 필요할 때까지 각 원소에 대한 연산을 지연(lazy)한다. 
이렇게 지연되어 연산이 수행되는 방식을 "lazy evaluation" 라고 한다. 
즉, 시퀀스의 모든 연산은 필요한 경우에만 수행되고, 이러한 방식으로 연산을 처리하면, 중간 결과를 모두 저장하지 않으므로 메모리 사용량도 줄어들고, 연산이 필요한 원소만 연산을 수행하기 때문에 처리 시간도 줄어든다


### takeWhile
시퀀스나 리스트에서 주어진 조건이 만족되는 동안의 원소를 추출하는 함수이다.

예를 들어, listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10) 리스트가 있을 때, takeWhile을 사용하여 짝수인 원소만 추출하고 싶다면 다음과 같이 사용할 수 있다

```kotlin
val result = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10).takeWhile { it % 2 == 0 }
```

위의 코드에서 takeWhile은 it % 2 == 0 조건이 만족되는 동안의 원소를 추출하므로, result 리스트에는 [2, 4]가 저장된다.

takeWhile은 조건이 만족되지 않는 원소가 처음으로 나오면 즉시 추출을 멈추고 결과를 반환한다. 이러한 특성 때문에, takeWhile은 시퀀스나 리스트의 일부분을 추출할 때 유용하게 사용될 수 있다.


## 람다와 무명객체 차이

람다식은 새로운 객체를 생성하는 것이 아니라 함수 호출 시 매번 재사용이 가능한 객체로 컴파일된다. 따라서 람다식을 변수에 대입하고 여러 번 사용하더라도 같은 객체가 사용된다.

하지만, 람다식에서 자신이 사용하는 변수가 외부 스코프에 선언된 것인 경우, 람다식은 그 변수를 포획(capture)하게 된다. 이 경우 람다식에서 사용하는 변수가 외부 스코프의 변수와 동일한 값을 유지하기 위해서는 람다식이 새로운 객체를 생성하지 않고 외부 스코프의 변수를 참조해야 한다. 이때, 매 호출마다 같은 인스턴스를 사용할 수 없는 이유는 람다식에서 사용하는 변수가 외부 스코프에서 변경될 가능성이 있기 때문이다.

즉, 람다식에서 외부 스코프의 변수를 포획하게 되면 해당 변수가 변경될 때마다 매번 새로운 객체를 생성해야 한다. 이렇게 새로운 객체를 생성해야 하는 람다식을 "캡처링 람다(capturing lambda)"라고 한다.

```kotlin
fun foo(): () -> Unit {
    val x = 1
    val f = { println(x) } // 새로운 객체 생성
    val g: () -> Unit = { println(x) } // 메서드 호출마다 같은 객체 재사용
    return g
}
```

## inline

코틀린에서 println 내부를 보면

```kotlin
@kotlin.internal.InlineOnly
public actual inline fun println(message: Any?) {
    System.out.println(message)
}
```
이렇게 inline 으로 설정되어 있다. 

파라미터로 함수를 받는 경우에는 함수를 호출하는 과정에서 오버헤드가 발생할 수 있다.
이 때 inline 함수를 사용하면 함수 호출을 최적화할 수 있다.

inline 함수를 사용하면, 컴파일러가 해당 함수를 호출하는 코드에서 함수의 본문 코드를 직접 복사하여 삽입한다. 이렇게 하면 함수 호출의 오버헤드를 줄일 수 있으며, 함수 호출 대신 본문 코드가 직접 실행되므로 실행 시간도 단축된다.

inline 함수 내에서 파라미터로 전달된 함수를 사용하는 경우, 해당 함수도 inline으로 선언되어야 한다.
이는 inline 함수 내에서 파라미터 함수를 호출할 때, 해당 함수도 함수 호출 대신 함수 본문 코드를 직접 삽입하여 최적화하기 때문이다

```kotlin
inline fun calculate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}
```

위 함수는 a와 b를 인자로 받고, operation이라는 함수를 받아서 해당 함수를 호출하여 결과값을 반환한다

```kotlin
val result = calculate(10, 20) { a, b -> a + b }

```
위 코드에서는 10과 20을 더하는 함수를 operation 파라미터로 전달하는데, 이 때, calculate 함수를 inline으로 선언하면, 해당 함수 호출 시 operation 함수를 직접 호출하는 코드로 대체된다


```kotlin
inline fun calculate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

val result = 10 + 20 // operation 함수가 호출되지 않음
```
따라서 inline 함수를 사용하면 함수 호출에 따른 오버헤드를 줄일 수 있으며, 파라미터로 전달된 함수도 inline으로 선언하여 최적화할 수 있다.


하지만 일반적인 함수와는 달리, 람다 식이나 함수 타입 매개변수를 인라인 함수에서 사용하면 새로운 무명 클래스 객체가 생성된다. 이는 인라인 함수 호출 시 해당 람다 식 또는 함수 타입을 처리하기 위해 런타임에 객체가 필요하기 때문이다.

따라서 inline 함수를 사용하여 람다 식 또는 함수 타입 매개변수를 처리하는 경우, 무명 클래스 객체를 생성하지 않으려면 함수 선언에 inline 키워드를 추가해야 한다. 이렇게 하면 인라인 함수 호출 시 해당 람다 식 또는 함수 타입 매개변수가 컴파일 시간에 인라인 되므로, 무명 클래스 객체를 생성할 필요가 없어진다.

## infix 함수

infix는 함수 선언 시에 사용되는 특별한 키워드이다. infix를 사용하면 해당 함수를 중위 표기법(infix notation)으로 호출할 수 있게 된다.

일반적으로 함수 호출은 전위 표기법(prefix notation)으로 수행된다. 예를 들어, a.plus(b)와 같은 방식으로 함수를 호출한다. 그러나 infix를 사용하여 선언된 함수는 a plus b와 같은 중위 표기법으로 호출할 수 있다.


ex)

```kotlin
infix fun Int.plusFive(other: Int): Int {
    return this + other + 5
}
```
위 함수는 Int 타입의 두 개의 값을 더하고, 5를 더한 결과를 반환한다. 이 함수를 중위 표기법으로 호출하면 다음과 같다.

```kotlin
val result = 10 plusFive 5
```

위 코드에서는 plusFive 함수를 중위 표기법으로 호출하고 있다. infix 키워드를 사용하여 함수를 선언하면 함수 호출이 더욱 간결해지고 가독성이 향상될 수 있다.


## 커링 Currying
커링(Currying)은 여러 개의 인자를 갖는 함수를 하나의 인자를 받는 함수들의 중첩된 함수로 만드는 것을 의미한다.
이를 통해, 함수의 재사용성과 가독성을 높일 수 있다.

커링을 이용하면 함수의 일부 파라미터를 미리 고정하여 새로운 함수를 생성할 수 있다. 이를 통해, 함수를 인자로 받는 함수를 만들 수 있는 고차 함수를 더욱 쉽게 작성할 수 있다.

이 때, infix 함수를 이용하면 함수를 더욱 직관적으로 작성할 수 있습니다. infix 함수는 중위 표기법을 사용하여, 함수 이름과 함수 호출 사이에 인자를 넣어 함수를 호출할 수 있는 함수이다.

ex)

```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}
```
이 함수를 커링하여 새로운 함수를 만들어 볼 수 있다. 이 때, infix를 이용하면, 아래 처럼 된다.

```kotlin
infix fun Int.add(b: Int): Int {
    return this + b
}
```
이제, 다음과 같이 인자 하나만을 받는 새로운 함수를 만들 수 있다.

```kotlin
val add5 = 5.add // add 함수를 인자 하나만 받는 함수로 커링
val result = add5(3) // 5 + 3 = 8
```
위 코드에서, add5는 add 함수를 커링하여 인자 하나만을 받는 새로운 함수를 생성한 것이다. 이렇게 infix 함수를 이용하면 함수를 더욱 직관적으로 작성할 수 있으며, 커링과 함께 사용하면 함수의 재사용성과 가독성을 높일 수 있다


# with, apply

with와 apply는 둘 다 객체를 다루는 함수로, 객체의 속성을 변경하거나 연산을 수행할 때 사용된다

- with
with 함수는 객체를 인자로 받아서 객체의 속성에 접근할 수 있는 블록을 실행하고, 마지막 표현식의 값을 반환한다.
 with 함수를 사용하면 일시적으로 객체의 속성에 접근할 수 있는 스코프를 만들 수 있다.

 ```kotlin
class Person(var name: String, var age: Int)
 ```

 이런 함수가 있다고 가정을 했을 때, 

```kotlin
 val person = Person("John Doe", 30)
with(person) {
    name = "Jane Doe"
    age += 1
}
```

 with(person) 블록 내에서 person 객체의 name 속성과 age 속성에 직접 접근하여 값을 변경할 수 있다.

 - apply
apply 함수는 객체를 인자로 받아서 객체 자신을 반환한다. apply 함수를 사용하면 객체의 속성을 변경하고, 변경된 객체를 반환하는 코드를 간결하게 작성할 수 있다

```kotlin
val person = Person("John Doe", 30).apply {
    name = "Jane Doe"
    age += 1
}

```

with 함수는 일시적인 스코프를 만들어 객체의 속성에 접근할 수 있게 하고, 
apply 함수는 객체 자신을 반환하여 객체의 속성을 변경하고 반환하는 코드를 간결하게 작성할 수 있게 해준다.

## let 과 run 차이

let과 run의 차이점:

let: 호출한 객체를 인자로 받아 lambda 내에서 사용 가능하도록 한 후 결과를 반환
```kotlin
val str = "Hello World"
val length = str.let { it.length }
```
run: 호출한 객체를 인자로 받아 lambda 내에서 사용 가능하도록 한 후 lambda의 마지막 표현식을 반환
```kotlin
val str = "Hello World"
val uppercaseStr = str.run { this.toUpperCase() }
```