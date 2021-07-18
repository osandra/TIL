### TIL
---

###  Udamy Angela Yu Swift 강의 노트
----
Main.storyboard → 사용자에게 보여지는 UI 디자인. <br>
- 해당 UI 요소를 뷰에서 쓰기 위해 @IBOutlet사용. @IBOutlet을 통해 컴포넌트를 코드로 변경할 수 있다.(control 누른 채로 드래그)<br>
- 반대로 사용자의 특정 반응(버튼 터치)에 따라 실행할 코드를 작성하기 위해선 @IBAction 매크로 사용. 
 
**랜덤 숫자 생성**: 
- Int.random(in: 0...5) → 0부터 5까지 랜던 숫자 생성
- 배열에서의 랜덤 숫자 →  rray.randomElement()

**왜 Auto Layout을 공부해야 하는가**
Auto Layout: 제약조건(Constraints), 정렬, 뷰를 이용해 여러 기기에서도 UI요소가 잘 보이게끔 도와주는 것.
아이폰은 기종마다 화면의 크기가 다르고 가로모드 및 세로모드일 때도 화면의 크기가 달라진다. 
- constraints 는 특정 뷰 혹은 요소로부터 위아래 양옆으로 얼마나 떨어지게 위치시킬 것인가.
- alignment: 특정 뷰로 부터 수평적, 수직적 정렬 가능
- stack view → vertical, horizontal 둘 다 가능. 각 뷰의 간격도 조정 가능. Fill equally -> 스택 뷰에 포함된 뷰들의 높이(vertical인 경우) 동등하게 함.
