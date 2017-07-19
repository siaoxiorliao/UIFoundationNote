# 资源存放问题(**重点**)
1226-10-6
开发阶段:源代码+资源+类库
测试阶段:打包->ipa
上架: ipa+描述信息->发布到AppStore

## 拖入文件的选项说明(重要)
**1226-10-15**
![](/1226/images/WX20170717-202950.png)
> * 1.copy items if needed 
拷贝一份到项目中,存在于项目的目录
> * 2.Create groups 
创建组,虚拟的文件结构,里面的文件可被编译(删除的时候不会删除文件夹);
> * 3.Create folder references
创建引用,有真实的文件结构,里面的文件不会被编译;
> * 4.加入ipa包中(如果是组,不带结构创建进ipa包,反之)

![](/1226/images/WX20170717-200057.png)

![](/1226/images/WX20170717-200218.png)


## 拖入文件的方式说明(重要)

1. 放入assets的
> 可以通过imageName:@"1"访问
> 不可以通过imageWithContentsOfFile(路径)访问,因为在ipa包中是在assets.car中的(取不到路径,而imageName是有xcode自己的方式)

2. copy,通过组创建,加入ipa包中
> 两种都可以拿到

3. 不copy,通过组创建,加入ipa包中(copy问题)
> 两种都可以拿到
> 改变文件夹名字,编译报错

4. copy,通过组创建,不加入ipa包中(ipa包问题)
> 两种都拿不到

5. copy,通过folder创建,加入ipa包中(访问方式问题)
> 可以通过imageName访问,但要加入路劲(/测试/测试/这里.png)
> 不可以通过imageWithContentsOfFile访问

## 加载图片的方式:
   1. imageNamed:
   2. imageWithContentsOfFile:
   
   
   1. 加载Assets.xcassets这里面的图片:
    1> 打包后变成Assets.car
    2> 拿不到路径
    3> 只能通过imageNamed:来加载图片
    4> 不能通过imageWithContentsOfFile:来加载图片
 
   2. 放到项目中的图片:
    1> 可以拿到路径
    2> 能通过imageNamed:来加载图片
    3> 也能通过imageWithContentsOfFile:来加载图片
    

## 加载文件的两种方式选择

![](/1226/images/WX20170717-200218.png)


* 方式1.imaeNamed: 既可以访问assets中的,也可以访问ipa包中的
* 方式2.只能访问ipa;
![](/1226/images/WX20170717-201301.png)

>    图片的两种加载方式:
    1> imageNamed:
      a. 就算指向它的指针被销毁,该资源也不会被从内存中干掉
      b. 放到Assets.xcassets的图片,默认就有缓存
      c. 图片经常被使用
 
>    2> imageWithContentsOfFile:
      a. 指向它的指针被销毁,该资源会被从内存中干掉
      b. 放到项目中的图片就不会有缓存
      c. 不经常用,大批量的图片
      
**下面主要是说一下他们的区别(重要):**

* imageNamed: 

用这个方法加载图片分为两种情况:

> 系统缓存有这个图片: 直接从缓存中取得(放入assets中运行一开始默认就有缓存)

> 系统缓存没有这个图片 :
通过传入的文件名对整个工程进行遍历 (在application bundle的顶层文件夹寻找名字的图象 ), 如果如果找到对应的图片 , iOS 系统首先要做的是将这个图片放到系统缓存中去,以备下次使用的时候直接从系统缓存中取 , 接下来重复第一步,即直接从缓存中取

> 那么试想一下 , 如果要加载的这个图片的文件量很多,文件大小很大,内存不足,内存泄露,甚至是程序的崩溃都是很容易发生的事.

* imageWithContentsOfFile:

>用这个方法只有一种情况,那就是仅仅加载图片 , 图像数据不会被缓存 . 因此在加载较大图片的时候 , 以及图片使用情况很少的时候可以使用这两个方法 , 降低内存消耗.

