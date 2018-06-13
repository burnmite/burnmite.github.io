---
title: 关于node的一些事宜
---
# 问题来源
其实这篇博客解决的不是一个问题，而是对tikz中的node的一些用法的总结归纳，学过tikz的人都知道tikz的语法其实是有点杂乱的，而node又是tikz中很重要的一个部分，所以对于node的总结归纳我觉得是及其有必要的。

# 基本用法
node的作用之一就是用node本身将一段文本包裹，并在node的边界或者是内部导出一些符号供下一步的运算，下面是node两种基本用法：
```latex
\node [anchor=west] (name) at (3, 4) {zhou};
\node (name) [below of=name] {zhou};
```
1. 在第一种用法中，node被放在了一个确定的位置，并将node放在了文本`zhou`的下面，如果接下来有代码要引用到`name`这个点，比如一个箭头，那么箭头就会指向`zhou`的下面。也可以将`[anchor=west]`删掉。
2. 在第二个用法中，node没有指定具体的位置，但是我通过另外的语法`below of`指定了这个几点的相对位置，也可以将`[below of=name]`删掉。补充一点，`[]`里面可以包含下面的语法：
```latex
[above=offset]
[above left=offset]
[above right=offset]
[left=offset]
[right=offset]
[below=offset]
[below left=offset]
[below right=offset]
[above of=node]
[above right of=node]
[above left of=node]
[left of=node]
[right of=node]
[below of=node]
[below left of=node]
[below right of=node]
```

# 将node放在`\draw`命令里面的用法
经过对书本《Tikz And PGF》的阅读，发现如果将node放进了`\draw`里面，主要是画一条线的时候，不管是曲线还是直线，使得我们可以方便地在线上标明一个点，其中语法如下：
```latex
\draw (a, b) -- "node["postion"] ("name") {"node specfication"}"+ (c, d);
```
可能这里写的略微晦涩了一点，举个例子就明白了。
```latex
\begin{tikzpicture}[near end]
    \draw (0cm,6em) -- node[at start] {R} (3cm,6em);
    \draw (0cm,5em) -- node[at end] {E} (3cm,5em);
    \draw (0cm,4em) -- (3cm,4em) node{A} -- node[near end] {B} (6cm, 4em);
    \draw (0cm,3em) -- node{B} (3cm,3em);
    \draw (0cm,2em) -- node[midway] {C} node[at start] {A} (3cm,2em);
    \draw (0cm,1em) -- (4cm,1em) node[midway] {D} ;
\end{tikzpicture}
```
这里补充一个小点，即每个pos的比例：
```latex
near start = 0.25
near end = 0.75
very near start = 0.125
very near end = 0.875
at start = 0.0
at end = 1.0
midway = 0.5
```
所形成的图像如下：
![](http://p8gk4u1ta.bkt.clouddn.com/18-6-13/55932020.jpg)
下面分别解释每条语句的含义：
1. 第一条语句在点`(0cm, 6em)`和`(3cm, 6em)`连成的线的开始处标明一个点，且内容是`R`，同理这里的`[]`里面也可以增加其他的属性，比如`[anchor=west]`
2. 和第一条一样，只不过node是放在线段的结尾。
3. 在前面的简单的形式下又增加了一条线段，并且也同时制定了属性，只不过`[near end]`属性适用于`(3cm, 4em)`到`(6cm, 4em)`之间。如果出现了更多次，则可以换行描述，更清晰。
```latex
\draw (0cm,4em) 
    -- node {A} (3cm,4em)  
    -- node[near end] {B} (6cm, 4em);
```
4. 由于这里的node后面没有使用`[]`指定属性，所以使用了`\begin{tikzpicture}`后面指定的属性`[near end]`
5. 这一条和第三条有点相似，只不过在两个点之间同时出现了`node[midway] {C}`和` node[at start] {A}`，这就符合之前的语法中的后面的那个小加号，表示对于node指定可以出现多次，这里一次出现了在了中间，一次出现在了开始。
6. 这里仅仅是换了一个位置而已，自我感觉上面几条语句所使用的语法更加符合我的思路。

#另外一个小记
曾经有一种`\path`命令来描述`node`，如下：
```latex
\begin{tikzpicture}
    \path node at ( 0,2) [shape=circle,draw] {}
          node at ( 0,1) [shape=circle,draw] {}
          node at ( 0,0) [shape=circle,draw] {}
          node at ( 1,1) [shape=rectangle,draw] {}
          node at (-1,1) [shape=rectangle,draw] {};
\end{tikzpicture}
```
其代替性质的写法是：
```latex
\begin{tikzpicture}
    \node at ( 0,2) [circle,draw] {};
    \node at ( 0,1) [circle,draw] {};
    \node at ( 0,0) [circle,draw] {};
    \node at ( 1,1) [rectangle,draw] {};
    \node at (-1,1) [rectangle,draw] {};
\end{tikzpicture}
```
自我感觉后一种写法更加清晰，所以以后可以提醒自己，不必再去思考`\path`的写法。

