数组操作
========

NumPy中包含了一些函数/方法用于处理数组, 大致可以分为以下几类:

    * 修改数组形状
    * 翻转数组
    * 修改数组维度
    * 连接数组
    * 分割数组
    * 数组元素的添加与删除


修改数组形状
------------

数组中元素的个数(``ndarray.size``)没有改变, 但是数组的形状(``ndarray.shape``)发生了改变.

============= ================================================================
``reshape()`` 修改数组的\ ``shape``\ (只是修改\ ``shape``\ , 不改变\ ``size``)
``flat``      数组元素迭代器
``flatten()`` 返回一份数组拷贝, 对拷贝所做的修改不会影响原始数组
``ravel()``   返回展开的数组
============= ================================================================

``ndarray.reshape()``
^^^^^^^^^^^^^^^^^^^^^

修改数组的\ ``shape``\ , 返回一个新的数组(视图).

修改后的\ ``shape``\ 要和数组的\ ``size``\ 对得上. 
比如\ ``size``\ 是15, 修改后的\ ``shape``\ 可以是(15,), 可以是(3, 5)或(5, 3), 但如果是(4, 4)或(2, 3)就不行.

该方法的参数是一个元组, 表示数组的形状, 如果是一维数组, 还可以就是一个数字.

Example:

.. code-block:: python
    :emphasize-lines: 1, 4, 9

    >>> a = np.arange(12)
    >>> a
    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
    >>> b = a.reshape(3, 4)
    >>> b
    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])
    >>> c = a.reshape(3, 3) # shape和size对不上
    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
    ValueError: cannot reshape array of size 12 into shape (3,

If a dimension is given as -1 in a reshaping operaiton, the other dimensions are automatically calculated:

.. code-block:: python
    :emphasize-lines: 1

    >>> a.reshape((3, -1))
    array([[ 1,  2,  3,  4],
           [ 5,  6,  7,  8],
           [ 9, 10, 11, 12]])
    

``ndarray.flat``
^^^^^^^^^^^^^^^^

对一个数组的迭代, 默认是在第一个轴上迭代. 

Example:

.. code-block:: python
    :emphasize-lines: 2

    >>> a = np.arange(9).reshape(3, 3)
    >>> for row in a:
    ...     print(row)
    ... 
    [0 1 2]
    [3 4 5]
    [6 7 8]

``ndarray.flat``\ 是一个数组元素的迭代器, 可以对它进行迭代从而实现对各个数组元素的迭代.

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


``ndarray.flatten()``
^^^^^^^^^^^^^^^^^^^^^^

返回一个数组元素的拷贝(一维数组).

其实也就是将数组展开(默认按行展开), 返回展开后的一维数组.

Example:

.. code-block:: python
    :emphasize-lines: 4

    >>> a
    array([[0, 1, 2, 3],
           [4, 5, 6, 7]])
    >>> a.flatten()
    array([0, 1, 2, 3, 4, 5, 6, 7])


``ndarray.ravel()``
^^^^^^^^^^^^^^^^^^^^

返回一个展开后的数组, 所谓展开后的数组, 是指将数组中的所有元素按顺序依次展开, 生成一个一维数组. 
默认是按行展开.

**返回的数组是视图, 修改会影响原始数组.**

Example:

.. code-block:: python
    :emphasize-lines: 8

    > a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]])
    >>> a
    array([[ 1,  2,  3],
           [ 4,  5,  6],
           [ 7,  8,  9],
           [10, 11, 12]])

    >>> a.ravel()
    array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12])


翻转数组
--------

=============== ============================
``transpose()`` 转置数组
``ndarray.T``   和\ ``self.transpose``\ 相同
=============== ============================

``numpy.transpose()``
^^^^^^^^^^^^^^^^^^^^^

转置(行列互换), 即把行变为列, 列变为行.

.. code-block:: python
    :emphasize-lines: 6

    >>> a = np.arange(12).reshape(3, 4)
    >>> a
    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])
    >>> np.transpose(a)
    array([[ 0,  4,  8],
           [ 1,  5,  9],
           [ 2,  6, 10],
           [ 3,  7, 11]])

``ndarray.T``
^^^^^^^^^^^^^

也是表示转置数组.

.. code-block:: python
    :emphasize-lines: 1

    >>> a.T
    array([[ 0,  4,  8],
           [ 1,  5,  9],
           [ 2,  6, 10],
           [ 3,  7, 11]])


连接数组
--------

============ ==========================
``hstack()`` 沿水平方向(列方向)连接数组
``vstack()`` 沿垂直方向(行方向)连接数组
============ ==========================

``numpy.hstack()``
^^^^^^^^^^^^^^^^^^

沿水平方向连接数组.

    * 以一个元组定义要连接的数组;
    * 各个数组的行数必须相同, 否则会报错.

Example:

.. code-block:: python
    :emphasize-lines: 3

    >>> a = np.array([[1], [2], [3]])
    >>> b = np.array([[2], [3], [4]])
    >>> np.hstack((a, b))
    array([[1, 2],
           [2, 3],
           [3, 4]])


``numpy.vstack()``
^^^^^^^^^^^^^^^^^^

沿垂直方向连接数组, 用法和\ ``numpy.hstack()``\ 类似.

Example:

.. code-block:: python
    :emphasize-lines: 1

    >>> np.vstack((a, b))
    array([[1],
           [2],
           [3],
           [2],
           [3],
           [4]])


分割数组
--------

Split one array into several smaller ones.

============ ==============================
``hsplit()`` 将一个数组水平分割为多个子数组
``vsplit()`` 将一个数组垂直分割为多个子数组
============ ==============================

``numpy.hsplit()``
^^^^^^^^^^^^^^^^^^

水平分割数组, 格式如下:

.. code-block:: python

    numpy.hsplit(ary, indices_or_sections)

* ``ary``\ : 被分割的数组
* ``indices_or_sections``: 

    - 如果是一个整数N, 表示将数组等分为N个数组, 如果该数组不能等分为N则报错;
    - 如果是一个一维数组, 表示沿轴的方向按数组中的索引切分(左开右闭). 
      比如, 在水平方向切分时, [2, 3]表示切分为以下几个部分:

        + [:2]
        + [2:3]
        + [3:]

Example:

.. code-block:: python
    :emphasize-lines: 7, 15

    >>> x = np.arange(16.0).reshape(4, 4)
    >>> x
    array([[ 0.,  1.,  2.,  3.],
           [ 4.,  5.,  6.,  7.],
           [ 8.,  9., 10., 11.],
           [12., 13., 14., 15.]])
    >>> np.hsplit(x, 2) # 水平方向等分数组
    [array([[ 0.,  1.],
           [ 4.,  5.],
           [ 8.,  9.],
           [12., 13.]]), array([[ 2.,  3.],
           [ 6.,  7.],
           [10., 11.],
           [14., 15.]])]
    >>> np.hsplit(x, np.array([2, 3])) # 通过一个一维数组, 水平方向按切片分割数组
    [array([[ 0.,  1.],
           [ 4.,  5.],
           [ 8.,  9.],
           [12., 13.]]), array([[ 2.],
           [ 6.],
           [10.],
           [14.]]), array([[ 3.],
           [ 7.],
           [11.],
           [15.]])]


``numpy.vsplit()``
^^^^^^^^^^^^^^^^^^^

垂直分割数组, 和\ ``numpy.hsplit()``\ 的用法类似.

Example:

.. code-block:: python
    :emphasize-lines: 6, 10

    >>> x
    array([[ 0.,  1.,  2.,  3.],
           [ 4.,  5.,  6.,  7.],
           [ 8.,  9., 10., 11.],
           [12., 13., 14., 15.]])
    >>> np.vsplit(x, 2) # 垂直方向等分数组
    [array([[0., 1., 2., 3.],
           [4., 5., 6., 7.]]), array([[ 8.,  9., 10., 11.],
           [12., 13., 14., 15.]])]
    >>> np.vsplit(x, np.array([2, 3])) # 通过一个一维数组, 垂直方向按切片分割数组
    [array([[0., 1., 2., 3.],
           [4., 5., 6., 7.]]), array([[ 8.,  9., 10., 11.]]), array([[12., 13., 14., 15.]])]


数组元素的添加与删除
--------------------

============ ======================
``resize()`` 修改数组的\ ``size``
``append()`` 在数组的尾部追加数据
``insert()`` 在数组中插入数据
``delete()`` 删除数据
``unique()`` 去除数组中重复的元素
============ ======================


``ndarray.resize()``
^^^^^^^^^^^^^^^^^^^^

修改\ **当前数组**\ 的\ ``size``\ , 其参数可以是一个数字, 表示数组的\ ``size``\ , 也可以是一个元组, 表示数组的\ ``shape``\ . 
如果新的\ ``size``\ 大于原始的\ ``size``\ , 则包含原始的元素.

Example:

.. code-block:: python
    :emphasize-lines: 9, 14

    >>> a = np.array([[ 1,  2,  3],
                      [ 4,  5,  6],
                      [ 7,  8,  9],
                      [10, 11, 12]])
    >>> a.shape
    (4, 3)
    >>> a.size
    12
    >>> a.resize(10) # 重新定义size
    >>> a
    array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10])
    >>> a.size
    10
    >>> a.resize((2, 3)) # 通过shape重新定义size
    >>> a
    array([[1, 2, 3],
           [4, 5, 6]])
    >>> a.size
    6
    >>> a.shape
    (2, 3)

.. note::

    ``reshape()``\ 和\ ``resize()``\ 的区别:

        * ``reshape()``\ 修改数组的\ ``shape``, 而数组的\ ``size``\ 不变, 要求\ ``shape``\ 和\ ``size``\ 能够对得上.
          ``resize()``\ 修改的是数组的\ ``size``;

        * ``reshape()``\ 返回一个视图(浅复制), 而\ ``resize()``\ 则是对原数组的修改.


``ndarray.append()``
--------------------

.. code-block:: python

    numpy.append(arr, values, axis = None)

在当前数组的尾部追加数据, 返回一个新的数组.

    * ``arr``: 输入数组;
    * ``values``: 要追加的值, 如果有多个, 用一个列表/元组表示;
    * ``axis``: 指定沿哪个轴追加数据, 如果没有指定, 则默认将当前数组展开为一维数组.

      对于二维数组:

         - ``axis = 0``: 追加行(列数要和当前数组相同);
         - ``axis = 1``: 追加列(行数要和当前数组相同).

Example:

.. code-block:: python
    :emphasize-lines: 5, 6, 8, 9, 14, 15

    >>> a = np.arange(6).reshape(2, 3)
    >>> a
    array([[0, 1, 2],
           [3, 4, 5]])
    # 没有指定axis, 将当前数组展开为一维数组, 追加的数据values也展开为一维数组
    >>> np.append(a, [[6, 7, 8], [9, 10, 11]])
    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
    # axis = 0, 追加行, values为一个列表, 每个元素为一行, 追加为新的行
    >>> np.append(a, [[6, 7, 8], [9, 10, 11]], axis = 0)
    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 9, 10, 11]])
    # axis = 1, 追加列, values为一个列表, 每个元素为一行, 依次追加到当前行的尾部
    >>> np.append(a, [[6, 7, 8], [9, 10, 11]], axis = 1)
    array([[ 0,  1,  2,  6,  7,  8],
           [ 3,  4,  5,  9, 10, 11]])


``numpy.insert()``
------------------

.. code-block:: python

    numpy.insert(arr, index, values, axis = None)

在指定索引位置插值, 返回一个新的数组.

    * ``arr``: 输入数组;
    * ``index``: 要插值的索引位置;
    * ``values``: 要插入的值, 如果要同时插入多个值, 用一个列表/元组定义;
    * ``axis``: 指定沿哪个轴插值, 如果没有指定则数组会被展开为一维数组.

      对于二维数组:

        - ``axis = 0``: 插入行(列数要和原始数组相同)
        - ``axis = 1``: 插入列(行数要和原始数组相同)

Example:

.. code-block:: python
    :emphasize-lines: 6, 7, 9, 10, 16, 17

    >>> a = np.arange(6).reshape(3, 2)
    >>> a
    array([[0, 1],
           [2, 3],
           [4, 5]])
    # 没有定义axis, 默认将当前数组展开为一维数组
    >>> np.insert(a, 3, [100, 200])
    array([  0,   1,   2, 100, 200,   3,   4,   5])
    # axis = 0, 行插入, values为一列表, 每个元素是一行
    >>> np.insert(a, 1, [[100, 200], [300, 400]], axis = 0)
    array([[  0,   1],
           [100, 200],
           [300, 400],
           [  2,   3],
           [  4,   5]])
    # axis = 1, 列插入, values为一列表, 每个元素为一列
    >>> np.insert(a, 1, [[100, 200, 300], [1000, 2000, 3000]], axis = 1)
    array([[   0,  100, 1000,    1],
           [   2,  200, 2000,    3],
           [   4,  300, 3000,    5]])

如果插入行或者列时, ``values``\ 只是一个数值, 表示插入一行或一列, 该行或列的元素都是这个数值.

Example:

.. code-block:: python
    :emphasize-lines: 5, 10

    >>> a
    array([[0, 1],
           [2, 3],
           [4, 5]])
    >>> np.insert(a, 1, 100, axis = 0) # 插入一行, 该行的元素都是100
    array([[  0,   1],
           [100, 100],
           [  2,   3],
           [  4,   5]])
    >>> np.insert(a, 1, 100, axis = 1) # 插入一列, 该列的元素都是100
    array([[  0, 100,   1],
           [  2, 100,   3],
           [  4, 100,   5]])


``numpy.delete()``
------------------

.. code-block:: python

    numpy.delete(arr, index, axis = None)

删除数组中指定的元素, 返回一个新的数组.

    * ``arr``: 输入数组;
    * ``index``: 指定要删除的索引, 可以是整数, 整数数组或切片;
    * ``axis``: 指定沿着哪个轴删除, 如果没有指定则输入数组会被展开为一维数组.
        
        对于二维数组:

            - ``axis = 0``: 按行删除
            - ``axis = 1``: 按列删除

Example:

.. code-block:: python
    :emphasize-lines: 6, 8, 11, 15, 19

    >>> a = np.arange(12).reshape(3, 4)
    >>> a
    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])
    >>> np.delete(a, 5) # 没有指定axis, 展开为一维数组后再按索引删除
    array([ 0,  1,  2,  3,  4,  6,  7,  8,  9, 10, 11])
    >>> np.delete(a, 1, axis = 0) # axis = 0, 删除指定的行
    array([[ 0,  1,  2,  3],
           [ 8,  9, 10, 11]])
    >>> np.delete(a, 2, axis = 1) # axis = 1, 删除指定的列
    array([[ 0,  1,  3],
           [ 4,  5,  7],
           [ 8,  9, 11]])
    >>> np.delete(a, [2, 3], axis = 1) # 用一个整数数组指定要删除的元素
    array([[0, 1],
           [4, 5],
           [8, 9]])
    >>> np.delete(a, np.s_[::2], axis = 1) # 用一个切片指定要删除的元素
    array([[ 1,  3],
           [ 5,  7],
           [ 9, 11]])


``numpy.unique()``
------------------

.. code-block:: python

    numpy.unique(arr, return_index = False, return_inverse = False, return_counts = False)

去除数组中重复的元素, 返回一个新的数组.

    * ``arr``: 输入数组, 如果不是一维数组则会先展开;
    * ``return_index``: 返回新数组中的元素在旧数组中的索引, 并以一维数组的形式存储;
    * ``return_inverse``: 返回旧数组中的元素在新数组中的索引, 并以一维数组的形式存储;
    * ``return_counts``: 返回新数组中的每个元素在旧数组中出现的次数, 并以一维数组的形式存储.

Example:

.. code-block:: python
    :emphasize-lines: 4, 6, 8, 10

    >>> a = np.array([5,2,6,2,7,5,6,8,2,9])
    >>> a
    array([5, 2, 6, 2, 7, 5, 6, 8, 2, 9])
    >>> np.unique(a)
    array([2, 5, 6, 7, 8, 9])
    >>> np.unique(a, return_index = True)
    (array([2, 5, 6, 7, 8, 9]), array([1, 0, 2, 4, 7, 9]))
    >>> np.unique(a, return_inverse = True)
    (array([2, 5, 6, 7, 8, 9]), array([1, 0, 2, 0, 3, 1, 2, 4, 0, 5]))
    >>> np.unique(a, return_counts = True)
    (array([2, 5, 6, 7, 8, 9]), array([3, 2, 2, 1, 1, 1]))
    
