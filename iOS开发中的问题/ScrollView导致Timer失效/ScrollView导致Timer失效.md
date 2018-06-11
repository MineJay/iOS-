# iOS-Note



# ScrollView导致Timer失效

### 问题描述
当ScrollView 和 Timer 一起用，ScrollView 滑动时，会导致 Timer 失效

### 问题分析
RunLoop的原因，Timer 的 RunLoopMode 是 defaultMode，在滑动 ScrollView时，主线程的 RunLoop 会切换到 TrackingMode，就会导致 Timer 失效。

### 问题解决

将 Timer 的 RunLoopMode 改为 RunLoopCommonMode 






