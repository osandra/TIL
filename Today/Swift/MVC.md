## TIL

- MVC 패턴
- self is immmutable → 구조체는 값타입이고 클래스는 reference type.
- 구현한 기능 코드 위에 간단하게 해당 기능이 무엇을 하는 지 작성하기.
- cs50 코칭 스터디 과제 + 연결 리스트 강의 학습

구조체는 값 타입(값을 복사하는 것)이므로 let으로 선언된 인스턴스의 값은 변경 불가.

**구조체 안의 함수에서 구조체 안의 프로퍼티 값을 바꾸는 것은 기본적으로 불가함.** 구조체 안에서 정의된 함수 스코프 안은 기본적으로 let 으로 정의되어있음. 기본적으로 let으로 적용되어 있는 이유는 구조체가 값타입이므로, 이를 보장해주기 위함임. 만약 함수 안에서 다른 프로퍼티 값을 바꾸고 싶다면 함수 앞에 mutating 키워드 붙여줘야 함. **mutating 키워드를 붙여주면, 스코프 내부가 var처럼 작동하게 된다.**

**만약 해당 프로퍼티 혹은 인스턴스가 let으로 정의되었다면 mutating 키워드 붙여도 변경이 불가함.**

```swift
struct Class {
	var numberofStudents: Int
	var students: [String]
	func plusNumber(){ //불가
		numberofStudents + 
	}
	mutating func plusNumber(){ //가능
		numberofStudents + 
	}
}
```

**Udamy Angela Yu Swift 강의 노트**

> M(odel)V(iew)C(ontroller) 패턴은 디자인 패턴 중 하나이다.

요구사항, 프로젝트 별로 다른 디자인 패턴이 필요할 수도 있음.

MVC패턴 왜 쓰나? → 한 파일에 모든 코드(데이터, 뷰 관련 코드, 작동 관련 코드)가 있으면, 추후 질문 리스트가 100개가 생기면 너무 지저분해 진다. → MVC패턴을 사용해 모델과 뷰, 컨트롤러에게 특정 역할을 맡기기

- model → data & logic 담당
- view → 유저에게 보여지는 부분 담당(user interface)
- controller → view로부터 받은 이벤트 요청을 모델에게 전달하고, 모델이 보내준 데이터를 뷰에 응답하는 역할.  ex) 모델아 이것(데이터) 좀 갖고 와줘~, 뷰야 네가 요청 데이터 이거니까 이걸로 화면 변경해줘 , 뷰야 버튼 색깔 좀 변경해줘~


[참조 1) self is immutable 관련](https://chris.eidhof.nl/post/structs-and-mutation-in-swift/)

[참조 2) Udamy Angela Yu Swif 강의](https://www.udemy.com/course/ios-13-app-development-bootcamp)