---
layout: post
title: 'iOS关于手势返回造成应用卡死问题'
subtitle: 'iOS手势卡死'
date: 2017-10-25
categories: 技术
cover: 'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1508926620057&di=6aaedd85f9b63b735d0f26bb7ed25da4&imgtype=0&src=http%3A%2F%2Fi0.hdslb.com%2Fbfs%2Farchive%2Fb47ea80748cc87f019b3b39d96b6bfdd2f82e622.jpg'
tags: iOS 问题 卡死
---

## iOS关于手势返回造成应用卡死问题

### 手势返回造成应用卡死

 有时候我们通过手势返回之后，在区点击按钮push到另外一个页面，会出现页面出现一半或者是点击无反应的现象，造成应用卡死，用home键最小化后再次打开，有时候就会看见已经调到了push到了新的页面。

造成这一现象我发现是在UINavigationController的root页面上用手势滑动返回了一次，虽然没有闪退，但是已经造成的UINavigationController的下次push问题。

我的解决方法是在自定义一个UINavigationController去继承默认的UINavigationController，并且重写一下方法
```objectivec
-(void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated{
    if([self respondsToSelector:@selector(interactivePopGestureRecognizer)]&&animated == true){
        if(self.childViewControllers.count<=1){
            self.interactivePopGestureRecognizer.enabled = NO;
        }else{
            self.interactivePopGestureRecognizer.enabled = YES;
        }
    }
    [super pushViewController:viewController animated:animated];
}

-(UIViewController *)popViewControllerAnimated:(BOOL)animated{
    if([self respondsToSelector:@selector(interactivePopGestureRecognizer)]&&animated == true){
        if(self.childViewControllers.count<=1){
            self.interactivePopGestureRecognizer.enabled = NO;
        }else{
            self.interactivePopGestureRecognizer.enabled = YES;
        }
    }
    return [super popViewControllerAnimated:animated];
}

-(NSArray<UIViewController *> *)popToViewController:(UIViewController *)viewController animated:(BOOL)animated{
    if([self respondsToSelector:@selector(interactivePopGestureRecognizer)]&&animated == true){
        if(self.childViewControllers.count<=1){
            self.interactivePopGestureRecognizer.enabled = NO;
        }else{
            self.interactivePopGestureRecognizer.enabled = YES;
        }
    }
    return [super popToViewController:viewController animated:animated];
}

-(void)navigationController:(UINavigationController *)navigationController didShowViewController:(UIViewController *)viewController animated:(BOOL)animated{
    if([self respondsToSelector:@selector(interactivePopGestureRecognizer)]&&animated == true){
        if(self.childViewControllers.count<=1){
            self.interactivePopGestureRecognizer.enabled = NO;
        }else{
            self.interactivePopGestureRecognizer.enabled = YES;
        }
    }
}
```
