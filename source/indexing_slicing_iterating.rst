索引, 切片和迭代
================

``ndarray``\ 对象的内容可以通过索引或切片来访问和修改, 与Python中对列表的操作一样.


一维数组
--------

一维数组的索引和切片操作, 与Python列表的索引和切片操作类似.

Example:

.. code-block:: python
    :emphasize-lines: 5, 7, 9, 11, 14

    >>> import numpy as np
    >>> a = np.arange(10)**3
    >>> a
    array([  0,   1,   8,  27,  64, 125, 216, 343, 512, 729])
    >>> a[2] # 索引
    8
    >>> a[-1] # 负值索引
    729
    >>> a[2:5] # 切片
    array([ 8, 27, 64])
    >>> a[:6:2] = 1000 # 切片赋值
    >>> a
    array([1000,    1, 1000,   27, 1000,  125,  216,  343,  512,  729])
    >>> a[::-1]
    array([ 729,  512,  343,  216,  125, 1000,   27, 1000,    1, 1000])


多维数组
--------

* 多维数组的每个轴都有一个索引, 以逗号分隔的元组的形式给出各个轴的索引;
* 多维数组有多个轴, 每个轴都可以使用一个切片表示一组索引.

Example:

.. code-block:: python
    :emphasize-lines: 11, 13, 15, 17, 20, 24

    >>> def f(x, y):
    ...     return 10*x + y
    ... 
    >>> b = np.fromfunction(f, (5, 4), dtype=int)
    >>> b
    array([[ 0,  1,  2,  3],
           [10, 11, 12, 13],
           [20, 21, 22, 23],
           [30, 31, 32, 33],
           [40, 41, 42, 43]])
    >>> b[2, 3] # 多维数组的索引
    23
    >>> b[0:5, 1] # 切片
    array([ 1, 11, 21, 31, 41])
    >>> b[:, 1]
    array([ 1, 11, 21, 31, 41])
    >>> b[1:3, :]
    array([[10, 11, 12, 13],
           [20, 21, 22, 23]])
    >>> b[::2, ::2]
    array([[ 0,  2],
           [20, 22],
           [40, 42]])
    >>> b[-1]
    array([40, 41, 42, 43])

在切片中, 还可以使用\ ``...``\ . 
The **dots(...)** represents as many colons as needed to produce a complete indexing tuple. 

.. note::

    ``...``\ 表示若干的\ ``:``\ , 用来构建一个完整的索引.

For example, if ``x`` is an array  with 5 axes, then 

    * ``x[1, 2, ...]`` is equivalent to ``x[1, 2, :, :, :]``
    * ``x[..., 3]`` to ``x[:, :, :, :, 3]``
    * ``x[4, ..., 5]`` to ``x[4, :, :, :, 5]``

Exmaple:

.. code-block:: python
    :emphasize-lines: 7, 10

    >>> c = np.array([[[0, 1, 2], 
    ...                [10, 12, 13]],
    ...                [[100, 101, 102],
    ...                 [110, 112, 113]]])
    >>> c.shape
    (2, 2, 3)
    >>> c[1,...]
    array([[100, 101, 102],
           [110, 112, 113]])
    >>> c[..., 2]
    array([[  2,  13],
           [102, 113]])


高级索引
--------

NumPy比一般的Python序列提供了更多的索引方式. 
除了之前看到的用整数和切片的索引外, 数组还可以有整数数组索引, 布尔索引以及花式索引.


整数数组索引
^^^^^^^^^^^^

* **整数索引**\ : 索引是一个整数值, 表示一个元素;
* **整数数组索引**\ : 索引是一个整数数组, 表示若干个元素.

.. note::

    整数数组索引, 表示若干个元素. 
    将所有元素的第一轴的索引放在第一个数组, 第二轴的索引放在第二个数组, ...

Example:

.. code-block:: python
    :emphasize-lines: 4

    >>> a = np.arange(10, 20)
    >>> a
    array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19])
    >>> a[[0, 2, 4]] # 用整数数组索引表示若干个数组元素组成的一个新数组
    array([10, 12, 14])

如果是多维数组, 每个轴的索引分别用一个数组表示.

以一个二维数组为例:

.. code-block:: python
    :emphasize-lines: 6

    >>> a = np.array([[1, 2], [3, 4], [5, 6]])
    >>> a
    array([[1, 2],
           [3, 4],
           [5, 6]])
    >>> a[[0, 1, 2], [0, 1, 0]] # 二维数组的整数数组索引, 表示(0, 0), (1, 1), (2, 0)位置的元素
    array([1, 4, 5])


在整数数组索引中, 如果某个数组是以一个整数值的形式给出, 则表示以这个值扩展为数组.

Example:

.. code-block:: python
    :emphasize-lines: 1, 3

    >>> a[[0, 1, 2], 0] # 表示(0, 0), (1, 0), (2, 0)三个元素
    array([1, 3, 5])
    >>> a[1, [0, 1]] # 表示(1, 0), (1, 1)两个元素
    array([3, 4])
    

布尔索引
^^^^^^^^

布尔索引, 通过布尔运算符(如: 比较运算符)来获取符合指定条件的元素组成的新数组.

例如, 获取大于5的元素:

.. code-block:: python
    :emphasize-lines: 8

    >>> import numpy as np
    >>> x = np.arange(12).reshape(4, 3)
    >>> x
    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])
    >>> x[x > 5] # 大于5的元素
    array([ 6,  7,  8,  9, 10, 11])

以下实例使用了\ ``~``\ (取补运算符)来过滤\ ``NaN``\ :

.. code-block:: python
    :emphasize-lines: 4

    >>> a = np.array([np.nan, 1, 2, np.nan, 3, 4, 5])
    >>> a
    array([nan,  1.,  2., nan,  3.,  4.,  5.])
    >>> a[~np.isnan(a)]
    array([1., 2., 3., 4., 5.])

以下实例演示如何从数组中过滤掉非复数元素:

.. code-block:: python
    :emphasize-lines: 2

    >>> a = np.array([1, 2+6j, 5, 3.5+5j])
    >>> a[np.iscomplex(a)]
    array([2. +6.j, 3.5+5.j])


花式索引
^^^^^^^^

花式索引指的是利用整数数组进行索引. 

花式索引根据索引数组的值作为目标数组的某个轴的下标来取值. 
对于使用一维整数数组作为索引, 如果目标是一维数组, 那么索引的结果就是对应位置的元素; 
如果目标是二维数组, 那么就是对应下标的行.

花式索引跟切片不一样, 它总是将数据复制到新数组中.

* 传入顺序索引数组

.. code-block:: python
    :emphasize-lines: 11

    >>> x = np.arange(32).reshape(8, 4)
    >>> x
    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11],
           [12, 13, 14, 15],
           [16, 17, 18, 19],
           [20, 21, 22, 23],
           [24, 25, 26, 27],
           [28, 29, 30, 31]])
    >>> x[[4, 2, 1, 7]] # 第4, 2, 1, 7行
    array([[16, 17, 18, 19],
           [ 8,  9, 10, 11],
           [ 4,  5,  6,  7],
           [28, 29, 30, 31]])

* 传入倒序索引数组

.. code-block:: python
    :emphasize-lines: 1

    >>> x[[-4, -2, -1, -7]]
    array([[16, 17, 18, 19],
           [24, 25, 26, 27],
           [28, 29, 30, 31],
           [ 4,  5,  6,  7]])

* 传入多个索引数组(要使用\ ``np.ix_``)

.. code-block:: python
    :emphasize-lines: 1

    >>> x[np.ix_([1, 5, 7, 2], [0, 3, 1, 2])]
    array([[ 4,  7,  5,  6],
           [20, 23, 21, 22],
           [28, 31, 29, 30],
           [ 8, 11,  9, 10]])

使用\ ``np.ix_``\ 传入两个索引数组时, 第一个表示行, 第二个表示列, 依次组合, 得到新数组M * N个元素.


迭代
----

对数组的迭代, 默认是在the first axes上迭代.

Example:

.. code-block:: python
    :emphasize-lines: 8

    >>> import numpy as np
    >>> a = np.arange(20).reshape(4, 5)
    >>> a
    array([[ 0,  1,  2,  3,  4],
           [ 5,  6,  7,  8,  9],
           [10, 11, 12, 13, 14],
           [15, 16, 17, 18, 19]])
    >>> for row in a:
    ...     print(row)
    ... 
    [0 1 2 3 4]
    [5 6 7 8 9]
    [10 11 12 13 14]
    [15 16 17 18 19]

如果想逐个元素迭代, 可以对数组的\ ``flat``\ 属性进行迭代.

Example:

.. code-block:: python
    :emphasize-lines: 1

    >>> for element in a.flat:
    ...     print(element)
    ... 
    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19


``numpy.nditer``
^^^^^^^^^^^^^^^^

NumPy的迭代器对象\ ``numpy.nditer``\  提供了一种灵活访问一个或者多个数组元素的方式.

迭代器最基本的任务就是完成对数组元素的访问. 

Example:

.. code-block:: python
    :emphasize-lines: 6

    >>> import numpy as np
    >>> a = np.arange(6).reshape(2, 3)
    >>> a
    array([[0, 1, 2],
           [3, 4, 5]])
    >>> for element in np.nditer(a):
    ...     print(element, end = ',')
    ...
    0,1,2,3,4,5,

默认的迭代顺序是和数组的内存布局一致的, 这样做是为了提升访问的效率. 
我们可以通过迭代上述数组的转置来看到这一点:

.. code-block:: python
    :emphasize-lines: 1

    >>> for element in np.nditer(a.T):
    ...     print(element, end = ',')
    ...
    0,1,2,3,4,5,

``a.T``\ 返回的是a的转置数组, 是一个视图, 底层的数据和内存布局没有改变, 所以迭代的数据也没有改变.

.. note::

    ``numpy.nditer``\ 的迭代, 是对\ ``ndarray``\ 底层的数据存储区, 即\ ``ndarray.data``\ 的迭代.


控制遍历顺序
++++++++++++

* ``for x in np.nditer(a, order = 'F')``\ : Fortan order, 即列优先;
* ``for x in np.nditer(a. order = 'C')``\ : C order, 即是行优先.

Example:

.. code-block:: python
    :emphasize-lines: 9, 15, 26

    >>> a = np.arange(0, 60, 5)
    >>> a
    array([ 0,  5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55])
    >>> a = a.reshape(3, 4)
    >>> a
    array([[ 0,  5, 10, 15],
           [20, 25, 30, 35],
           [40, 45, 50, 55]])
    >>> b = a.T # 转置数组
    >>> b
    array([[ 0, 20, 40],
           [ 5, 25, 45],
           [10, 30, 50],
           [15, 35, 55]])
    >>> c = b.copy(order = 'C') # 行优先, 创建一个副本
    >>> c
    array([[ 0, 20, 40],
           [ 5, 25, 45],
           [10, 30, 50],
           [15, 35, 55]])
    >>> for element in np.nditer(c):
    ...     print(element, end = ',')
    ... 
    0,20,40,5,25,45,10,30,50,15,35,55,
    >>> 
    >>> d = b.copy(order = 'F') # 列优先, 创建一个副本
    >>> for element in np.nditer(d):
    ...     print(element, end = ',')
    ... 
    0,5,10,15,20,25,30,35,40,45,50,55,

可以通过显式设置, 指定\ ``nditer``\ 对象使用某种迭代顺序:

.. code-block:: python
    :emphasize-lines: 1, 5

    >>> for element in np.nditer(a, order = 'C'):
    ...     print(element, end = ',')
    ... 
    0,5,10,15,20,25,30,35,40,45,50,55,
    >>> for element in np.nditer(a, order = 'F'):
    ...     print(element, end = ',')
    ... 
    0,20,40,5,25,45,10,30,50,15,35,55,


修改数组中元素的值
++++++++++++++++++

``nditer``\ 对象有另一个可选参数\ ``op_flags``\ . 

默认情况下, ``nditer``\ 将数组视为只读的(``readonly``), 为了在遍历数组的同时, 实现对数据元素的修改, 必须指定为\ ``readwrite``\ 或\ ``writeonly``\ 模式.

Example:

.. code-block:: python
    :emphasize-lines: 6

    >>> a = np.arange(0, 60, 5).reshape(3, 4)
    >>> a
    array([[ 0,  5, 10, 15],
           [20, 25, 30, 35],
           [40, 45, 50, 55]])
    >>> for element in np.nditer(a, op_flags = ['readwrite']):
    ...     element *= 10
    ... 
    >>> a
    array([[  0,  50, 100, 150],
           [200, 250, 300, 350],
           [400, 450, 500, 550]])


广播数组
++++++++

如果两个数组是可广播的, ``nditer``\ 组合对象能够同时迭代它们. 

例如, 数组a的维度为3*4, 数组b的维度为1*4, 同时迭代a和b:

.. code-block:: python
    :emphasize-lines: 10

    >>> a = np.arange(0, 60, 5)
    >>> a = np.arange(0, 60, 5).reshape(3, 4)
    >>> a
    array([[ 0,  5, 10, 15],
           [20, 25, 30, 35],
           [40, 45, 50, 55]])
    >>> b = np.array([1, 2, 3, 4])
    >>> b
    array([1, 2, 3, 4])
    >>> for x, y in np.nditer([a, b]):
    ...     print(f'{x}:{y}', end = ', ')
    ... 
    0:1, 5:2, 10:3, 15:4, 20:1, 25:2, 30:3, 35:4, 40:1, 45:2, 50:3, 55:4,

