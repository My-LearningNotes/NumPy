索引, 切片和迭代
================

``ndarray``\ 对象的内容可以通过索引或切片来访问和修改, 与Python中list的操作一样.


一维数组
--------

一维数组的索引和切片操作, 与Python列表的索引和切片操作类似.

Example:

.. code-block:: python
    :emphasize-lines: 5, 7, 9, 11

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

