# swift_study
Swift 공식 문서 읽고 정리하기


# The Basics

## Constants and Variables 상수와 변수

상수와 변수는 이름과 특정 타입의 값을 연결한다.
상수 Constants 의 값은 최초 지정 후 변경이 불가능
변수 Variable 의 값은 다른 값으로 변경 가능

### 1. Declaring Constants and Variables 상수와 변수 선언

let 상수 / var 변수

### 2. Type Annotations 타입 명시

상수 혹은 변수 이름 뒤에 콜론과 공백 한 칸 뒤에 사용할 타입 이름을 적어 타입을 명시함

```swift
var welcomeMessage: String
```

### 3. Naming Constants and Variables 상수와 변수의 이름

유니코드 문자를 포함하여 대부분의 문자 포함 가능
포함 불가능: 공백, 수학적 기호, 화살표, 개인전용 유니코드 스칼라 값, 또는 선과 박스를 그리는 문자

### 4. Printing Constants and Variables 상수와 변수 출력

`print(_:separator:terminator:)` 함수로 상수 또는 변수의 현재 값을 출력

`separator` 와 `terminator` 파라미터는 기본값을 가지고 있으므로 생략할 수 있음
줄바꿈 없이 값을 출력하려면 `print(someValue, terminator: "")`

상수 또는 변수의 현재 값으로 대체할 수 있는 *문자열 삽입 (String interpolation)*을 사용: `\(welcomeMessage)`

## Comments 주석

한 줄 주석 //
여러 줄 주석 /* */

여러 줄 주석 안에 다른 여러줄 주석을 중첩시킬 수 있음

## Semicolons 세미콜론

필수 아님 
그러나 여러 구문을 한 줄로 작성할 경우 필수로 작성

## Integers 정수

부호가 있는 정수 (signed - 양수, 0, 음수) 또는 부호가 없는 정수 (unsigned - 양수, 0)
8, 16, 32, 64비트 형태의 부호가 있는 정수와 부호가 없는 정수를 지원함
정수 타입은 대문자로 시작함

### 1. Integer Bounds 정수 범위

각 정수 타입의 min과 max 프로퍼티를 통해 각 정수 타입의 최소값과 최대값을 가져올 수 있음

### 2. Int

- 32-bit 플랫폼에서 `Int` 는 `Int32` 와 같은 크기를 가짐
- 64-bit 플랫폼에서 `Int` 는 `Int64` 와 같은 크기를 가짐

### 3. UInt

- 32-bit 플랫폼에서 `UInt` 는 `UInt32` 와 같은 크기를 가짐
- 64-bit 플랫폼에서 `UInt` 는 `UInt64` 와 같은 크기를 가짐

## Floating-Point Numbers 부동 소수점 숫자

- `Double` 은 64-bit 부동 소수점 숫자를 표기
- `Float` 는 32-bit 부동 소수점 숫자를 표기

## Type Safety and Type Inference 타입 세이프티와 타입 유추

타입 세이프 언어 - 코드가 사용할 수 있는 값의 타입을 명확하게 알 수 있음

타입 검사는 다른 타입의 값으로 작업할 때 오류를 피하는 데 도움이 됨

필요한 값의 특정 타입을 지정하지 않으면 Swift는 적절한 타입으로 *타입 유추*를 사용함

## Numeric Literals 숫자 리터럴

정수 리터럴:

- 접두사 없는 *10진수*
- `0b` 접두사로 *2진수*
- `0o` 접두사로 *8진수*
- `0x` 접두사로 *16진수*

## Numeric Type Conversion 숫자 타입 변환

### 1. Integer Conversion 정수 변환

### 2. Integer and Floating-Point Conversion 정수와 부동 소수점 변환

## Type Aliases 타입 별칭

이미 존재하는 타입을 다른 이름으로 정의함

`typealias` 를 사용하여 정의할 수 있음

## Booleans 부울

## Tuples 튜플

여러 값을 단일 복합 값으로 그룹화함

어떠한 타입도 가능하며 서로 같은 타입일 필요는 없음

## Optionals 옵셔널

값이 있고 옵셔널을 풀어서 값에 접근하거나 값이 없을 수 있음

### 1. nil

nil - 값이 없는 상태를 나타냄

### 2. If Statements and Forced Unwrapping If 구문과 강제로 풀기




# Collection Types 콜렉션

배열 Array, 집합 Set, 딕셔너리 Dictionary

## Mutability of Collections 콜렉션의 가변성

변수에 할당 → 변경 가능 mutable. 콜렉션이 생성된 후에 콜렉션의 아이템을 추가, 삭제 또는 변경할 수 있음

상수에 할당 → 변경 불가능. 크기와 콘텐츠를 변경할 수 없다.

## Arrays 배열

**순서**대로 **같은 타입**의 값을 저장

같은 값은 배열에 다른 순서로 존재할 수 있음

### Array Type Shorthand Syntax 배열 타입 구문

`Array<Element>` or `[Element]` 로 작성

### Creating an Empty Array 빈 배열 생성
