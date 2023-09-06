### RDMA

**创建符号链接**

```
ln -s /usr/local/lib64/libibumad.so.3 /usr/lib/libibumad.so.3
```

![image-20230825141603979](./image-20230825141603979.png)





把 `/usr/local/lib64` 添加到共享库搜索路径中。

```
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
```

### 天脉

查看磁盘 `/dev/sda` 的分区信息

```
sudo fdisk -l /dev/sda

```

查看根目录分区的使用情况

```
df -h /dev/sda3

```

### softroce

configure: error: cannot guess build type; you must specify one

```
./configure --build=aarch64-unknown-linux-gnu

```

确认当前内核是否支持RXE

```
cat /boot/config-$(uname -r) | grep RXE
```

加载内核驱动

```
modprobe rdma_rxe
```

用户态配置

```
sudo rdma link add rxe_0 type rxe netdev ens33
```

用rdma工具查看是否添加成功：

```
rdma link
```

