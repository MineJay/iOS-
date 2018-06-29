# 当APP进入前台、后台时，添加通知


在 viewWillAppear（） 添加如下代码：

	override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(true)
        
        //进入前台
        NotificationCenter.default.addObserver(self, selector: yourSelector, name: NSNotification.Name.UIApplicationWillEnterForeground, object: nil)
        
        //进入后台
        NotificationCenter.default.addObserver(self, selector: yourSelector, name: NSNotification.Name.UIApplicationWillEnterBackground, object: nil)
        
    }


别忘了在 viewDidDisappear（）中移除通知！

	override func viewDidDisappear(_ animated: Bool) {
        super.viewDidDisappear(true)
        NotificationCenter.default.removeObserver(self, name: NSNotification.Name.UIApplicationWillEnterForeground, object: nil)
        
        NotificationCenter.default.removeObserver(self, name: NSNotification.Name.UIApplicationWillEnterBackground, object: nil)
        
    }

然后在 selector 中做相应的事就好了








