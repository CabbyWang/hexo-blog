---
title: 猴子补丁
# date: 2018-06-05 23:56:24
tags: [猴子补丁, python]
---

# 猴子补丁（Monkey Patch)
`猴子补丁`， 顾名思义...完全不知道是什么玩意有木有， 可能有些历史原因， 这里就不管了， 步入正题。

`猴子补丁`就是在模块运行的时候替换模块中的某些方法。上代码可能比较容易理解

```python
>>> class A(object):
	    def func(self):
		    print('This is a func!')
>>> A().func()
# This is a func!
```
<!--more-->
上述代码很好理解， 正常调用， 然后执行`func`函数， 咱们继续
```python
>>> def func2(self):
	   print('This is func2!')
>>> A.func = func2
>>> A().func()
# This is func2!
```
这里定义了一个`func2`函数， 然后将这个函数赋值给`A.func`， 然后执行同样的语句`A().func()`， 最后的输出不一样了， 这其实就是`猴子补丁`的实现

下面是网上看到的一个感觉很不错的用法

- 很多代码用到了import json， 但是后来发现ujson的性能更高， 这个时候可以用`import ujson as json`， 但是要每个文件都去修改， 显然很麻烦， 这里就可以用到`猴子补丁`

```python
import json
import ujson

def monkey_patch_json():
	json.__name__ = 'ujson'
	json.dumps = ujson.dumps
	json.loads = ujson.loads
	# json.dump = ujson.dump
	# json.load = ujson.load

monkey_patch_json()
```

