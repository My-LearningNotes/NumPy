安装
====


使用已有的发行版本
------------------

``Anaconda``\ : 免费的Python发行版,  用于进行大规模数据处理,  预测分析和科学计算,  致力于简化包的管理和部署. 
支持Linux, Windows和Mac系统.

.. note::

    发行版包括了Numpy, SciPy, Matplotlib, IPython, SymPy等以及Python核心自带的其他包.


使用\ ``pip``\ 安装
-------------------

.. code-block:: python

    pip3 install --user numpy scipy matplotlib ipython jupyter pandas sympy nose

``--user``\ 选项可以设置只安装在当前用户目录下, 而不是写入系统目录.


Linux下安装
-----------

* Ubuntu

.. code-block:: sh

    sudo apt-get install python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

