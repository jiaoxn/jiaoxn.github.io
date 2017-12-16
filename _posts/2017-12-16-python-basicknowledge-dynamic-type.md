---
layout: post
title: Python基础知识之动态类型
categories: Python
---

本文简单介绍了Python语言中的动态类型<!-- more -->

Python的核心概念之一就是动态类型。

与动态类型对应的是静态类型，绝大多数编程语言使用的是静态类型。静态类型中规定一个变量固定作为内存中某个单元的标识，并且该单元只能存储特定类型的数据，即如果一个变量声明为整型，内存会开辟对应的空间，它只能接受整型变量的输入，不能接受其它类型数据的输入，否则会报错。

动态类型则不一样，以Python为例，变量并不是内存中某个单元的标识，也就不需要预先定义的类型。Python中的变量是对内存中存储的某个数据的引用，这个引用是可以修改的。变量的类型就是它所引用的数据的类型，对变量的每一次赋值，都可能改变变量的类型。

## 代码示例 ##

	# -*- coding: utf-8 -*-
	
	
	def dynamic_type_test():
	    a = 2
	    print id(a)  # a的内存地址
	    print type(a)  # a的类型
	
	    a = 3
	    print id(a)  # a的内存地址
	    print type(a)  # a的类型
	
	    a = 'abc'
	    print id(a)  # a的内存地址
	    print type(a)  # a的类型
	    
	    
	if __name__ == '__main__':
	    dynamic_type_test()