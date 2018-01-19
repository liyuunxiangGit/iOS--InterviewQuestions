# iOS进阶篇--第2️⃣天~iOS 代理为啥要用weak修饰?




在开发中我们经常使用代理，或自己写个代理，而代理属性都用weak(assign)修饰，看过有些开发者用strong(retain)，但并没发现有何不妥，也不清楚weak(assign)与strong(retain)修饰有何区别 <br>

功能实现就行了，考虑这么多干嘛~~~我只能哈哈哈

* `weak`:`指明该对象并不负责保持delegate这个对象，delegate这个对象的销毁由外部控制`
```
@property (nonatomic, weak) id<HSDogDelegate>delegate;
```
* `strong`：`该对象强引用delegate，外界不能销毁delegate对象，会导致循环引用(Retain Cycles)`
```
@property (nonatomic, strong) id<HSDogDelegate>delegate;
```
可能你还不太理解，没关系，下面先举例，看结果，再分析！

### 举例
* HSDog类
`HSDog.h:`
```
@protocol HSDogDelegate <NSObject>
@end

@interface HSDog : NSObject

@property (nonatomic, weak) id<HSDogDelegate>delegate;

@end
```
`HSDog.m:`
```
#import "HSDog.h"

@implementation HSDog

- (void)dealloc
{
NSLog(@"HSDog----销毁");
}

@end
```
* HSPerson类
`HSPerson.h:`
```
@interface HSPerson : NSObject

@end
```
`HSPerson.m:`
```
#import "HSPerson.h"
#import "HSDog.h"

@interface HSPerson()<HSDogDelegate>
/** 强引用dog*/
@property (nonatomic, strong) HSDog *dog;
@end

@implementation HSPerson

- (instancetype)init
{
self = [super init];
if (self) {
// 实例化dog
self.dog = [[HSDog alloc] init];
// dog的delegate引用self,self的retainCount，取决于delegate修饰，weak：retainCount不变，strong：retainCount + 1
self.dog.delegate = self;

}
return self;
}

- (void)dealloc
{
NSLog(@"HSPerson----销毁");
}

@end
```
* 在ViewController实现：
```
#import "ViewController.h"
#import "HSPerson.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
[super viewDidLoad];
// 实例化person, self对person弱引用，person的retainCount不变
HSPerson *person = [[HSPerson alloc] init];

}
@end
```
`结果`：

* weak修饰代理
```
@property (nonatomic, weak) id<HSDogDelegate>delegate;
```
`运行->打印`：
```
HSPerson----销毁
HSDog----销毁
```
* strong修饰代理
```
@property (nonatomic, strong) id<HSDogDelegate>delegate;
```
`运行->打印`：

`....并未打印，说明HSPerson、HSDog对象没调用dealloc方法，两个对象未销毁
这也是我们经常说的内存泄露，该释放的内存并未释放！`

`分析`：

* 使用strong

![](https://github.com/liyuunxiangGit/iOS--InterviewQuestions/blob/master/imageFile/使用strong描述代理.png)
`person对dog强引用`
```
@property (nonatomic, strong) HSDog *dog; person
```
`self.dog.delegate又对person强引用，使person的retainCount + 1`
```
@property (nonatomic, strong) id<HSDogDelegate>delegate;
```
`当viewController不对person引用后，dog.delegate对person还强引用着，person的retainCount为1，所以person不会释放，dog固然也不会释放，这就是造成循环引用的导致内存泄露的原因！`

* 使用weak

![](https://github.com/liyuunxiangGit/iOS--InterviewQuestions/blob/master/imageFile/使用weak描述代理.png)
`person对dog强引用`
```
@property (nonatomic, strong) HSDog *dog; person
```
`self.dog.delegate只对person弱引用，并未使person的retainCount + 1`
```
@property (nonatomic, weak) id<HSDogDelegate>delegate;
```
`所以当viewController不对person引用后，person的retainCount为0，即person会被释放，那么dog也被释放`

