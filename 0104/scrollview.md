# UIScrollView 滚动视图

### 不能滚动的原因
* contentSize小于或等于scrollView的的尺寸
* scrollView.scrollEnable = NO;//仅让scrollView不能滚动
* scrollView.userInteractionEnabled = NO;//不响应用户所有操作
