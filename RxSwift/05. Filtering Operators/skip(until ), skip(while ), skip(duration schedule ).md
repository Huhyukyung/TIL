# skip(until:), skip(while:), skip(duration:scheduler:) Operator (RxSwift 6)

## skip(until:)

```swift
let disposeBag = DisposeBag()

let subject = PublishSubject<Int>()
let trigger = PublishSubject<Int>()

subject.skip(until: trigger) // skipUntil(trigger)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

subject.onNext(1) // 전달되지 않음, 아직 trigger가 방출한 요소가 없기 때문

trigger.onNext(0) // 이 이후로 방출되는 요소만 전달됨

subject.onNext(2)
// next(2)
```

## skip(while:)

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .skip(while: { !$0.isMultiple(of: 2) }) // .skipWhile { !$0.isMultiple(of: 2) }
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

## skip(duration:scheduler:)

- 지정한 시간동안 이벤트 방출을 무시

```swift
let disposeBag = DisposeBag()

let o = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)

o.take(10)
    .skip(.seconds(3), scheduler: MainScheduler.instance)
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
// completed

// 어? 3부터 전달되어야 되는데? -> 약간의 시간 오차가 발생해서
```