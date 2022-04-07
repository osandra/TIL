## 클래스

- 때론 테스트 코드에 접근을 허용하기 위해 변수나 유틸 함수를 protected로 선언한다. 그러나 캡슐화를 풀어주는 건 언제나 최후의 수단이다.
- 클래스는 작아야 한다.
  - "얼마나 작아야 하는가?" → `클래스가 맡은 책임을 센다`
- 클래스 이름은 클래스의 책임을 기술해야 한다. **간결한 이름이 떠오르지 않는 이유는 클래스가 너무 크기 때문이다.**

### 단일 책임 원칙 (Single Responsibility Principle)
- SRP는 모든 클래스가 하나의 책임만을 가지며 클래스는 그 책임을 온전히 캡슐화해야 함을 말한다. 즉 
클래스나 모듈을 **변경할 이유가 단 하나** 뿐이어야 한다는 원칙이다.
- `변경할 이유를 파악`하려 애쓰다 보면 코드를 추상화하기 쉬워진다.
- 작은 클래스는 각자 맡은 책임이 하나, 변경할 이유가 하나며 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.
### 응집도
- 일반적으로 클래스 메서드가 변수를 더 많이 사용할 수록 응집도가 높다고 말한다.
- 응집도가 높다는 말은 클래스에 속한 메서드와 변수가 서로 의존하며 논리적인 단위로 묶인다는 의미다.
**몇몇 메서드만이 사용하는 인스턴스 변수가 많아진다면 이는 클래스를 쪼개야 한다는 신호다.**

### 변경하기 쉬운 클래스
어떤 변경이든 클래스에 손대면 다른 코드를 망가뜨릴 잠재적인 위험이 존재한다.

#### **개방-폐쇄 원칙 (Open Closed Principal)**
- OCP란 클래스가 확장엔 개방적이고 수정엔 폐쇄적이어야 한다는 원칙이다.
- 기능에는 개방적이어서 새로운 기능을 추가할 경우 클래스 추가 혹은 모듈을 확장하는 방식으로 진행된다.
동시에 기존 클래스의 수정엔 닫혀있어 수정에 폐쇄적이다.
- 이상적인 시스템이라면 새 기능을 추가할 때 시스템을 확장할 뿐 기존 코드를 변경하지 않는다.

```kotlin
// 변경이 필요할 경우 손대야 하는 클래스의 예
class Sql(val table: String, val columns: List<Column>) {
    fun create(): String
    fun insert(fields: List<Object>): String
    fun selectAll(): String
    fun select(column: Column, pattern: String): String
    ...
}
```
위와 같이 정의할 경우 변경할 이유가 두 가지다.
1. 기존 SQL 수정할 경우
2. 새로운 SQL문 지원할 경우

따라서 위의 Sql 클래스는 SRP를 위반한다. **그러나 실제로 개선에 뛰어드는 계기는 시스템이 변해서여야 한다.**
가까운 미래에 새로운 SQL문이 필요하지 않는다면 Sql 클래스를 그대로 두는 것이 좋다.

```kotlin
// SRP를 지키면서 닫힌 클래스의 예
abstract class Sql(val table: String, val columns: List<Column>) {
    abstract fun generate(): String
}


class CreateSql(table: String, columns: List<Column>) : Sql(table,columns) {
    override fun generate(): String {
        return "" // 구현 로직
    }
}
class SelectSql(table: String, columns: List<Column>) : Sql(table,columns) {
    override fun generate(): String {
        return "" // 구현 로직 
    }
}
...
```
위와 같이 Sql 클래스를 작성하면 새로운 SQL 문을 추가할 때 기존 클래스의 변경 없이 새로운 클래스를 추가할 수 있다.


#### **변경으로부터의 격리**
- 상세한 구현에 의존하는 클라이언트 클래스는 구현이 바뀌면 위험에 빠진다.
  - **인터페이스와 추상 클래스를 사용해 구현이 미치는 영향을 격리할 수 있다**
- 상세한 구현에 의존하는 코드는 테스트가 어렵다

Portfolio 클래스가 외부 TokyoStockExchange API를 사용해서 주식 매매 값을 계산할 경우,
계속 값이 달라지는 API로는 테스트 코드를 작성하기 어렵다. \
따라서 TokyoStockExchange라는 구체적인 API를 직접 호출하는 대신 StockChange라는 인터페이스를 생성하는 방식으로
테스트 코드를 작성해나갈 수 있다.

```kotlin
interface StockChange {
    fun currentPrice(symbol: String): Money
}

class PortFolio(private val exchange: StockChange) {
    // count 개수만큼 해당 주식 구매한 가격을 반환
    fun add(count: Int, symbol: String): Money {
        return Money(count * exchange.currentPrice(symbol).value)
    }
}

class Money(val value: Int)
```

이와 같이 작성했을 경우 테스트 코드에선 TokyoStockExchange를 흉내내는 테스트용 클래스를 만들어 사용할 수 있다.
```kotlin

// 항상 고정된 주가를 반환하는 테스트용 클래스 생성
class FixedStockExchangeStub: StockChange {
    private val symbolValueMap = HashMap<String, Int>()
    
    override fun currentPrice(symbol: String): Money {
        return symbolValueMap[symbol]?.let { Money(it) } ?: throw Exception("해당하는 회사의 주가 정보가 없습니다.")
    }
    
    fun fix(symbol: String, price: Int) {
        symbolValueMap[symbol] = price
    }
}

class PortFolioTest {
    private lateinit var stockChange: FixedStockExchangeStub
    private lateinit var portFolio: PortFolio

    @BeforeEach
    fun setup(){
        stockChange = FixedStockExchangeStub()
        stockChange.fix("Apple", 100)
        portFolio = PortFolio(stockChange)
    }

    @Test
    fun `count 개수만큼 해당 주식 구매한 가격을 반환한다"`() {
        val resultMoney = portFolio.add(5, "Apple")
        assertEquals(500, resultMoney.value)
    }
}
```

위와 같이 테스트 할 경우, 시스템의 결합도가 낮아져 유연성과 재사용성이 높아진다.\
결합도가 낮다는 소리는 각 시스템 요소가 다른 요소로부터 그리고 변경으로 부터 잘 격리되어있다는 의미다.

결합도를 줄이면 다른 클래스 설계 원칙인 DIP를 따르는 클래스가 나온다.
#### **의존관계 역전 원칙 (Dependency Inversion Principle)**
DIP는 2가지 원칙을 갖는다.
1. 상위 모듈은 하위 모듈에 의존해선 안된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.
2. 추상화는 세부 사항에 의존하면 안된다.

즉 클래스가 상세한 구현이 아니라 추상화에 의존해야 한다는 원칙이다.
