# 气象数据格式笔记

[ReadTheDocs](https://meteodataformatnote.readthedocs.io/zh_CN/latest/index.html)

记录各类气象数据格式的特性及其使用方法   

## 内容索引
* [GRIB](###GRIB)
    * [数据格式简介](###数据格式简介)
    * [处理工具及方法](###处理工具及方法)
        * [ecCodes](####ecCodes)
* [NetCDF](#NetCDF)
* [HDF](#HDF)
* [GeoTIFF](#GeoTIFF)
* [shapefile](#shapefile)
* [GeoJSON](#GeoJSON)

## GRIB
### 数据格式简介
GRIB(General Regularly distributed Information in Binary form)，是由世界气象组织（WMO）设计和维护的一种用于存储和传输网格数据的标准数据格式，它是一种自描述的二进制压缩格式，通常具有扩展名.grib，.grb或.gb。

世界气象组织一共发布了3各版本的GRIB标准：

* GRIB 版本 0: 已淘汰，无技术支持，目前几乎不再使用。
* GRIB 版本 1: 版本1是GRIB的历史遗留版本，已停止开发。由于它已在国际民航组织（ICAO）的世界范围预报系统中使用，因此仍得到WMO的认可。
* GRIB 版本 2: 版本2格式是GRIB标准的扩展和强化，它与版本1相比在压缩比等性能上有更优异的表现。一些国家的数值天气预报机构（尤其是美国和欧洲）正在逐步采用此版本，版本2不能与版本1兼容。

想了解更多GRIB1和GRIB2的信息，请参考：[Introduction to
GRIB Edition1 and GRIB Edition 2](https://www.wmo.int/pages/prog/www/WMOCodes/Guides/GRIB/Introduction_GRIB1-GRIB2.pdf)

GRIB数据格式是以一个被称为“报文”(Message)的数据结构为基本单元的集合体。每个“报文”中会存储一套经纬度、变量数组以及所有描述性的属性信息，而每个GRIB文件里会按顺序排列存储多个“报文”。
### 处理工具及方法
#### ecCodes
cCodes是一个由ECMWF开发的程序包，它可以提供用于解码和编码GRIB格式的API和工具。我们可以使用conda来安装：`$ conda install -c conda-forge eccodes`

ecCodes提供了一套处理grib数据的命令行工具，你可以使用 `grib_dump` ， `grib_ls` 和 `grib_get` 来查看文件内容，也可以使用 `grib_set` 和 `grib_filter` 去修改内容，还可以用 `grib_copy` 去把部分内容复制出来，或者使用 `grib_get_data` 从文件中把经纬度和变量值提取出来。 `grib_compare` 还可以按照键去对不同GRIB文件进行对比。

##### grib_ls
`grib_ls` 命令行主要用于查看GIRB文件的内容信息
1. 查看GRIB文件所有报文的所有参数
```
$ grib_ls ERA5_20191231.grib
ERA5_20191231.grib
edition      centre       typeOfLevel  level        dataDate     stepRange    dataType       shortName    packingType  gridType     
1            ecmf         surface      0            20191231     0            an             10u          grid_simple  regular_ll  
1            ecmf         surface      0            20191231     0            an             10v          grid_simple  regular_ll  
1            ecmf         surface      0            20191231     0            an             2d           grid_simple  regular_ll  
...
384 of 384 messages in ERA5_20191231.grib

384 of 384 total messages in 1 files
```

2. 仅查看报文信息中的shortName和dataType参数
```
$ grib_ls -p shortName,dataType ERA5_20191231.grib 
ERA5_20191231.grib
shortName   dataType    
10u         an         
10v         an         
2d          an         
...
384 of 384 messages in ERA5_20191231.grib

384 of 384 total messages in 1 files
```

3. 筛选参数shortName为tp的报文信息
```
$ grib_ls -w shortName=tp ERA5_20191231.grib 
ERA5_20191231.grib
edition      centre       typeOfLevel  level        dataDate     stepRange    dataType     shortName    packingType  gridType     
1            ecmf         surface      0            20191230     5-6          fc           tp           grid_simple  regular_ll  
1            ecmf         surface      0            20191230     6-7          fc           tp           grid_simple  regular_ll  
1            ecmf         surface      0            20191230     7-8          fc           tp           grid_simple  regular_ll  
...
384 of 384 messages in ERA5_20191231.grib

384 of 384 total messages in 1 files
```

4. 查看距离(25°N,100°E)最近点的paramId,name和值
```
$ grib_ls -l 25,100,1 -p paramId,name ERA5_20191231.grib 
ERA5_20191231.grib
paramId     name         value 
165         10 metre U wind component  -0.229126   
166         10 metre V wind component  -0.69986    
168         2 metre dewpoint temperature  272.847     
...
384 of 384 total messages in 1 files
Input Point: latitude=25.00  longitude=100.00
Grid Point chosen #2 index=39460 latitude=25.00 longitude=100.00 distance=0.00 (Km)
Other grid Points
- 1 - index=39461 latitude=25.00 longitude=100.25 distance=25.18 (Km)
- 2 - index=39460 latitude=25.00 longitude=100.00 distance=0.00 (Km)
- 3 - index=39180 latitude=25.25 longitude=100.25 distance=37.48 (Km)
- 4 - index=39179 latitude=25.25 longitude=100.00 distance=27.78 (Km)
```
更多详细的参数说明及使用方法可以执行 `grib_ls -h` 查看帮助文档或阅读ECMWF官方文档[GRIB tools](https://confluence.ecmwf.int/display/GRIB/GRIB+tools)

## NetCDF
## HDF
## GeoTIFF
## shapefile
## GeoJSON