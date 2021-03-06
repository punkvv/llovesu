---
title: Hello Markdown
date: 2017-10-19 14:26:53
categories: Hello World
tags:
  - Hello World
  - Markdown
---

Markdown 入门

<!-- more -->

# 区块元素

## 标题
在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶。

```
	# H1
	# H2
 ###### H6
```

## 区块引用 Blockquotes
在每行的最前面加上 > ，或者在第一行最前面加上 > 。

```
	> aaaaaaaaaaaaaaaaaaa
```

## 列表
无序列表使用星号、加号或是减号作为列表标记。
有序列表则使用数字接着一个英文句点。

```
	- 1
	- 2
```

## 代码区块
缩进 4 个空格或是 1 个制表符就可以

```
这是一个普通段落：
	这是一个代码区块。
```

## 分隔线
在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西


```
	---
```

# 区段元素

## 链接
行内式

```
	[文字](http://llovesu.com/ "标题")
```

参考式

```
	[文字][id]
	[id]: http://llovesu.com/ "标题"
```

## 强调
使用一个或者两个星号（*）和底线（_）作为标记强调字词的符号

```
	*aa*
	**aa**
	_a_
	__aa__
```

## 代码
如果要标记一小段行内代码，你可以用反引号把它包起来

```
	a `b` c
```

## 图片
行内式

```
	![文字](/path/to/img.jpg "标题")
```

参考式

```
	![文字][id]
	[id]: /path/to/img.jpg "标题"
```

# 其他

## 自动链接

```
	<http://llovesu.com/>
```

## 反斜杠
利用反斜杠来插入一些在语法中有其它意义的符号
支持以下这些符号：

```
	\   反斜线
	`   反引号
	*   星号
	_   底线
	{}  花括号
	[]  方括号
	()  括弧
	#   井字号
	+   加号
	-   减号
	.   英文句点
	!   惊叹号
```

参考 [Markdown 语法说明 (简体中文版)](http://www.appinn.com/markdown/#block)



