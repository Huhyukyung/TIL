# distinctUntilChanged Operator

- 동일한 항목이 연속적으로 방출되지 않도록 필터링

```swift
let disposeBag = DisposeBag()
let numbers = [1, 1, 3, 2, 2, 3, 1, 5, 5, 7, 7, 7]

Observable.from(numbers)
    .distinctUntilChanged()
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// next(3)
// next(2)
// next(3)
// next(1) - 앞서 1이 방출된 적 있다 하더라도 1이 연속으로 나온 것이 아니라면 상관없음
// next(5)
// next(7)
// completed
```

```swift
struct Person {
    let name: String
    let age: Int
}

let disposeBag = DisposeBag()
let numbers = [1, 1, 3, 2, 2, 3, 1, 5, 5, 7, 7, 7]

// comparer
Observable.from(numbers)
    .distinctUntilChanged { !$0.isMultiple(of: 2) && !$1.isMultiple(of: 2) } // 연속적으로 홀수인 경우 같은 값으로 분류
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(1)
// next(2)
// next(2)
// next(3)
//completed

let tuples = [(1, "하나"), (1, "일"), (1, "one")]
let persons = [
    Person(name: "Sam", age: 12),
    Person(name: "Paul", age: 12),
    Person(name: "Tim", age: 56)
]

// keySelector - 특정값을 기준으로 비교
Observable.from(tuples)
    .distinctUntilChanged { $0.0 }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next((1, "하나"))
// completed

Observable.from(tuples)
    .distinctUntilChanged { $0.1 }
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next((1, "하나"))
// next((1, "일"))
// next((1, "one"))
// completed

// KeyPath
Observable.from(persons)
    .distinctUntilChanged(at: \.age)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next(Person(name: "Sam", age: 12))
// next(Person(name: "Tim", age: 56))
// completed
```