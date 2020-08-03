创建数组
========

There are several ways to create arrays. 

``numpy.array()``
-----------------

You can create an array from a regular Python list or tuple using the ``array`` function. 
The type of the resulting array is deduced from the type of the elements in the sequence.

Example:

.. code-block:: python

    >>> import numpy as np
    >>> a = np.array([1, 2, 3])
    >>> a
    array([1, 2, 3])
    >>> a.dtype
    dtype('int64')
    >>> b = np.array([1.2, 2.3, 3.4])
    >>> b
    array([1.2, 2.3, 3.4])
    >>> b.dtype
    dtype('float64')

如果指定的列表或元组是多维的, 则创建的数组也是多维的:

.. code-block:: python

    >>> c = np.array([(1.5, 2, 3), (4, 5, 6)])
    >>> c
    array([[1.5, 2. , 3. ],
           [4. , 5. , 6. ]])

还可以通过\ ``dtype``\ 参数指定数组元素的类型:

.. code-block:: python

    >>> d = np.array([[1, 2], [3, 4]], dtype = complex)
    >>> d
    array([[1.+0.j, 2.+0.j],
           [3.+0.j, 4.+0.j]])
    >>> d.dtype
    dtype('complex128')


Often, the elements of an array are unknown, but its size is known. 
Hence, NumPy offers several functions to create arrays with initial placeholder content. 
These minimize the necessity of growing arrays, an expensive operation.

The function ``zeros`` creates an array full of zeros, the function ``ones`` creates an array full of ones, 
and the function ``empty`` creates an array whose initial content is random and depends on the state of the memory. 
By default, the ``dtype`` of the created array is ``float64``\ .


``numpy.empty()``
-----------------

``numpy.empty()``\ 方法用来创建以一个指定形状(``shape``)和数据类型(``dtypt``)且未初始化的数组:

.. code-block:: python

    numpy.empty(shape, dtype=float)

``shape``\ 是一个列表或元组, 表示数组的维度; 
如果是一维数组, 也可以只用一个数字来表示.

.. attention::

    因为数组没有初始化, 所以数组元素都是随机值.


``numpy.zeros()``
-----------------

创建指定大小的数组, 数组元素都是0来填充:

.. code-block:: python

    numpy.zeros(shape, dtype=float)


``numpy.ones()``
----------------

创建指定形状的数组, 数组元素都以1来填充:

.. code-block:: python

    numpy.ones(shape, dtype=float)


根据数值范围创建数组
--------------------

To create sequences of numbers, NumPy provides the ``arange`` function which is analogous to the Python built-in ``range``\ , 
but returns an array.

``numpy.arange()``
^^^^^^^^^^^^^^^^^^

根据指定的数值范围创建数组:

.. code-block:: python

    numpy.arange(start, stop, step, dtype)

``start``\ , ``stop``\ 和\ ``step``\ 的含义和Python中的\ ``range()``\ 函数类似.

========= ============================================
``start`` 起始值, 默认为0
``stop``  终止值(不包含)
``step``  步长, 默认为1
``dtype`` 数据类型, 如果没有提供, 则使用输入数据的类型
========= ============================================

Example:

.. code-block:: python

    >>> np.arange(10, 30, 5)
    array([10, 15, 20, 25])
    >>> np.arange(0, 2, 0.3) # it accepts float arguments
    array([0. , 0.3, 0.6, 0.9, 1.2, 1.5, 1.8])


``numpy.linspace()``
^^^^^^^^^^^^^^^^^^^^

``numpy.linspace``\ 函数用于创建一个一维数组, 数组是一个等差数列构成的:

.. code-block:: python

    numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)

============= =======================================================
``start``     序列的起始值
``stop``      序列的终止值, 如果endpoint为True, 该值包含在数组中
``num``       要生成的等步长的样本数量, 默认为50
``endpoint``  该值为True时, 数组中包含stop值, 反之不包含, 默认是True
``retstep``   如果为True时, 生成的数组中会显示间距, 反之不显示
``dtype``     数据类型
============= =======================================================

Example:

.. code-block:: python

    >>> import numpy as np
    >>> a = np.linspace(1, 10, 10)
    >>> a
    array([ 1.,  2.,  3.,  4.,  5.,  6.,  7.,  8.,  9., 10.])
    >>> b = np.linspace(1, 1, 10)
    >>> b
    array([1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])
    >>> c = np.linspace(10, 20, 5, endpoint = False)
    >>> c
    array([10., 12., 14., 16., 18.])
    >>> d = np.linspace(1, 10, 10, retstep = True)
    >>> d
    (array([ 1.,  2.,  3.,  4.,  5.,  6.,  7.,  8.,  9., 10.]), 1.0)


``numpy.logspace()``
^^^^^^^^^^^^^^^^^^^^

``numpy.logspace()``\ 函数用于创建一个等比例数组:

.. code-block:: python

    numpy.logspace(start, stop, num=50, endpoint=True, base=10.0, dtype=None)

============= ====================================================================
``start``     序列的起始值为: base ** start
``stop``      序列的终止值为: base ** stop, 如果endpoint为True, 该值包含于数组中
``num``       要生成的样本数量, 默认为50
``endpoint``  该值为True时, 数组中包含stop值, 否则不包含
``base``      基数
``dtype``     数据类型
============= ====================================================================

Example:

.. code-block:: python
    :emphasize-lines: 2

    >>> import numpy as np
    >>> a = np.logspace(1.0, 2.0, num = 10)
    >>> a
    array([ 10.        ,  12.91549665,  16.68100537,  21.5443469 ,
            27.82559402,  35.93813664,  46.41588834,  59.94842503,
            77.42636827, 100.        ])

将基数设置为2:

.. code-block:: python
    :emphasize-lines: 1

    >>> a = np.logspace(0, 9, 10, base = 2)
    >>> a
    array([  1.,   2.,   4.,   8.,  16.,  32.,  64., 128., 256., 512.])

