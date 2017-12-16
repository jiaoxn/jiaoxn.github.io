---
layout: post
title: Python基础知识之*args与**kwargs
categories: GIS
---

本文简单介绍了Python语言中的*args与**kwargs<!-- more -->

### Python中*args的使用 ###

Python中*args用来传递非键值对的可变数量的参数列表（位置参数）给一个函数，如：

	def var_args_test(farg, *args):
	    print "formal arg:", farg
	    for arg in args:
	        print "another arg:", arg
	
	
	if __name__ == '__main__':
	    var_args_test(1, "two", 3)

输出结果为：

	formal arg: 1
	another arg: two
	another arg: 3

### Python中**kwargs的使用 ###

Python中**kwargs用来键值可变长参数列表，如：

	def var_kwargs_test(farg, **kwargs):
	    print "formal arg:", farg
	    for key in kwargs:
	        print "another keyword arg: %s: %s" % (key, kwargs[key])
	
	if __name__ == '__main__':
	    var_kwargs_test(farg=1, myarg2="two", myarg3=3)

输出结果为：

	formal arg: 1
	another keyword arg: myarg2: two
	another keyword arg: myarg3: 3

### 函数调用中使用*args或**kwargs ###

函数调用中使用*args:

	def test_var_args_call(arg1, arg2, arg3):
	    print "arg1:", arg1
	    print "arg2:", arg2
	    print "arg3:", arg3
	
	args = ("two", 3)
	test_var_args_call(1, *args)

输出为：

	arg1: 1
	arg2: two
	arg3: 3

函数调用中使用**kwargs:

	def test_var_args_call(arg1, arg2, arg3):
	    print "arg1:", arg1
	    print "arg2:", arg2
	    print "arg3:", arg3
	
	kwargs = {"arg3": 3, "arg2": "two"}
	test_var_args_call(1, **kwargs)

输出为：

	arg1: 1
	arg2: two
	arg3: 3

### 标准参数、*args和**kwargs共同使用时的顺序 ###

标注参数、```*args```和```**kwargs```共同使用的顺序：``` some_func(fargs, *args, **kwargs) ```