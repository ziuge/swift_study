# 자동 참조 카운팅 Automatic Reference Counting

**자동 참조 카운팅이란?**

- 앱의 메모리 사용량을 추적하고 관리하기 위해 사용
- 인스턴스가 더이상 필요하지 않을 때 인스턴스에 의해 사용된 메모리를 할당 해제한다.

---

## ARC의 작동 원리 How ARC Works

- 클래스의 새로운 인스턴스가 생성될 때마다 - ARC는 인스턴스에 대한 정보를 저장하기 위해 적정 크기의 메모리를 할당함 - 프로퍼티 값과 인스턴스 타입 정보 저장
- 인스턴스가 더이상 필요하지 않을 때 - ARC는 해당 메모리가 다른 목적으로 사용될 수 있도록 할당 해제함
- 그러나 ARC가 아직 사용중인 인스턴스를 할당 해제하면 접근/호출 불가능함
- 인스턴스가 필요할 동안에는 사라지지 않도록 ARC는 어떤 프로퍼티/상수/변수가 각 클래스 인스턴스에 참조하고 있는지 추적하고, 참조가 하나라도 존재하는 한 할당 해제하지 않는다.
- 이것을 가능하게 하기 위해: 프로퍼티/상수/변수에 클래스 인스턴스를 할당할 때마다 해당 프로퍼티/상수/변수는 인스턴스에 **강한 참조***(strong reference)*를 만듦.

---

## ARC 동작 ARC in Action

ARC 동작 예제

```swift
// name이라는 상수 프로퍼티를 정의하는 클래스 Person
class Person {
    let name: String
    init(name: String) {
        self.name = name
				// 초기화 진행 중 표시
        print("\(name) is being initialized")
    }
    deinit {
				// 해지된다면 프린트
        print("\(name) is being deinitialized")
    }
}

var reference1: Person?
var reference2: Person?
var reference3: Person?

// 하나의 변수에 Person 인스턴스 생성해 참조
reference1 = Person(name: "John Appleseed")
// Prints "John Appleseed is being initialized"

reference2 = reference1
reference3 = reference1
// 모두 같은 Person 인스턴스 참조. 참조 횟수는 3

reference1 = nil
reference2 = nil
// 참조 횟수 1, Person 인스턴스 해지되지는 않음

reference3 = nil
// Prints "John Appleseed is being deinitialized"
// 참조 횟수 0, 메모리에서 Person 인스턴스 해지
```

---

## 클래스 인스턴스간 강한 참조 순환 Strong Reference Cycles Between Class Instances

절대로 메모리에서 해제되지 않는 경우?
→ 클래스 인스턴스간 강한 상호 참조를 하고 있는 경우: **강한 참조 순환**

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
// 서로 변수로 서로를 소유하고 있음

var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
// Person 인스턴스의 참조 횟수 2
// Apartment 인스턴스의 참조 횟수 2
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f992fe71-30f5-41af-b9c1-7ae232071407/Untitled.png)

```swift
// 참조 해지
john = nil
unit4A = nil

// 변수가 각각 상호 참조 하고있어 참조 횟수는 1
// 두 인스턴스는 해지되지 않고 메모리 누수가 발생함
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f5640099-52b0-4253-8275-d2b0db8ed0da/Untitled.png)

---

## 클래스 인스턴스간 강한 참조 순환 문제의 해결 Resolving Strong Reference Cycles Between Class Instances

1. 약한 참조
2. 미소유 참조

### 1. 약한 참조 Week References

- 참조하고 있는 인스턴스가 먼저 메모리에서 해제될 때 사용
- ARC는 약한 참조로 선언된 참조 대상이 해지되면 런타임에 자동으로 참조하고 있는 변수에 nil 할당

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person? // 약한 참조
    deinit { print("Apartment \(unit) is being deinitialized") }
}

// Person 인스턴스와 Apartment 인스턴스의 변수에서 각각 인스턴스를 상호 참조하도록 할당
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/647d49c9-9761-4b25-a684-49991c019dec/Untitled.png)

```swift
john = nil
// Prints "John Appleseed is being deinitialized"
// 아래 그림과 같이 Person 인스턴스를 메모리에서 해지함
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/54ce5ae9-68a4-4fae-8db4-f076e379fc4e/Untitled.png)

```swift
unit4A = nil
// Prints "Apartment 4A is being deinitialized"
// Apartment 인스턴스를 참조하는 개체도 사라져, Apartment 인스턴스도 메모리에서 해지됨
```

> 가비지 콜렉션을 사용하는 시스템에서 weak pointer를 단순한 시스템 캐싱 목적으로 사용하기도 함. (메모리 소모가 많아지면 가비지 콜렉터를 실행해서 강한 참조가 없는 객체를 메모리에서 해제하는 식으로 동작하기 때문) 하지만 ARC는 이 경우와 다르게 참조 횟수가 0이 되는 즉시 해당 인스턴스를 제거하기 때문에 약한 참조를 이런 목적으로 사용할 수 없음.
> 

### 2. 미소유 참조 Unowned References

- 참조하고 있는 인스턴스가 같은 시점 혹은 더 뒤에 해제될 때 사용
- 참조 대상이 되는 인스턴스가 현재 참조하고 있는 것과 같거나 더 긴 생애주기(longer lifetime)를 갖기 때문에 항상 참조에 그 값이 있다고 기대함.
- 그래서 ARC는 미소유 참조에는 절대 nil을 할당하지 않음. 미소유 참조는 옵셔널 타입을 사용하지 않음.

> 미소유 참조는 참조 대상 인스턴스가 항상 존재한다고 생각 → 해제됐는데 접근하면 런타임 에러
> 

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
    deinit { print("Card #\(number) is being deinitialized") }
}

// Customer는 card 변수로 CreditCard 인스턴스 참조
// CreditCard는 customer로 Customer 인스턴스 참조
// customer는 미소유 참조 unowned로 선언 (신용카드는 없더라도 사용자는 남아있기 때문)

var john: Customer?

john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
// John이 Customer 인스턴스 참조, CreditCard 인스턴스 Customer 인스턴스 미소유 참조
// Customer 인스턴스에 대한 참조 횟수는 1
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3dac7008-6973-43b4-92b5-c90ea2fdde69/Untitled.png)

참조를 끊으면 아래 그림과 같음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab9be91e-265a-4f1a-9d7d-b21e5fc5c167/Untitled.png)

```swift
john = nil
// Prints "John Appleseed is being deinitialized"
// Prints "Card #1234567890123456 is being deinitialized"
// 더이상 Customer 인스턴스를 강하게 참조하는 인스턴스 없음
// Customer 인스턴스 해제됨 -> CreditCard 인스턴스 참조하는 개체도 사라짐 -> CreditCard 인스턴스도 메모리에서 해제
```

### 미소유 참조와 암시적 옵셔널 프로퍼티 언래핑 Unowned References and Implicitly Unwrapped Optional Properties

해당 참조가 nil이 될 수 있는지 여부에 따라 → 약한 참조, 미소유 참조

제3의 경우도 발생할 수 있음! :
두 프로퍼티가 항상 값을 갖지만 한 번 초기화 되면 절대 nil이 되지 않는 경우
→ 미소유 프로퍼티를 암시적 옵셔널 프로퍼티 언래핑을 사용해 참조 문제를 해결

```swift
class Country {
    let name: String
    var capitalCity: City! // 강제 언래핑
    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity = City(name: capitalName, country: self)
    }
}

class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}

/* 
Country의 capitalCity는 초기화 단계에서 City 클래스에 초기화된 후 사용됨
옵셔널이 되어야 하는데 여기서는 강제 언래핑 시킴 
암시적 언래핑이 되어 Country에서 name이 초기화 되는 시점에 self를 사용할 수 있게 됨
*/ 
// City에서는 강한 참조 순환을 피하기 위해 미소유 참조로 country를 선언해서 사용

var country = Country(name: "Canada", capitalName: "Ottawa")
print("\(country.name)'s capital city is called \(country.capitalCity.name)")
// Prints "Canada's capital city is called Ottawa"
```

---

## 클로저에서의 강한 참조 순환 Strong Reference Cycles for Closures

강한 참조 순환 클로저에서도 발생 - 클로저에서는 self를 캡쳐하기 대문
→ 클로저 캡쳐 리스트 사용

```swift
class HTMLElement {
    let name: String
    let text: String?
    lazy var asHTML: () -> String = {
        if let text = **self.text** { // -> self 캡쳐
            return "<\(**self.name**)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }
    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}

// asHTML 클로저는 다른 클로저로 변경될 수 있음
let heading = HTMLElement(name: "h1")
let defaultText = "some default text"
heading.asHTML = {
    return "<\(heading.name)>\(heading.text ?? defaultText)</\(heading.name)>"
}
print(heading.asHTML())
// Prints "<h1>some default text</h1>"

var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Prints "<p>hello, world</p>"
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/89c489a2-4ddc-4634-a064-1c81bcfa513b/Untitled.png)

인스턴스와 클로저 간에 강한 참조를 하게 되어 강한 참조 순환에 빠지게 됨

```swift
paragraph = nil
// HTMLElement 인스턴스는 해제되지 않음.
```

---

## 클로저에서 강한 참조 순환 문제의 해결 Resolving Strong Reference Cycles for Closures

캡쳐 참조에 강한 참조 대신 약한 참조 혹은 미소유 참조를 지정할 수 있음
(둘 중 어느 것? ← 상호 관계에 달림)

### 캡쳐 리스트 정의 Defining a Capture List

클로저의 파라미터 앞에 소괄호[]를 넣고 그 안에 각 캡쳐 대상에 대한 참조 타입을 적어줌

```swift
lazy var someClosure: (Int, String) -> String = {
    [unowned self, weak delegate = self.delegate!] (index: Int, stringToProcess: String) -> String in
    // closure body goes here
}
```

클로저의 파라미터가 없고 반환 값이 추론에 의해 생략 가능한 경우 캡쳐리스트 정의를 in 앞에 적어줌

```swift
lazy var someClosure: () -> String = {
    [unowned self, weak delegate = self.delegate!] in
    // closure body goes here
}
```

### 약한 참조와 미소유 참조 Weak and Unowned References

참조가 먼저 해제되는 경우 → **약한 참조**

참조가 같은 시점이나 나중 시점에 해제되는 경우 → **미소유 참조**

> 만약 캡쳐리스트가 절대 nil이 될 수 없으면 미소유 참조 리스트로 캡쳐되어야 함

```swift
class HTMLElement {
    let name: String
    let text: String?
    lazy var asHTML: () -> String = {
        [unowned self] in
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }
    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}

var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Prints "<p>hello, world</p>"
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcbb8b70-4f00-4610-8ea1-7a0ca0450eda/Untitled.png)

```swift
// paragraph의 참조를 제거하면 HTMLElement 인스턴스가 바로 메모리에서 해제되는 것 확인
paragraph = nil
// Prints "p is being deinitialized"
```
