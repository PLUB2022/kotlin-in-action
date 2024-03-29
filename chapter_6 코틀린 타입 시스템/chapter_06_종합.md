## P.266
```kotlin
fun <T> printHashCode(t: T) { //T는 null이 될 수 있다,
  println(t?.hashCode()) //t는 null이 될 수 있으므로 안전한 호출을 써야만 한다.
}

>>>printHashCode(null) //T의 타입은 Any?로 추론된다.
null
```   

타입 파라미터 T를 클래스나 함수 안에서 타입 이름으로 사용하면 이름 끝에 물음표가 없더라도 T가 널이 될 수 있는 타입이다. 타입 파라미터가 널이 아님을 확실히 하려면 널이 될 수 없는 타입 상한을 지정해야한다. 이렇게 널이 될 수 없는 타입 상한을 지정하면 널이 될 수 있는 값을 거부하게 된다.   

```kotlin
fun <T: Any> printHashCode(t: T) {
  println(t.hashCode())
}  

>>> printHashCode(null)
Error: Type ...~~~
```   

## P.270
자바 API를 다룰 때는 조심해야 한다. 대부분의 라이브러리는 널 관련 애노테이션을 쓰지 않는다.   

## P.274
실행 시점에 숫자 타입은 가능한 한 가장 효율적인 방식으로 표현된다. 대부분의 경우(변수, 프로퍼티, 파라미터, 반환 타입 등) 코틀린의 Int 타입은 자바 int 타입으로 컴파일 된다.   

## P.289
컬렉션 인터페이스를 사용할 때 항상 염두에 둬야 할 핵심은 읽기 전용 컬렉션이라고 해서 꼭 변경 불가능한 컬렉션일 필요는 없다는 점이다. 읽기 전용 인터페이스 타입인 변수를 사용할 때 그 인터페이스는 실제로는 어떤 컬렉션 인스턴스를 가리키는 수많은 참조 중 하나일 수 있다. 컬렉션을 참조하는 다른 코드를 호출하거나 병렬 실행한다면 컬렉션을 사용하는 도중에 다른 컬렉션이 그 컬렉션의 내용을 변경하는 상황이 생길 수 있고, 이런 상황에서는 ConcurrentModificationException이나 다른 오류가 발생할 수 있다. 따라서 읽기 전용 컬렉션이 항상 스레드 안전하지 않다는 점을 명심해야 한다.   

## P.298
원시 타입의 배열을 표현하는 별도 클래스를 각 원시 타입마다 하나씩 제공한다. 예를 들어 Int 타입의 배열은 IntArray다. 코틀린은 ByteArray, CharArray, BooleanArray 등의 원시 타입 배열을 제공한다. 이런 배열의 값은 박싱하지 않고 가장 효율적인 방식으로 저장된다. 
