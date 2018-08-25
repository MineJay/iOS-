# 为什么要用 RxSwift？

我们先看一下 RxSwift 能帮助我们做什么：

### Target Action

传统实现方法：

    button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)

    func buttonTapped() {
        print("button Tapped")
    }
    
通过 Rx 来实现：

    button.rx.tap
    .subscribe(onNext: {
        print("button Tapped")
    })
    .disposed(by: disposeBag)

你不需要使用 Target Action，这样使得代码逻辑清晰。

### 代理

传统实现方法：

    class ViewController: UIViewController {
    ...
        override func viewDidLoad() {
            super.viewDidLoad()
            scrollView.delegate = self
        }
    }

    extension ViewController: UIScrollViewDelegate {
        func scrollViewDidScroll(_ scrollView: UIScrollView) {
            print("contentOffset: \(scrollView.contentOffset)")
        }
    }
    
通过 Rx 来实现：

    lass ViewController: UIViewController {
        ...
        override func viewDidLoad() {
            super.viewDidLoad()
            
            scrollView.rx.contentOffset
                .subscribe(onNext: { contentOffset in
                    print("contentOffset: \(contentOffset)")
                })
                .disposed(by: disposeBag)
        }
    }
    
你不需要书写代理的配置代码，就能获得想要的结果。

### 闭包回调

传统实现方法：

    URLSession.shared.dataTask(with: URLRequest(url: url)) {
        (data, response, error) in
        guard error == nil else {
            print("Data Task Error: \(error!)")
            return
        }
        
        guard let data = data else {
            print("Data Task Error: unknown")
            return
        }
        
        print("Data Task Success with count: \(data.count)")
    }.resume()
    
通过 Rx 来实现：

    URLSession.shared.rx.data(request: URLRequest(url: url))
        .subscribe(onNext: { data in
            print("Data Task Success with count: \(data.count)")
        }, onError: { error in
            print("Data Task Error: \(error)")
        })
        .disposed(by: disposeBag)
        
回调也变得十分简单。

### 通知

传统实现方法：

    var ntfObserver: NSObjectProtocol!

    override func viewDidLoad() {
        super.viewDidLoad()
        
        ntfObserver = NotificationCenter.default.addObserver(
              forName: .UIApplicationWillEnterForeground,
              object: nil, queue: nil) { (notification) in
            print("Application Will Enter Foreground")
        }
    }

    deinit {
        NotificationCenter.default.removeObserver(ntfObserver)
    }
    
通过 Rx 来实现：

    override func viewDidLoad() {
        super.viewDidLoad()
        
        NotificationCenter.default.rx
            .notification(.UIApplicationWillEnterForeground)
            .subscribe(onNext: { (notification) in
                print("Application Will Enter Foreground")
            })
            .disposed(by: disposeBag)
    }
    
你不需要去管理观察者的生命周期，这样你就有更多精力去关注业务逻辑。

### KVO

传统实现方法：

    private var observerContext = 0

    override func viewDidLoad() {
        super.viewDidLoad()
        user.addObserver(self, forKeyPath: #keyPath(User.name), options: [.new, .initial], context: &observerContext)
    }
    
    override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
        if context == &observerContext {
            let newValue = change?[.newKey] as? String
            print("do something with newValue")
        } else {
            super.observeValue(forKeyPath: keyPath, of: object, change: change, context: context)
        }
    }
    
    deinit {
        user.removeObserver(self, forKeyPath: #keyPath(User.name))
    }
    
通过 Rx 来实现：

    override func viewDidLoad() {
    super.viewDidLoad()

    user.rx.observe(String.self, #keyPath(User.name))
        .subscribe(onNext: { newValue in
            print("do something with newValue")
        })
        .disposed(by: disposeBag)
    }
    
这样实现 KVO 的代码更清晰，更简洁并且更准确。

# 那为什么要使用 RxSwift ？

1. 复合：Rx 就是复合的代名词
2. 复用：因为它易复合
3. 清晰：因为声明都是不可变更的
4. 易用：因为他抽象了异步编程，使我们统一了代码风格。
5. 稳定：因为 Rx 是完全通过单环测试的。