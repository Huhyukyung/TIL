# Replay Subject

- 지정된 버퍼크기만큼 이벤트를 저장
    - Behavior Suject는 가장 최신의 이벤트 하나만을 저장, 그 외에는 사라짐
    - 2개 이상의 이벤트를 저장해두고 새로운 구독자에게 전달하고 싶을 때 사용
- Subscribe와 동시에 버퍼에 저장된 이벤트를 방출
- 버퍼는 메모리에 저장되므로 메모리 사용량에 주의, 필요한 만큼만 버퍼 크기 사용

```swift
let rs = ReplaySubject<Int>.create(bufferSize: 3) // 생성자가 아닌 create 메소드 사용
																								  // 3개의 이벤트 저장
(1...10).forEach { rs.onNext($0) }

rs.subscribe { print("Observer 1 >>", $0) }
    .disposed(by: disposeBag)
// Observer 1 >> next(8)
// Observer 1 >> next(9)
// Observer 1 >> next(10)

rs.subscribe { print("Observer 2 >>", $0) }
    .disposed(by: disposeBag)
// Observer 2 >> next(8)
// Observer 2 >> next(9)
// Observer 2 >> next(10)

rs.onNext(11)
// Observer 1 >> next(11)
// Observer 2 >> next(11)

rs.subscribe { print("Observer 3 >>", $0) }
    .disposed(by: disposeBag)
// Observer 3 >> next(9)
// Observer 3 >> next(10)
// Observer 3 >> next(11)

rs.onCompleted()
// Observer 1 >> completed
// Observer 2 >> completed
// Observer 3 >> completed

rs.subscribe { print("Observer 4 >>", $0) }
    .disposed(by: disposeBag)
// Observer 4 >> next(9)
// Observer 4 >> next(10)
// Observer 4 >> next(11)
// Observer 4 >> completed
// 버퍼에 저장된 이벤트가 전달된 다음 completed가 전달됨
// 종료 여부에 관계없이 버퍼에 저장되어 있는 이벤트를 새로운 구독자에게 전달, error도 마찬가지
```