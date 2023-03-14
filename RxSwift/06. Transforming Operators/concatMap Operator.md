# concatMap Operator

- 인터리빙없이 방출순서를 보장

```swift
let disposeBag = DisposeBag()

let redCircle = "🔴"
let greenCircle = "🟢"
let blueCircle = "🔵"

let redRectangle = "🟥"
let greenRectangle = "🟩"
let blueRectangle = "🟦"

Observable.from([redCircle, greenCircle, blueCircle])
    .concatMap { circle -> Observable<String> in
        switch circle {
        case redCircle:
            return Observable.repeatElement(redRectangle)
                .take(5)
        case greenCircle:
            return Observable.repeatElement(greenRectangle)
                .take(5)
        case blueCircle:
            return Observable.repeatElement(blueRectangle)
                .take(5)
        default:
            return Observable.just("")
        }
    }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(🟥)
// next(🟥)
// next(🟥)
// next(🟥)
// next(🟥)
// next(🟩)
// next(🟩)
// next(🟩)
// next(🟩)
// next(🟩)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// next(🟦)
// completed
```