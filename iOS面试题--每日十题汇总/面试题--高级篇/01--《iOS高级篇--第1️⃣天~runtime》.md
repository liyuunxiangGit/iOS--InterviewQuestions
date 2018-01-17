# 什么是runtime?

----

runtime是运行时机制，我们写的OC语言，最终被转换成runtime的语句。<br>
#### runtime中是怎么找到对应的方法的？
他的本质都是让对象（实例对象，类对象）发送消息，每一个类都有方法列表methodList,每一个方法在方法列表中都有对应的方法编号，所以首先根据对象的方法编号通过方法映射表去找到对应的方法Method（方法名）.然后根据方法名找到函数实现。

#### 方法的交换

有没有动态添加过方法：（有没有使用过performSelector）<br>
使用过，OC中是懒加载，有的方法很久不会调用，所以就用动态添加方法。<br>

#### 给分类添加属性：
原理：给一个类声明属性，其实本质就是给这个类添加关联，并不是直接把这个值的内存空间添加到类存空间。<br>
```
- (NSString *)name
{
// 根据关联的key，获取关联的值。
return objc_getAssociatedObject(self, key);
}

- (void)setName:(NSString *)name
{
// 第一个参数：给哪个对象添加关联
// 第二个参数：关联的key，通过这个key获取
// 第三个参数：关联的value
// 第四个参数:关联的策略
objc_setAssociatedObject(self, key, name, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
```
