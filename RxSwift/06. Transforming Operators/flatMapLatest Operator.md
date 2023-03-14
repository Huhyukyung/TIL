# flatMapLatest Operator

- 가장 최근 이너 옵저버블에서만 이벤트를 방출
- 새로운 이너 옵저버블이 생성되면 기존 옵저버블은 중단

```swift
let redCircle = "🔴"
let greenCircle = "🟢"
let blueCircle = "🔵"

let redRectangle = "🟥"
let greenRectangle = "🟩"
let blueRectangle = "🟦"

let sourceObservable = PublishSubject<String>()
let triggger = PublishSubject<Void>()

sourceObservable
    .flatMapLatest { circle -> Observable<String> in
        switch circle {
        case redCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map{ _ in redRectangle }
                .take(until: triggger)
        case greenCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map{ _ in greenRectangle }
                .take(until: triggger)
        case blueCircle:
            return Observable<Int>.interval(.milliseconds(200), scheduler: MainScheduler.instance)
                .map{ _ in blueRectangle }
                .take(until: triggger)
        default:
            return Observable.just("")
        }
    }
    .subscribe { print($0) }
    .disposed(by: disposeBag)

sourceObservable.onNext(redCircle)

DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
    sourceObservable.onNext(greenCircle)
}

DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    sourceObservable.onNext(blueCircle)
}

DispatchQueue.main.asyncAfter(deadline: .now() + 10) {
    triggger.onNext(())
}
// next(🟥)
// next(🟥)
// next(🟥)
// next(🟥)
// next(🟥)
// next(🟩)
// next(🟩)
// next(🟩)
// next(🟩)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
...
```