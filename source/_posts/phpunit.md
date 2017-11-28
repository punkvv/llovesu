---
title: PHPUnit
date: 2017-11-28 09:51:11
categories: PHP
tags: PHP
---

[PHPUnit][PHPUnit] 学习

<!-- more -->

## 编写测试

1. 测试的依赖关系
  PHPUnit 支持对测试方法之间的显示依赖关系进行声明。这种依赖关系并不是定义在测试方法的执行顺序中，而是允许生产者 `producer` 返回一个测试基境 `fixture` 的实例，并将此实例传递给依赖于它的消费者 `consumer` 们。
    - 生产者 `producer`，是能生成被测单元并将其作为返回值的测试方法。
	- 消费者 `consumer`，是依赖于一个或多个生产者及其返回值的测试方法。
	
  使用 `@depends` 标注来表达测试方法之间的依赖关系。如果需要传递对象的副本而非引用，则应当用 `@depends clone` 替代 `@depends`。

2. 数据供给器
  测试方法可以接受任意参数。这些参数由数据供给器方法提供。用 `@dataProvider` 标注来指定使用哪个数据供给器方法。
  数据供给器方法必须声明为 `public`，其返回值要么是一个数组，其每个元素也是数组；要么是一个实现了 `Iterator` 接口的对象，在对它进行迭代时每步产生一个数组。每个数组都是测试数据集的一部分，将以它的内容作为参数来调用测试方法。
  如果测试同时从 `@dataProvider` 方法和一个或多个 `@depends` 测试接受数据，那么来自于数据供给器的参数将先于来自所依赖的测试的。
  {% note success %} 如果一个测试依赖于另外一个使用了数据供给器的测试，仅当被依赖的测试至少能在一组数据上成功时，依赖于它的测试才会运行。使用了数据供给器的测试，其运行结果是无法注入到依赖于此测试的其他测试中的。 {% endnote %}
 	
  {% note success %} 所有的数据供给器方法的执行都是在对 `setUpBeforeClass` 静态方法的调用和第一次对 `setUp` 方法的调用之前完成的。因此，无法在数据供给器中使用创建于这两个方法内的变量。这是必须的，这样 PHPUnit 才能计算测试的总数量。 {% endnote %}

  











[PHPUnit]: http://www.phpunit.cn/manual/6.4/zh_cn/writing-tests-for-phpunit.html