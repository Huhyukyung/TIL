# toArray Operator

- Observable이 방출하는 요소를 하나의 배열로 만들어 방출
- Single
    - 하나의 요소를 방출하거나 error

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

let subject = PublishSubject<Int>()

subject
    .toArray()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2) // 종료되기 전까진 전달되지 않음
subject.onCompleted()
// success([1, 2])
```