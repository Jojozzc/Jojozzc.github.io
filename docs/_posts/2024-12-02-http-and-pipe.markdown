---
layout: post
title:  "进程间通信实践-pipe和http"
date:   2024-12-02 22:00:00 +0800
categories: tech-cs
---
最近在项目中遇到了一个需要使用Java调用Python的场景：我们的应用是Java编写的，同时项目中需要将图片格式转换成需要的格式，但Java的相关三方库较少，而Python的库支持较好，且易用性交强。于是我们在使用Python跑通了原型之后决定使用Python编写相关转换