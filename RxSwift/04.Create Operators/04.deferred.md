# deferred

- 특정 조건에 따라 observable 생성

```swift
let disposeBag = DisposeBag()
let animals = ["🐶", "🐱", "🐹", "🐰", "🦊", "🐻", "🐯"]
let fruits = ["🍎", "🍐", "🍋", "🍇", "🍈", "🍓", "🍑"]
var flag = true

let factory: Observable<String> = Observable.deferred {
    flag.toggle()
    
    if flag {
        return Observable.from(animals)
    } else {
        return Observable.from(fruits)
    }
}

factory
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(🍎)
// next(🍐)
// next(🍋)
// next(🍇)
// next(🍈)
// next(🍓)
// next(🍑)
// completed

factory
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(🐶)
// next(🐱)
// next(🐹)
// next(🐰)
// next(🦊)
// next(🐻)
// next(🐯)
// completed
```