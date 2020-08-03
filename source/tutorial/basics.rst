The Basics
==========

``ndarray``
-----------

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

.. note::

    对于一个ndarray对象, 底层有一个数据存储区用来存储数据, 可以把该存储区看做一个一维数组, 其中的一个元素就是一个基本类型的数据, ``size``\ 就是元素的个数.

    ``shape``\ 数组的形状, 决定了如何组织存储区中的基本数据, 构建数组.


NumPy数据类型
-------------

NumPy支持的数据类型比Python内置的类型要多很多, 基本上可以和C语言的数据类型对应上, 其中部分类型对应Python的内置类型.

`NumPy Data types <https://numpy.org/doc/1.19/user/basics.types.html>`_
