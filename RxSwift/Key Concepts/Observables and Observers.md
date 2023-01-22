# Observables and Observers

**Observable** → **Observer**

- Observable: 이벤트를 전달
- Observer(Subscriber): Observable을 구독하고 있다가 전달되는 이벤트를 처리
- Observable이 전달하는 3가지 event
    1. Next: 이벤트 발생 (Emission)
    2. Error: 에러 발생 후 해당 Observable 종료(Notification)
    3. Completed: 정상적으로 종료 후 해당 Observable 종료(Notification)
- Observable 생성 방법
    1. create 연산자를 이용한 직접 생성
    2. 다른 연산자를 활용하여 생성
    
    ```swift
    // #1
    Observable<Int>.create { (observer) -> Disposable in
        observer.on(.next(0))
        observer.onNext(1)
        
        observer.onCompleted() // completed 이벤트가 전달되고 종료, 이후에 다른 이벤트를 보낼 수 없음
        
        return Disposables.create()
    }
    
    // #2
    Observable.from([0, 1]) // 배열 안의 값을 순서대로 방출하고 completed 이벤트를 전달
    ```
    
    Observable 생성은 이벤트가 어떤 순서로 전달되어야할지 정의했을 뿐 , 이벤트가 전달되지는 않음
    
    이벤트가 전달되고 처리되는 시점? Observer가 Observable을 구독하는 시점
    
- Observers
    - Observer가 구독을 시작하는 방법은 subscribe 호출
    
    ```swift
    let o1 = Observable<Int>.create { (observer) -> Disposable in
       observer.on(.next(0))
       observer.onNext(1)
       
       observer.onCompleted()
       
       return Disposables.create()
    }
    
    // #1 - 모든 이벤트를 똑같이 처리할 때
    o1.subscribe { // 이 클로저에서 이벤트가 전달되고 전달받은 이벤트를 처리
        print($0)
    
        if let elem = $0.element { // 옵셔널 바인딩
            print(elem)
        }
    }
    // next(0)
    // 0
    // next(1)
    // 1
    // completed
    
    // #2 - 이벤트 별로 별도의 클로저에서 처리하고 싶을 때, 처리하지 않을 이벤트는 생략 가능
    o1.subscribe(onNext: ((Int) -> Void)?,
    						 onError: ((Error) -> Void)?,
    						 onCompleted: (() -> Void)?,
    						 onDisposed: (() -> Void)?)
    
    o1.subscribe(onNext: { elem in
        print(elem) // elem 바로 사용 가능
    })
    // 0
    // 1
    ```
    
    옵저버는 동시에 두 가지 이상의 이벤트를 처리하지는 않음.
    
    하나의 이벤트 처리 이후 다음 이벤트 처리