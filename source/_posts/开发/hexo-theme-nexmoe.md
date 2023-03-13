---
title: Hexo 主题 Nexmoe
author: 折影轻梦
abbrlink: hexo-theme-nexmoe
tags:
  - 博客
  - Hexo
  - 主题
date: 2019-08-05 09:00:00
---

一个比较特别的 Hexo 主题

<!--more-->

网易云音乐ID：5027380

## Github
https://github.com/theme-nexmoe/hexo-theme-nexmoe
欢迎 Star，贡献，如遇到问题请留言

![](https://img.shields.io/github/stars/theme-nexmoe/hexo-theme-nexmoe.svg)
![](https://img.shields.io/github/forks/theme-nexmoe/hexo-theme-nexmoe.svg)
![](https://img.shields.io/github/issues/theme-nexmoe/hexo-theme-nexmoe.svg)
![](https://img.shields.io/github/last-commit/theme-nexmoe/hexo-theme-nexmoe.svg?label=commits)
![](https://img.shields.io/github/license/theme-nexmoe/hexo-theme-nexmoe.svg)

## 文档

https://docs.nexmoe.com

## 获取更新
使用 watch Releases only 以获取新版本更新提示

![pasted-5](../../images/hexo-theme-nexmoe/pasted-5.png)

## 开源协议
Apache License 2.0

## 后续更新
就读高中，
star 以及 issue 是我更新的动力
欢迎加群 482634342 划水

## 交流
论坛：https://club.chainwon.com/t/nexmoe

## 特性
- [相片集](#相片集)
- [友情链接](#友情链接)
- [文章封面图](#文章封面图)

## 国际化
目前中文翻译较全，其他语言翻译不完全，有余力的大佬可以来贡献一下
语言文件在 languages 里，参考 `zh-CN.yml` 进行翻译即可

## 相片集
    ![珠江](https://i.loli.net/2018/09/01/5b8a59551a4d8.jpg)|![珠江](https://i.loli.net/2018/09/01/5b8a6ab761262.jpg)|![某收门票公园](https://i.loli.net/2018/09/01/5b8a5994b6e28.jpg)
    - | - | -
    ![某收门票公园](https://i.loli.net/2018/09/01/5b8a5994b6e28.jpg)|![某收门票公园](https://i.loli.net/2018/09/01/5b8a5c8c34439.jpg)|![珠海](https://i.loli.net/2018/09/01/5b8a59d5c50f3.jpg)

原理就是在 markdown 的表格里面插入图片

 ![珠江](../../images/hexo-theme-nexmoe/5b8a59551a4d8.jpg)    | ![珠江](../../images/hexo-theme-nexmoe/5b8a6ab761262.jpg)    | ![某收门票公园](../../images/hexo-theme-nexmoe/5b8a5994b6e28.jpg) 
 ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ 
 ![某收门票公园](../../images/hexo-theme-nexmoe/5b8a5994b6e28.jpg) | ![某收门票公园](../../images/hexo-theme-nexmoe/5b8a5c8c34439.jpg) | ![珠海](../../images/hexo-theme-nexmoe/5b8a59d5c50f3.jpg)    

## 友情链接

新建 page 使用 `layout: py` 

    - [![标题](https://example.com/logo.png)](https://example.com/ "标题")

例如

    - [![轻惋导航](https://www.chainwon.com/static/logo.png)](https://www.chainwon.com/ "轻惋导航")

演示地址：https://nexmoe.com/PY.html

## 文章封面图
在 Front-matter 中为 cover 赋值即可，如 
    
    ---
    title: Hello World
    date: 2013/7/13 20:46:25
    cover: https://i.loli.net/2019/07/21/5d33d5dc1531213134.png
    ---