# take(while:), take(until:), take(for:scheduler:) Operator (RxSwift 6)

## take(while:)

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .take(while: { !$0.isMultiple(of: 2) })
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// completed

Observable.from(numbers)
    .take(while: { !$0.isMultiple(of: 2) }, behavior: .inclusive) // 마지막에 확인한 값이 false더라도 방출
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// next(2)
// completed
```

## take(until:)

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let subject = PublishSubject<Int>()
let trigger = PublishSubject<Int>()

// 1 - Observable을 파라미터로 받는
subject.take(until: trigger)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

subject.onNext(1)
// next(1)
subject.onNext(2)
// next(2)

trigger.onNext(0)
// completed - trigger에서 요소를 방출하면 completed가 전달됨

subject.onNext(3)
// 더 이상 요소를 전달하지 않음

// 2 - closure를 파라미터로 받고 조건이 false인 동안 전달
subject.take(until: { $0 > 5 })
    .subscribe { print($0) }
    .disposed(by: disposeBag)

subject.onNext(1)
// next(1)
subject.onNext(2)
// next(2)
subject.onNext(6)
// completed

subject.onNext(2) // 이미 종료되어서 전달되지 않음
```

## take(for:scheduler:)

```swift
let disposeBag = DisposeBag()

let o = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)

o.subscribe { print($0) }
    .disposed(by: disposeBag)
// next(0)
// next(1)
// completed
```