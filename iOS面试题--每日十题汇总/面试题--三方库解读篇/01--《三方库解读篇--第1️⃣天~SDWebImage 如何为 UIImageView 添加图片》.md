# SDWebImage 如何为 UIImageView 添加图片


----  


SDWebImage 中为 UIView 提供了一个分类叫做 WebCache, 这个分类中有一个最常用的接口, sd_setImageWithURL:placeholderImage:, 这个分类同时提供了很多类似的方法, 这些方法最终会调用一个同时具有 optionprogressBlock completionBlock 的方法, 而在这个类最终被调用的方法首先会检查是否传入了 placeholderImage 以及对应的参数, 并设置 placeholderImage.

然后会获取 SDWebImageManager 中的单例调用一个 downloadImageWithURL:... 的方法来获取图片, 而这个 manager 获取图片的过程有大体上分为两部分, 它首先会在 SDWebImageCache 中寻找图片是否有对应的缓存, 它会以 url 作为数据的索引先在内存中寻找是否有对应的缓存, 如果缓存未命中就会在磁盘中利用 MD5 处理过的 key 来继续查询对应的数据, 如果找到了, 就会把磁盘中的缓存备份到内存中.

然而, 假设我们在内存和磁盘缓存中都没有命中, 那么 manager 就会调用它持有的一个 SDWebImageDownloader 对象的方法 downloadImageWithURL:... 来下载图片, 这个方法会在执行的过程中调用另一个方法 addProgressCallback:andCompletedBlock:forURL:createCallback: 来存储下载过程中和下载完成的回调, 当回调块是第一次添加的时候, 方法会实例化一个 NSMutableURLRequest 和 SDWebImageDownloaderOperation, 并将后者加入 downloader 持有的下载队列开始图片的异步下载.

而在图片下载完成之后, 就会在主线程设置 image 属性, 完成整个图像的异步下载和配置.

---- 

* 注：【博客详解点击下方链接⤵️】  
[SDWebImage 如何为 UIImageView 添加图片：http://blog.csdn.net/liyunxiangrxm/article/details/52142064](http://blog.csdn.net/liyunxiangrxm/article/details/52142064)
