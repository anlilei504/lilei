---
layout: post
title: 'ReactiveCocoa初探'
subtitle: 'RAC初探'
date: 2017-11-14
categories: iOS
cover: 'https://raw.githubusercontent.com/ReactiveCocoa/ReactiveCocoa/master/Logo/PNG/logo.png'
tags: iOS RAC 
---

## 前言

很多博客都说ReactiveCocoa好用，并且非常强大，于是决定自己学一下，结果果然是不同凡响，只要一用，就肯定会爱上它。下面我就来描述ReactiveCocoa中的一些知识和概念，顺便当做笔记了。

## ReactiveCocoa简介

ReactiveCocoa简称RAC,是由Github开源的一个应用于iOS和OS开发的新框架。在使用中经常会看到前缀名为rac的方法或者是类名，使用起来也非常的好记。

ReactiveCocoa有两个版本，[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)是Swift版本，[ReactiveObjC](https://github.com/ReactiveCocoa/ReactiveObjC)是object-c版本

## RAC常见类

### 1.RACSiganl信号类

信号类用户数据传递

```objectivec
RACSignal *single = [RACSignal createSignal:^RACDisposable * _Nullable(id<RACSubscriber> _Nonnull subscriber) {
    return nil;
}];
```

### 2.RACSubscriber订阅者

```objectivec
RACSignal *single = [RACSignal createSignal:^RACDisposable * _Nullable(id<RACSubscriber> _Nonnull subscriber) {
    [subscriber sendNext:@1];//订阅者发送信号
    [subscriber sendCompleted];//订阅者发送信号完成，如果不在发送信号，调用此方法取消订阅信号
    return nil;
}];
```

### 3.RACDisposable取消订阅信号类

```objectivec
RACSignal *single = [RACSignal createSignal:^RACDisposable * _Nullable(id<RACSubscriber> _Nonnull subscriber) {
    return [RACDisposable disposableWithBlock:^{
        NSLog(@"信号被销毁");
    };
}];
```

### 4.RACSubject和RACReplaySubject



