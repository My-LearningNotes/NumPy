NumPy ndarray对象
=================

NumPy的核心是N维数组对象\ ``ndarray``\ , 它是一系列相同类型数据的集合. 

    * 可以使用索引访问数组元素, 索引从0开始;
    * 数组的\ **维数(dimensions)**\ 称为\ **秩(rank)**\ , 在NumPy中称为\ **轴(axes)**\ . 

Example:

.. code-block:: python

    # 3D空间中的坐标
    # 一维数组, 所以有1个axes
    # 这个axes有3个元素, 所以它的长度是3
    [1, 2, 1]


    # 二维数组, 所以有2个axes
    # 第一个axes的长度是2
    # 第二个axes的长度是3
    [[1, 0, 0],
     [0, 1, 2]]


``ndarray``\ 对象的一些重要属性:

    * ``ndarray.ndim``

    秩, 即轴的数量或维度的数量.

    * ``ndarray.shape``

    数组的维度(用元组表示), 对于矩阵, 就是指矩阵是几行几列的.

    * ``ndarray.size``

    数组元素的总个数.

    * ``ndarray.dtype``

    数组元素的数据类型.

    * ``ndarray.itemsize``

    每个数组元素的大小, 以字节为单位.

    * ``ndarray.data``

    包含实际数组元素的缓冲区.


Example:

.. code-block:: python

    >>> import numpy as np
    >>> a = np.arange(15).reshape(3, 5)
    >>> a
    array([[ 0,  1,  2,  3,  4],
           [ 5,  6,  7,  8,  9],
           [10, 11, 12, 13, 14]])
    >>> a.ndim
    2
    >>> a.shape
    (3, 5)
    >>> a.dtype.name
    'int64'
    >>> a.size
    15
    >>> a.itemsize
    8
    >>> a.data
    <memory at 0x7f14eabcb120>
    >>> type(a)
    <class 'numpy.ndarray'>
    >>> b = np.array([6, 7, 8])
    >>> b
    array([6, 7, 8])
    >>> type(b)
    <class 'numpy.ndarray'>

