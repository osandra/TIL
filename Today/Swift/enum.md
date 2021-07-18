### TIL
- [이전에 작성한 enum 예제 코드](https://github.com/osandra/Swift_Practice/commit/b70683b9ca62595e52b933e5818dafcd503b7f17)
- [다양한 enum 활용 예시 관련 사이트](https://developerinsider.co/advanced-enum-enumerations-by-example-swift-programming-language/)
```swift
enum Store: Int {
    //enum의 타입을 적어야만 각 케이스의 rawValue값 지정 가능
    case pecil = 1000
    case Highlighter = 1500
}
let AStore = Store.pecil.rawValue

// 각 케이스에서 연관값을 가지는 경우
// Menus타입의 연관값 가지는 restaurant
// String 타입의 연관값 가지는 stationeryStore
enum Store2 {
    case restaurant(type: Menus) //type: 부분 넣어도 되고 안 넣어도 됨
    case stationeryStore(String) //이름 없는 경우
    case something
    var printTest: String? {
        //계산 프로퍼티를 통해 각 케이스에 따라 다른 값 리턴하기
        //열거형 타입에서 저장 프로퍼티는 사용할 수 없음
        switch self {
        case .restaurant(let name): //let 뒤 이름(name) 원하는 문자로 설정 가능
        return "\(name) restaurant"
        case .stationeryStore:
        return "stationery"
        default:
        return "something"
        }
    }
}
enum Menus {
    case 밥
    case 면
    case 빵
}
    
var Bstore = Store2.restaurant(type: .밥) //선언할 때는 파라미터 이름 type 으로 작성해야 함
//타입을 지정해준다면 열거형 구조체 이름 생략 가능
var Cstore: Store2 = .stationeryStore("SSS")
var Something: Store2 = .something
print(Bstore.printTest ?? "") // 밥 restaurant
print(Cstore.printTest ?? "") //stationery
print(Something.printTest!) //something
    
//함수를 통해 각 케이스에 따른 다른 값 출력하기
func printTest(storeType: Store2){
    switch storeType {
    case .restaurant(let name):
        print("\(name) restautant!")
    case .stationeryStore(let name2):
        print("\(name2) stationery Store!")
    case .something:
        print("something")
    }
}
printTest(storeType: Bstore) //밥 restautant!출력
printTest(storeType: Cstore) // SSS stationery Store!
printTest(storeType: Something) //something
```