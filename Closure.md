# Closures 클로저

자체 포함된 기능 블럭, 코드 안에서 돌아다니고 사용할 수 있음

정의된 컨텍스트에서 모든 상수와 변수에 대한 참조를 캡쳐하고 저장할 수 있음. 이러한 상수와 변수를 폐쇄(closing over)라고 함. 

- 클로저의 3가지 형테
    1. Global funcions 전역 함수: 이름을 가지고 어떠한 값도 캡처하지 않는 클로저
    2. Nested functions 중첩 함수: 이름을 가지고 둘러싼 함수로부터 값을 캡처할 수 있는 클로저
    3. Closure expressions 클로저 표현식: 주변 컨텍스트에서 값을 캡처할 수 있는 경량 구문으로 작성된 이름이 없는 클로저

클로저 표현식 최적화:

- 컨텍스트에서 파라미터와 반환값 타입 유추
- 단일 표현식 클로저의 암시적 반환
- 약식 인자 이름
- 후행 클로저 구문

## Closure Expressions 클로저 표현식

중첩 함수 Nested Functions는 더 큰 함수에 포함된 코드 블럭의 이름을 지정하고 정의하기 편리한 수단이나, 완전한 선언과 이름없이 함수와 유사한 구조의 짧은 버전을 작성하는 것이 유용할 때도 있다. (예를 들어 함수를 하나 이상의 인자로 사용하는 함수 혹은 메서드로 작업하는 경우)

클로저 표현식은 간단하고 집중적인 구문으로 인라인 클로저로 작성함

짧은 형태로 클로저를 작성하기 위한 몇 가지 구문 최적화를 제공함

### The Sorted Method 정렬 메서드
`sorted(by:)`

사용자가 제공하는 정렬 클로저의 출력을 기반으로, 알려진 타입의 값들의 배열을 정렬하는 메서드

정렬 프로세스가 완료되면 원본 배열을 정렬하여 새로운 배열로 return함 (원본 배열은 수정되지 않음)

```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// reversedNames is equal to ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

### Closure Expression Syntax 클로저 표현구

```swift
{ (parameters) -> return type in 
	statements
}
```

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
// same
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

### Inferring Type From Context 컨텍스트로 타입 유추

정렬 클로저는 메서드에 인자로 전달되기 때문에 Swift는 파라미터 타입과 반환되는 값의 타입을 유추할 수 있음 → 타입이 적힌 소괄호 생략 가능

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

### Implicit Returns from Single-Expression Closures 단일 표현 클로저의 암시적 반환

`return` 키워드를 생략하여 단일 표현식으로 암시적으로 값을 반환할 수 있음

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

### Shorthand Argument Names 짧은 인자 이름

인라인 클로저에 `$0`, `$1`, `$2` 등 클로저의 인자값으로 참조하는데 사용할 수 있는 짧은 인자 이름 제공

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

### Operator Methods 연산자 메서드

```swift
reversedNames = names.sorted(by: >)
```

---

## Trailing Closures 후행 클로저

함수의 마지막 인자로 클로저 표현식을 전달해야 하고 클로저 표현식이 긴 경우 후행 클로저로 작성하는 것이 유용할 수 있음

함수의 인자이지만 함수 호출의 소괄호 다음에 작성함

---

## Capturing Values 캡처값

클로저는 상수와 변수를 캡처하여, 정의한 원래 범위가 존재하지 않더라도 바디 내에서 해당 상수와 변수의 값을 참조하고 수정할 수 있음

---

## Closures Are Reference Types 클로저는 참조 타입

함수나 클로저를 상수 또는 변수에 할당할 때마다 실제로 해당 상수 또는 변수를 함수나 클로저에 대한 **참조**로 설정함
서로 다른 2개의 상수 또는 변수에 클로저를 할당한다면 2개의 상수 또는 변수는 모두 같은 클로저를 참조한다는 것을 의미

---

## Escaping Closures 이스케이프 클로저

---

## Autoclosures 자동 클로저

자동 클로저는 함수에 인자로 전달되는 표현식을 래핑하기 위해 자동으로 생성되는 클로저

인자를 가지지 않음. 호출될 때 내부에 래핑된 표현식의 값을 반환함
