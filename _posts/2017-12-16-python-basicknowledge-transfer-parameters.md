---
layout: post
title: Python中可变类型与不可变类型
categories: Python
---

本文简单介绍了Python语言中的参数传递<!-- more -->

## Python中可变类型与不可变类型 ##

Python中可变类型有**dict**和**list**，不可变类型有**int**、**string**、**float**和**tuple**。

此处提到的可变与不可变是指是否能够在原来的内存地址上修改值。

不可变类型的重新赋值过程：首先在内存中创建一个新的变量，然后将当前变量的引用指向新创建的变量，最后把没有引用的变量从内存中删除。以int型为例，i += 1 并不是在原有对象上加1，而是重建创建一个value+1的对象，然后将i引用到这个新的对象上。

可变类型的话，因为是可变的，所有可以直接在当前内存地址上修改原来的值。以list为例，a = [1, 2]，a.append(3) append前后a的内存地址是一样的。

## Python中参数传递 ##

Python规定参数传递是传递引用，也就是传递给函数的是原变量实际所指向的内存空间，修改的时候就会根据该引用的指向去修改该内存中的内容。

如果参数传递的是可变类型对象的引用，在函数内部修改也会影响函数外部的变量；如果传递的是不可变类型对象的引用，在函数内部修改不会影响到函数外部的变量。

## 代码示例 ##
	# -*- coding: utf-8 -*-
	def variable_type_test():
	    """ 测试可变类型
	    """
	    a = list([1, 2, 3])
	    print id(a)  # 35457608
	
	    a.append(4)
	    print id(a)  # 35457608
	
	
	def immutable_type_test():
	    """测试不可变类型
	    """
	
	    a = 1
	    print id(a)  # 30966792
	
	    a += 1
	    print id(a)  # 30966768
	
	
	def parameters_passing_test_with_variable_type(a):
	    """ 使用可变类型测试参数传递
	
	    Args:
	        a: list
	            可变类型对象的引用
	    """
	
	    a.append(4)
	
	
	def parameters_passing_test_with_immutable_type(b):
	    """ 使用不可变类型测试参数传递
	
	    Args:
	        b: int
	            不可变类型对象的引用
	    """
	
	    b = 4
	
	if __name__ == '__main__':
	    variable_type_test()
	    immutable_type_test()
	    
	    a = list([1, 2, 3])
	    print a  # [1, 2, 3]
	    parameters_passing_test_with_variable_type(a)
	    print a  # [1, 2, 3, 4]
	    
	    b = 2
	    print b  # 2
	    parameters_passing_test_with_immutable_type(b)
	    print b  # 2


