# compactMap Operator

- nil을 필터링하고 값을 언래핑해서 방출

```swift
let disposeBag = DisposeBag()

let subject = PublishSubject<String?>()

subject
    .filter { $0 != nil }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {
    Observable<Int>.interval(.milliseconds(300), scheduler: MainScheduler.instance)
        .take(10)
        .map { _ in Bool.random() ? "⭐️" : nil } // nil이나 별을 랜덤으로 방출
        .subscribe(onNext: { subject.onNext($0) })
        .disposed(by: disposeBag)
}
// next(Optional("⭐️"))
// next(Optional("⭐️"))
// next(Optional("⭐️"))
// next(Optional("⭐️"))
// next(Optional("⭐️"))
// nil은 필터링되지만 값을 사용하려면 언래핑해야함
```

```swift
subject
    .compactMap { $0 }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

DispatchQueue.main.asyncAfter(deadline: .now() + 0.5) {
    Observable<Int>.interval(.milliseconds(300), scheduler: MainScheduler.instance)
        .take(10)
        .map { _ in Bool.random() ? "⭐️" : nil }
        .subscribe(onNext: { subject.onNext($0) })
        .disposed(by: disposeBag)
}
// next(⭐️)
// next(⭐️)
// next(⭐️)
// next(⭐️)
```