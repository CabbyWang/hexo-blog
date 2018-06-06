---
title: 浅谈python元类
# date: 2018-06-05 23:56:24
tags: [python, 元类]
---


# 浅谈元类
已经有[大牛总结](http://python.jobbole.com/21351/)，我这里只是凭自己的理解，总结加深印象, 班门弄斧一下- -!

谈到元类，这里就要引入python中的==一切皆对象==的概念了。 ==类==是用来描述如何去生成一个对象的代码段。
```python
>>> class A(object):
	    pass
>>> A
# <class '__main__.A'>
>>> b = A
>>> b
# <class '__main__.A'>
```

<!--more-->

上述代码一部分体现了python的==一切皆对象==，类也是一个对象， 可以用来赋值/增加属性/参数传递

下面是一个动态创建类的例子
```python
>>> def create_class(name):
	if name == 'foo':
		class Foo(object):
			pass
		return Foo
	else:
		class Bar(object):
			pass
		return Bar

>>> a = create_class('foo')
>>> a
# <__main__.create_class.<locals>.Foo at 0x105762588>
>>> print(a)
# <class '__main__.create_class.<locals>.Foo'>
# 这里有个疑问， 为什么print以后的输出结果和不使用print不太一样
```

通过上面的例子， 可以知道类也是可以动态创建的， 但是这样去创建很繁琐，需要自己编写整个类完整的代码， 而且似乎也只是创建一个类，和在其他地方创建没有什么区别

下面介绍一下内建函数`type`
- 判断类型（这是我们最常用的，也是众所周知的）
```python
>>> print(type(1))
# <class 'int'>
>>> print(type('aa'))
# <class 'str'>
```
- 创建类
```python
# type(object_or_name, bases, dict)
# object_or_name: 要创建的类名
# bases: 父类元组（指定继承哪些类）
# dict: 类的属性字典
>>> A = type('A', (), {})
>>> print(A)
# <class '__main__.A'>
```
```python
>>> class A(object):
>>> 	bar = 'string'
```
相当于：
```python
>>> A = type('A', (), {'bar': 'string'})
```
这里感觉应该不难理解。

理解了内建函数`type`，感觉就离理解==元类==又近了一步

其实`type`就是一个元类， python用`type`在背后创建所有类（这里可以回忆一下我们是怎么创建类的，然后type为我们做了什么）

对象的`__class__`属性可以用来查看他们的类是谁(==一切皆对象==)

```python
>>> a = 1
>>> a.__class__
# int
>>> b = 'abc'
>>> b.__class__
# str
>>> class C(object): pass
>>> c = C()
>>> c.__class__
# __main__.C
```

int， str， __main__.C都是类， 由于类也是对象， 下面看看这些类的类是什么

```python
>>> a.__class__.__class__
# type
>>> b.__class__.__class__
# type
>>> c.__class__.__class__
# type
```

所有类的类都是`type`，因此， 元类就是用来创建类的 (可能有点抽象，这里再解释一下， 上面的int，str，__main__.C都是类，类是用来创建对象的， `type`又是他们的类，所有他们是`type`创建的， 而之前又谈论过`type`其实就是元类， 所以元类就是用来创建类的)

`type`是python使用的内建元类，python同时也允许开发者建自己的元类， 而自己定义就用到了`__metaclass__`。

## `__metaclass__`

