# iOS进阶篇--第2️⃣天~遇到tableView卡顿嘛？会造成卡顿的原因大致有哪些？TableView的性能优化

## 总结如下：
## tableView卡顿解决方案：

1.最常用的就是cell的重用， 注册重用标识符（每当需要显示cell 的时候，都会先去缓冲池中寻找可循环利用的cell，如果没有再重新创建cell）<br>

2.减少cell中控件的数量（view是很大的对象，创建它会消耗较多资源，并且也影响渲染的性能。所以不用的不要加上去，不适用的可以先隐藏 ）<br>

3.少使用addView 给cell动态添加view.<br>

4.使用不透明视图（半透明情况下app需要消耗性能去渲染，不透明的视图可以极大地提高渲染的速度）。<br>

5.使用局部更新(如果只是更新某组的话，使用reloadSection进行局部更新)<br>

6.加载网络数据，下载图片，使用异步加载，并缓存.<br>

7.不要实现无用的代理方法，tableView只遵守两个协议.<br>

8.使用正确的数据结构来存储数据。<br>

9.当处理一些全屏大图一类的耗资源的操作，可以用预渲染图像，在bitmap context里先将其画一遍，导出成UIImage对象，然后再绘制到屏幕。<br>

-----

## 下方详解：
* 注：【博客详解点击下方链接⤵️】  
[遇到tableView卡顿嘛？会造成卡顿的原因大致有哪些？TableView的性能优化：http://blog.csdn.net/liyunxiangrxm/article/details/75174401](http://blog.csdn.net/liyunxiangrxm/article/details/75174401)


------

遇到tableView卡顿嘛？会造成卡顿的原因大致有哪些？<br>

可能造成tableView卡顿的原因有： <br>
1.最常用的就是cell的重用， 注册重用标识符<br>

如果不重用cell时，每当一个cell显示到屏幕上时，就会重新创建一个新的cell；<br>
如果有很多数据的时候，就会堆积很多cell。<br>
如果重用cell，为cell创建一个ID，每当需要显示cell 的时候，都会先去缓冲池中寻找可循环利用的cell，如果没有再重新创建cell<br>

2.避免cell的重新布局<br>

cell的布局填充等操作 比较耗时，一般创建时就布局好<br>
如可以将cell单独放到一个自定义类，初始化时就布局好<br>
3.提前计算并缓存cell的属性及内容<br>

当我们创建cell的数据源方法时，编译器并不是先创建cell 再定cell的高度而是先根据内容一次确定每一个cell的高度，高度确定后，再创建要显示的cell，滚动时，每当cell进入凭虚都会计算高度，提前估算高度告诉编译器，编译器知道高度后，紧接着就会创建cell，这时再调用高度的具体计算方法，这样可以方式浪费时间去计算显示以外的cell<br>
4.减少cell中控件的数量<br>

尽量使cell得布局大致相同，不同风格的cell可以使用不用的重用标识符，初始化时添加控件，
不适用的可以先隐藏
5.不要使用ClearColor，无背景色，透明度也不要设置为0<br>

渲染耗时比较长
6.使用局部更新<br>

如果只是更新某组的话，使用reloadSection进行局部更新<br>
7.加载网络数据，下载图片，使用异步加载，并缓存<br>

8.少使用addView 给cell动态添加view<br>

9.按需加载cell，cell滚动很快时，只加载范围内的cell<br>

10.不要实现无用的代理方法，tableView只遵守两个协议<br>

11.缓存行高：estimatedHeightForRow不能和HeightForRow里面的layoutIfNeed同时存在，这两者同时存在才会出现“窜动”的bug。所以我的建议是：只要是固定行高就写预估行高来减少行高调用次数提升性能。如果是动态行高就不要写预估方法了，用一个行高的缓存字典来减少代码的调用次数即可<br>

12.不要做多余的绘制工作。<br>

在实现drawRect:的时候，它的rect参数就是需要绘制的区域，这个区域之外的不需要进行绘制。
例如上例中，就可以用CGRectIntersectsRect、CGRectIntersection或CGRectContainsRect判断是否需要绘制image和text，然后再调用绘制方法。<br>

13.预渲染图像。<br>

当新的图像出现时，仍然会有短暂的停顿现象。解决的办法就是在bitmap context里先将其画一遍，导出成UIImage对象，然后再绘制到屏幕；<br>

14.使用正确的数据结构来存储数据。<br>

