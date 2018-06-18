---
title: 关于tikz中layer的使用
---

# 问题来源
在我自己编写tikz代码的时候，并没有碰到要使用layer的地方，而是在自己浏览TeXample网站上面别人写的代码的时候，其他的编程者用到了layer，所以这里翻阅tikz的文档，发现也并不难，说不定以后还有很大的用处，这里就稍微记录一下

# 主要的命令
```latex
\pgfdeclarelayer{name}

\pgfsetlayers{layer list}

\begin{pgfonlayer}{layer name}
...
\end{pgfonlayer}
```
话说回来，这三个命令真的是非常简单了，现在有时候看到一些简单的命令自己都觉得是不是假的，总觉得会有更多的参数来辅助什么的，看来这里并不需要。

第一个命令是声明一个layer，就是定义一个名字，什么都不用做。

第二个命令是将声明的layer排序，这里是一个栈的形式，其中里面一定要有一个layer的name为`main`。

第三个group可以在tikzpicture环境里面的任何一个地方使用，如果不同的group使用了不同的layer，那么pgf会将他们汇聚。

# 例子
例子一看就明白了。
```latex
\pgfdeclarelayer{background layer}
\pgfdeclarelayer{foreground layer}
\pgfsetlayers{background layer,main,foreground layer}
\begin{tikzpicture}
    % On main layer:
    \fill[blue] (0,0) circle (1cm);
    \begin{pgfonlayer}{background layer}
    \fill[yellow] (-1,-1) rectangle (1,1);
    \end{pgfonlayer}
    \begin{pgfonlayer}{foreground layer}
    \node[white] {foreground};
    \end{pgfonlayer}
    \begin{pgfonlayer}{background layer}
    \fill[black] (-.8,-.8) rectangle (.8,.8);
    \end{pgfonlayer}
    % On main layer again:
    \fill[blue!50] (-.5,-1) rectangle (.5,1);
\end{tikzpicture}
```
所形成的的图像如下:
![](http://p8gk4u1ta.bkt.clouddn.com/18-6-18/91594896.jpg)