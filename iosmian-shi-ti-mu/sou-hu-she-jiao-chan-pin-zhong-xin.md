### 1.图片圆角处理方法和优化

方法一：通过设置layer属性，最简单的一种，但是很影响性能，一般在正常开发中很少使用。就是用到了imageView.layer的两个属性，分别为cornerRadius = imageView.bounds.size.width/2和 maskToBounds = YES

```objc
UIImageView *imageView = [UIImageView alloc] initWithFrame:CGRectMake(0,0,200,200)];
imageView.image = [UIImage imageNamed:@"tupian"];
imageView.layer.cornerRadius = imageView.bounds.size.width/2;
imageView.layer.maskToBounds = YES;
imageView.center = self.view.cenger;
[self.view addSubview:imageView];
```

方法二：使用UIBezierPath 和 Core Graphics框架画一个圆角

### 2.http和https的区别，https用了什么加密算法？

1. https协议需要到ca申请证书，一般免费证书很少，需要交费。
2. https是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443.
4. http的连接很简单，是无状态的；https协议是有ssl+http协议构建的可进行加密传输、身份认证的网络，比http协议安全。



