引用, 副本和视图
================

* **引用**, 就是别名;
* **副本**\ 是一个对象的完整拷贝(深拷贝), 如果我们对副本进行修改, 它不会影响到原始数据;
* **视图**\ 是一种浅拷贝, 它是一个新的对象, 但它的实际数据部分和原始对象是共享的, 即是内存中的同一块数据, 所以视图中对数据的修改会影响原始对象.

.. note::

    视图, 也是一个\ ``ndarray``\ 类型的对象, 只不过它是原始对象的"影子", 要从原始对象创建而来.

    对于一个\ ``ndarray``\ 对象, 可以通过\ ``ndarray.base``\ 属性查看其是否是视图, 以及它的原始数据. 
    如果\ ``ndarray.base``\ 为空, 表示该数组对象不是视图. 

    NumPy文档中对\ ``ndarray.base``\ 属性的解释:

        If the array is a view into another array, that array is its 'base'(Unless that array is also a view). 
        The 'base' array is where the array data is actually stored.

    ``ndarray.base``\ 就是浅复制时的原始数据.


视图一般发生在:

    * numpy的切片操作返回原始数据的视图;
    * 调用\ ``ndarray``\ 的\ ``view()``\ 方法产生一个视图.

副本一般发生在:

    * 调用\ ``ndarray``\ 的\ ``copy()``\ 方法产生一个副本.


When operating and manipulating arrays, their data is sometimes copied into a new array and sometimes not.
There are three cases:

Not Copy at all
---------------

Simple assignments make no copy of objects or their data.

简单的赋值操作, 即把一个数组赋值给另一个数组, 属于引用传递, 不会复制数据.

.. code-block:: python

    >>> a = np.array([[0, 1, 2, 3], 
    ...               [4, 5, 6, 7],
    ...               [8, 9, 10, 11]])
    >>> b = a
    >>> b is a
    True

把数组作为函数的参数传递时, 也是引用传递.

.. code-block:: python

    >>> def f(x):
    ...     print(id(x))
    ... 
    >>> id(a)
    140570502274704
    >>> f(a)
    140570502274704


View or Shallow Copy
--------------------

视图, 就是一种浅拷贝. 
可以通过\ ``ndarray.view()``\ 方法创建一个视图, 它是一个新的\ ``ndarray``\ 对象, 但是它和原始对象共享底层的数据.

.. code-block:: python

    >>> c = a.view() # 创建一个视图a
    >>> c is a
    False
    >>> c.base is a # c is a view of the data owned by a
    True
    >>> c.flags.owndata
    False
    >>> a.base
    >>> c.base
    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])
    >>> c.resize((2, 6)) # a's shape doesn't change
    >>> a.shape
    (3, 4)
    >>> c.shape
    (2, 6)
    >>> c[0, 4] = 1234 # a's data changes
    >>> c
    array([[   0,    1,    2,    3, 1234,    5],
           [   6,    7,    8,    9,   10,   11]])
    >>> a
    array([[   0,    1,    2,    3],
           [1234,    5,    6,    7],
           [   8,    9,   10,   11]])

注意, ``ndarray``\ 的切片返回的是一个视图(浅复制), 而Python中序列的切片是深复制, 返回的是一个副本.

.. code-block:: python

    >>> s = a[:, 1:3] # 切片
    >>> s.base is a
    True
    >>> s
    array([[ 1,  2],
           [ 5,  6],
           [ 9, 10]])
    >>> s[:] = 10
    >>> s
    array([[10, 10],
           [10, 10],
           [10, 10]])
    >>> a
    array([[   0,   10,   10,    3],
           [1234,   10,   10,    7],
           [   8,   10,   10,   11]])

对于一个\ ``ndarray``\ 对象, 如果\ ``ndarray.base``\ 属性为\ ``None``\ , 表示这是一个原始数组, 否则它就是一个视图.

.. code-block:: python

    # The base of an array that owns its memory is None:
    >>> x = np.array([1, 2, 3, 4])
    >>> x.base is None
    True

    # Slicing creates a view, whose memory is shared with x:
    >>> y = x[2:]
    >>> y.base is x
    True


Deep Copy
---------

The ``copy`` method makes a complete copy of the array and its data.

``copy``\ 方法属于深复制, 会创建一个数组的副本.

.. code-block:: python

    >>> d = a.copy() # A new array object with new data is created
    >>> d is a
    False
    >>> d.base is a  # d doesn't share anything with a
    False
    >>> d[0, 0] = 9999
    >>> a
    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])

注意, 因为\ ``ndarray``\ 对象的切片返回的是视图(浅复制), 所以如果想返回切片的副本, 就应该在切片后调用\ ``copy()`` 方法:

.. code-block:: python

    >>> s = a[:, 1:3].copy()
    >>> s.base is a
    False

    
.. note::

    在对数组进行操作时, 需要注意操作的是原始数组, 还是其副本或视图.


******

判断是否共享内存
----------------

* ``ndarray.base``

通过\ ``ndarray.base``\ 属性判断当前的数组是否是另一个数组的视图.

Example:

.. code-block:: python
    :emphasize-lines: 3

    >>> a = np.arange(10)
    >>> b = a.reshape(2, 5)
    >>> b.base is a

* ``np.may_share_memory()``

通过\ ``np.may_share_memory()``\ 函数判断两个数组对象是否共享内存.

Example:

.. code-block:: python
    :emphasize-lines: 1

    >>> np.may_share_memory(a, b)
    True

* 通过数组对象的\ ``flags``\ 属性

通过数组对象的\ ``flags``\ 属性中的\ ``OWNDATA``\ 字段判断.

Example:

.. code-block:: python
    :emphasize-lines: 1, 3

    >>> b.flags['OWNDATA']
    False
    >>> a.flags['OWNDATA']
    True


view的用法
----------

对于内存中的数据, 其意义取决于如何解析它.

在\ ``numpy.ndarray.view``\ 中, 提供对内存区域不同的切割方式, 来完成数据类型的转换, 而无需对数据进行额外的copy, 从而节约时间和空间.

Example:

.. code-block:: python

    >>> import numpy as np
    >>> x = np.arange(10, dtype = np.int)
    >>> x
    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

上面的代码, 定义了一个元素类型为\ ``np.int``\ 的数组. 
进一步的, 创建一个它的视图:

.. code-block:: python

    >>> y = x.view(np.byte)
    >>> y
    array([0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 0,
           0, 0, 3, 0, 0, 0, 0, 0, 0, 0, 4, 0, 0, 0, 0, 0, 0, 0, 5, 0, 0, 0,
           0, 0, 0, 0, 6, 0, 0, 0, 0, 0, 0, 0, 7, 0, 0, 0, 0, 0, 0, 0, 8, 0,
           0, 0, 0, 0, 0, 0, 9, 0, 0, 0, 0, 0, 0, 0], dtype=int8)

    >>> x.shape
    (10,)
    >>> y.shape
    (80,)

可以看到, 同一段内存空间, 可以按照不同的方式对其进行解析, 得到不同的结果.

除了可以对数据类型做不同的解析外, 还可以对数组的shape做不同的解析. 
对于连续区域的数组, 可以重新定义它的shape; 而对于非连续的区域数据, 重新定义shape将产生错误.

.. code-block:: python
    :emphasize-lines: 7

    >>> a = np.zeros((10, 2))
    # A transpose make the array non-continuous
    >>> b = a.T
    # Taking a view makes it possible to modify the shape without modiying the 
    # initial object.
    >>> c = b.view()
    >>> c.shape = 20
    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
    AttributeError: Incompatible shape for in-place modification. Use `.reshape()` to make a copy with the desired shape.

接下来, 看一看\ ``numpy.reshape()``\ 函数, 对于其返回值的解释:

    This will be a new view object if possible; otherwise, it will be a copy.

    note: It is not always possible to change the shape of an array without copying the data. 

          If you want an error to be raise if the data is copied, you should assign the new shape to the shape attribute of the array.

    其返回值可能是一个view, 或是一个copy.

    当数据区域是连续的时候, 返回一个view; 否则返回一个copy.

    如果希望其在返回一个copy时报错的话, 改变shape的方式就不能使用\ ``reshape()``\ 函数, 而应该直接改变这个数组的shape属性.
    如上例中的\ ``c.shape = (20,)``\ , 在上例中, 如果使用\ ``c = b.view.reshape(20)``, 则此时c是b的一个copy.

 
.. note::

    一个数组对象, 创建一个它的视图, 在不复制数据的情况下, 以另外的意义解析它.



在数字图像处理中的应用
----------------------

当需要对输入图像三个通道进行相同的处理时, 使用\ ``cv2.split``\ 和\ ``cv2.merge``\ 是相当浪费资源的, 因为任何一个通道的数据对处理来说都是一样的, 
可以用view来将其转换为一维矩阵后再做处理, 这就不需要额外的内存和时间开销.

.. code-block:: python

    def createFlatView(array):
        '''Return a 1D view of an array of any dimensionally'''
        flatView = array.view()
        flatView.shape = array.size
        return flatView

