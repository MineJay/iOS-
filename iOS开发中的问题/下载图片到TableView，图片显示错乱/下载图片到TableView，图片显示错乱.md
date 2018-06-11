# iOS-Note



# 下载图片到TableView，图片显示错乱

### 问题描述

下载图片到 tableView 中，上拉或者下拉的时候，图片显示错乱。

### 问题分析

在 iOS 开发中，tableView Cell 是有重用机制的，为了节约内存，提升性能，所以在开发中都会用下面这行代码：
	
	tableView.dequeueReusableCell(withIdentifier: String, for: IndexPath)
	
这里用的是 Reusable Cell，就是可重用的 Cell。

如果用了这种方式，当你的手指向上滑动时，被移出屏幕的 Cell 会复用到屏幕最下方，成为将要展示的 Cell，就像一个循环一样。

这就是问题的所在了，在下载图片的时候，因为没有做图片和响应 Cell 的绑定，在上拉或者下拉中，图片就会反复的重新被下载，浪费时间，浪费资源。还有就是如果网速不好，在正在下载图片的时候，就进行上拉或者下拉，导致这个 Cell 的图片还没下载好，可能就被复用了，后果就可想而知了。

### 问题解决

#### 1. 下载好图片以后，进行图片与 Cell 的绑定
例如，在下载图片以后，用一个数组存起来，将其与 Cell 绑定起来，再次请求图片的时候，就可以直接去数组里取。可想而知，这样代码非常繁琐，而且需要很多判断。

#### 2. 这里推荐一种好的方法！第三方库了解一下？感谢喵大！

	Pod 'Kingfisher'
	
	import Kingfisher


接下来只要一行代码就可以了

	cell.yourImageView.kf.setImage(with: URL)


非常简单实用！







