=========
GRIB
=========

数据格式简介
---------------
GRIB(General Regularly distributed Information in Binary form)，是由世界气象组织（WMO）设计和维护的一种用于存储和传输网格数据的标准数据格式，它是一种自描述的二进制压缩格式，通常具有扩展名.grib，.grb或.gb。

世界气象组织一共发布了3各版本的GRIB标准：

* GRIB 版本 0: 已淘汰，无技术支持，目前几乎不再使用。
* GRIB 版本 1: 版本1是GRIB的历史遗留版本，已停止开发。由于它已在国际民航组织（ICAO）的世界范围预报系统中使用，因此仍得到WMO的认可。
* GRIB 版本 2: 版本2格式是GRIB标准的扩展和强化，它与版本1相比在压缩比等性能上有更优异的表现。一些国家的数值天气预报机构（尤其是美国和欧洲）正在逐步采用此版本，版本2不能与版本1兼容。

想了解更多GRIB1和GRIB2的信息，请参考：`Introduction to
GRIB Edition1 and GRIB Edition 2 <https://www.wmo.int/pages/prog/www/WMOCodes/Guides/GRIB/Introduction_GRIB1-GRIB2.pdf>`_

GRIB数据格式是以一个被称为“消息”(Message)的数据结构为基本单元的集合体。每个“消息”中会存储一套经纬度、变量数组以及所有描述性的属性信息，而每个GRIB文件里会按顺序排列存储多个“消息”。

处理工具及方法
-----------------
ecCodes
^^^^^^^^^
ecCodes是一个由ECMWF开发的程序包，它可以提供用于解码和编码GRIB格式的API和工具。我们可以使用conda来安装：``$ conda install -c conda-forge eccodes``

ecCodes提供了一套处理grib数据的命令行工具，你可以使用 ``grib_dump`` ， ``grib_ls`` 和 ``grib_get`` 来查看文件内容，也可以使用 ``grib_set`` 和 ``grib_filter`` 去修改内容，还可以用 ``grib_copy`` 去把部分内容复制出来，或者使用 ``grib_get_data`` 从文件中把经纬度和变量值提取出来。 ``grib_compare`` 还可以按照键去对不同GRIB文件进行对比。

详细的使用方法及示例见ECMWF官方文档： `GRIB tools <https://confluence.ecmwf.int/display/GRIB/GRIB+tools>`_

CDO
^^^^^
Python
^^^^^^^^
NCL
^^^^^

示例解析
----------
