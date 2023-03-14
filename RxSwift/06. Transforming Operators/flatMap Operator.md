# flatMap Operator

- 원하는 작업을 실행하고 그 결과를 새로운 observable을 통해 방출
- 최종적으로는 모든 inner observable의 결과를 하나로 합친 result observable을 리턴

![Untitled](flatMap%20Operator%20f99df7f8bde74c4393c0dcaa09a04474/Untitled.png)

```swift
let redCircle = "🔴"
let greenCircle = "🟢"
let blueCircle = "🔵"

let redRectangle = "🟥"
let greenRectangle = "🟩"
let blueRectangle = "🟦"

Observable.from([redCircle, greenCircle, blueCircle])
    .flatMap { circle -> Observable<String> in
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
// next(🟩)
// next(🟥)
// next(🟩)
// next(🟦)
// next(🟥)
// next(🟩)
// next(🟦)
// next(🟥)
// next(🟩)
// next(🟦)
// next(🟩)
// next(🟦)
// next(🟦)
// completed
```

- result observable로 지연없이 방출하여 순서가 섞여있음 → Interleaving
- cf. concatMap은 interleaving을 허용하지 않으므로 순서를 지킴