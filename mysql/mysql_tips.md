# 编译

## Linux

在源码目录`mysql-server-5.7`下新建一个`debug`目录用于存放编译临时文件，防止污染源码。

```BASH
cd mysql-server-5.7
mkdir debug
cd debug
cmake ..\
    -DBUILD_CONFIG=mysql_release\
    -DCMAKE_INSTALL_PREFIX=~/mysql\
    -DWITH_DEBUG=ON\
    2>&1 | tee cmake.log
make
```
