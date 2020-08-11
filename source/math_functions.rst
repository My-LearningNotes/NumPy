NumPy数学函数
=============

NumPy包含大量的各种数学运算的函数, 包括三角函数, 算术运算的函数, 复数处理函数等.


三角函数
--------

NumPy提供了标准的三角函数: ``sin()``, ``cos()``, ``tan()``\ , ``arcsin()``, ``arccos()``\ 和\ ``arctan()``\ 是对应的反三角函数.

三角函数的参数是\ **弧度**\, 反三角函数返回的也是弧度. 
可以通过\ ``numpy.degrees()``\ 函数将弧度转换为角度, 可以通过\ ``np.deg2rad()``\ 函数将角度转换为弧度.

Example:

.. code-block:: python
    :emphasize-lines: 4, 7, 10

    >>> import numpy as np
    >>> a = np.array([0, 30, 45, 60, 90])
    # 通过乘以 pi/180 将角度转换为弧度
    >>> np.sin(a * np.pi / 180)
    array([0.        , 0.5       , 0.70710678, 0.8660254 , 1.        ])
    # 通过np.deg2rad()函数, 将角度转换为弧度
    >>> np.cos(np.deg2rad(a))
    array([1.00000000e+00, 8.66025404e-01, 7.07106781e-01, 5.00000000e-01,
           6.12323400e-17])
    >>> np.tan(np.deg2rad(a))
    array([0.00000000e+00, 5.77350269e-01, 1.00000000e+00, 1.73205081e+00,
           1.63312394e+16])


舍入函数
--------

``numpy.around()`` - 四舍五入
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    numpy.around(a, decimals)

参数说明:
   
    * a: 数组
    * decimals: 舍入的小数位数. 默认为0, 如果为负, 整数将四舍五入到小数点左侧的位置.

.. note::

    小数点后的各个小数位的索引为: 0, 1, 2, ..., 小数点左侧各个整数位的索引为: -1, -2, -3, ...., 
    ``decimals``\ 表示将那一位四舍五入.

Example:

.. code-block:: python
    :emphasize-lines: 5, 7, 9

    >>> import numpy as np
    >>> a = np.array([1.0, 5.55, 123, 0.567, 25.532])
    >>> a
    array([  1.   ,   5.55 , 123.   ,   0.567,  25.532])
    >>> np.around(a)
    array([  1.,   6., 123.,   1.,  26.])
    >>> np.around(a, 1)
    array([  1. ,   5.6, 123. ,   0.6,  25.5])
    >>> np.around(a, -1)
    array([  0.,  10., 120.,   0.,  30.])

``numpy.floor()`` - 向下取整
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

返回小于或者等于指定表达式的最大整数.

Example:

.. code-block:: python
    :emphasize-lines: 5

    >>> import numpy as np
    >>> a = np.array([-1.7, 1.5, -0.2, 0.6, 10])
    >>> a
    array([-1.7,  1.5, -0.2,  0.6, 10. ])
    >>> np.floor(a)
    array([-2.,  1., -1.,  0., 10.])


``numpy.ceil()`` - 向上取整
^^^^^^^^^^^^^^^^^^^^^^^^^^^

返回大于或者等于指定表达式的最小整数.

Example:

.. code-block:: python  
    :emphasize-lines: 5

    >>> import numpy as np
    >>> a = np.array([-1.7, 1.5, -0.2, 0.6, 10])
    >>> a
    array([-1.7,  1.5, -0.2,  0.6, 10. ])
    >>> np.ceil(a)
    array([-1.,  2., -0.,  1., 10.])

