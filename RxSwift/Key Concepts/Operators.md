# Operators

- 대부분의 연산자는 Observable 상에서 동작하고 새로운 Observable 리턴
- 새로운 Observable 리턴하기 때문에 연달아 사용 가능

```swift
let bag = DisposeBag()

Observable.from([1, 2, 3, 4, 5, 6, 7, 8, 9])
    .take(5) // 5개만 받기
    .filter { $0.isMultiple(of: 2) } // 짝수만 filtering
    .subscribe { print($0) }
    .disposed(by: bag)
// next(2)
// next(4)
// completed

Observable.from([1, 2, 3, 4, 5, 6, 7, 8, 9]) 
    .filter { $0.isMultiple(of: 2) } // 짝수만 filtering
		.take(5) // 5개만 받기
    .subscribe { print($0) }
    .disposed(by: bag)
// next(2)
// next(4)
// next(6)
// next(8)
// completed
```

연산자 순서에 주의!