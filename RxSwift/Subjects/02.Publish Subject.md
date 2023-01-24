# Publish Subject

- Subscribe 이전의 Event는 무시.
- Subscribe 이후의 Event만 구독.

```swift
let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let subject = PublishSubject<String>()

subject.onNext("Hello") // 구독 이전 이벤트는 전달되지 않음

let o1 = subject.subscribe { print(">> 1", $0)}
o1.disposed(by: disposeBag)

subject.onNext("RxSwift")
// >> 1 next(RxSwift)

let o2 = subject.subscribe { print(">> 2", $0)}
o2.disposed(by: disposeBag)

subject.onNext("Subject")
// >> 1 next(Subject)
// >> 2 next(Subject)     // 두 구독자 모두에게 전달됨

subject.onCompleted()
// >> 1 completed
// >> 2 completed     // 두 구독자 모두 completed

let o3 = subject.subscribe { print(">> 3", $0)}
o3.disposed(by: disposeBag)
// >> 3 completed     // 바로 completed 이벤트 전달
```

```swift
subject.onError(MyError.error)

let o3 = subject.subscribe { print(">> 3", $0)}
o3.disposed(by: disposeBag)
// >> 1 error(error)
// >> 2 error(error)
// >> 3 error(error)     // 새로운 구독자에게도 error 바로 전달
```

구독 전에 발생한 event는 사라짐, 사라지는 게 문제가 된다면? Replay Subjects