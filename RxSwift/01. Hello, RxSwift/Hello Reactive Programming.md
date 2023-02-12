# Hello Reactive Programming

- 명령형 코드(Swift) vs 반응형 코드(RxSwift)
    
    ```swift
    var a = 1
    var b = 2
    a + b // 3
    
    a = 12 // a의 값이 변경되어도 이 전의 결과는 변하지 않음
    			 // a, b의 값이 변할 때마다 결과값도 변하게 만들고 싶다면? => 반응형 프로그래밍
    ```
    
    ```swift
    let a = BehaviorSubject(value: 1)
    let b = BehaviorSubject(value: 2)
    
    Observable.combineLatest(a, b) { $0 + $1 }
        .subscribe(onNext: {print($0)})
        .disposed(by: disposeBag)
    // 3
    a.onNext(12)
    // 14 - 새로 계산된 결과가 출력됨
    ```