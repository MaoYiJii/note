---
description: 裝飾模式 / 修飾模式
---

# Decorator

### 簡述

*   Wiki\
    向某個對象動態地添加更多的功能。修飾模式是除類繼承外另一種擴充功能的方法。

    > 原文: Attach additional responsibilities to an object dynamically keeping the same interface. Decorators provide a flexible alternative to subclassing for extending functionality.
* 個人實作見解\
  一個基底類別分別由主體類別與裝飾類別繼承，將主體物件放入裝飾類別的建構子做為一個新的主體，再放入下一個裝飾類別的建構子。

### 參考

[https://dotblogs.com.tw/joysdw12/2013/04/08/100986](https://dotblogs.com.tw/joysdw12/2013/04/08/100986)
