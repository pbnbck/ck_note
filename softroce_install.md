设置ip地址与主机在同一网段下

```
sudo ifconfig 网卡名字 ip地址/24
```

传输rdma-core-27.10至天脉系统下

```
stfp 天脉Linux名字@ip
```

设置时间

```
date -s 当前时间
```

解压rdma-core-27.10

```
cd rdma-core-27.10
```

编译安装

```
./build.sh
cmake .
make
```

问题一

```
root@acorelinux631:~/tools/rdma-core-27.10# ibv_devices
ibv_devices: error while loading shared libraries: libibverbs.so.1: cannot open shared object file: No such file or directory

```

解决方法1.1：指定环境变量

```
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH

```

解决方法1.2：手动软链接

* 在根目录下查找`libibverbs.so.1`地址

```
find -name libibverbs.so.1
```

* 进行软链接到usr/lib

```
ln -s /usr/local/lib64/libibverbs.so.1 /usr/lib/libibverbs.so.1
```

  

传输perftest至天脉linux下

解压perftest

```
cd perftest-master
./autogen.sh
./configure
make
make install
```

当重启一个终端可能会发生问题一的错误，解决方法1.1/1.2



export脚本

```
#!/bin/bash
nicname=$1
echo "$nicname"
interrupt_line=$(grep "$nicname" /proc/interrupts)
interrupt_number=$(echo "$interrupt_line" | awk '{print $1}' | sed 's/://')
echo 2 > /proc/irq/$interrupt_number/smp_affinity_list
echo "$nicname $interrupt_number"

export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
sudo rxe_cfg start > /dev/null 2>&1
sudo rxe_cfg add $nicname > /dev/null 2>&1
sudo ifconfig $nicname down > /dev/null 2>&1
sudo ifconfig $nicname up > /dev/null 2>&1
sudo rxe_cfg start > /dev/null 2>&1
echo 1       4       1      7 > /proc/sys/kernel/printk

```





开机用户登录时运行脚本，将环境变量写入

```
sudo cp export.sh /etc/profile.d/
```

