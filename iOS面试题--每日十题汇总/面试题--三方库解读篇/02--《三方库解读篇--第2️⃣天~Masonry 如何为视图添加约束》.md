# Masonry 如何为视图添加约束


----  


Masonry和其他第三方开源框架一样，都是运用了分类的方式。我们用到的mas_makeConstraints: 方法位于 UIView 的分类 MASAdditions 中.这个方法接受了一个block,这个block中有一个MASConstraintMaker 类型的对象，该对象中有一些我们约束的数组，这里保存着我们所有的加入到视图中的约束。通过该block对这些约束进行配置。当配置结束后，就会调用maker的install方法，而这个install方法会遍历他持有的约束数组，对其中的每一个约束发送install消息，在这里就会使用上一步配置的属性，初始化NALayoutConstraint的子类MASLayoutConstraint并添加到合适的视图上。
视图的选择会通过一个方法mas_closestCommonSuperview:来返回两个视图的最近公共视图。

---- 

* 注：【博客详解点击下方链接⤵️】  
[Masonry 如何为视图添加约束：http://blog.csdn.net/liyunxiangrxm/article/details/52142064#t1](http://blog.csdn.net/liyunxiangrxm/article/details/52142064#t1)
