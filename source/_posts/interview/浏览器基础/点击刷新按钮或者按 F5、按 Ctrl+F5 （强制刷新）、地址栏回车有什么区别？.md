---
title: 点击刷新按钮或者按 F5、按 Ctrl+F5 （强制刷新）、地址栏回车有什么区别？
date: 2022-11-22
categories: 
- 面试准备
- 浏览器基础
tag: 缓存
---

## 点击刷新按钮或者按 F5、按 Ctrl+F5 （强制刷新）、地址栏回车有什么区别？

### 点击刷新按钮或按F5
点击刷新按钮或按F5，会使浏览器的缓存过期，并且带上if-Modified-Since和if-None-Match，会使服务端检查请求文件的新鲜度，可能返回304，也可能返回200。

### 按Ctrl+F5（强制刷新）
按Ctrl+F5强制刷新时，浏览器会使缓存文件过期，并且不带上if-Modified-Since和if-None-Match，相当于第一次请求，返回的全都是200。

### 地址栏回车
地址栏回车是走的正常流程，本地检查是否过期，然后服务器检查新鲜度，返回内容。