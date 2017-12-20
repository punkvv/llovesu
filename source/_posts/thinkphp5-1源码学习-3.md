---
title: ThinkPHP5.1源码学习 3
date: 2017-12-13 09:13:27
categories: PHP
tags:
  - PHP
  - ThinkPHP5
---

ThinkPHP5.1源码学习-3：门面（Facade）

<!-- more -->

## 作用
门面为容器中的类提供了一个静态调用接口，可以为任何的非静态类库定义一个 `facade` 类。

## Facade
### 主要方法
1. `::bind($name, $class = null)` 绑定类的静态代理（把继承了 `Facade` 的类和对应的实际的类，存放于 `::$bind` 中）如：
```php
::$bind = [
	'think\facade\App' => 'think\App',
]
```
2. `::createFacade($class = '', $args = [], $newInstance = false)` 创建类的实例，把对应 `::$bind` 里的实际类取出，然后调用容器的 `make()` 方法创建类的实例
3. `::__callStaic($method, $params)` 在静态上下文中调用一个不可访问方法时调用该方法，调用实际类的方法。

参考内容
[call_user_func_array][call_user_func_array]
[魔术方法][magic]

[magic]: http://php.net/manual/zh/language.oop5.magic.php
[call_user_func_array]: http://php.net/manual/zh/function.call-user-func-array.php

