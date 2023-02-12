# ignoreElements Operator

## ignoreElements Operator (RxSwift 5)

```swift
public func ignoreElements() -> Completable
```

- Completable
    - Completed 와 Error에 대한 이벤트만 방출하고 Next 이벤트는 무시함
    - 옵저버블의 성공 or 실패에 대한 부분만 알고 싶을 때 사용

```swift
let disposeBag = DisposeBag()
let fruits = ["🍏", "🍎", "🍋", "🍓", "🍇"]

Observable.from(fruits)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(🍏)
// next(🍎)
// next(🍋)
// next(🍓)
// next(🍇)
// completed
```

```swift
let disposeBag = DisposeBag()
let fruits = ["🍏", "🍎", "🍋", "🍓", "🍇"]

Observable.from(fruits)
    .ignoreElements() // ignoreElements에서 next이벤트들을 filtering하기 때문에
                      // next 이벤트들은 전달되지 못하고 completed만 전달됨
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// completed
```

## ignoreElements Operator (RxSwift 6)

```swift
public func ignoreElements() -> Observable<Never>
```

- RxSwift 5에서는 Completable을 리턴했지만 RxSwift 6부터는 Observable<Never>을 리턴
- 동작은 전과 같음