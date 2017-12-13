# 通过进程PID查找可执行文件的路径

```bash
readlink /proc/{pid}/exe
```

# lsof列出打开的文件

`lsof`命令可以列出打开的文件，所谓文件包括普通文件、目录、块设备、网络套接字等，
这样就能方便地分析资源占用问题。使用`root`身份运行能获得更全面的结果。

## 列含义

`lsof`会输出打开文件的相关信息，各列的含义如下：

```bash
COMMAND    PID    USER     FD    TYPE    DEVICE    SIZE/OFF    NODE NAME
init         1    root    cwd     DIR     253,0        4096       2 /
```

+ **COMMAND**: 进程名，默认显示前9个字符，可用`+c w`选项设为显示前`w`个字符
+ **PID**: 进程ID
+ **USER**: 进程所属用户
+ **FD**: 文件描述符，如：
    - cwd: 工作目录
    - txt: 程序文件(text)
    - mem: 内存映射文件
+ **TYPE**: 文件类型，如：
    - DIR: 目录
    - REG: 普通文件
    - sock: socket
+ **DEVICE**: 设备
+ **SIZE/OFF**: 文件大小/偏移
+ **NODE**: 对于磁盘文件是inode，对于socket文件是TCP/UDP
+ **NAME**: 文件名，对于普通文件是路径，对于socket文件是网络地址

## 选项

不加参数地使用`lsof`会列出所有打开的文件，如果只想列出特定的文件，可以使用下列选项来筛选。

+ **names**: 列出指定文件，若有多个则是或的关系，若指定挂载点，则列出文件系统中所有打开的文件
+ **-u user**: 列出指定用户打开的文件
+ **-d fdset**：指定逗号分隔的描述符集合，列出符合的文件。(描述符指输出中的`FD`列)
+ **+d /dir\/**： 列出指定目录下的文件
+ **+D /dir/**: 列出指定目录下的文件，并递归列出子目录
+ **-a**: 对指定的多个条件执行与运算，否则是或运算
+ **-i**: 列出指定的socket文件，文件名格式为`[46][protocol][@hostname|hostaddr][:service|port]`
    - **46**: IPv4/IPv6
    - **protocol**: TCP/UDP
    - **hostname**: 主机名
    - **hostaddr**: IP地址
    - **service**: 网络服务名，可以是逗号分隔的多个值，取值列表详见`/etc/services`
    - **port**: 端口号，可以是逗号分隔的多个值

例如：

```bash
# 列出test.txt的打开情况
lsof test.txt

# 列出root用户打开的文件
lsof -u root

# 列出root用户打开的程序文件
lsof -a -u root -d txt

# 列出当前目录下打开的文件
lsof +d .

# 列出80端口打开情况
lsof -i :80

# 列出ftp服务打开情况
lsof -i :ftp
```

# 查看Linux启动时间

```bash
uptime
```

`uptime`输出在一行内输出系统相关信息：

> 12:34:23 up 37 days, 18:54, 20 users,  load average: 1.97, 1.87, 2.71

+ 当前时间
+ 系统运行时间
+ 登录用户数
+ 过去1/5/15分钟的系统负载
