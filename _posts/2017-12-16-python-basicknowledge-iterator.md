---
layout: post
title: Python基础知识之迭代器与生成器
categories: Python
---

本文简单介绍了Python语言中的迭代器与生成器<!-- more -->

### 容器（Container） ###

容器是一种把多个元素组织在一起的结构，容器中的元素可以逐个地迭代获取，可以用in、not in关键字判断元素是否在容器中，Python中常用的容器有：list、dict、tuple、set、str...

尽管大多数

代码示例：

- 询问元素是否在一个list中：

	    a = list([1, 2, 3, 4])
	
	    print 4 in a  # True
	    print 5 in a  # False

- 询问元素是否在一个dict的键值中：

		a = {1: 'jiao', 2: 'chang', 3: 'xia'}
		
		print 1 in a  # True
		print 5 in a  # False

- 询问字符/子字符串是否在一个字符串中：

	    a = 'jiaochangxia'
	
	    print 'j' in a  # True
	    print 'jiao' in a  # True
	    print 'jc' in a  # False
	    print 'z' in a  # False

### 可迭代对象（Iterable） ###

能够实现```__iter__```方法，即能够返回迭代器的对象可称之为可迭代对象。迭代器与容器一样，是一种通俗的叫法，并不指具体的某种数据类型。

### 迭代器（Iteration） ###

迭代器是一种带状态的对象，能够在调用next()方法时返回容器中下一个元素值，任何实现```__iter__```和```next()```方法的对象都是迭代器。其中，```__iter__```返回自身，```next()```返回下一个值，如果容器中没有下一个值，则抛出StopIteration异常。迭代器有一种特定的迭代器类型，例如```list_iterator```和```set_iterator```。

### 生成器（Generator） ###

生成器是一种特殊的迭代器，它不需要实现```__iter__```和```next()```方法，只需要一个```yield```关键字。生成器肯定是一种迭代器，反之不成立。```yield```是Python语法糖，内部实现支持了迭代器协议，同时```yield```内部是一个状态机，维护者挂起和继续的状态。

使用生成器实现20以内斐波那契数列代码示例：

	def fib(max_fib):
	    prev, curr = 0, 1
	    while curr <= max_fib:
	        yield curr
	        prev, curr = curr, curr + prev
	
	if __name__ == '__main__':
	    print list(fib(20))

### 生成器表达式 ###

生成器表达式和列表表达式类似，形式上将列表表达式的```[]```转为```()```，例如：

- 列表表达式： ```[x*x for x in range(0, 3)]```

- 生成器表达式：```(x*x for x in range(0, 3))```

当序列过长且每次只需要一个元素时，应当考虑使用生成器，而不是使用列表解析，因为生成器使用了“惰性计算”，只有在检索时才被赋值，而不像列表解析那样讲序列全部保存在内存中，因此在序列比较长时，使用生成器会使内存消耗更加有效。

### 递归生成器 ###

生成器可以向函数一样进行递归使用的。

使用递归生成器对一个序列进行全序排列的代码示例：

	def permutations(li):
	    if len(li) == 0:
	        yield li
	    else:
	        for i in range(len(li)):
	            li[0], li[i] = li[i], li[0]
	            for item in permutations(li[1:]):
	                yield [li[0]] + item
	
	if __name__ == '__main__':
	    for item in permutations(range(3)):
	        print item

输出：

	[0, 1, 2]
	[0, 2, 1]
	[1, 0, 2]
	[1, 2, 0]
	[2, 0, 1]
	[2, 1, 0]

### 生成器的```send```和```close```方法 ###

- send(value)

send是除next以外恢复生成器的方法。Python2.5以后，```yield```语句变成```yield```表达式，也就是说```yield```可以有一个值，这个值就是send的参数，该参数默认为None，所以send()、send(None)和yield是等价的。在使用send函数时需要注意的是：在调用send()方法之前，迭代器必须处于挂起状态，否则抛出异常，即第一使用时，要使用next()或send()，否则因为没有yield语句来接收值而报异常。

- close()

close()可以关闭生成器，对关闭后的生成器调用next或send方法，则会抛出StopIteration异常。

代码示例：

	def Zrange(n):
	    i = 0
	
	    while i < n:
	        val = yield i
	        print 'val is:', str(val)
	        i += 1
	
	if __name__ == '__main__':
	    zrange = Zrange(5)
	    print zrange.next()
	    print zrange.next()
	    print zrange.send('hello')
	    print zrange.close()
	    print zrange.next()