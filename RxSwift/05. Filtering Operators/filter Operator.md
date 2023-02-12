# filter Operator

- filter 연산자를 활용해서 특정 조건에 맞는 항목을 필터링

```swift
let disposeBag = DisposeBag()
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

Observable.from(numbers)
    .filter{ $0.isMultiple(of: 2) } // 짝수만 필터, 조건에서 true인 요소들만 방출
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(2)
// next(4)
// next(6)
// next(8)
// next(10)
// completed
```