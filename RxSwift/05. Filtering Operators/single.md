# single

- 첫 번째 요소만 방출
- 하나의 요소가 방출되고, 2개이상이 방출될 때는 1개의 요소를 방출하고 error를 방출.
- 반드시 한개의 요소만 방출하는걸 보장해야 될 때 사용.

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.just(1)
    .single()
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// completed

Observable.from(numbers)
    .single()
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// error(Sequence contains more than one element.) - 요소가 두 개 이상 방출되면 error

// 조건에 충족하는 한 개의 요소를 방출
Observable.from(numbers)
    .single { $0 == 3 }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(3)
// completed

let subject = PublishSubject<Int>()

subject.single()
    .subscribe { print($0) }
    .disposed(by: disposeBag)

subject.onNext(100)
// next(100) - 바로 complete 되지는 않음, 원본에서 complete 전달할 때까지 대기
```