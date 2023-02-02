# just, of, from

## just

- 하나의 항목을 방출하는 observable을 생성

```swift
let disposeBag = DisposeBag()
let element = "😀"

Observable.just(element)
    .subscribe { event in print(event) }
    .disposed(by: disposeBag)
// next(😀)
// completed

// 배열을 전달하면?ㅎ
Observable.just([1, 2, 3])
    .subscribe { event in print(event) }
    .disposed(by: disposeBag)
// next([1, 2, 3])
// completed    // 2개 이상의 요소를 방출하는 observable을 만들고싶다면? =? of 연산자
```

## of

- 2개 이상의 요소를 방출하는 observable 생성

```swift
let disposeBag = DisposeBag()
let apple = "🍏"
let orange = "🍊"
let kiwi = "🥝"

Observable.of(apple, orange, kiwi)
    .subscribe { event in print(event) }
    .disposed(by: disposeBag)
// next(🍏)
// next(🍊)
// next(🥝)
// completed

Observable.of([1, 2], [3, 4], [5, 6])
    .subscribe { event in print(event) }
    .disposed(by: disposeBag)
// next([1, 2])
// next([3, 4])
// next([5, 6])
// completed     // 배열 안의 요소를 하나씩 전달하고 싶다면? => from 연산자
```

## from

- 배열에 저장된 요소를 순서대로 하나씩 방출하는 observable을 생성

```swift
let disposeBag = DisposeBag()
let fruits = ["🍏", "🍎", "🍋", "🍓", "🍇"]

Observable.from(fruits)
    .subscribe { event in print(event) }
    .disposed(by: disposeBag)
// next(🍏)
// next(🍎)
// next(🍋)
// next(🍓)
// next(🍇)
// completed
```