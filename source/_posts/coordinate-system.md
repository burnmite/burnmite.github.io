---
title: tikz中的坐标表示法
---
# 坐标表示概要
本章主要是一个笔记，主要内容是在tikz中如何表示一个坐标，除了有最标准的`(x, y)`表示法以外，还有多种表示坐标的方法，其中`(x, y)`表示法的形式为：
```latex
(3, 2)
```
在tikz中，基本上，坐标都是被框在一个`()`里面，表示一个坐标的基本语法是：
```latex
([options]coordinate specification)
```
其中，`coordinate specification`不仅仅是所谓的`(3, 4)`这样的表示，而是更加通用的表示：
```latex
(coordinate system cs:list of key-value pairs specific to the coordinate system)
```

# 表示法
## 普通坐标表示法canvas
包括如下几种方法：
1. 二维坐标表示：
```latex
(canvas cs:x = 3, y = 3)
```
2. 二维极坐标表示
```latex
(canvas polar cs:radius = 3cm, angle = 40)
```
3. 三维坐标表示
```latex
(xyz cs:x = 1, y = 3, z = 3)
```

## 交点坐标表示法intersection
这种表示法在不能确定准确坐标，而能靠两条线的交点确定坐标时特别有用，语法形式如下：
```latex
(intersection cs:
    firstline={(one point)--(another point)}, 
    secondline={(ont point)--(another point)})
```
其中这种表示法的简写形式为：
```latex
(intersection of p -- q and a -- b)
```
其中`pqab`四个点没有被括号包起来

## 垂直坐标表示法perpendicular
这种表示法我感觉时交点坐标表示法的特殊版本，形式如下：
```latex
(perpendicular cs:
    horizontal line through=(A), 
    vertical line through=(B))
```
虽然说前几种方法里面的简写形式可以用可以不用，但是垂直坐标表示法中，简写形式还是非常有用的，简写形式如下：
```latex
(p |- q) or (p -| q)
```
这两种形式表示的含义不同，第一种是通过p画一条竖直的垂线，垂直于穿过q的水平线；第二种是通过p画一条水平的垂直线，垂直于穿过q的竖直线。

## 辅助坐标表示：节点坐标表示法node
看了文档之后发现，这种表示并不算是一种独立的坐标表示法，而是在给出一个节点`node`之后，用tikz给出的语法表示出节点上面的位置。语法如下：
```latex
(node cs:name=somename, angle=40)
(node cs:name=somename, anchor=south)
```
