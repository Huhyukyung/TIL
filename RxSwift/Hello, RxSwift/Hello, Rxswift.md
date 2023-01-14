# Hello, Rxswift

- Rx - ReactiveX
    - RxSwift - ReacitveX를 Swift로 구현한 것
- why use rx?
    
    ```swift
    Observable.combineLatest(firstName.rx.text, lastName.rx.text) { $0 + " " + $1}
    		.map { "Greetings, \($0)" }
    		.bind(to: greetingLabel.rx.text)
    ```
    
    ```swift
    viewModel
        .rows
        .bind(to: resultsTableView.rx.items(cellIdentifier: "WikipediaSearchCell", cellType: WikipediaSearchCell.self)) { (_, viewModel, cell) in
            cell.title = viewModel.title
            cell.url = viewModel.url
        }
        .disposed(by: disposeBag)
    ```
    
    단순하고 직관적인 코드를 작성할 수 있다!