# UIScrollView 滚动视图
* 继承自UIView

##scrollView属性
contentSize 内容大小
contenOffset 滚动的位置
scrollEnable 是否可滚动 默认YES
bounces  是否有弹性 默认YES
showHorizontalScrollIndicatior 是否显示水平滚动条 默认YES
showVerticalScrollIndicaior 是否显示竖直滚动条 默认YES
alwaysBounceHorizontal 是否水平滚动 默认NO,如果bounces是YES,也可以水平滚动
alwaysBounceVertical 是否竖直滚动 默认NO,如果bounces是YES,也可以竖直滚动

### scrollView获取子控件注意
* 获取subviews是无序且子控件是不一定的(滚动条也是imageView) 
* 设置contentSize后滚动条也会改变大小,所以滚动条会排到后面.

### 不能滚动的原因
* contentSize小于或等于scrollView的的尺寸
* scrollView.scrollEnable = NO;//仅让scrollView不能滚动
* scrollView.userInteractionEnabled = NO;//不响应用户所有操作
