# Behavior Subject

- 초기에 Buffer를 한개 저장함.
- 가장 최근 next 이벤트를 저장했다가 새로운 구독자에게 전달
- Subscribe와 동시에 초기값을 전달해주기 때문에 Property를 세팅할때 가장 많이 사용.

```swift
let disposeBag = DisposeBag()

enum MyError: Error {
   case error
}

let p = PublishSubject<Int>()
p.subscribe { print("PublishSubject >>", $0) }
    .disposed(by: disposeBag)

let b = BehaviorSubject<Int>(value: 0)
b.subscribe { print("BehaviorSubject >>", $0) }
    .disposed(by: disposeBag)
// BehaviorSubject >> next(0)    // 생성자로 전달한 초기값 0이 바로 전달됨

b.onNext(1)
// BehaviorSubject >> next(1)

b.subscribe { print("BehaviorSubject2 >>", $0) }
    .disposed(by: disposeBag)
// BehaviorSubject2 >> next(1)  // 항상 초기값이 아니라 가장 최신 이벤트를 observer로 전달

b.onCompleted() // b.onError(MyError.error)의 경우도 마찬가지
// BehaviorSubject >> completed
// BehaviorSubject2 >> completed

b.subscribe { print("BehaviorSubject3 >>", $0) }
    .disposed(by: disposeBag)
// BehaviorSubject3 >> completed    // 가지고 있던 최신 이벤트 값이 아닌 completed가 즉시 전달되고 종료됨

```