# range, generate

정수를 지정된 수만큼 방출하는 observable

## range

- 최초값에서 1씩 증가하는 정수값을 방출

```swift
let disposeBag = DisposeBag()

Observable.range(start: 1, count: 10)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// next(6)
// next(7)
// next(8)
// next(9)
// next(10)
// completed
// 1씩 증가, 증가되는 크기를 바꾸거나 감소하는 시퀀스? => generate
```

## generate

- initialState: 시작값 (가장 먼저 방출될 값)
- condition: true를 리턴하는 경우에만 요소가 방출, false를 리턴하면 completed 이벤트를 전달하고 바로 종료
- iterate: 값을 변화시키는 코드

```swift
let disposeBag = DisposeBag()
let red = "🔴"
let blue = "🔵"

Observable.generate(initialState: 0, condition: { $0 <= 10 }, iterate: { $0 + 2 })
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(0)
// next(2)
// next(4)
// next(6)
// next(8)
// next(10)
// completed

Observable.generate(initialState: red, condition: { $0.count < 15 }, iterate: { $0.count.isMultiple(of: 2) ? $0 + red : $0 + blue })
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(🔴)
// next(🔴🔵)
// next(🔴🔵🔴)
// next(🔴🔵🔴🔵)
// next(🔴🔵🔴🔵🔴)
// next(🔴🔵🔴🔵🔴🔵)
// next(🔴🔵🔴🔵🔴🔵🔴)
// next(🔴🔵🔴🔵🔴🔵🔴🔵)
// next(🔴🔵🔴🔵🔴🔵🔴🔵🔴)
// next(🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵)
// next(🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵🔴)
// next(🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵)
// next(🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵🔴)
// next(🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵🔴🔵)
// completed
```