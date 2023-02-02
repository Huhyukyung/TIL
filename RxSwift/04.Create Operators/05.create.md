# create

- 옵저버블을 동작을 직접 구현

```swift
let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

Observable<String>.create { (observer) -> Disposable in
    guard let url = URL(string: "https://www.apple.com") else {
        observer.onError(MyError.error)
        return Disposables.create() // Disposable.create()가 아님을 주의!
    }
    
    guard let html = try? String(contentsOf: url, encoding: .utf8) else {
        observer.onError(MyError.error)
        return Disposables.create()
    }
    
    observer.onNext(html)
    observer.onCompleted()
    
    return Disposables.create()
}
    .subscribe { print($0) } // create로 생성한 observable을 구독
    .disposed(by: disposeBag)
```

1. 요소를 방출할 때는 onNext(방출할 요소)
2. observable을 종료하기 위해서는 onError, oncompleted를 호출