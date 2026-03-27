이 문서는 Altero 프로젝트의 코드 일관성과 유지보수 효율을 위해 작성되없습니다.

기본적으로 [Apple API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) 를 따릅니다.

## 1. 네이밍

### 1-1. Case 규칙

#### upperCamelCase

- 모든 Type (Class, Struct, Enum, Protocol, Extension)

```swift
struct UserProfile {
	// ...
}

enum EmotionType {
	// ...
}
```

#### lowerCamelCase

- 변수, 상수, 함수, Enum Cases, 매개변수

```swift
let userName: String

func fetchData() {
	// ...
}

enum EmotionType {
	case happy
	case angry
}
```

### 1-2. 명확성

- 약어보다는 의미가 명확한 단어를 사용합니다. (bgColor → backgroundColor)
- 함수는 일반적으로 동사로 시작합니다.
- 데이터를 가져오는 함수의 경우, get 사용을 지양하고 request와 fetch를 적정하게 사용합니다.

```swift
// 좋은 예
func fetchData() -> Data?
func requestUserInfo() -> UserInfo?

// 나쁜 예
func getData() -> Data?
func getUserInfo() -> UserInfo?
```

## 2. 코드 구조 및 배치

### 2-1. 들여쓰기 및 빈 줄

- 인덴트는 스페이스바 2개로 통일합니다. (탭1 = 스페이스바2)
- 함수와 함수 사이에는 1줄의 빈 줄을 배치합니다.
- 중괄호 시작( { ) 직후와 종료( } ) 직전에는 빈 줄을 만들지 않습니다.
- 매개변수가 길어질 경우 첫 번째 매개변수부터 줄바꿈하여 왼쪽 정렬합니다.

### 2-2. 주석 활용

- 아직 완료되지 않은 코드가 있다면 TODO를, 수정해야할 사항이 있다면 FIXME를 작성해주세요.

```swift
// TODO: - SNS 로그인 추가 구현 필요
// FIXME: - 로그인 시 메인화면으로 이동에 시간이 걸림
func login() -> User? {
	// ...
}
```

- 연관 코드가 있다면 // MARK: - 를 사용하여 코드영역을 구분합니다.
- struct를 통해 뷰를 나눌 때는 // MARK: - (S) 를 사용합니다.
- @ViewBuilder를 통해 뷰를 나눌 때는 // MARKL - (F) 를 사용합니다.

```swift
class MainViewModel {
    // MARK: - Properties
    private var emotions: [Emotion] = []
    
    // MARK: - Initialization
    init() { ... }
    
    // MARK: - Methods
    func addEmotion() { ... }
}

// MARK: - (S)HomeView
Struct HomeView {
	var body: some View {
		// ...
	}

	// MARK: - (F)item
	@ViewBuilder
	private func item(name: String) -> some View {
		// ...
	}
}
```

## 3. 프로그래밍 스타일

### 3-1. Swift 지향성

- 클래스와 구조체 내부에서는 self를 명시적으로 사용합니다.
- 모든 프로퍼티와 메서든 기본적으로 private로 선언하고, 필요한 경우에 범위를 확장합니다.

### 3.2. 타입 선언

- 명식적 타입 선언을 지향합니다.
- Class, Struct 등 커스텀 타입은 반드시 타입을 명시합니다.
- 명확한 기본 타입(String, Int, Bool 등)에 한해 타입 추론을 허용합니다.

### 3-3. 클로저(Closure)

- 매개변수가 없거나 반환 타입이 없는 경우 ( ) → Void 를 사용합니다.

```swift
let completionBlock: (() -> Void)?
```

- 후행 클로저 문법을 활용합니다.

```swift
// 좋은 예
Button {
	print("버튼 클릭")
} label: {
	Text("Button")
}

// 나쁜 예
Button (
	action: { print("버튼 클릭") },
	label: { Text("Button") }
)
```

### 3-4. 프로토콜 채택

- 클래스 선언부에는 상속받는 클래스만 적고, 프로토콜 구현은 extension으로 분리합니다.

```swift
class ViewController: UIViewController { ... }

// MARK: - UITableViewDataSource
extension ViewController: UITableViewDataSource { ... }
```