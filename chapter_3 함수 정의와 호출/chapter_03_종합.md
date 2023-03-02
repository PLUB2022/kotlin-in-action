# Ch_03. 함수 정의와 호출

## 제네릭 vs. Any vs. Raw Type(와일드카드, ?)

제네릭 : 타입의 일반화, 유연한 타입으로 컴파일 시점에 타입 체크를 할 수 있는 문법

Any : 코틀린의 최상위 클래스

Raw Type : 제네릭 파라미터의 ? 값으로 아무 타입이나 가능하다는 뜻

(1) mutableListOf<Any>()와 (2) mutableListOf<?>의 차이

(1)은 add를 할 땐 아무 타입이나 가능하지만, 꺼내서 사용할 때는 캐스팅이 필요하다.

그리고 어떤 타입이 가능한지 컴파일 시점에 알 수 없다. (런타임 시점에만 파악 가능 → 단점)

(2)애초에 컴파일 조차 되지 않는다.

결론 : 정적 코드 상으로도 타입을 한 눈에 체크할 수 있는 제네릭을 사용하고, 와일드카드와 Any는 지양한다.

## @JvmStatic

코틀린에는 static 키워드가 없다. 코틀린 파일을 컴파일한 바이트 코드를, 다시 자바로 되돌렸을 때 static 키워드가 붙게하려면 @JvmStatic을 붙혀주면 된다.

```kotlin
companion object {
    @JvmStatic
    fun newInstance() {
    }
}
```

이 기능은 어떨때 필요한가? 코틀린 함수를 자바에서 호출 할 때 static이 붙은 키워드로 호출하고 싶을 때 사용한다.

## JoinToString()

해당 함수는 자바에는 존재하지 않는다. 코틀린 차원에서 확장함수로 구현한 것이다.

## @JvmName

코틀린에서는 최상위 함수를 선언할 수 있다. 이는 자바코드로 변환하면, 동일한 이름의 클래스를 만들어주는데 이 때 해당 클래스 이름을 변경하고 싶으면 사용한다.

코틀린 함수 (join.kt)

```kotlin
fun joinToString(...): String { ... }
```

자바 변환 (코틀린 파일의 이름이 클래스 이름으로 된다.)

```java
public class JoinKt {
    public static String joinToString(...) { ... }
}
```

자바에서 해당 코틀린 함수 호출하기

```java
JoinKt.joinToString(...);
```

코틀린 파일에서 해당 클래스 이름 변경하기

```kotlin
@file:JvmName("StringFunctions")
// 위 어노테이션 뒤에 패키지가 와야한다.
package strings

fun joinToString(...): String { ... }

/* Java 에서 호출 */
StringFunctions.joinToString(...);
```

## OS마다 개행 문자 차이

[https://www.devkuma.com/docs/windows/windows-unix-end-of-line/](https://www.devkuma.com/docs/windows/windows-unix-end-of-line/)

![image](https://user-images.githubusercontent.com/85692623/222476439-50b65b14-ff0f-40c7-b6ac-a132c0c5ddbb.png)




## (Spring Boot) 확장 함수 응용 : 서비스 레이어에서 엔티티 확장하기

확장함수를 활용하면 엔티티의 부가 메서드를 서비스레이어에서 정의할 수 있다.

```kotlin
class User {
    var name: String = ""
    var age: Int = 0
}

data class UserDto(
    var name: String = "",
    var age: Int = 0
)

class UserService {
    // 엔티티의 캡슐화를 지키면서, 필요한 서비스 계층에서만 확장 후 사용
    private fun User.toDto() = UserDto(name, age)
}
```

## 확장 함수의 이점

확장 함수는 자바 기준 static으로 컴파일 되기 때문에, 런타임 시점에서 추가 생성 비용이 존재하지 않는다.

## 가변인자는 Array만 가능

가변인자를 받는 스프레드 연산자 (*)은 Array만 가능하다.

listOf(), MutableListOf()는 안 된다.
