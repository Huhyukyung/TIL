# buffer Operator

- 특정 주기동안 요소를 수집하고 하나의 배열로 방출
- Controlled Buffering
- count는 최대숫자

```swift
let disposeBag = DisposeBag()

Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .buffer(timeSpan: .seconds(5), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next([0, 1, 2])
// next([3, 4, 5])
// next([6, 7, 8])
// next([9, 10, 11])
// next([12, 13, 14])
// completed

Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .buffer(timeSpan: .seconds(2), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe { print($0) }
    .disposed(by: disposeBag)
// next([0]) - count가 3이 되지 않았더라도 지정된 주기가 지났으면 그동안 모은 요소 방출
// next([1, 2, 3])
// next([4, 5])
// next([6, 7])
// next([8, 9])
// completed
```