## 코틀린의 가시성 변경자와 상속 제어 변경자

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
// open 변경자를 붙인 Person 클래스는 열려있으므로, 다른 클래스가 해당 클래스를 상속할 수 있다.
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

### 코틀린의 상속 제어 변경자 (open, final, abstract)
자바에서는 클래스에 final 을 명시해야 해당 클래스의 상속을 금자할 수 있다. \
**이와 달리 코틀린의 클래스와 메소드는 기본적으로 상속을 제어하는 변경자가 final 이며 상속에 대해 닫혀있다.**

위에서 언급한 바와 `같이 클래스의 상속을 허용하려면 클래스 앞에 open 변경자`를 붙이고, 오버라이드를 허용하고 싶은 메소드나 프로퍼티 앞에도 open 변경자를 붙여야 한다.

- `final`: 오버라이드 불가
- `open`: 오버라이드 가능
- `abstract`: 추상 클래스의 추상 멤버는 반드시 오버라이드를 해야 함

상속 제어 변경자는 가시성 변경자와는 다른 의미다. **기본적으로 클래스의 가시성 접근자는 public 이고 클래스의 상속 제어 변경자는 final 이다.**
즉 코틀린에선 기본적으로 멤버 및 함수에 외부 접근을 가능하지만 하위 클래스에서 오버라이딩 하는 것은 막고 있다.

```kotlin
open class Person(
    val name: String,
    internal var className: String? = null, // 같은 모듈에서만 접근 가능
    protected open var job: String? = null // 하위 클래스 및 클래스 내부에서 접근 가능
){
    /*
    getInfo 함수: 기본 가시성 변경자는 public, 기본 상속 제어 변경자는 final이므로 다양한 곳에서 함수를 호출할 수 있지만, 하위 클래스에서 해당 함수를 오버라이딩 할 수 없다.
    */
    fun getInfo(): String { 
        return "name = $name, className:$className"
    }
    // deleteInfo 함수: open 변경자를 사용했으므로 하위 클래스에서의 오버라이딩을 허용한다.
    open fun deleteInfo(){ 
        className = null
    }
}

class Student(name: String) : Person(name) {
    override var job: String? = "student"
    
    override fun deleteInfo() {
        super.deleteInfo()
        job = null
    }
}
```

override를 구현할 때 이후 하위 클래스에서 오버라이드하지 못하게 금지하려면 오버라이드 메소드 앞에 `final`을 명시해야 한다.
```kotlin
open class Student(name: String) : Person(name) {
    override var job: String? = "student"
    
    // Student의 하위 클래스에서 deleteInfo 메서드 재정의 불가
    final override fun deleteInfo() {
        super.deleteInfo()
        className = null
    }
}

class UniversityStudent(name: String) : Student(name) {
    override var job: String? = "university student"
    // deleteInfo 오버라이딩 불가
}
```
- **추상클래스** \
추상 클래스의 추상 멤버는 하위 클래스에서 오버라이드 해야만 한다. 따라서 **추상 멤버 앞에 open 변경자를 명시할 필요가 없다.** 추상클래스에 존재하는 비추상 함수는 기본적으로 final 이지만 open 변경자를 사용하여 오버라이드를 허용할 수 있다.

````kotlin
abstract class Animated { //추상 클래스
    abstract fun animate() // 추상 멤버이므로 오버라이드 해야만 한다.
    
    open fun stopAnimating(){ // 비추상 함수이지만 open 변경자를 사용하여 오버라이드가 허용됨
        println("stop")
    }
    
    fun animateTwice(){ // 비추상 함수이며 기본적으로 오버라이드가 불가함
        println("twice")
    }
}

// 추상클래스 Animated 를 상속한 클래스 Test
class Test : Animated() {
    override fun animate() {
        println(" 추상 멥버는 반드시 구현해야 한다.")
    }

    override fun stopAnimating() {
        println("open 변경자로 인해 오버라이딩 가능하다")
    }
}
````


### 출처
- [코틀린 문서](https://kotlinlang.org/docs/visibility-modifiers.html)
- [코틀린 인 액션 4장](http://www.yes24.com/Product/Goods/55148593)