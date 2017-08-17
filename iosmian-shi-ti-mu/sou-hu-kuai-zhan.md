### 1.倒计时如何实现？

用NSTimer来实现，但是NSTimer会保存其目标的对象，等到自身“失效”的时候在释放此对象，调用invalidate方法可以可令计时器失效，一次性的计时器执行完相关任务之后也会自动变成失效，开发者若将计时器设置成重复执行模式，那么必须自己手动调用invalidate方法，令NSTimer失效。这样看来当开发者将计时器设置成重复执行的模式的时候，容易造成循环引用，如果当前类有一个NSTimer的属性，或者成员变量。那么self对NSTimer是强引用，当调用NSTimer的schedule方法将NSTimer添加到运行循环里面的时候

这个可以用block来解决，定义一个NSTimer的分类

```objc
#import <Foundation/Foundation.h>
@interface NSTimer (GLBlockSupport)
+ (NSTimer *) gvl_scheduledTimerWithTimeInterval:(NSTimerInterval)interval block:(void(^)())block repeats:(BOOL)repeats;
@end

@implementation NSTimer (GLBlockSupport)
+ (NSTimer *) gvl_scheduledTimerWithTimeInterval:(NSTimerInterval)interval block:(void(^)())block repeats:(BOOL)repeats{
    //这个函数里面调用NSTimer的sheduled方法
    return [self scheduledTimeWithTimeInterval:interval target:self selector:@selector(gvl_blockInvoke:) 
                                                                    userInfo:[block copy]
                                                                    repeats:repeats];
}

+ (void)gvl_blockInvoke:(NSTimer *)timer {
    //???
    void (^block)() = timer.userInfo;
    if(block){
        block();
    }
}
@end
```

这段代码将计时器所执行的任务封装成了block，在调用计时器函数的时候，把它作为userInfo参数穿进去。**该参数可用来存放“不透明值”,只要计时器还有效，就会一直保留它**，传入的参数要通过将block拷贝到堆上面，否则等到要执行它的时候，该块已经无效了。

这里计时器的target设置成了NSTimer类对象，这是个单例，因此计时器是否会保留它，都无所谓了，因为此类对象无需回收

### 2.UIButton的继承关系？

从上到下继承关系依次是NSObject --&gt; UIResponder --&gt; UIView --&gt; UIControl --&gt; UIButton

### 3.iOS中可以进行输入的控件有哪些？

iOS中可以进行输入的控件有UITextField和UITextView

* 它们的区别是：

1.  UITextView支持多行输入，并且可以滚动显示浏览全文，而UITextField只能单行输入
2. UITextView之所以能滚动因为它的父类是UIScrollView，而UITextField的父类是UIControl
3. UITextView没有placeholder属性，而UITextField有placeholder

在iOS中常常要需要限制用户输入字数的要求，我们可以如下处理

对于UITextView

```objc
- (void)textViewDidChange:(UItextView *)textView
//检测到输入变化的时候

- (BOOL)textView:(UITextView *)textView shouldChangeTextInRange:(NSRange) range 
                                        replacementText:(NSString *)text
  //超过一定字数返回NO即可   
```

对于UITextField来讲

```objc
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange) range
                                           replacementString:(NSString *)string 
```

                                                  

### 4.快排的实现原理？

分为两个步骤：

1. 调用partition\(int arr\[\], int low, int high\)函数将数组元素分成两部分，一部分比某个值大，一部分比某个值小
2. 递归处理左边部分和右边部分

下面是实现partition\(int arr\[\], int low, int high\)函数

```c
int partition(int arr[], int low, int high){
    //拿到中位数
    int pivot = arr[high];
    //设置起始点一般就是low-1
    int i = low - 1;
    //循环从low到j-1就好了，因为最后一个数你设置成了pivot
    for(int j = low; j < high; j++){
        if(arr[j] <= pivot){
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    //最后将pivot和i+1位置的数交换
    swap(&arr[i+1], &arr[high]);
    
    //返回分割点的索引
    return i+1;
} 
```

在quickSort里面递归的处理左边部分和右边部分（partition函数返回值划分的左右）

```c
void quickSort(int arr[], int low, int high){
    if(low < high){
        int pi = partition(arr, low ,high)
        //经过partition之后pi位置上的元素已经不需要在动了，所以左边是从low到pi-1,右边是pi+1到high
        quickSort(arr,low, pi - 1);
        quickSort(arr,pi+1, high);
    }
}
```

### 5.短信验证会有倒计时功能吗？第一次验证失败还可以在进行验证吗？

ios短信验证码倒计时按钮实现

实现思路:

* 创建按钮, 添加点击方法;
* 用NSTimer定时器, 每秒执行一次, 定时改变Button的title,改变Button的样式, 设置Button不可点击;
* 若倒计时结束, 定时器关闭, 并改变Button的样式, 可以点击;

```objc
// 开启倒计时效果
-(void)openCountdown{

    __block NSInteger time = 59; //倒计时时间

    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    dispatch_source_t _timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, queue);

    dispatch_source_set_timer(_timer,dispatch_walltime(NULL, 0),1.0*NSEC_PER_SEC, 0); //每秒执行

    dispatch_source_set_event_handler(_timer, ^{

        if(time <= 0){ //倒计时结束，关闭

            dispatch_source_cancel(_timer);
            dispatch_async(dispatch_get_main_queue(), ^{

                //设置按钮的样式
                [self.authCodeBtn setTitle:@"重新发送" forState:UIControlStateNormal];
                [self.authCodeBtn setTitleColor:[UIColor colorFromHexCode:@"FB8557"] 
                                  forState:UIControlStateNormal];
                self.authCodeBtn.userInteractionEnabled = YES;
            });

        }else{

            int seconds = time % 60;
            dispatch_async(dispatch_get_main_queue(), ^{

                //设置按钮显示读秒效果
                [self.authCodeBtn setTitle:[NSString stringWithFormat:@"重新发送(%.2d)", seconds] 
                                  forState:UIControlStateNormal];
                [self.authCodeBtn setTitleColor:[UIColor colorFromHexCode:@"979797"] 
                                  forState:UIControlStateNormal];
                self.authCodeBtn.userInteractionEnabled = NO;
            });
            time--;
        }
    });
    dispatch_resume(_timer);
}
```

注意点：

> 我们在创建Button时, 要设置Button的样式:当type为UIButtonTypeCustom时 , 是读秒的效果；当type为其他时, 是一闪一闪的效果.



