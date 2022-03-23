# Enumerations 열거형

열거형은 관련된 값으로 이루어진 그룹을 공통의 타입으로 선언해 type-safety를 보장하며 코드를 다룰 수 있음

case값 - String, Character, Integer, Floating

1급 클래스 형 first-class types 이어서 계산된 프로퍼티를 제공하거나 초기화를 지정하거나 초기 선언을 확장해 사용할 수 있음

## Enumeration Syntax 열거형 문법

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

> C나 Objective-C 와는 다르게 Swift에서 열거형은 생성될 때 각 case 별로 기본 integer값을 할당하지 않는다. 위 `CompassPoint`
를 예로 들면, north, south, east, west는 각각 암시적으로 0, 1, 2, 3값을 갖지 않는다. 대신 Swift에서 열거형의 각 case는 CompassPoint으로 선언된 온전한 값이다.
> 

타입의 이름은 대문자로 시작해야 함

타입이 한 번 정의되면 다음 값을 할당할 때 타입을 생략한 dot syntax를 이용해 값을 할당하는 축약형 문법을 사용할 수 있음

```swift
var directionToHead = CompassPoint.west
directionToHead = .east
```

---

## Matching Enumeration Values with a Switch Statement - Switch 구문에서 열거형 값 매칭하기

각 열거형 값을 `Switch` 문에서 매칭할 수 있음

`switch` 문은 반드시 열거형의 모든 경우 cases 를 완전히 포함해야 함

`default` case 사용

---

## Associated Values 관련 값

열거형의 각 case에 custom type의 추가적인 정보를 저장할 수 있음

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

---

## Raw Values - Raw 값

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

`Character` , `String`, `Character`, `Integer`, `Float` 등의 타입 사용 가능

단, 각 raw값은 열거형 선언에서 유일한 값

### Implicitly Assigned Raw Values 암시적으로 할당된 Raw 값

열거형에서 Raw 값으로 Integer나 String 값을 사용할 수 있는데, 각 case별로 명시적으로 Raw 값을 할당할 필요는 없음. Swift에서 자동으로 값 할당

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
// mercury = 1, venus = 2, earth = 3, ... .

// String일 경우 -> case 텍스트가 Raw 값으로 할당됨
```

raw값은 `rawValue` 프로퍼티를 사용해 접근

### Initializing from a Raw Value - Raw 값을 이용한 초기화

Raw 값을 이용해 열거형 변수를 초기화할 수 있음

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.uranus
// raw값 7을 갖는 값을 열거형 변수의 초기 값으로 지정
```

### Recursive Enumerations 재귀 열거자

재귀 열거자는 다른 열거 인스턴스를 관계 값으로 갖는 열거형

재귀 열거자 case는 앞에 `indirect` 키워드를 붙여 표시함

모든 열거형 case → enum 키워드 앞에 `indirect` 키워드를 붙여 표시함
