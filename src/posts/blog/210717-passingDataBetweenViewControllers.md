---
icon: "📲"
title: '뷰 컨트롤러 간 데이터 전달 방법 7가지 (Passing Data Between ViewControllers)'
category: "iOS"
date: "2021-06-29 12:00:00 +09:00"
desc: ""
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

# A → B (다음 화면에 데이터 전달) 
</br>
<img src="https://user-images.githubusercontent.com/52592748/126033978-7174f17f-d777-4c39-87fd-196773748eba.png" width="400"/>

## 1. Properties (segue 없이)

`SourceVC` → `DestinationVC`로 이동할 때 `SourceVC`에서 바로 `DestinationVC`의 프로퍼티에 접근해서 데이터를 전달

#### FirstVC

```swift
class SourceViewController: UIViewController {
    @IBOutlet weak var sourceTextField: UITextField!
  
    @IBAction func answerButtonTapped(_ sender: Any) {
        let name = sourceTextField.text ?? ""
        guard let destinationVC = storyboard?.instantiateViewController(identifier: "destinationVC") as? DestinationViewController else {
            return
        }
        
        destinationVC.name = name
        present(destinationVC, animated: true, completion: nil)
    }
}
```

#### SecondVC

```swift
class DestinationViewController: UIViewController {
    @IBOutlet weak var destinationScreenLabel: UILabel!
    var name = ""
    
    override func viewDidLoad() {
        super.viewDidLoad()
        destinationScreenLabel.text = "Hi \(name)"
    }
}
```

---

## 2. Segues

- 스토리보드에서 Source와 Destination을 segue로 연결해준다
- Source에서 Destination으로 `prepare(for segue:sender:)`를 통해 정보를 전달할 수 있다

#### **Source**

```swift
class SourceViewController: UIViewController {
    @IBOutlet weak var sourceTextField: UITextField!
    
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        let name = sourceTextField.text ?? ""
        guard let destination = segue.destination as? DestinationViewController else {
            return
        }
        
        destination.name = name
    }
}
```

#### **Destination**

Destination에서는 `name` 을 받아서 사용할 것이기 때문에 비워둬도 된다

```swift
class DestinationViewController: UIViewController {
    @IBOutlet weak var destinationScreenLabel: UILabel!
    var name = ""
    
    override func viewDidLoad() {
        super.viewDidLoad()
        destinationScreenLabel.text = "Hi \(name)"
    }
}
```

### ⚠️ 화면이동의 구현: 스토리보드 vs 코드

스토리보드는 다양한 화면이동을 제한하고 개발을 복잡하게 만든다고 한다. 게다가 스토리보드나 XIB가 복잡해질 수록 Interface Builder가 많이 느려진다고 한다. 그래서 화면이동을 코드적으로 구현하는 것이 노력도 적게 들고 더 많은 것을 할 수 있다고 한다.

---

# A ← B (되돌아갈 때 데이터 전달)
</br>
<img src="https://user-images.githubusercontent.com/52592748/126033986-68e97da4-b951-4b3a-b12f-0930aee510aa.png" width="600"/>

## 3. Properties and Functions

- secondVC에서 firstVC로 돌아갈 때 데이터 전달
    - secondVC는 firstVC를 옵셔널 프로퍼티로 갖고 있다
    - secondVC로 이동하기 직전, firstVC는 secondVC의 firstVC를 self로 세팅한다
    - 그럼 나중에 secondVC에서 firstVC의 프로퍼티와 메서드를 접근할 수 있다

### present로 이동한 뒤 dismiss로 돌아가는 코드

스토리보드에서 segue를 연결한 경우는 `prepare(for segue:)` 에서 처리해주면 된다

#### FirstVC

```swift
class FirstViewController: UIViewController {
    @IBOutlet weak var juiceReadyLabel: UILabel!

    @IBAction func orderJuiceButtonTapped(_ sender: Any) {
        guard let secondVC = storyboard?.instantiateViewController(identifier: "juiceMenu") as? SecondViewController else {
            return
        }
        secondVC.firstVC = self
        present(secondVC, animated: true, completion: nil)
    }
    
    func juiceMenuIsSelected(_ menu: String) {
        juiceReadyLabel.text = "\(menu) is Ready!"
    }
}
```

#### SecondVC

```swift
class SecondViewController: UIViewController {
    var firstVC: FirstViewController?
    
    @IBAction func juiceMenuButtonTapped(_ sender: UIButton) {
        let menu = (sender.titleLabel?.text)!
        firstVC?.juiceMenuIsSelected(menu)
        dismiss(animated: true, completion: nil)
    } 
}
```

### navigationController를 연결해 pop으로 돌아가는 코드

#### FirstVC

```swift
class FirstViewController: UIViewController {
    @IBOutlet weak var juiceReadyLabel: UILabel!
    
    @IBAction func orderJuiceButtonTapped(_ sender: Any) {
        guard let secondVC = storyboard?.instantiateViewController(identifier: "juiceMenu") as? SecondViewController else {
            return
        }
        secondVC.firstVC = self
        self.navigationController?.pushViewController(secondVC, animated: true)
    }
    
    func juiceMenuIsSelected(_ menu: String) {
        juiceReadyLabel.text = "\(menu) is Ready!"
    }
}
```

#### SecondVC

```swift
class SecondViewController: UIViewController {
    var firstVC: FirstViewController?
    
    @IBAction func juiceMenuButtonTapped(_ sender: UIButton) {
        let menu = (sender.titleLabel?.text)!
        firstVC?.juiceMenuIsSelected(menu)
        navigationController?.popViewController(animated: true)
    } 
}
```

### ⚠️  하지만

- firstVC와 secondVC의 관계는 타이트해서 모듈화가 되지 않는다. 이렇게 서로 엮이고 엮인 코드가 많아지면 spaghetti code가 된다고 한다.
- 위 코드 예시는 retain cycle을 가진다. firstVC가 메모리에서 해제되지 않는이상 secondVC 또한 메모리에서 해제될 수 없다 (firstVC를 프로퍼티로 들고 있기 때문). 하지만 firstVC도 역시 secondVC가 메모리에 남아있는한 해제될 수 없다. (secondVC로 이동하는 코드가 있어서) ⇒ weak 키워드를 사용하는 것도 한 방법
- 두 개발자가 각각 firstVC와 secondVC를 따로 맡아 개발하기 힘들다. 왜냐하면 두 VC들이 서로 타이트하게 연관되어있어 각 개발자들이 상대방의 VC에 대해서도 잘 이해하고 있어야하기 때문이다.

---

## 4. Delegate design pattern - NavigationController

기본이 되는 base class 가 secondVC에게 functionality를 제공한다. firstVC는 base class에게 일을 시키고 secondVC는 base class가 받은 일을 처리한다. protocol을 사용함으로써 두 VC는 decouple 된다.

- `사장화면`에서 받은 정보를 `비서화면`에 전달
    - `비서화면`이 `사장화면`의 delegate
- 역할

#### 프로토콜

```swift
protocol Biseo {
    func juiceMenuIsSelected(_ menu: String)
}
```

#### **비서 (FirstVC)**

```swift
class BiseoViewController: UIViewController {
    @IBOutlet weak var juiceReadyLabel: UILabel!

    @IBAction func orderJuiceButtonTapped(_ sender: Any) {
        guard let sajangVC = storyboard?.instantiateViewController(identifier: "juiceMenu") as? SajangViewController else {
            return
        }

        sajangVC.delegate = self
        self.navigationController?.pushViewController(sajangVC, animated: true)
    }
}

extension BiseoViewController: Biseo {
    func juiceMenuIsSelected(_ menu: String) {
        juiceReadyLabel.text = "\(menu) is Ready!"
    }
}

```

#### **사장 (SecondVC)**

```swift
class SajangViewController: UIViewController {
    weak var delegate: Biseo?

    @IBAction func juiceMenuButtonTapped(_ sender: UIButton) {
        let menu = (sender.titleLabel?.text)!

        delegate?.juiceMenuIsSelected(menu)

        navigationController?.popViewController(animated: true)
    }
}

```

### 🥊  3번에 비해 좋은 점

- 비서화면과 사장화면을 각각 개발하는 개발자들은 Protocol에 대해서만 알고있으면 된다. 각자 채택해서 적용하거나 말거나 마음대로
- 두 화면 사이의 direct connection 이 없다. 이전 예시보다 더 loosely coupled 됐다고 볼 수 있다.
- 프로토콜은 비서화면말고 다른 VC들도 채택할 수 있다

---

## 5. Closures

#### FirstVC

```swift
class FirstViewController: UIViewController {
    @IBOutlet weak var juiceReadyLabel: UILabel!
    
    @IBAction func orderJuiceButtonTapped(_ sender: Any) {
        guard let secondVC = storyboard?.instantiateViewController(identifier: "secondVC") as? SecondViewController else {
            return
        }

        secondVC.juiceMenuIsSelected = {
            self.juiceReadyLabel.text = "\($0) is Ready!"
        }
        present(secondVC, animated: true, completion: nil)
    }
}
```

#### SecondVC

```swift
class SecondViewController: UIViewController {
    var juiceMenuIsSelected: ((String) -> Void)?
    
    @IBAction func juiceMenuButtonTapped(_ sender: UIButton) {
        let menu = (sender.titleLabel?.text)!
        juiceMenuIsSelected?(menu)
        
        dismiss(animated: true, completion: nil)
    } 
}
```


### 🥊  클로저 사용의 이점과 주의점

- protocol과 delegation 사용없이 간단하고 빠르게 정보 전달이 가능
- 클로저를 여러 클래스에 전달하고 싶을 때 좋다
- 클로저를 데이터를 전달받은 곳에서 직접 구현하고 싶을 때

> One of the risks of using closures to pass data between view controllers is that your code can become very dense. It’s smartest to only use closures to pass data between view controllers if it makes sense to use closures over any other method – instead of just using closures because they’re so convenient!

---

## 6. Notification Center

#### Notification 생성하기

- Notification을 원하는 이름으로 생성한다
    - notification들은 각각 자신을 구분할 이름이 있다
    - 생성 후 전역 변수에 담아 어디서든 접근할 수 있도록 한다
    - `상위클래스.myNotification` 으로 접근

```swift
static let juiceNotification = Notification.Name("juiceIsReady")
```

- `Notification.Name` extension
- `NotificationCenter`에는 **세 가지 역할**이 있다: **observing, posting, receiving**


### 🥊  NotificationCenter의 용도

- 데이터를 전달하고 싶은 VC나 클래스들이 연결되어 있지 않을 때
    - ex. REST API가 새로운 데이터를 받은 것을 tableViewController가 알고 싶을 때
- 앱의 일부가 뒤늦게 생성되거나 짧은 lifecycle을 가질 때
    - tableView가 화면에 나타나기 전에 REST API가 데이터를 이미 받은 경우 (아마 tableView가 생성되면 notificationCenter에서 REST API가 보내둔 데이터를 가져오는 것 같음)
- 여러 VC들이 하나의 notification에 반응해야할 때
- 하나의 VC가 여러 notification에 반응해야할 때
- 반복적이고 주기적으로 데이터를 보내고 싶을 때

---

## 7. Singleton design pattern

앱이 특정 클래스의 인스턴스 하나만 갖고 있을 것을 보장하고 해당 인스턴스는 앱 어디에서든지 접근이 가능하다

싱글턴은 안티패턴이지만 사용하기 적당한 때가 있다

- ex. 앱의 세팅을 관리할 때
    - 사용자가 소리를 끄면 앱 전반에 반영되어야 한다

#### Singleton

```swift
class JuiceMenu {
    static let juiceMenu = JuiceMenu()

    private(set) var menu: String?
    
    private init() {}
    
    func select(menu: String) {
        self.menu = menu
    }
}
```

#### FirstVC

SecondVC가 FirstVC에게 JuiceMenu가 바뀌었는지 알려줄 방법이 없다

- 따라서 `viewWillAppear()` 에서 매번 라벨을 바꾸도록 설정했다
- 단, 일반 모달은 첫화면이 사라지지 않기 때문에 `viewWillAppear()` 가 처음 한번만 호출되고 그 뒤는 호출되지 않는다 (SecondVC를 아무리 띄웠다 돌아가도 FirstVC의 `viewWillAppear()` 는 호출되지 않음)
- 따라서 FirstVC의 `viewWillAppear()` 를 띄우기 위해 presentation style을 fullScreen으로 설정해줬다

```swift
class FirstViewController: UIViewController {
    @IBOutlet weak var juiceReadyLabel: UILabel!
    
    @IBAction func orderJuiceButtonTapped(_ sender: Any) {
        guard let secondVC = storyboard?.instantiateViewController(identifier: "secondVC") as? SecondViewController else {
            return
        }
        secondVC.modalPresentationStyle = .fullScreen
        present(secondVC, animated: true, completion: nil)
    }
    
    override func viewWillAppear(_ animated: Bool) {
        if let menu = JuiceMenu.juiceMenu.menu {
            juiceReadyLabel.text = "\(menu) is Ready!"
        } else {
            juiceReadyLabel.text = "Menu Ready"
        }
    }
}
```

#### SecondVC

```swift
class SecondViewController: UIViewController {
    
    @IBAction func juiceMenuButtonTapped(_ sender: UIButton) {
        let menu = (sender.titleLabel?.text)!
        JuiceMenu.juiceMenu.select(menu: menu)
        dismiss(animated: true, completion: nil)
    }
}
```

---

> 참고 링크 </br>
[5 Ways to Pass Data Between View Controllers](https://betterprogramming.pub/5-ways-to-pass-data-between-view-controllers-18acb467f5ec)</br>
[Pass Data Between View Controllers - LearnAppMaking](https://learnappmaking.com/pass-data-between-view-controllers-swift-how-to/#back-delegation)
