---
layout: post
title: TomekLinks算法简介及核心代码分析
categories: 不平衡分类
---


## 文章摘要 ##
本文介绍了不平衡分类算法TomekLinks的核心思想以及分析了Python的**imbalanced-learn**包实现**TomekLinks**的核心代码 <!-- more -->

## TomekLinks算法核心思想  ##

**TomekLinks**算法属于欠采样方法的一种，其核心思想是：假如两个不同类别的样本，它们的最近邻都是对方，那么它们就是一对**TomekLink**；**TomekLinks**算法将数据集中的所有的**TomekLink**对全部删除，方法是如果一对**TomekLink**中有一个样本属于多数类，则将该样本从数据集中删除。

## TomekLinks算法伪代码 ##

![image](http://ows59ec4r.bkt.clouddn.com/TomekLinks-algorithm-pseudo-code.png)


## TomekLinks算法实现核心代码解析 ##

**imbalanced-learn**包实现**TomekLinks**算法用到四个类，分别为*TomekLinks*、*BaseClearningSampler*、*BaseSampler*和*SampleMixin*，它们依次为**继承**关系，即类*TomekLinks*继承类*BaseClearningSampler*。

在**Python**代码中使用**TomekLinks**算法时需要用到*TomekLinks*类，使用方式如下：

	from imblearn.under_sampling import TomekLinks

	tl_sampling_method = TomekLinks(ratio='majority')

	X_after_tl, y_after_tl = tl_sampling_method.fit_sample(X, y)

### TomekLinks类 ###

*is_tomek*：获取数据集中所有的TomekLink对

&nbsp;&nbsp;&nbsp;&nbsp;该方法为类的静态方法，也就是说在类的外部代码中可以通过类的实例化访问也可以通过类名直接访问，具体代码如下：

	@staticmethod
	def is_tomek(y, nn_index, class_type):
	    """
	    Args:
	        y : ndarray, shape (n_samples, )
	            数据集对应的分类数据

	        nn_index : ndarray, shape (len(y), )
	            数据集中每个样本的最近邻样本在数据集中的索引值

	        class_type : dict
	            class_type的key值表示从对应的类中删除样本，例如，如果class_type的key为2，则表示从第2类中删除样本从而删除所有的TomekLink对

	    Returns:
	        is_tomek : ndarray, shape (len(y), )
	            如果值为True，表示该样本需要删除，反之，不需要删除
	    """
	    
	    links = np.zeros(len(y), dtype=bool)  # 默认都为False，用来存储哪些样本需要被删除，即当值为True时，该样本需要被删除

	    # 记录哪些类的样本不用被删除
	    class_excluded = [c for c in np.unique(y) if c not in class_type]

	    for index_sample, target_sample in enumerate(y):
	        if target_sample in class_excluded:
	            continue
	        
	        # 判断该样本与该样本的最近邻样本是否为同类
	        if y[nn_index[index_sample]] != target_sample:
	            # 如果该样本和其最近邻样本是一对TomekLik，则将该样本删除
	            if nn_index[nn_index[index_sample]] == index_sample:
	                links[index_sample] = True

	    return links
