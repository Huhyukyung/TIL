# debounce, throttle Operator

- 짧은 시간동안 반복적으로 발생하는 이벤트를 제어

## debounce

- next 이벤트를 방출한 다음 지정한 시간동안 next 이벤트를 방출하지 않는다면 가장 마지막 이벤트를 전달
- 지정한 시간 내에 next 이벤트를 방출했다면 타이머를 초기화하고 다시 지정한 시간만큼 대기

```swift
let disposeBag = DisposeBag()

let buttonTap = Observable<String>.create { observer in
    DispatchQueue.global().async {
        for i in 1...10 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.3)
        } // 0.3초마다 방출
        
        Thread.sleep(forTimeInterval: 1) // 1초 쉬고
        
        for i in 11...20 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.5)
        } // 0.5초마다 방출
        
        observer.onCompleted()
    }
    
    return Disposables.create {
        
    }
}

buttonTap
    .debounce(.milliseconds(1000), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(Tap 10)
// next(Tap 20)
// completed
```

## throttle

- 지정된 주기마다 하나씩 Next이벤트를 방출

```swift
let disposeBag = DisposeBag()

let buttonTap = Observable<String>.create { observer in
    DispatchQueue.global().async {
        for i in 1...10 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.3)
        }
        
        Thread.sleep(forTimeInterval: 1)
        
        for i in 11...20 {
            observer.onNext("Tap \(i)")
            Thread.sleep(forTimeInterval: 0.5)
        }
        
        observer.onCompleted()
    }
    
    return Disposables.create {
        
    }
}

buttonTap
    .throttle(.milliseconds(1000), scheduler: MainScheduler.instance)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(Tap 1)
// next(Tap 4)
// next(Tap 7)
// next(Tap 10)
// next(Tap 11)
// next(Tap 12)
// next(Tap 14)
// next(Tap 16)
// next(Tap 18)
// next(Tap 20)
completed
```