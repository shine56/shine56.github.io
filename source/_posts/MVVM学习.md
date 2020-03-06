---
title: MVVM学习
date: 2020-02-22 17:37:34
edited:
tags: Android学习
categories: Android
---

用MVVM重新写了一遍CareFree之后所感：MVVM好像和MVP区别不大，就是Presenter变成了ViewModel。vm层持有view层和model层对象，通过接口回调进行交互。
感觉最大区别在于vm绑定了视图和数据，试图所有变化同步反应在vm，vm中数据的改变也会引起试图的改变。