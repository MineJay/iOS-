# iOS 系统架构


> 

### iOS 系统可分为四级结构，由上至下分别是可触摸层（Cocoa Touch Layer）、媒体层（Media Layer）、核心服务层（Core Services Layer）、核心系统层（Core OS Layer），每个层级提供不同的服务。低层级结构提供基础服务如文件系统、内存管理、I/O操作等。高层级结构建立在低层级结构之上提供具体服务如UI控件、文件访问等。

![](https://github.com/MineJay/iOS-Note/blob/master/iOS%20%E5%AD%A6%E4%B9%A0%E9%83%A8%E5%88%86/iOS%20%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84/%E7%B3%BB%E7%BB%9F%E5%B1%82%E7%BA%A7.png)


### 一、可触摸层（Cocoa Touch Layer）
可触摸层主要提供用户交互相关的服务，如界面控件、事件管理、通知中心、地图等

![](https://github.com/MineJay/iOS-Note/blob/master/iOS%20%E5%AD%A6%E4%B9%A0%E9%83%A8%E5%88%86/iOS%20%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84/touch.jpg)


### 二、媒体层（Media Layer）
媒体层主要提供图像引擎、音频引擎、视频引擎框架。

![](https://github.com/MineJay/iOS-Note/blob/master/iOS%20%E5%AD%A6%E4%B9%A0%E9%83%A8%E5%88%86/iOS%20%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84/media.jpg)


### 三、核心服务层（Core Services Layer）
核心服务层为程序提供基础的系统服务，例如网络访问、浏览器引擎、定位、文件访问、数据库访问等。

![](https://github.com/MineJay/iOS-Note/blob/master/iOS%20%E5%AD%A6%E4%B9%A0%E9%83%A8%E5%88%86/iOS%20%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84/services.jpg)

### 四、核心系统层（Core OS Layer）
核心系统层为上层结构提供最基础的服务，如系统内核服务、本地认证（指纹等）、安全、加速等

![](https://github.com/MineJay/iOS-Note/blob/master/iOS%20%E5%AD%A6%E4%B9%A0%E9%83%A8%E5%88%86/iOS%20%E7%B3%BB%E7%BB%9F%E6%9E%B6%E6%9E%84/OS.jpg)
