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



### 3.判断单向链表中是否有环？



