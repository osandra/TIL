### 코틀린 인 액션
- 드미트리 제메로프, 스베트라나 이사코바 저/오현석 역 
- [책 구매 링크](http://www.yes24.com/Product/Goods/55148593)
---
- [1장. 코틀린은 무엇이며 왜 필요한가?](#1장.-코틀린은-무엇이며-왜-필요한가?)
- [2장. 코틀린 기초](#2장.-코틀린-기초)

---
## `1장. 코틀린은 무엇이며 왜 필요한가?`

- 자바와 마찬가지로 코틀린도 `정적 타입 지정 언어`다
- 정적 타입 언어란 모든 프로그램 구성 요소 타입을 컴파일 시점에 알 수 있고, 프로그램 안에서 객체의 필드나 
    메소드를 사용할 때 마다 컴파일러가 타입을 검증해준다는 뜻이다.
- 동적 타입 지정 언어는 타입과 관계없이 모든 값을 변수에 넣을 수 있고 메소드나 필드 접근에 대한 검증이 실행 시점에 일어나며
  데이터의 구조를 더 유연하게 사용할 수 있지만 입력 실수 등을 컴파일 시 걸러내지 못한다는 단점이 있다.

### 자바와 코틀린 차이점
1. **자바와 달리 코틀린에선 모든 변수의 타입을 프로그래머가 직접 명시할 필요가 없다, 코틀린 컴파일러가 문맥으로부터 변수 타입을 자동으로 유추하기 떄문!**
이러한 기능을 `타입 추론`이라고 한다.
2. 코틀린은 `널(null)이 될 수 있는 타입을 지원`한다. 따라서 컴파일 시점에 널 포인트 예외가 발생할 수 있는지 여부를 검사할 수 있어서 프로그램의 신뢰성을 높일 수 있다.
3. 함수 타입에 대해 지원한다. 함수 타입의 변수들은 값으로 익명 함수를 저장할 수 있다. \
ex) var sum: (Int,Int) -> Int = {a, b-> a + b}


### **함수형 프로그래밍 핵심 개념**
1 . 일급시민인 함수: 함수를 변수에 저장할 수도, 함수를 인자로 다른 함수에 전달할 수도 있고, 함수에서 새로운 함수를 만들어서 반환할 수 있다. \
2 . 불변성: 만들어지고 나면 내부 상태가 절대로 바뀌지 않는 불변객체를 사용해 프로그래밍을 작성한다. \
3 . side effect 가 없다 - 입력이 같으면 항상 같은 출력을 내놓고, 다른 객체의 상태를 변경하지 않는다.

> 함수형 프로그래밍 장점
- 간결하다 → 함수를 값으로 사용할 수 있으면 더 강력한 추상화가 가능
- 다중 스레드를 사용해도 안전하다. → 불변 데이터 구조를 사용하고 순수함수를 그 데이터에 적용한다면 다중 스레드 환경에서도 같은 데이터를 여러 스레드가 변경할 수 없다.
- 테스트하기 쉽다. → 순수함수는 테스트 할 때 따로 필요한 환경을 구성하지 않아도 되므로 독립적으로 테스트할 수 있다.

### 코틀린 코드 컴파일 
코틀린 소스코트는 .kt 라는 확장자를 갖고 있다. \
코틀린 컴파일러는 코틀린 소스코드를 분석해서 .class 파일을 만들어낸다. \
코틀린 컴파일러로 컴파일한 코드는 코틀린 런타인 라이브러리를 의존하므로, 코틀린으로 컴파일한 애플리케이션을 배포할 때는 런타임 라이브러리도 함께 배포해야 한다.

- 터미널 창에서 코틀린 코드 컴파일 후 실행하는 방법 (Mac 기준)
```shell
# 만약 코틀린이 설치되어 있지 않다면 설치를 진행
brew install kotlin

# 코틀린 파일이 있는 경로에서 jar 파일 생성
# kotlinc <소스파일 혹은 디렉터리> -include-runtime -d <jar 이름>
kotlinc Template.kt -include-runtime -d test.jar

# -d 옵션은 생성된 클래스 파일들의 출력 경로(디렉토리 혹은 jar 파일) 말한다.
# -include-runtime 옵션은 Kotlin 런타임 라이브러리를 포함하여 jar 파일을 독립적이고 실행가능하게 만드는 옵션이다.

# jar 파일 실행
java -jar test.jar
```

## `2장. 코틀린 기초`

> 코틀린 특징
- 함수를 최상위 수준에서 적용할 수 있다. (클래스 안에 함수를 넣어야 할 필요 없다)
- 파라미터 이름 뒤에 타입을 쓴다.
- 줄 끝에 세미콜론을 붙이지 않아도 된다.

```kotlin
fun main(args: Array<String>) {
    println("hello world")
}
```
<br/>

### 함수
- fun 키워드로 시작한다.
- 괄호와 반환 타입 사이를 콜론(:) 으로 구분한다.

```kotlin
fun main(args: Array<String>) {
    println(max(3,10)) // 10
    println(max(12,10)) // 12
}

// 매개변수 a, b의 타입은 Int 이고, max 함수의 반환값 타입도 Int 다.
fun max(a:Int, b: Int): Int {
    return if (a > b) a else b
}

// 함수가 if 식 하나로 이뤄진 경우, 중괄호를 없애고 리턴을 제거하며 등호(=)를 붙여 표현할 수 있다.
fun max(a:Int, b: Int): Int = if (a > b) a else b

// if 와 같이 식이 본문인 함수의 반환타입은 생략할 수 있다. 
fun max(a:Int, b: Int) = if (a > b) a else b
```
코틀린에서는 **if, when, try** 등의 식으로 이뤄진 함수는 `식이 본문인 함수`라고 부르며,
식이 본문인 함수는 반환 타입을 명시하지 않아도, 컴파일러가 함수 본문 식을 분석해서 식의 결과 타입을 함수 반환타입으로 정해준다.
<br/><br/>

### 변수
변수 값을 초기화한 경우 컴파일러가 초기화 식을 분석해서 초기화 식의 타입을 변수 타입으로 지정한다. \
ex) var value = 10 → 컴파일러가 Int 타입으로 인식

초기화 식을 사용하지 않고 변수 선언하려면 변수 타입을 명시해야 한다. \
ex) var value : Int

#### ✓ **변경 가능한 변수와 변경 불가능한 변수**
- **val**: 변경 불가능한 참조를 저장하는 변수. 즉 초기화 후 재대입이 불가하다 (자바에서 final 변수와 역할이 같다.)
- **var**: 변경 가능한 참조. 변수의 값은 변경할 수 있지만 `변수의 타입은 고정돼 바뀌지 않는다.` 


val 변수는 참조 자체는 불변이지만 참조가 가리키는 객체의 내부 값은 변경될 수 있다.

```kotlin
fun main(args: Array<String>) {
    val languages = arrayListOf("Java")
    languages.add("Kotlin") // languages가 가리키는 객체의 내부 값은 변경될 수 있다.
    println(languages) //[Java, Kotlin]
}
```
<br/> 

### 문자열 템플릿
문자열 리터럴의 필요한 곳에 $ 기호와 함께 변수를 넣을 수 있다.
또한 복잡한 식도 중괄호({})로 둘러싸서 문자열 템플릿 안에 넣을 수 있다.
```kotlin
fun main(args: Array<String>) {
  val testArray  = arrayListOf("Test1","Test2","Test3")
  val name = "june"
  println("Hello, $name") // Hello, june
  println("Size: ${testArray.size}") // Size: 3
  println("Test: ${testArray[0]}") // Test: Test1
  println("Size, ${if (testArray.size > 10) "Full" else "Not Full"}") // Size, Not Full
}
```
<br/>

### 클래스와 프로퍼티
- 같은 Person 클래스를 자바와 코틀린으로 정의하면 다음과 같다.
```java
public class Person {
    private final String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```
**✓ 자바로 정의한 경우**
- 기본 접근 제어자는 default(동일한 패키지 내에서만 접근 가능)다.
- 데이터를 필드에 저장하며, 멤버 필드의 가시성은 보통 비공개(private)다.
- getter setter 등의 접근자 메소드를 직접 생성하며 멤버 필드를 접근한다.

```kotlin
// 코틀린
class Person(val name: String)
```
**✓ 코틀린으로 정의한 경우**
- 기본 가시성 변경자가 public 이므로 public 을 생략할 수 있다.
- `프로퍼티를 선언하는 방식(val, var)에 따라서 프로퍼티의 접근자 또한 함께 선언할 수 있다.`
- **val 로 선언한 프로퍼티는 읽기 전용이며 비공개 필드와 공개 getter를 생성하고, \
var 로 선언한 프로퍼티는 변경이 가능하여 비공개 필드와 공개 getter와 공개 setter를 만들어낸다.**

```kotlin
class Person(val name: String, // 비공개 필드, 공개 getter 생성
             var className: String? = null // 비공개 필드, 공개 getter, 공개 setter 생성, 기본값: null
            )

fun main(args:Array<String>) {
    val person = Person("nameTest")
    person.className = "CS201A" // 자동으로 setter를 호출해준다.
    
    // person.name = "newName" -> name 필드는 val 로 선언했으므로 공개 게터만 정의되어있다.
    
    println(person.name) // 자동으로 getter 호출해준다. 출력: nameTest
    println(person.className) // 출럭: CS201A

    val newPerson = Person("nameTest2","classNameTest2")
    println(newPerson.name) // 출럭: nameTest2
    println(newPerson.className) // 출럭: classNameTest2
}
```
코틀린에선 만약 is로 시작하는 프로퍼티가 있다면 게터에는 get이 붙지 않고 원래 이름을 그대로 사용하며, 세터에는 is를 세터로 바꾼 이름을 사용한다. \
ex) isExpired 이라는 이름을 가진 프로퍼티의 게터는 isExpired 이고, 세터는 setExpired 이다.
<br /><br />

### 프로퍼티 접근자(커스텀 접근자)를 직접 작성하는 방법
아래의 예는 노트북이 1킬로보다 무거운지에 대한 값을 프로퍼티에 값으로 저장하지 않고 게터로 정의하여,
`해당 프로퍼티(isMoreThanOneKilo)에 접근할 때마다 게터가 프로퍼티 값을 계산하는 방식을 보여준다.` 

파라미터가 없는 함수를 정의하는 방식과 커스텀 게터를 정의하는 방식은 비슷하나 일반적으로 클래스의 특성을 정의하고 싶다면 프로퍼티로 정의해야 한다.
```kotlin
class Notebook(val weight: Double, val name: String){
  // 1. 커스텀 게터를 정의하는 방식
  val isMoreThanOneKilo: Boolean
    get(){  // 프로퍼티 게터 선언
      return weight > 1
    }
  
  // 2. 파라미터 없는 함수를 정의하는 방식
  fun isMoreThanOneKiloFunc(): Boolean = (weight > 1)
}

fun main() {
  val gram = Notebook(0.9, "LG Gram")
  println(gram.isMoreThanOneKilo) // false
  println(gram.isMoreThanOneKiloFunc()) //false

  val m1Mac = Notebook(1.5,"M1 mac")
  println(m1Mac.isMoreThanOneKilo) // true
  println(m1Mac.isMoreThanOneKiloFunc()) //true
}
```
<br/>

### 선택 표현과 처리: enum과 when
enum 상수를 정의할 때는 상수에 해당하는 프로퍼티 값을 지정해야 하며, 상수 목록과 메소드 사이에 세미콜론을 넣어야 한다.

```kotlin
enum class Notebook(val weightKg: Double,val price: Int, var discountRate: Double) {
    GRAM(0.9,1_500_000,0.1),
    MAC(1.5,2_000_000,0.5),
    GalaxyBook(1.3,1_700_000,0.3);

    fun getDiscountPrice() = price * discountRate
}
```

코틀린에선 **switch 대신 `when`을 사용하여 분기 조건 등을 설정할 수 있다. \
자바와 달리  분기의 끝에 break를 넣지 않아도 된다.**
```kotlin
fun getName(notebook: Notebook) = when (notebook) {
    Notebook.GRAM -> "LG 그램 노트북"
    Notebook.MAC -> "애플 맥북"
    Notebook.GalaxyBook -> "삼성 갤럭시 북"
}
fun main() {
    println(getName(Notebook.GRAM)) //LG 그램 노트북
}
```
<br/>

### 대상을 이터레이션: while & for loop
코틀린에는 자바의 for 루프에 해당하는 요소가 없는 대신 `범위를 사용한다.` \
ex) 자바 for 루프 => for (int i = 0; i<10; i ++) {}

범위는 두 값으로 이뤄진 구간이며, 시작값과 끝 값을 .. 연산자로 연결해서 범위를 생성한다.
```kotlin
fun main() {
  for (i in 1..10)
    print("$i ") // 1 2 3 4 5 6 7 8 9 10
  println()
  
  // 증가 값을 음수로 만들려면 downTo를 사용해서 역방향 수열로 만들면 된다.
  // downTo의 기본 증가 값은 -1 이다.
  for (i in 10 downTo 1) // 10 9 8 7 6 5 4 3 2 1
    print("$i ")
  println()
  
  // step 에 값을 설정하면 해당 값만큼 역방향으로 이동한다.
  for (i in 10 downTo 1 step 2)
    print("$i ") // 10 8 6 4 2
}
```
<br/>

### 코틀린 예외 처리
- 자바와 동일하게 예외를 처리할 때 try, catch, finally 를 사용한다.
- 자바와 달리 함수에서 던질 수 있는 예외를 명시할 필요가 없다.
```kotlin
// 코틀린에서 정의한 경우
fun readNum(reader: BufferedReader): Int? {
    try{
        val line = reader.readLine()
        return Integer.parseInt(line)
    } catch(e: NumberFormatException){
        return null
    } finally {
        reader.close()
    }
}
```
```java
// 자바에서 정의할 경우, 처리하지 않은 예외를 throws 절에 명시해야 한다.
public Integer readLine(BufferedReader reader) throws IOException {
    try {
        String val = reader.readLine();
        return Integer.parseInt(val);
    } catch (NumberFormatException e) {
        return null;
    } finally {
        reader.close();
    }
}
```

자바에서는 체크 예외를 명시적으로 처리해야 하지만, **코틀린은 체크 예외와 언체크 예외를 구별하지 않고, \
함수가 발생한 예외를 잡지 않아도 된다.**
- 체크 예외: RuntimeException을 상속하지 않는 예외를 말하며, 자바에선 명시적으로 예외를 처리해야 한다. 대표적인 예로 IOException, SQLException이 있다.
- 언체크 예외: RuntimeException을 상속하며, 자바에선 명시적으로 예외를 처리하지 않아도 된다. 대표적인 예로 NullPointerException이 있다.
<br><br>

**try 키워드 또한 if, when 과 같은 식이므로 값을 변수에 대입하 수 있다. 그러나 if 와 달리 본문을 중괄호로 둘러싸야 한다.**
```kotlin
fun readNumber(reader: BufferedReader) {
    val number = try {
          Integer.parseInt(reader.readLine())
        } catch (e: NumberFormatException) {null} // 예외 발생시 null 값을 사용한다.
    println(number)
}

// if 문은 중괄호 없이 값을 변수에 대입할 수 있다.
fun testIfStatement(value: Int) {
  val isOverTen = if (value > 10) "bigger" else "smaller"
  println(isOverTen)
}
```