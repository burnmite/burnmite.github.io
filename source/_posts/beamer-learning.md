---
title: beamer&latex学习
category: beamer, latex
---

# 学习来源
学习来源是一个关于LLVM的slides，感觉做的非常简约大气，所以分析里面的自定义的command和environment是如何设计的，这里分享这份文档的链接:[酷炫的slides](http://llvm.org/devmtg/2014-10/Slides/Cormack-BuildingAnLLVMBackend.pdf)

# 定义代码文件名
观察下面的图片:
![](http://p8gk4u1ta.bkt.clouddn.com/18-6-2/21929161.jpg)
可以看到代码的右下角有一个青色的文件名，并且每个代码框的右下角都能够添加文件名，而且位置非常合适，这是如何设计的呢，代码如下：
```latex
\newcommand{\codecaption}[2][-4.4ex]{%
  \vspace{#1}
  \hfill\mbox{\footnotesize{\textcolor{uicyan}{#2}}\hspace{0.5em}}
  \vspace*{2ex}}
```
这里将分别解释每个部分的含义：
1. 一个方括号里面一个2：表示这个新的命令接受两个参数
2. -4.4ex：这里表示的是第一个参数的默认值，可以想想如果这里的值写的是0ex，那么这个文件名就会跑到代码框的下面，如图：![](http://p8gk4u1ta.bkt.clouddn.com/18-6-2/92067138.jpg)这里写一个-4.4ex就可以让这个文件名跑到代码框里面去。
3. `\vspace`用第一个参数设置竖直间距，这里已经有默认值填充了，为-4.4ex
4. 可以把`\hfill`和最右边的`\hspace`结合起来看，`\hfill`意思就是将左边水平空间尽可能的填满，`\hspace`将右边的水平边距指定为0.5em
5. `\vspace*`暂时不清楚和`\vspace`有什么区别
6. 其中根据css中的定义，`em`和`ex`都是一个比例单位，其中，`em`为当前字体尺寸，即如果当前字体尺寸为12pt，则`1em=12pt`，`ex`是一个字体的`x-height`，`x-height`通常是当前字体尺寸的一半。

## 定义命令和使用命令的一些概要
> 我就把吐槽放在这里了，一个wiki等于十个百度，emmmmmm......
在这个ppt开头的定义中，有这么两个命令：
```latex
\newcommand{\sourcebox}[3][firstline=1]{%
  \begin{tcolorbox}\VerbatimInput[#1]{#2}\end{tcolorbox}
  \codecaption{#3}}

\newcommand{\examplebox}[2][firstline=1]{\sourcebox[#1]{examples/#2}{#2}}
```
可以看出是`\examplebox`使用了`\sourcebox`来定义自己，其中`\newcommand`的格式为：
```latex
\newcommand{name}[num][default]{definition}
```
而使用命令的格式为：
```latex
\name{arg1}{arg2}...
```
可是如果根据这种格式可以发现上面的`\sourcebox`是什么鬼：
```latex
\sourcebox[#1]{examples/#2}{#2}
```
最后根据wiki的说法，这里说说我自己的理解，即如果新定义的命令的前n个参数有默认值，但是需要在使用命令的时候显式地写出来，那么就不能使用`{}`而要使用`[]`，即这里使用`\sourcebox`的意思是：
1. 显示指定第一个参数为`firstline=1`
2. 指定第二个参数和第三个参数为`examples/#2`和`#2`

## 定义代码框的格式
代码框的格式如下：
```latex
\newenvironment{codebox}
 {\VerbatimEnvironment
  \begin{tcolorbox}%
  \begin{Verbatim}}
 {\end{Verbatim}\end{tcolorbox}}
```
其中，这是通过新定义一个环境来决定的，定义环境的格式为：
```latex
\newenvironment{新环境名称}[参数个数][参数默认值]{开始部分定义}{结束部分定义}
\renewenvironment{新环境名称}[参数个数][参数默认值]{开始部分定义}{结束部分定义}
```
同时可以看出这里的开始部分定义和结束部分定义还是非常对程的，除了有一个不和谐的地方`\VerbatimEnvironment`，这个以后补充，似乎这个命令和一个宏包有关，`fancyvrb`