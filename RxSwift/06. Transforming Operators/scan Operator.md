# scan Operator

- 작업 결과를 누적시키면서 방출
- 최종 결과만 필요하다면 reduce를 사용

```swift
let disposeBag = DisposeBag()

Observable.range(start: 1, count: 10)
    .scan(0, accumulator: +)
    .subscribe{ print($0) }
    .disposed(by: disposeBag)
// next(1)
// next(3)
// next(6)
// next(10)
// next(15)
// next(21)
// next(28)
// next(36)
// next(45)
// next(55)
// completed
```