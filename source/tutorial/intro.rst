简介
====

``NumPy``\ (Numerical Python)是Python语言的一个扩展程序库, 支持大量的维度数组与矩阵计算, 此外也针对数组运算提供大量的数学函数库.

``NumPy``\ 是一个运行速度非常快的数学库, 主要用于数组计算, 包含:

-  一个强大的N维数组对象\ ``ndarray``;
-  广播功能函数;
-  整合C/C++/Fortan代码的工具;
-  线性代数，傅里叶变换，随机数生成功能.


应用
----

``NumPy``\ 通常与\ ``SciPy``\ 和\ ``Matplotlib``\ 一起使用, 这种组合广泛用于替代MatLab, 是一个强大的科学计算环境.

``SciPy``\ 是一个开源的Python算法库和数学工具包.
``SciPy``\ 包含的模块有最优化, 线性代数, 积分, 插值, 特殊函数, 快速傅里叶变换, 信号处理和图像处理, 常微分方程求解和其它科学与工程中常用的计算.

``Matplotlib``\ 是Python编程语言及其数值数学扩展包NumPy的可视化操作界面. 
它为利用通用的图形用户界面工具, 如Tkinter, wxPython, Qt或GTK+向应用程序嵌入式绘图提供了应用程序接口.


安装
----

使用已有的发行版本
^^^^^^^^^^^^^^^^^^

``Anaconda``\ : 免费的Python发行版,  用于进行大规模数据处理,  预测分析和科学计算,  致力于简化包的管理和部
署. 支持Linux, Windows和Mac系统.

.. note::

    发行版包括了Numpy, SciPy, Matplotlib, IPython, SymPy等以及Python核心自带的其他包.


使用\ ``pip``\ 安装
^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    pip3 install --user numpy scipy matplotlib ipython jupyter pandas sympy nose

``--user``\ 选项可以设置只安装在当前用户目录下, 而不是写入系统目录.

Linux下安装
^^^^^^^^^^^

* Ubuntu

.. code-block:: sh

    sudo apt-get install python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose


相关链接
--------

-  NumPy官网: http://www.numpy.org/
-  NumPy源代码: https://github.com/numpy/numpy

-  SciPy官网: https://www.scipy.org/
-  SciPy源代码: https://github.com/scipy/scipy

-  Matplotlib官网: https://matplotlib.org/
-  Matplotlib源代码: https://github.com/matplotlib/matplotlib

