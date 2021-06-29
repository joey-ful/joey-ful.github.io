---
title: 'Class vs Struct in Swift'
category: "Swift"
date: "2021-03-28 12:00:00 +09:00"
desc: ""
thumbnail: "./images/markdown-test/thumbnail.jpg"
alt: "markdown logo"
---

### Class와 Struct의 차이점 in Swift

1. class는 struct와 달리 memberwise initializer가 없다. 따라서 변수를 받으려면 **class는 직접 init() 함수를 만들어야 한다.**

```swift
//struct는 username변수를 외부에서 받지만 init()이 필요없다
struct User {
    var username: String
}

//class
class Dog {
	var type: "Dog"
    var name: String
    var breed: String

    init(name: String, breed: String) {
        self.name = name
        self.breed = breed
    }
}
```

2. 기존 class를 바탕으로 **자식 class를 만들 수 있다** (inheritance/subclassing)

```swift
//Dog class를 바탕으로 자식 클래스 생성1
class Pomeranian: Dog {
}

//Dog class를 바탕으로 자식 클래스 생성2
class Poodle: Dog {
    init(name: String) {
        super.init(name: name, breed: "Poodle")
    }
}
```

3. struct는 복사, **class는 참조한다**. 따라서 하나의 자식 class에서 부모 class의 프로퍼티 값을 변경하면 부모 및 모든 자식들의 값 또한 변경된다

```swift
//클래스
class Singer {
    var name = "Taylor Swift"
}

var singer = Singer()
print(singer.name)
//Taylor Swift

var singerCopy = singer
singerCopy.name = "Justin Bieber"
print(singerCopy.name)
print(singer.name)
//Justin Bieber
//Justin Bieber
```

4. class는 **deinitializer**가 있다. class는 ARC(Automatic Reference Counting)을 수행해 class 인스턴스의 참조 횟수를 센다. 더이상 참조되지 않으면 인스턴스의 deinit()이 실행되어 제거된다.

5. struct가 constant로 선언되면 struct내 변수들을 변경할 수 없다. 하지만 **class는 constant여도 class 내 변수들을 변경할 수 있다**. 특정 변수의 변경을 원하지 않으면 class 내 변수를 constant로 선언하면 된다

```swift
class Singer {
    let name = "Taylor Swift"
}
```