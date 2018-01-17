# iOS进阶篇--第2️⃣天~What are KVO and KVC


### KVC
答案：kvc:键 - 值编码是一种`间接访问`对象的属性使用字符串来标识属性，而不是通过调用存取方法，直接或通过实例变量访问的机制。
很多情况下可以简化程序代码。


####  KVC底层实现：
遍历字典里的所有key(uid)，一个一个获取key，会去模型里查找setKey: setUid:,直接调用这个方法，赋值 setUid:obj <br>
寻找有没有带下划线_key,_uid ,直接拿到属性赋值<br>
寻找有没有key的属性，如果有，直接赋值<br>
如果没有，他还会去valueForUndefineKey看有没有进行处理，如果没有，就会报错<br>

设计valueForUndefinedKey:方法的主要目的是当你使用-(id)valueForKey方法从对象中请求值时，对象能够在错误发生前，有最后的机会响应这个请求。<br>

### KVO
kvo:键值观察机制，他提供了观察某一属性变化的方法，极大的简化了代码。
具体用看到嗯哼用到过的一个地方是对于按钮点击变化状态的的监控。
比如我自定义的一个button
```
[self addObserver:self forKeyPath:@"highlighted" options:0 context:nil];
#pragma mark KVO
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context
{
if ([keyPath isEqualToString:@"highlighted"] ) {
[self setNeedsDisplay];
}
}

```
#### KVO的实现：
KVO的本质就是监听一个对象有没有调用set方法。<br>
监听方法`本质`：并不需要修改方法的实现，仅仅想判断下`有没有调用`。<br>
 
用到了响应式编程思想：响应式编程思想：不需要考虑调用顺序，只需要知道考虑结果，类似于蝴蝶效应，产生一个事件，会影响很多东西，这些事件像流一样的传播出去，然后影响结果，借用面向对象的一句话，万物皆是流。<br>
 
#### KVO底层实现 <br>
*  1.自定义NSKVONotifying_Person子类<br>
*  2.重写setName,在内部恢复父类做法,通知观察者<br>
*  3.如何让外界调用自定义Person类的子类方法,`修改`当前对象的`isa指针`,指向NSKVONotifying_Person<br>


