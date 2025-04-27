---
layout: post
title:  "进程间通信实践-pipe和http"
date:   2024-12-02 22:00:00 +0800
categories: tech-cs
---
最近在项目中遇到了一个需要使用Java调用Python的场景：我们的应用是Java编写的，同时项目中需要将图像格式转换成需要的格式，但Java的相关三方库较少，而Python的库支持较好，且易用性交强。于是我们在使用Python跑通了原型之后决定使用Python编写相关转换。具体的交互方式是Java把图像的base64编码传给Python，Python在引用三方库转换后返回给Java进程。


Java和Python的通信其本质就是进程间通信，进程间的通信方式这里就不再列举了，可以直接参考：[Wiki-IPC](https://en.wikipedia.org/wiki/Inter-process_communication).在需求背景下（性能要好、工期较紧），我们主要考虑代码易用性及性能，于是我们将在Pipe和Http之间选择。我们从使用方式和请求耗时两方面进行测试。

| QPS | 耗时(ms):HTTP        | 耗时(ms):pipe  | 
|----------|-----------------|----------------|
|    10      |       55         |       120    |
|    20      |       56         |       150    |

可以看出，HTTP的耗时比pipe的一半还少。
初步可以猜想原因是http方式比pipe方式减少了进程创建的过程，只有本地通信的损耗。