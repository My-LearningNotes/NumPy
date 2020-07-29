Copies and Views
================

``ndarray``\ 是一个数组对象, 除了其中的实际数据之外, 还有一些其它的内容.

* 引用, 就是别名;
* 副本是一个对象的完整拷贝, 如果我们对副本进行修改, 它不会影响到原始数据;
* 视图, 它的作用是用来查看原始对象的数据, 它就像原始对象的一个影子, 正如影子不能脱离本体存在一样, 视图也不能脱离原始对象, 必须从原始对象来创建. 
  视图,是一种浅拷贝, 它是一个新的对象, 但它的实际数据部分和原始对象是共享的, 即是内存中的同一块数据, 所以视图中对数据的修改会影响原始对象.

.. note::

    视图, 也是一个\ ``ndarray``\ 类型的对象, 只不过它是原始对象的"影子", 要从原始对象创建而来.

    对于一个\ ``ndarray``\ 对象, 可以通过\ ``ndarray.base``\ 属性查看其是否是视图, 以及它的原始数据.

    NumPy文档中对\ ``ndarray.base``\ 属性的解释:

    If the array is a view into another array, that array is its 'base'(Unless that array is also a view    ). The 'base' array is where the array data is actually stored.

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

简单的赋值操作, 即把一个数组赋值给另一个数组, 属于浅复制, 传递的是引用, 不会复制数据.

.. code-block:: python

    >>> a = np.array([[0, 1, 2, 3], 
    ...               [4, 5, 6, 7],
    ...               [8, 9, 10, 11]])
    >>> b = a
    >>> b is a
    True

把数组作为函数的参数传递时, 也是浅复制.

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

