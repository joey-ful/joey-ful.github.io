---
icon: "⚔️"
title: 'if let과 guard let (unwrapping optionals)'
category: "Swift"
date: "2021-03-28 12:00:00 +09:00"
desc: "If vs Guard"
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

### if let vs guard let
- **guard let**은 unwrap에 **실패하면 함수나 루프를 탈출**한다. 따라서 함수에 guard let 이후의 내용은 실행되지 않는다
  - 다음 코드를 진행하기 위해 절대 실패하면 안되는 것을 guard 문에 담는다
  - 실패시 바로 return하거나 throw error
- **guard let**으로 unwrap한 optional은 **guard 이후에 값이 남아있는다**. 따라서 guard let을 시행 후 해당 변수를 계속 사용할 수 있다.

```swift
func yourName(is name: String?) {
    guard let unwrapped = name else {
        print("What's your name?")
        return
    }

    print("Hello, \(unwrapped)!")
}

var name: String? = nil
yourName(is: name)
//What's your name?
yourName(is: "Joey")
//Hello, Joey!
```

### 사용법
- 단순히 특정 optional을 unwrap하고 싶다면 **if let**을 사용
- 코드 실행 전 특정 변수나 조건들을 미리 체크하고 싶다면 **guard let**을 사용 (예를 들어 method 상단에서 guard로 optional들을 모두 unwrap한 후 아래에 코드 작성)