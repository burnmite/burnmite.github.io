---
title: beamer&latex学习
---

# 学习来源
学习来源是一个关于LLVM的slides，感觉做的非常简约大气，所以分析里面的自定义的command和environment是如何设计的

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