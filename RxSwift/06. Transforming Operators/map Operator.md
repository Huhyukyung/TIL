# map Operator

- 실행 결과로 변환해서 방출

```swift
let disposeBag = DisposeBag()
let skills = ["Swift", "SwiftUI", "RxSwift"]

Observable.from(skills)
    .map { "Hello, \($0)" }
    .subscribe{ print($0) }
    .disposed(by: disposeBag)
// next(Hello, Swift)
// next(Hello, SwiftUI)
// next(Hello, RxSwift)
// completed

Observable.from(skills)
    .map { $0.count } // 리턴하는 타입이 같을 필요는 없음
    .subscribe{ print($0) }
    .disposed(by: disposeBag)
// next(5)
// next(7)
// next(7)
// completed
```