# Disposables

- 리소스 정리에 사용되는 객체

```swift
let subscribe = Observable.from([1,2,3])
        .subscribe(onNext: { elem in
            print("Next", elem)
        }, onError: { (Error) in
            print(Error)
        }, onCompleted: {
            print("onCompleted")
        }, onDisposed: {
            print("onDisposed") // 모든 리소스가 제거된 후 호출됨
        })
// Next 1
// Next 2
// Next 3
// Completed
// Disposed

Observable.from([1, 2, 3])
    .subscribe {
        print($0)
    }
// next(1)
// next(2)
// next(3)
// completed -> 자동으로 리소스 해지됨. Disposed는? disposed는 observable이 전달하는 이벤트가 아님.
```

```swift
let subscription1 = Observable.from([1, 2, 3])
    .subscribe(onNext: { elem in
        print("Next", elem)
    }, onError: { error in
        print("Error", error)
    }, onCompleted: {
        print("Completed")
    }, onDisposed: {
        print("Disposed")
    })

subscription1.dispose() // 직접 호출보다 disposeBag 사용 권장

var bag = DisposeBag()

Observable.from([1, 2, 3])
    .subscribe {
        print($0)
    }
    .disposed(by: bag) // bag이 해지되는 시점에 bag에 담긴 desposible 해지

bag = DisposeBag() // DisposeBag 리소스 해지하려면 새로 생성 or nil 할당
```

```swift
// dispose를 활용한 실행 취소
let subscription2 = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .subscribe(onNext: { elem in
        print("Next", elem)
    }, onError: { error in
        print("Error", error)
    }, onCompleted: {
        print("Completed")
    }, onDisposed: {
        print("Disposed")
    })

DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
    subscription2.dispose()
}
// dispose 된 후 이벤트가 전달되지 않는 점을 이용하여 실행 취소를 할 수 있지만
// takeUntil과 같은 연산자 활용이 더 권장됨
```