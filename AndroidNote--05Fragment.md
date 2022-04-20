# 5 探究Fragment

## 5.1 Fragment是什么

*Fragment是Android3.0后引入的一个新的API，他出现的初衷是为了适应大屏幕的平板电脑， 当然现在他仍然是平板APP UI设计的宠儿，而且我们普通手机开发也会加入这个Fragment， 我们可以把他看成一个小型的Activity，又称Activity片段！想想，如果一个很大的界面，我们 就一个布局，写起界面来会有多麻烦，而且如果组件多的话是管理起来也很麻烦！而使用Fragment 我们可以把屏幕划分成几块，然后进行分组，进行一个模块化的管理！从而可以更加方便的在 运行过程中动态地更新Activity的用户界面！另外Fragment并不能单独使用，他需要嵌套在Activity 中使用，尽管他拥有自己的生命周期，但是还是会受到宿主Activity的生命周期的影响，比如Activity 被destory销毁了，他也会跟着销毁！*[菜鸟教程](https://www.runoob.com/w3cnote/android-tutorial-fragment-base.html)

![Fragment分别对应手机与平板间不同情况的处理图](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/365/41442282.jpg)

## 5.2 Fragment的使用方式



## 5.3 Fragment的生命周期

## 5.4 动态加载布局的技巧

## 5.5 Fragment的最佳实践