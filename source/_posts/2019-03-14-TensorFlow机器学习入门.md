---
layout: post
title: TensorFlow 机器学习入门
subtitle:   TensorFlow 基础
date:       2019-03-14
tags:
  - TensorFlow
---


## 什么是TenseFlow
[TensorFlow](https://github.com/tensorflow/tensorflow) 是一个开源的机器学习库，在github的机器学习项目中高居第一位。

**TF提供了非常多的机器学习API**

##Tense 表示张量

特征向量是机器学习中最有用的概念之一，因为它们的简单性（只是一个数字列表）。每个数据项通常由一个特征向量组成，而一个好的数据集有成百上千个这样的特征向量。毫无疑问，你经常会同时处理多个向量。一个矩阵可以简洁地表示一个向量列表，其中矩阵的每一列都是一个特征向量。

>张量是矩阵的一种泛化，可以用任意数量的索引来指定一个元素。

张量是更多层的嵌套向量。例如，2×3×2 的张量是 ```[[1, 2], [3, 4], [5, 6]], [[7, 8], [9, 10], [ 11, 12]]]```，它可以被认为是两个大小为 3×2 的矩阵构成。因此，我们说这个张量的秩为 3。一般来说，张量的秩是指定一个元素所需的索引的数量。TensorFlow 中的机器学习算法作用于张量，因此真正理解如何使用它们非常重要.

下面的代码片段试图用三种方法表示相同的 2×3 矩阵。该矩阵表示两个维度的两个特征向量。

```python
import tensorflow as tf
import numpy as np 

m1 = [[1.0, 2.0],
    [3.0, 4.0]] 

m2 = np.array([[1.0, 2.0],
    [3.0, 4.0]], dtype=np.float32) 

m3 = tf.constant([[1.0, 2.0],
    [3.0, 4.0]]) 

print(type(m1)) 
print(type(m2)) 
print(type(m3)) 

t1 = tf.convert_to_tensor(m1, dtype=tf.float32) 
t2 = tf.convert_to_tensor(m2, dtype=tf.float32) 
t3 = tf.convert_to_tensor(m3, dtype=tf.float32) 

print(type(t1)) 
print(type(t2)) 
print(type(t3)) 

```


第一个变量（m1）是一个列表，第二个变量（m2）是 NumPy 中的 ndarray，最后一个变量（m3）是我们使用 tf.constant 初始化的 TensorFlow 常量 Tensor 对象。

**TensorFlow 中的所有运算符（例如负数）都是为张量对象而设计的**

tf.convert_to_tensor(...) 是一个方便的函数，能够确保我们处理张量而不是其他类型。实际上，即使忘记了使用，TensorFlow 中的大多数函数也已经（冗余地）执行了此函数。使用 tf.convert_to_tensor(...) 是可选的，在这里展示它有助于揭开整个库处理隐式类型系统的神秘面纱。之前的代码片段输出以下结果三次：

```
<class 'tensorflow.python.framework.ops.Tensor'>
```



