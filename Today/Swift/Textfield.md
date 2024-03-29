### Textfield 관련

```swift
// 선언과 동시에 delegate 선언
@IBOutlet weak var textField: UITextField! {
 didSet {
		textField.delegate = self
	}
}

//키보드에서 return키 누르면 키보드 창 닫기
func textFieldSholudReturn(_ textField: UITextField) -> Bool {
	textField.resignFirstResponder()
	return true
}

//텍스트 필드가 아닌 화면 클릭 시 키보드 닫기
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
      self.view.endEditing(true)
  }
```

- TextField 및 TextViewField가 키보드에 가려지는 것을 방지하기 위해 키보드가 보이면 해당 키보드 높이만큼 뷰의 y값 조정
```swift
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)

    if #available(iOS 14.0, *) {
        self.navigationController?.navigationBar.topItem?.backBarButtonItem = UIBarButtonItem(title: "")
    }
    let rightBarbutton = UIBarButtonItem(title: "저장", style: .plain, target: self, action: #selector(recordAdded))
    self.navigationItem.setRightBarButton(rightBarbutton, animated: true)
    //keyboard가 보이고 가려지는 순간에 대한 알림 등록
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow), name: UIResponder.keyboardWillShowNotification, object: nil)
    NotificationCenter.default.addObserver(self, selector:#selector(keyboardWillHide), name: UIResponder.keyboardWillHideNotification, object: nil)
}

override func viewWillDisappear(_ animated: Bool) {
    super.viewWillDisappear(animated)
    //keyboard가 보이고 가려지는 순간에 대한 알림 해제
    NotificationCenter.default.removeObserver(self, name: UIResponder.keyboardWillShowNotification, object: nil)
    NotificationCenter.default.removeObserver(self, name: UIResponder.keyboardWillHideNotification, object: nil)
}
//키보드가 보이면 키보드의 높이 만큼 현재 뷰를 아래로 밀기
@objc func keyboardWillShow(notification: NSNotification){
    guard let userInfo = notification.userInfo else {return}
    guard let keyboardSize = userInfo[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue else {return}
    let keyboardFrame = keyboardSize.cgRectValue
    if self.view.frame.origin.y == 0 {
        self.view.frame.origin.y -= keyboardFrame.height
    }
}
@objc func keyboardWillHide(notification: NSNotification){
    if self.view.frame.origin.y != 0 {
        self.view.frame.origin.y = 0
    }
}
```