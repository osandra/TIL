## TIL

---
**Udamy Angela Yu Swift 강의 노트**

- 같이 묶이는 값들을 딕셔너리에 저장하면 if else 구문으로 찾지 않고 key 값을 통해 바로 해당하는 value를 얻을 수 있음

>  계란 삶는 시간이 지나면 타이틀 이름 done 으로 바꾸기
1. 타이틀 선언  @IBOutlet weak var titleDone: UILabel!
2. 남은 시간이 0초가 되면 타이틀을 바꾸는 changeTitle 함수 선언
3. 남은 시간을 업데이트 하는 함수에서 남은 시간이 0이면 해당 함수 호출
4. 계란 버튼을 누를 때마다 타이틀이 계란을 삶고 있다고 바뀌게 하기 

```swift
import UIKit
var counter = 60
class ViewController: UIViewController {
        //1번
    @IBOutlet weak var titleDone: UILabel!
        //2번
        func changeTitle() {
        if counter == 0 {
            self.titleDone.text = "Done!" //self 없어도 가능
        }
        }
    var timer = Timer()
    let eggTimes = ["Soft": 3, "Medium": 4, "Hard": 7]
    @IBAction func hardnessSelected(_ sender: UIButton) {
        timer.invalidate()
        let hardness = sender.currentTitle!
                //4번
        self.titleDone.text = "Boiling \(hardness) eggs..."
        counter = eggTimes[hardness]!
        timer = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector: #selector(updateCounter), userInfo: nil, repeats: true)
    }
    
    @objc func updateCounter() {
        if counter > 0 {
            print("\(counter) second.")
            counter -= 1
        }
        if counter == 0{
                            //3번
            changeTitle()
        }
    }
```

→ BUT 해당 과정은 복잡한 편 + 남는 시간이 0초인데도 타이머를 멈추지 않아 changeTitle함수를 계속 호출하는 단점, 텍스트 하나를 바꾸는 데 함수가 필요하진 않음 

→ 수정 | 남는 시간이 0이 되면 **타이머를 무효화하여 계속 타이머가 진행되는 것을 막고, 라벨을 바꾼다.**

```swift
class ViewController: UIViewController {
	 //1번
    @IBOutlet weak var titleDone: UILabel!
    var timer = Timer()
    let eggTimes = ["Soft": 300, "Medium": 420, "Hard": 720] //초 단위
    @IBAction func hardnessSelected(_ sender: UIButton) {
        timer.invalidate()
        let hardness = sender.currentTitle!
        self.titleDone.text = "Boiling \(hardness) eggs..."
        counter = eggTimes[hardness]!
        timer = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector: #selector(updateCounter), userInfo: nil, repeats: true)
    }
    
    @objc func updateCounter() {
        if counter > 0 {
            print("\(counter) second.")
            counter -= 1
        }
        if counter == 0{
						//2번
            timer.invalidate()
						//3번
            titleDone.text = "Done"
        }
    }
}
```

> 계란 삶는 속도 진행사항 보여주기. <br>진행사항 Progress Bar 는  float 형
1. Progress Bar를 보여주는 함수 생성 (파라미터로 삶는 시간의 숫자형을 받는다) . 먼저 전역 변수로 totalTime 선언하고, 버튼(계란) 클릭시 실행되는 함수에서 totalTime 값을 해당 버튼(계란)의 시간 값으로(ex 30) 변경. <br>
counter 변수는 시간이 지날수록 1씩 줄어드므로 계속 숫자가 줄어들지 않는 시간을 담는 변수totalTime이 필요함. <br>
progessNum가 float형이라서 분자와 분모 모두 float형이어야 함. 
2. updateCounter에서 showProgess함수 호출. 

```swift
func showProgess(eggTime: Int){
        let totalTime = eggTime
        var progessNum: Float = 0.0
        progessNum = Float(totalTime - (counter-1)) / Float(totalTime)
        print(progessNum)
        timeProgessBar.progress = progessNum
    }
@objc func updateCounter() {
        showProgess(eggTime: totalTime)
        if counter > 0 {
            print("\(counter) second.")
            counter -= 1
        }
        if counter == 0{
            timer.invalidate()
            titleDone.text = "Done"
        }
    }
```

→ 수정 | updateCounter() 안에서 해당 프로그래스 바 값을 바로 변경할 수 있음. 전체시간을 나타내는 totalTime과 지나간 시간을 나타내는 변수 secondsPassed를 선언해서, if 조건문을  변경하면 가능. 
기존 코드를 변경하면 더 나은 코드 작성할 수 있음. **기존 코드에서 선언한 변수 중에 없애고 새로 작동 방식을 바꿀 코드가 있는 지 한 번 살펴보고 코드를 작성하기** +  버튼을 다시 클릭하면 기존 진행 바 및 변수 초기화하기 

```swift
class ViewController: UIViewController {
		//.../생략
		var totalTime = 0
		var timePassed = 0
		@IBAction func hardnessSelected(_ sender: UIButton) {
		        timer.invalidate()
		        let hardness = sender.currentTitle!
		        titleDone.text = "Boiling \(hardness) eggs..."
		        totalTime = eggTimes[hardness]!
		        timeProgessBar.progress = 0
		        timePassed = 0
		        timer = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector: #selector(updateCounter), userInfo: nil, repeats: true)
		    }
		    @objc func updateCounter() {
		        if timePassed < totalTime {
		            timePassed += 1
		            let progressNumber = Float(timePassed) / Float(totalTime)
		            timeProgessBar.progress = progressNumber
		            
		        }
		        else {
		            timer.invalidate()
		            titleDone.text = "Done"
		        }
		    }
]
```
- override func viewDidLoad() 함수는 처은 로딩 될 때 한 번만 실행함
- 2D array →  배열을 담은 배열

```swift
 let array [
	[1, 2, 3].
	[4. 5. 6].
	[7, 8, 9].
]
//숫자 9는 array[2][2]
//숫자 2는 array[0][1]
//array Type : [[Int]]
```

- **self  키워드는 추후 구조체에서 생성되는 개체를 참조함**