---
title: ThinkPHP5.1源码学习 1
date: 2017-11-07 10:44:07
categories: PHP
tags:
  - PHP
  - ThinkPHP5
---

ThinkPHP5.1源码学习-1：自动加载的实现

<!-- more -->
## 类的自动加载
[spl_autoload_register()][spl_autoload_register] 函数可以注册任意数量的自动加载器，当使用尚未被定义的类和接口时自动去加载。

## PSR-4 自动载入规范
一个完整的类名需具有以下结构：
`\<命名空间>(\<子命名空间>)*\<类名>`
1. 完整的类名必须要有一个顶级命名空间，被称为 "vendor namespace"；
2. 完整的类名可以有一个或多个子命名空间；
3. 完整的类名必须有一个最终的类名；
4. 完整的类名中任意一部分中的下滑线都是没有特殊含义的；
5. 完整的类名可以由任意大小写字母组成；
6. 所有类名都必须是大小写敏感的。
7. 当根据完整的类名载入相应的文件
8. 完整的类名中，去掉最前面的命名空间分隔符，前面连续的一个或多个命名空间和子命名空间，作为“命名空间前缀”，其必须与至少一个“文件基目录”相对应；
9. 紧接命名空间前缀后的子命名空间必须与相应的”文件基目录“相匹配，其中的命名空间分隔符将作为目录分隔符。
10. 末尾的类名必须与对应的以 .php 为后缀的文件同名。
11. 自动加载器（autoloader）的实现一定不能抛出异常、一定不能触发任一级别的错误信息以及不应该有返回值。

## Loader.php
### 主要属性

1. `::$prefixLengthsPsr4` 记录以`顶级命名空间` 首字母为键，以`顶级命名空间`与`顶级命名空间`字符长度组成的数组为值：
```php
::$prefixLengthsPsr4 = [
	't' => [
		'think\'  => 6,
		'traits\' => 7,
	],
]
```

2. `::$prefixDirsPsr4` 记录以 `顶级命名空间` 为键，对应的目录为值的数组：
```php
::$prefixDirsPsr4 = [
	'think\' => [
		0 => 'D:\tp5\thinkphp\library\think/'
	],
	'traits\' => [
		0 => 'D:\tp5\thinkphp\library\think/../traits/'
	],
]
```

3. `::fallbackDirsPsr4` 记录需要自动加载类库的目录：
```php
::$fallbackDirsPsr4 = [
	0	=> 'D:\tp5\extend',
]
```
4. `::$classAlias` 类库别名

### 主要方法
1. `register()` 方法实现系统自动加载机制。
注册 `Loader::autoload()` 为`__autoload`的实现。注册`think`、`traits`命名空间定义。加载类库映射文件。Composer 自动加载支持。自动加载 extend 目录。

2. `autoload($class)` 方法实现加载类。
先判断`::$classAlias` 里是否定义，如果定义对类使用 [class_alias()][class_alias] 方法，并返回。没有定义则去查找文件`findFile($class)`。最后载入文件。

3. `findFile($class)` 通过完整类名查找文件
按顺序查找：
`::$map`；
通过查找 `::$prefixLengthsPsr4`再去查找`::$prefixDirsPsr4`,`::$prefixLengthsPsr4`保存的长度在这里起到了作用；
`::$fallbackDirsPsr4`;
查找 PSR-0 `::$prefixesPsr0`、`::$fallbackDirsPsr0`


[spl_autoload_register]: http://php.net/manual/zh/function.spl-autoload-register.php
[class_alias]: http://php.net/manual/zh/function.class-alias.php
