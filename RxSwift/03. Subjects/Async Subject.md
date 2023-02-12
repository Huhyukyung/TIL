# Async Subject

- Subscribe와 동시에 이벤트를 전달하지 않음
- onCompleted 전달되기 전까지 이벤트를 전달하지 않음, 그 시점의 최근 next event 하나를 전달
- Event가 없을 경우는 Completed만 실행하고 종료

```swift
let bag = DisposeBag()

enum MyError: Error {
   case error
}

let subject = AsyncSubject<Int>()

subject
    .subscribe { print($0) }
    .disposed(by: bag)

subject.onNext(1)
// 이벤트가 구독자에게 전달되지 않음

subject.onNext(2)
subject.onNext(3)

subject.onCompleted()
// next(3)           // complete가 전달된 시점 기준 가장 최근의 next 이벤트 하나만을 전달
// completed

subject.onError(MyError.error)
// error(error)      // next 이벤트가 전달되지 않고 error 이벤트만 전달되고 종료
```