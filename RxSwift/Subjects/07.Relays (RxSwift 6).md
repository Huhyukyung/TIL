# Relays (RxSwift 6)

- **PublishRelay**
    - 내부에 PublishSubject를 wrapping하고 있음
- **BehaviorRelay**
    - 내부에 BehaviorSubject를 wrapping하고 있음
- **ReplayRelay** << new! (RxSwift 6+)
    - 내부에 ReplaySubject를 wrapping하고 있음
- next 이벤트만 전달, completed, error 이벤트는 전달받지도 전달하지도 않음 → 종료되지 않음
- 구독자가 disposed 되기 전까지 계속 이벤트 처리 → 주로 UI이벤트 처리에 활용
- RxCocoa 프레임워크를 통해 제공

```swift
let bag = DisposeBag()

let pRelay = PublishRelay<Int>()
pRelay.subscribe { print("1: \($0)") }
    .disposed(by: bag)

pRelay.accept(1) // onNext가 아닌 accept 메소드 사용
// 1: next(1)

let bRelay = BehaviorRelay(value: 1)
bRelay.accept(2)

bRelay.subscribe { print("2: \($0)")}
    .disposed(by: bag)
// 2: next(2)  // 처음에 생성자로 만든 1이 아닌 가장 최근 이벤트인 2가 전달됨

bRelay.accept(3)
// 2: next(3)

print(bRelay.value) // BehaviorRelay가 저장하고 있는 next 이벤트값을 리턴, 읽기 전용
// 3

let rRelay = ReplayRelay<Int>.create(buffersize: 3)

(1...10).forEach { rRelay.accept($0) }

rRelay.subscribe { print("3: \($0)") }
    .disposed(by: bag)
// 3: next(8)
// 3: next(9)
// 3: next(10)
```