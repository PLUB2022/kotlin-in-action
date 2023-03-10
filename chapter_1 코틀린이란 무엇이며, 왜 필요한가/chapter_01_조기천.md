# 삼색볼펜법을 통한 스터디 내용 정리

## Ch_01. 코틀린이란 무엇이며, 왜 필요한가?

## 🔴 빨강

- p.35 코틀린은 간결하고 실용적이며, 자바 코드와의 상호운용성을 중시한다.
- p.36 성능도 자바와 같은 수준이다.
- p.37 `it` 이라는 이름을 사용하면 람다 식의 유일한 인ㅇ자로 사용할 수 있다.
- p.37 엘비스 연산자 (Elvis operator)
- p.38 코틀린은 정적타입 지정 언어이다.
- p.38 모든 프로그램 구성 요소의 타입을 컴파일 시점에 알 수 있고 프로그램 안에서 객체의 필드나 메서드를 사용할 때마다 컴파일러가 타입을 검증해준다.
- p.39 코틀린에서는 모든 변수의 타입을 프로그래머가 직접 명시할 필요가 없다.
- p.39 프로그래머는 타입 선언을 생략해도 된다.
- p.40 코틀린이 널이 될 수 있는 타입을 지원한다.
- p.30 함수 타입 (function type)
- p.46 안드로이드 개발 작업을 훨씬 더 적은 코드로 달성할 수 있고, 때로는 전혀 코드를 작성하지 않고 그렇게 할 수 있다. (컴파일러가 자동으로 필요한 코드를 생성해준다.)
- p.47 코틀린이 자바와의 상호운용성에 초점을 맞춘 실용적이고 간결하며 안전한 언어
- p.48 코틀린은 다른 프로그래밍 언어가 채택한 이미 성공적으로 검증된 해법과 기능에 의존한다.
- p.48 실용성에 있어 코틀린의 다른 측면은 도구를 강조한다는 점이다.
- p.49 코드가 더 간단하고 간결할수록 내용을 파악하기가 더 쉽다.
- p.51 실행 시점에 오류를 발생시키는 대신 컴파일 시점 검사를 통해 오류를 더 많이 방지해준다.
- p.51 가장 중요한 내용으로 코틀린은 프로그램의 `NullPointerException` 을 없애기 위해 노력한다.
- p.51 코틀린에서는 타입 검사와 캐스트가 한 연산자에 의해 이뤄진다.
- p.52 상호운용성과 관련해 자바 프로그래머들이 던지는 첫 번째 질문은 아마도 “기존 라이브러리를 그대로 사용할 수 있느냐?” 일 것이다. 코틀린의 경우 그에 대한 답은 “물론 그렇다” 이다.
- p.54 코틀린 컴파일러로 컴파일한 코드는 코틀린 런타임 라이브러리에 의존한다.
- p.54 런타임 라이브러리에는 코틀린 자체 표준 라이브러리 클래스와 코틀린에서 자바 API의 기능을 확장한 내용이 들어있다.
- p.57 코틀린은 타입 추론을 지원하는 정적 타입 지정 언어이다. 따라서 소스코드의 정확성과 성능을 보장하면서도 소스코드를 간결하게 유지할 수 있다.
- p.57 코틀린은 객체지향과 함수형 프로그래밍 스타일을 모두 지원한다.
- p.57 코틀린은 실용적이며 안전하고, 간결하며 상호운용성이 좋다.
- p.57 `NullPointerException` 과 같이 흔히 발생하는 오류를 방지하며, 읽기 쉽고 간결한 코드를 지원하면서 자바와 아무런 제약 없이 통합될 수 있는 언어를 만드는데 초점을 맞췄다는 뜻이다.

## 🔵 파랑

- p.38 코틀린은 구체적인 영역의 문제를 해결하거나 특정 프로그래밍 패러다임을 지원하는 여러 라이브러리와 아주 잘 융합된다.
- p.39 타입 추론 (type inference)
- p.39  성능 : 실행 시점에 어떤 메서드를 호출할지 알아내는 과정이 필요 없으므로 메서드 호출이 더 빠르다.
- p.39 신뢰성 : 컴파일러가 프로그램의 정확성을 검증하기 떄문에 실행 시 프로그램이 오류로 중단될 가능성이 더 적어진다.
- p.39 유지 보수성 : 코드에서 다루는 객체가 어떤 타입에 속하는지 알 수 있기 때문에 처음 보는 코드를 다룰 때도 더 쉽다.
- p.39 도구 지원 : 정적 타입 지정을 활용하면 더 안전하게 리팩토링 할 수 있고, 도구는 더 정확한 코드 완성 기능을 제공할 수 있으며, IDE의 다른 지원 기능도 더 잘 만들 수 있다.
- p.40 프로그래머가 직접 타입을 선언해야 함에 따라 생기는 불편함이 대부분 사라진다.
- p.40 일급 시민인 함수 : 함수를 일반 값처럼 다룰 수 있다. 함수를 변수에 저장할 수 있고, 함수를 인자로 다른 함수에 전달할 수 있으며, 함수에서 새로운 함수를 만들어서 반환할 수 있다.
- p.40 불변성 : 함수형 프로그래밍에서는 일단 만들어지고 나면 내부 상태가 절대로 바뀌지 않는 불변 객체를 사용해 프로그램을 작성한다.
- p.41 부수 효과 없음 : 함수형 프로그래밍에서는 입력이 같으면 항상 같은 출력을 내놓고 다른 객체의 상태를 변경하지 않으며, 함수 외부나 다른 바깥 환경과 상호작용하지 않는 순수 함수를 사용한다.
- p.41 첫째로 간결성을 들 수 있다. 함수형 코드는 그에 상응하는 명령형 코드에 비해 더 간결하고 우아하다. 함수를 값처럼 활용할 수 있으면 더 강력한 추상화를 할 수 있고 강력한 추상화를 사용해 코드 중복을 막을 수 있다.
- p.41 다중 스레드를 사용해도 안전하다는 사실이다.
- p.41 함수형 프로그램은 테스트하기 쉽다.
- p.44 자바 코드와 매끄럽게 상호운용할 수 있다는 점이 코틀린의 큰 장점이다.
- p.44 자바 클래스를 코틀린으로 확장해도 아무 문제가 없으며, 코틀린 클래스 안에 메서드나 필드에 특정 애노테이션을 붙여야 하는 경우에도 아무 문제가 없다. 그러면서도 시스템 코드는 더 간결해지고 더 신뢰성이 높아지며, 더 유지 보수하기 쉬워질 것이다.
- p.46 애플리케이션의 신뢰성이 더 높아진다는 점을 들 수 있다.
- p.46 코틀린 타입 시스템은 null 값을 정확히 추적하며 널 포인터로 인해 생기는 문제를 줄여준다.
- p.46 자바에서 `NullPointerException` 을 일으키는 유형의 코드는 대부분 코틀린에서는 컴파일도 되지 않는다.
- p.47 코틀린을 사용하더라도 성능 측면에서 아무 손해가 없다. 코틀린 컴파일러가 생성한 바이트코드는 일반적인 자바 코드와 똑같이 효율적으로 실행된다.
- p.47 람다를 사용해도 새로운 객체가 만들어지지 않으므로 객체 증가로 인해 가비지 컬렉션이 늘어나서 프로그램이 자주 멈추는 일도 없다.
- p.48 언어의 복잡도가 줄어들고 이미 알고 있는 기존 개념을 통해 코틀린을 더 쉽게 배울 수 있다.
- p.48 코틀린은 어느 특정 프로그래밍 스타일이나 패러다임을 사용할 것을 강제로 요구하지 않는다.
- p.49 게터, 세터, 생성자 파라미터를 필드에 대입하기 위한 로직 등 자바에 존재하는 여러 가지 번거로운 준비 코드를 코틀린은 묵시적으로 제공하기 떄문에 코틀린 소스코드는 그런 준비 코드로 인해 지저분해지는 일이 없다.
- p.50 코틀린을 JVM 에서 실행한다는 사실은 이미 상당한 안전성을 보장할 수 있는 뜻이다.
- p.50 JVM을 사용하면 메모리 안전성을 보장하고, 버퍼 오버플로를 방지하며, 동적으로 할당한 메모리를 잘못 사용함으로 인해 발생할 수 있는 다양한 문제를 예방할 수 있다.
- p.51 어떤 타입이 널이 될 수 있는지 여부를 표시하기 위해서는 오직 ? 한 글자만 추가하면 된다.
- p.51 코틀린이 방지해주는 다른 예외로는 `ClassCastException`이 있다.
- p.52 변경한 클래스가 프로젝트 안에서 어떤 역할을 하는지와는 관계없이 코틀린으로 바꾼 클래스가 어떤 것이든 프로젝트의 나머지 부분을 전혀 수정하지 않고도 컴파일 및 실행이 가능하다.
- p.52 상호운용성 측면에서 코틀린이 집중하는 다른 방향으로는 기존 자바 라이브러리를 가능하면 최대한 활용한다는 점을 들 수 있다.
- p.52 아무런 부가 비용을 야기하지 않는다.
- p.53 자바와 코틀린 소스 파일을 자유롭게 내비게이션 할 수 있다.
- p.53 여러 언어로 이뤄진 프로젝트를 디버깅하고 서로 다른 언어로 작성된 코드를 언어와 관계없이 한 단계씩 실행할 수 있다.
- p.53 자바 메서드를 리팩토링해도 그 메서드와 관련 있는 코틀린 코드까지 제대로 변경된다. 역으로 코틀린 메서드를 리팩토링해도 자바 코드까지 모두 자동으로 변경된다.
- p.54 코틀린은 그런 빌드 시스템과 호환된다. (메이븐, 그레이들, 앤드 등)
- p.57 불변 값 지원을 통해 다중 스레드 애플리케이션 개발과 테스트를 더 쉽게 할 수 있다.
- p.57 코틀린의 런타임 라이브러리는 크기가 작고, 코틀린 컴파일러는 안드로이드 API를 특별히 지원한다.

## 🟢 초록

- p.37 스마트카드 (자바 카드 기술)
- p.38 인텔의 멀티OS 엔진을 사용하면 코틀린을 iOS 디바이스에서 실행 할 수 있다.
- p.38 데스크탑 애플리케이션을 작성하고 싶다면 코틀린과 토네이도FX, 자바 FX 등을 함께 사용할 수 있다.
- p.38 젯브레인스는 코틀린 네이티브 백엔드를 개발 중이다.
- p.41 준비 코드 없이 독립적으로 테스트할 수 있다.
- p.42 코틀린 표준 라이브러리는 객체와 컬렉션을 함수형 스타일로 다룰 수 있는 API를 제공한다.
- p.45 코틀린이 제공하는 깔끔하고 간결한 DSL 기능을 활용할 수 있는 다른 예로는 영속성 프레임워크를 들 수 있다.
- p.46 젯팩 콤포즈 (Jetpack Compose)
- p.47 인라이닝 (inlining)
- p.48 더 간결한 구조로 바꿀 수 있는 대부분의 코드 패턴을 도구가 자동으로 감지해서 수정하라고 제안한다.
- p.49 코틀린 설계 목표에는 소스코드를 가능한 짧게 만든다는 내용은 들어있지 않다.
- p.52 다른 일부 JVM 언어와 달리 코틀린은 상호운용성 측면에서 훨씬 더 많은 것을 제공한다.
- p.52 코틀린은 자체 컬렉션 라이브러리를 제공하지 않는다. 코틀린은 자바 표준 라이브러리 클래스에 의존한다.
- p.56 정확한 코틀린 문법이 기억나지 않는 경우 이 변환기를 유용하게 써먹을 수 있다. 작성하고픈 코드를 자바로 작성해 복사한 후 코틀린 파일에 그 코드를 붙여 넣으면 변환기가 자동으로 같은 뜻의 코틀린 코드를 제안한다.