# iOS-Note



## Day1 - 多个界面跳转和传值(swift)
#### （PS：本人很菜，但如果这些能对你有帮助，我也是很开心的）
#### 现在有这么一个问题，一个界面中有多个按钮，点击按钮跳转到相应的界面。

如果按钮少了，则可以直接拖出几个 View Controller 放在 Storyboard 中，然后按住 CTRL 连接到相应的界面即可，如下图

![1](/Users/mac/Desktop/GitHub/iOS-Note/image/1.png)
![2](/Users/mac/Desktop/GitHub/iOS-Note/image/2.png)

但是按钮非常多呢？10个按钮？15个？更多？如果再按上述方法，拖出10多个新的 View Controller 放在 Storyboard 中，然后挨个连线？这样不仅你的 Storyboard 看起来非常的拥挤凌乱，而且会非常卡！尤其是在项目大了以后，卡的飞起！用大佬的话说：这显然很蠢！（虽然我现在还是个非常菜的菜鸟）

### 我们就以10个按钮跳转为例，解决这个问题


新建一个项目，在 Storyboard 中加入10个按钮，并嵌入 Navigation Controller（我这里为了方便，直接拖拽的（当然你也可以用代码创建）如下图 

![3](/Users/mac/Desktop/GitHub/iOS-Note/image/3.png)

然后将 Storyboard 中的按钮和 ViewController.swift 的代码连接起来（按住 CTRL ，然后你们懂得），这里我只是连接了一部分，如下图

![5](/Users/mac/Desktop/GitHub/iOS-Note/image/5.png)

新建一个 Cocoa Touch Class 文件，取名为 SecondViewController，继承于 UIViewController

![4](/Users/mac/Desktop/GitHub/iOS-Note/image/4.png)

在刚才新建的SecondViewController.swift 的 viewDidLoad() 方法中 加入如下代码

	 override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = UIColor.white
        
        let label = UILabel(frame: CGRect(x: 0, y: 0, width: 300, height: 300))
        //设置label的背景色，字体色
        label.backgroundColor = UIColor.black
        label.textColor = UIColor.white
        label.text = "hello world"
        self.view.addSubview(label)    
    }


然后回到 ViewController.swift 中，在按钮 1 的响应方法中，加入如下代码

	@IBAction func btn1(_ sender: Any) {
        
        let showView = SecondViewController()
        
        self.navigationController?.pushViewController(showView, animated: true)
        
    }


运行程序，并点击按钮 1 ，你应该会看到如下界面

![6](/Users/mac/Desktop/GitHub/iOS-Note/image/6.png)



#### 接下来实现传值


在 SecondViewController.swift 中，加入一个属性，

	var value = ""
	
把 

	label.text = "hello world"
改为

	label.text = value

回到 ViewController.swift ，在按钮 1 的响应方法中，加入如下代码

	showView.value = "hello iOS"
	
运行，点击按钮 1，会看到如下界面

![7](/Users/mac/Desktop/GitHub/iOS-Note/image/7.png)


#### 接下来实现代理反向传值






















