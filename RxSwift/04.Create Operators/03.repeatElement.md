# repeatElement

- 동일한 요소를 반복적으로 방출하는 observable
- 무한으로 반복

```swift
let disposeBag = DisposeBag()
let element = "❤️"

Observable.repeatElement(element)
    .take(7) // 방출되는 요소의 수를 제한하는 것이 중요
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(❤️)
// next(❤️)
// next(❤️)
// next(❤️)
// next(❤️)
// next(❤️)
// next(❤️)
// completed
```