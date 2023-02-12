# skip, skipWhile, skipUntil Operator (RxSwift 5)

## skip

- 지정된 수만큼  next 이벤트를 무시한 다음 이후 방출되는 요소들만 전달

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .skip(3) // index가 아니라 스킬할 count
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(4)
// next(5)
// next(6)
// next(7)
// next(8)
// next(9)
// next(10)
// completed
```

# skipWhile

- 조건이 true인 동안 요소를 무시
- false를 리턴하면 그 때부터 요소 방출, 이후에는 조건을 판단하지 않고 모두 방출

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .skipWhile { !$0.isMultiple(of: 2) } // 2일 때 false를 리턴하고 그 이후부터 모든 요소를 방출
    .subscribe { print($0) }
    .disposed(by: disposeBag)
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
```

## skipUntil

- observable을 파라미터로 받고, 해당 observable이 next 이벤트를 전달하기 전까지 원본 observable이 전달하는 이벤트를 무시
- 파라미터로 전달하는 observable을 trigger라고 부르기도 함

```swift
let disposeBag = DisposeBag()

let subject = PublishSubject<Int>()
let trigger = PublishSubject<Int>()

subject.skipUntil(trigger)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

subject.onNext(1) // 전달되지 않음, 아직 trigger가 방출한 요소가 없기 때문

trigger.onNext(0) // 이 이후로 방출되는 요소만 전달됨

subject.onNext(2)
// next(2)
```