# groupBy Operator

- 옵저버블이 방출하는 요소를 원하는 기준으로 그룹핑

```swift
let disposeBag = DisposeBag()
let words = ["Apple", "Banana", "Orange", "Book", "City", "Axe"]

Observable.from(words)
    .groupBy { $0.count }
    .subscribe(onNext: { groupedObservable in
        print("== \(groupedObservable.key)")
        groupedObservable.subscribe { print("  \($0)") }
    })
    .disposed(by: disposeBag)
// == 5
//   next(Apple)
// == 6
//   next(Banana)
//   next(Orange)
// == 4
//   next(Book)
//   next(City)
// == 3
//   next(Axe)
//   completed
//   completed
//   completed
//   completed

Observable.from(words)
    .groupBy { $0.count }
    .flatMap { $0.toArray() }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(["Banana", "Orange"])
// next(["Book", "City"])
// next(["Axe"])
// next(["Apple"])
// completed
```