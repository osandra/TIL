### 코틀린의 가시성 변경자(visibility modifiers)
코틀린에는 네 가지 가시성 변경자(private, protected, internal, public)가 있고, 기본 가시성 변경자는 public 이다.

#### **패키지 내에서 가시성 변경자와 함께 선언한 경우**

함수, 클래스, 인터페이스 등은 패키지 내에서 최상위에서 선언이 가능하지만, **protected 변경자를 사용한 경우 최상위에서 선언이 불가하다.**
- `public`: 어디서든 접근 가능
- `private`: 같은 파일 안에서만 접근 가능
- `internal`: 같은 모듈 안에서만 접근 가능

```kotlin
// test.kt
package test

private class Test(val value: String)
internal fun test(a: Int, b: Int) = a+b
public fun test(a: String, b: String) = a+b

// protected 변경자는 최상위에서 선언이 불가
// protected fun test2(a: Int, b: Int) = a+b
```
<br/>

#### **클래스 내에서 가시성 변경자와 함께 선언된 경우**
- `public`: 어디서든 접근 가능
- `private`: 해당 클래스 멤버는 클래스 안에서만 접근 가능
- `internal`: 같은 모듈 안에 존재할 경우에만 접근 가능
- `protected`: 해당 클래스 내부 및 하위 클래스에서 접근 가능

클래스의 상속을 허용하기 위해선 클래스 앞에 **open 변경자**를 사용해야 하며, **오버라이딩을 허용하고 싶다면 메서드 및 프로퍼티 앞에도 open 변경자를 사용해야 한다.**
```kotlin
open class Person(
    val name: String, // 기본: public 이므로 어디서든 접근 가능
    internal var className: String? = null, // 같은 모듈에서만 접근 가능
    private val address: String? = null, // 클래스 내부에서만 접근 가능
    protected open var job: String? = null // 하위 클래스 및 클래스 내부에서 접근 가능
)

// Person의 하위 클래스인 Student 에서 protected open 변경자로 선언한 변수 오버라이딩 가능
class Student(name: String) : Person(name) {
    override var job: String? = "student"
}
```


### 출처
- [코틀린 문서](https://kotlinlang.org/docs/visibility-modifiers.html)