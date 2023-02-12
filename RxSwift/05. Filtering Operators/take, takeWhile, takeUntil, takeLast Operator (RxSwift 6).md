# take, takeWhile, takeUntil, takeLast Operator (RxSwift 5)

## take

- 해당 숫자만큼만 요소를 방출
- next 이벤트를 제외한 나머지 이벤트(completed, error)에는 영향 X

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .take(5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// completed
```

## takeWhile

- 조건이 true이면 요소를 방출
- 조건이 false를 리턴하면 더 이상 요소를 방출하지 않음

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .takeWhile({ !$0.isMultiple(of: 2) }) // 홀수
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// completed
```

## takeUntil

- observable을 파라미터로 받고 파라미터로 받은 observable에서 next 이벤트를 전달하기 전까지 원본 observable의 next 이벤트를 전달

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let subject = PublishSubject<Int>()
let trigger = PublishSubject<Int>()

subject.takeUntil(trigger)
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
```

## takeLast

- 마지막으로 방출되는 n개의 요소가 전달됨
- 구독자로 전달되는 시점이 delay됨

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let subject = PublishSubject<Int>()

subject.takeLast(2)
    .subscribe { print($0) }
    .disposed(by: disposeBag)

numbers.forEach { subject.onNext($0) } // 최근 2개, 9, 10이 버퍼에 저장되어 있는 상태

subject.onNext(11) // 버퍼에 10, 11

subject.onCompleted() // 이 때 버퍼에 들어있던 n개의 요소가 방출되고 completed
// next(10)
// next(11)
// completed

enum MyError: Error {
    case error
}

subject.onError(MyError.error) // 에러 이벤트가 전달되면 버퍼에 있던 요소들은 전달되지 않고 error만 전달
// error(error) 
```