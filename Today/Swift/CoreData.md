### TIL

---
앱의 데이터를 지속성있게 저장하는 방법은 크게 single table(userDefault, Codable(custom객체 저장), keyChain(안전하게 저장)하는 방법과 데이터베이스를 통해 저장하는 방법이 있다. 

userDefault과 Codable을 통해 저장하는 방법은 작은 양의 데이터만 저장할 수 있기에 데이터로 저장해야 할 게 많고, 쿼리 문을 통해 원하는 데이터만 필터링하고 싶다면 데이터베이스를 사용하는 것이 낫다.

데이터베이스 솔루션 중 Core Data는 SQLite 를 영구저장소로 사용해 앱 내 주요 데이터를 생성, 수정, 삭제, 읽게 해주는 프레임워크다. 주로 Core data를 통해 MVC패턴 중 Model을 관리할 수 있다. 또한 Core Data를 통해 객체 그래프를 관리할 수 있다. 객체 그래프( Object Graph)은 객체들이 서로 연결되어 있는 관계를 말한다.

Core Data 는 영구저장소에서 모델 객체를 효율적으로 가져오고 변경 내용을 저장소에 다시 저장하는 기능 외에도 다양한 기능이 있다고 한다. [출처](https://developer.apple.com/library/archive/documentation/DataManagement/Devpedia-CoreData/coreDataOverview.html#//apple_ref/doc/uid/TP40010398-CH28-SW1)

[1) core data guide](https://www.raywenderlich.com/7569-getting-started-with-core-data-tutorial)

[2) core-data](https://medium.com/@maddy.lucky4u/swift-4-core-data-part-2-creating-a-simple-app-c4eded1fa55f)

[3) core-data-with-swift-4](https://medium.com/xcblog/core-data-with-swift-4-for-beginners-1fc067cca707)

[4) context- core data](https://developer.apple.com/library/archive/documentation/DataManagement/Devpedia-CoreData/managedObjectContext.html)

[5) core-data-code](https://www.swiftdevjournal.com/core-data-code-generation/)