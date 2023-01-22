# Infallible (RxSwift 6+)

- next와 completed만 방출
- error를 방출하지 않음

```swift
enum MyError: Error {
		case unknown
}

let observable = Observable<String>.create { observer in
		observer.onNext("Hello")
		observer.onNext("Observable")

		observer.onCompleted()
    
    return Disposables.create()
}

let infallible = Observable<String>.create { observer in
		//observer.onNext("Hello")
		observer(.next("Hello")) // error 이벤트를 방출 못 하게 컴파일러 레벨에서 보장
	
		observer(.completed)
    
    return Disposables.create()
}
```