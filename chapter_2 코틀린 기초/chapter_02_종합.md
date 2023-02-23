# Kotlin in Action
> Date: 2023-02-23   
> Place: [알베르](https://naver.me/52lhPJqg)


## 👨‍💻 참여자
| <img src="https://avatars.githubusercontent.com/u/95139402?v=4" width="80%"> | <img src="https://avatars.githubusercontent.com/u/85692623?v=4" width="80%"> | <img src="https://avatars.githubusercontent.com/u/81678959?v=4" width="80%"> | <img src="https://avatars.githubusercontent.com/u/83599356?v=4" width="80%"> | <img src="https://avatars.githubusercontent.com/u/55054505?v=4" width="80%"> |
| -- | -- | -- | -- | -- |
| [류민아](https://github.com/rrrmina) | [배성흥](https://github.com/mopil) | [이진욱(진행자)](https://github.com/koownij)  | [조경석](https://github.com/rudtjr1106) | [조기천](https://github.com/sectionr0)                                        |                                                  

## ✍ 토론, 질문, 논의
### P.61
> 📗 코틀린에서는 자바와 달리 배열 처리를 위한 문법이 따로 존재하지 않는다.   

코틀린에서는 자바의 `int arr[][] = new int[3][3];`와 같은 문법이 존재하지 않으며 arrayOf()를 사용하여 배열을 생성할 수 있습니다.   

### P.63
> 📗 반면 대입문은 자바에서는 식이었으나 코틀린에서는 문이 됐다. 그로 인해 자바와 달리 대입식과 비교식을 잘못 바꿔 써서 버그가 생기는 경우가 많다.   

식 : 결과를 만들어 냄   
문 : 값을 만들어내지 못하는 문장   

* 자바에서는 아래 코드가 정상 작동
```java
public class main
{
    public static void main(String[] args)
    {
        int a;
        int b;
        int c;

        a = b = c = 1;
        System.out.print(a);
    }
}
```   

* 코틀린에서는 아래 코드가 작동하지 않음.
```kotlin
fun main() {
    var a = 0
    var b = 0
    var c = 0
    a = b = c = 1
    print(a)
}
```   

### P.77
> 📗 코틀린에서는 enum class를 사용하지만 자바에서는 enum을 사용한다.
>  코틀린에서 enum은 **소프트 키워드(soft keyword)** 라고 부르는 존재다. enum은 class 앞에 있을 때는 특별한 의미를 지니지만
>  다른 곳에서는 이름에 사용할 수 있다. 반면 class는 키워드다. 따라서 class라는 이름을 사용할 수 없으므로 클래스를 표현하는 변수 등을 정의할 때는
>  clazz나 aClass와 같은 이름을 사용해야 한다.

   
`val enum = 3` 가능   

### P.79
> 📗 코틀린 when의 경우 자바와 달리 각 분기의 끝에 break를 넣지 않아도 된다.(자바에서는 break를 빼먹어서 오류가 생기는 경우가 종종 있다.)   

*자바 17부터는 코틀린의 when과 마찬가지로 break를 사용하지 않아도 된다.*   

### P.80
> 📗 분기 조건에 상수(enum 상수나 숫자 리터럴)만을 사용할 수 있는 자바 switch와 달리 코틀린 when의 분기 조건은 임의의 객체를 허용한다.   

*자바 17부터는 코틀린과 마찬가지로 임의의 객체를 허용한다.*   

### p.96
> 📗 발생한 예외를 함수 호출 단에서 처리하지 않으면 함수 호출 스택을 거슬러 올라가면서 예외를 처리하는 부분이 나올 때까지 배열을 다시 던진다. **(rethrow)**   

진욱 : 코틀린 - 코루틴 공식 문서에서 rethrow라는 단어가 나왔었음, 이때 rethrow라는 단어의 의미가 와닿지 않았는데 어떤 의미인지 알게 되었다.


