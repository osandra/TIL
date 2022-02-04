## 3장. 함수 정의와 호출
- [코틀린 함수 특징](#코틀린-함수-특징)
- [확장 함수](#확장함수)

코틀린 컬렉션은 표준 자바 컬렉션을 활용하여 더 많은 기능을 쓸 수 있도록 만들어졌다. 즉 **자체 컬렉션을 정의하지 않고, 자바 컬렉션을 확장하여 api를 제공한다.**

```kotlin
fun main() {
    val list = arrayListOf(1,7,40)
    // 코틀린에선 마지막 요소를 반환하는 last 메서드 제공 
    println(list.last()) // 40

    // 컬렉션의 각 요소를 해당 요소와 요소의 인덱스를 포함하는 IndexedValue로 반환하는 메서드 제공
    for ((idx, element) in list.withIndex()) {
        print("index: $idx, element= $element | ") 
    }
    // index: 0, element= 1 | index: 1, element= 7 | index: 2, element= 40 |
}
```
<br/>

### 코틀린 함수 특징
- 함수를 호출할 때는 함수에 전달하는 인자 중 **일부의 이름을 명시**할 수 있다.
- 함수 선언에서 **디폴트 파라미터 값을 지정할 수 있다.**

```kotlin
// 인자 a의 디폴트 값은 10이다.
fun test(a: Int = 10, b: Int, c: Int) = a + b + c

fun main() {
    println(test(10, b = 20, c= 30)) // 인자 중 일부 이름을 명시한 경우
    println(test(b=20,c=30)) // 출럭: 60 → a로 디폴트 파라미터 값인 10이 지정되었다.
}
````

#### ✓ **코틀린 함수에서 사용하는 디폴트 파라미터 값을 자바에서 호출할 때 사용하는 법**
코틀린 함수에서 사용하는 디폴트 파라미터 값을 자바에서 호출할 때 사용하고 싶다면, **코틀린 함수에 @JvmOverloads 애노테이션을 추가하면 된다.** 

**@JvmOverloads을 추가하면 전체 N개의 파라미터가 중 `M개의 파라마터가 디폴트 값이 있을 때 코틀린 컴파일러가 M개의 오버로딩 함수를 만들어준다.`** N-1개의 파라미터만 가지는 함수와 N-2개의 파라미터만 가지는 함수를 만들어주는 방식으로 오버로딩하는 함수를 M개 만들어준다.

```kotlin
@JvmOverloads
fun test(si: String = "서울시", gu: String="종로구", ro: String, number:Int) = "$si $gu $ro $number"

```
위와 같이 4개 중 2개의 디폴트 파라미터 존재한다면 @JvmOverloads 애노테이션을 붙였을 때 코틀린 컴파일러가 2개의 오버로딩 함수를 만들어준다.\
즉 si,gu,ro,number를 갖는 test 함수 외에도 3개 파라미터(si,ro,number)를 갖는 함수, 2개 파라미터(ro,number)를 갖는 오버로딩 함수를 만들어준다.
```java
// 자바에서 호출했을 경우 예시
public class Test {
    public static void main(String[] args) {
        System.out.println(test("세종대로",40)); // 출력: 서울시 종로구 세종대로 40
        System.out.println(test("성남시","장안구","창룡대로",21)); // 출력: 성남시 장안구 창룡대로 21
    }
}
```
<br/>

### 확장함수

확장함수란 **클래스의 멤버 메소드인 것처럼 호출할 수 있지만 그 클래스 밖에 선언된 함수**를 말한다.

사용 방법: 함수 이름 앞에 그 함수가 확장할 클래스의 이름을 덧붙인다. \
ex) 확장할-클래스-이름.추가하려는-함수이름() 

```kotlin
// 확장할 클래스 이름: String, 추가하려는 함수 이름: lastChar
fun String.lastChar():Char = this.get(this.length-1)

fun main() {
    print("Kotlin".lastChar()) // 출력: n
}
```
**클래스 이름을 수신 객체 타입이라고 부르며, 확장 함수가 호출되는 대상이 되는 값을 수신 객체라고 부른다.** (수신 객체: 그 클래스에 속한 인스턴스 객체) \
위 예제에선 String이 수신객체 타입이고, "Kotlin"이 수신 객체다.

**확장함수는 클래스 내부에서만 사용 가능한 비공개(private) 멤버나 보호된(protected) 멤버를 사용할 수 없으므로, 캡슐화를 지킨다.**
- **캡슐화**: 객체 지향 프로그래밍의 특징 중 하나다. 비슷한 역할을 하는 속성과 메소드를 하나의 클래스로 모아, 캡슐 내부의 감춰야 할 로직은 감추고 외부에 기능을 제공하는 것을 의미한다.
- [코틀린의 가시성 변경자(public, private, internal, public)](../visibility_modifier.md)

```kotlin
// House 라는 객체에는 비공개 멤버인 location과 공개 멤버인 nearSubway,nearBusStop 이 있다.
class House(private val location: String, var nearSubway: Boolean, var nearBusStop:  Boolean)

// 확장함수를 정의할 때 본문에서 this를 생략할 수 있다. 
// House 클래스에 정의된 확장함수 isNearSubWayOrBusStop
fun House.isNearSubWayOrBusStop(): Boolean = nearSubway || nearBusStop

// 아래와 같이 비공개 멤버인 location 을 사용하여 확장함수를 정의할 수 없다.
// fun House.isInSeoul():Boolean = this.location.contains("서울")

fun main() {
    val house1 = House("서울시 OO구 OO동", true,false)
    val house2 = House("수원시 ㅁㅁ구 ㅁㅁ동", false,false)

    println("house1: ${house1.isNearSubWayOrBusStop()}") // house1: true
    println("house2: ${house2.isNearSubWayOrBusStop()}") // house2: false
}
```
- **확장 함수를 다른 코틀린 소스코드에서 사용하는 법** \
임포트(import)를 해야 사용 가능하며, 임포트 할 경우 as 키워드를 사용하여 임포트 한 클래스나 함수를 다른 이름으로 부를 수 있다.\
ex) import chap3.strings.lastChar as last

- **자바에서 확장 함수 호출하는 법** \
확장 함수는 내부적으로 수신 객체를 첫 번째 인자로 받는 정적 메소드이므로, 정적 메소드를 호출하면서 인자를 넘기는 방식으로 자바에서 사용할 수 있다.
**lastChar 라는 확장함수를 코틀린에서 Extend.kt 파일에** 정의했다면 아래와 같이 호출할 수 있다.
    ```java
    import chap3.strings.ExtendKt;
    public class TestJava {
        public static void main(String[] args) {
            char lastChar = ExtendKt.lastChar("java");
            System.out.println(lastChar); // 출력: a
        }
    }
    ```
확장 함수를 사용하면 외부 라이브러리에 정의된 클래스를 포함해, 모든 클래스의 api를 **해당 클래스의 소스 코드 바꿀 필요 없이 확장할 수 있다.**