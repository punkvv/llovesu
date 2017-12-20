---
title: ThinkPHP5.1源码学习 2
date: 2017-12-12 13:48:52
categories: PHP
tags:
  - PHP
  - ThinkPHP5
---

ThinkPHP5.1源码学习-2：容器和依赖注入

<!-- more -->

## 依赖注入
依赖注入通过构造注入，函数调用或者属性的设置来提供组件的依赖关系。
```php
class Database
{
	protected $adapter;
	
	public function __construct(MySqlAdapter $adapter)
	{
		$this->$adapter = $adapter;
	}
}

class MySqlAdapter{}
```

## 控制反转
一个系统通过组织控制和对象的完全分离来实现`控制反转`。对于依赖注入，这就意味着通过在系统的其它地方控制和实例化依赖对象，从而实现了解耦。

## 依赖反转准则
依赖反转准则，倡导`依赖于抽象而不是具体`。简单来说就是依赖应该是接口、约定或者抽象类，而不是具体的实现。
```php
class Database
{
	protected $adapter;
	
	public function __construct(AdapterInterface $adapter)
	{
		$this->$adapter = $adapter;
	}
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {}
```

## 容器
依赖注入容器和依赖注入不是相同的概念。容器是帮助我们更方便地实现依赖注入的工具。容器用于管理很多不同的依赖对象。
依赖注入容器管理对象：从实例到配置。对象本身并不知道它们是由容器管理的，对容器一无所知。

## Container 类
ThinkPHP 中的容器类

### 主要属性
1. `::$instance` 容器对象实例
2. `$bind` 容器绑定标识
3. `$instances` 容器中的对象实例

### 主要方法
1. `::getInstance()` 获取当前容器的实例（单例）返回 `::$instance`
2. `bind($abstract, $concrete = null)` 绑定一个类、闭包、实例、接口到容器
3. `make()`: 创建类的实例
```php
/**
 * 创建类的实例
 * @param $abstract 类名或者标识
 * @param array $vars 变量
 * @param bool $newInstance 是否每次创建新的实例
 * @return mixed|object
 */
public function make($abstract, $vars = [], $newInstance = false)
{
    if (true === $vars) {
        // 总是创建新的实例化对象
        $newInstance = true;
        $vars        = [];
    }
	// 是否存在容器对象实例中
    if (isset($this->instances[$abstract]) && !$newInstance) {
        $object = $this->instances[$abstract];
    } else {
		// 是否存在绑定标识中
        if (isset($this->bind[$abstract])) {
            $concrete = $this->bind[$abstract];
 			// 是否为闭包
            if ($concrete instanceof Closure) {
                $object = $this->invokeFunction($concrete, $vars);
            } else {
				// 递归
                $object = $this->make($concrete, $vars, $newInstance);
            }
        } else {
			// 实例化类
            $object = $this->invokeClass($abstract, $vars);
        }

        if (!$newInstance) {
            $this->instances[$abstract] = $object;
        }
    }

    return $object;
}
```
4. `invokeClass()` 调用反射执行类的实例化 支持依赖注入:
```
/**
 * 调用反射执行类的实例化 支持依赖注入
 * @param $class 类名
 * @param array $vars 变量
 * @return object
 */
public function invokeClass($class, $vars = [])
{
	// 获得类的反射对象
    $reflect     = new ReflectionClass($class);
	// 取得类的构造函数
    $constructor = $reflect->getConstructor();

    if ($constructor) {
		// 绑定参数到构造函数
        $args = $this->bindParams($constructor, $vars);
    } else {
        $args = [];
    }
	// 从给出的参数创建一个新的类实例
    return $reflect->newInstanceArgs($args);
}
```
5. `::get($abstract, $vars = [], $newInstance = false)` 获取容器中的对象实例

6. `bindParams($reflect, $vars = [])` 绑定参数：
```
/**
 * 绑定参数
 * @param $reflect \ReflectionMethod|\ReflectionFunction 反射类
 * @param array $vars 变量
 * @return array
 */
protected function bindParams($reflect, $vars = [])
{
    $args = [];

	// 参数长度
    if ($reflect->getNumberOfParameters() > 0) {
        // 判断数组类型 数字数组时按顺序绑定参数
        reset($vars);
        $type   = key($vars) === 0 ? 1 : 0;
		// 获取参数
        $params = $reflect->getParameters();

        foreach ($params as $param) {
			// 获取参数名称
            $name  = $param->getName();
			// 获取参数类名
            $class = $param->getClass();
			// 如果参数为对象
            if ($class) {
				// 取的该类名称 递归实例化
                $className = $class->getName();
                $args[]    = $this->make($className);
            } elseif (1 == $type && !empty($vars)) {
                $args[] = array_shift($vars);
            } elseif (0 == $type && isset($vars[$name])) {
                $args[] = $vars[$name];
            } elseif ($param->isDefaultValueAvailable()) {
                $args[] = $param->getDefaultValue();
            } else {
                throw new InvalidArgumentException('method param miss:' . $name);
            }
        }
    }

    return $args;
}
```

参考内容
[依赖注入][依赖注入]、[Closure][Closure]、[反射][反射]

[依赖注入]: http://laravel-china.github.io/php-the-right-way/#dependency_injection
[Closure]: http://php.net/manual/zh/class.closure.php
[反射]: http://php.net/manual/zh/book.reflection.php