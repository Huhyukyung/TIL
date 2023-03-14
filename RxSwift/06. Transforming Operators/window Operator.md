# window Operator

- 원본 옵저버블이 방출하는 항목을 작은 단위의 옵저버블로 리턴. (cf. buffer는 배열로 리턴)

```swift
let disposeBag = DisposeBag()

Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .window(timeSpan: .seconds(2), count: 3, scheduler: MainScheduler.instance)
    .take(5)
    .subscribe{
        print($0)
        
        if let observable = $0.element {
            observable.subscribe { print(" inner: ", $0) }
        }
    }
    .disposed(by: disposeBag)
// next(RxSwift.AddRef<Swift.Int>)
//  inner:  next(0)
//  inner:  completed
// next(RxSwift.AddRef<Swift.Int>)
//  inner:  next(1)
//  inner:  next(2)
//  inner:  next(3)
//  inner:  completed
// next(RxSwift.AddRef<Swift.Int>)
//  inner:  next(4)
//  inner:  next(5)
//  inner:  completed
// next(RxSwift.AddRef<Swift.Int>)
//  inner:  next(6)
//  inner:  next(7)
//  inner:  completed
// next(RxSwift.AddRef<Swift.Int>)
// completed
//  inner:  next(8)
//  inner:  next(9)
//  inner:  completed
```