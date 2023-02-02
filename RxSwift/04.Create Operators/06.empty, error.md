# empty, error

- next 이벤트를 전달하지 않음

## empty

- 요소를 방출하지 않고 completed 이벤트를 전달하는 observable을 생성
- observer가 아무런 동작없이 종료해야할 때 주로 사용

```swift
let disposeBag = DisposeBag()

Observable<Void>.empty()
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// completed
```

## error

- 요소를 방출하지 않고 error 이벤트를 전달하고 종료하는 observable을 생성
- error를 처리할 때 활용

```swift
let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

Observable<Void>.error(MyError.error)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// error(error)
```