### RDMA

**创建符号链接**

```
ln -s /usr/local/lib64/libibumad.so.3 /usr/lib/libibumad.so.3
```



把 `/usr/local/lib64` 添加到共享库搜索路径中。

```
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
```

 rdma link

```
modprobe rdma_rxe
sudo rdma link add rxe_0 type rxe netdev ens33
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

查看当前控制台的打印级别

```
cat /proc/sys/kernel/printk
```

该文件有4个数字值，它们根据日志记录消息的重要性，定义将其发送到何处，上面显示的4个数据分别对应如下：

控制台日志级别：优先级高于该值得消息将被打印到到控制台；

默认的消息日志级别：将用该优先级来打印没有优先级的消息；

最低的控制台日志级别：控制台日志级别可被设置的最小值（最高优先级）；

默认的控制台日志级别：控制台日志级别的缺省值。

**内核消息打印级别：** 内核消息可以按照不同的级别进行打印。级别的含义如下：

- 0：KERN_EMERG（紧急） - 最高优先级的消息。
- 1：KERN_ALERT（警告） - 警告级别消息。
- 2：KERN_CRIT（关键） - 关键级别消息。
- 3：KERN_ERR（错误） - 错误级别消息。
- 4：KERN_WARNING（警告） - 警告级别消息。
- 5：KERN_NOTICE（通知） - 通知级别消息。
- 6：KERN_INFO（信息） - 信息级别消息。
- 7：KERN_DEBUG（调试） - 调试级别消息。





```
interrupt_number=$(echo "$interrupt_line" | awk '{print $1}' | sed 's/://')
```

1. `echo "$interrupt_line"`：这部分会将包含中断信息的 `interrupt_line` 变量的内容输出到标准输出。
2. `awk '{print $1}'`：使用 `awk` 命令，它会将输入的文本分割成字段（默认使用空格分隔），并输出第一个字段。`'{print $1}'` 表示输出第一个字段，通常是中断号。
3. `sed 's/://'`：使用 `sed` 命令，它用于查找冒号 (`:`) 并将其替换为空字符，从而去除中断号中的冒号。这个操作可以将中断号从形如 "9:" 的格式中提取为纯数字格式。



#### 中断脚本

```

#!/bin/bash
nicname=$1
array=($(cat /proc/interrupts | grep $nicname | awk '{gsub(/:/, "", $1); printf "%s%s", (NR==1 ? "" : " "), $1}'))
array_len=${#array[@]}
core_count=$(nproc)
output_cores=$(seq 0 $((core_count-1)))
output_cores=($output_cores)
output_core_count=${#output_cores[@]}
for ((index=0; index<array_len; index++)); do
        irq="${array[index]}"
        core="${output_cores[index % output_core_count]}"
        echo "$core" > /proc/irq/$irq/smp_affinity_list
done


```

```
array=($(cat /proc/interrupts | grep $nicname | awk '{gsub(/:/, "", $1); printf "%s%s", (NR==1 ? "" : " "), $1}'))
```

这行代码是一个Bash命令，用于读取`/proc/interrupts`文件的内容，然后筛选包含在变量`$nicname`中的值的行，使用`awk`命令去除第一个字段中的冒号，并将处理后的数值存储在名为`array`的数组中。以下是该命令的详细解释：

1. `cat /proc/interrupts`：这个命令显示`/proc/interrupts`文件的内容，该文件提供了关于系统中断请求的信息。
2. `grep $nicname`：`grep`命令用于在文件中筛选包含变量`$nicname`的值的行。
3. `awk '{gsub(/:/, "", $1); printf "%s%s", (NR==1 ? "" : " "), $1}'`：这部分是`awk`命令，它会去掉每行第一个字段中的冒号，并按照指定的格式输出处理后的值。具体地，`gsub(/:/, "", $1)`用于去除第一个字段中的冒号，`printf "%s%s", (NR==1 ? "" : " "), $1`则用于输出处理后的值，其中`(NR==1 ? "" : " ")`的作用是在第一个字段前面加上空格，确保输出的格式正确。
4. `array=($(...))`：最终处理后的数值通过命令替换的方式（`$()`)存储到名为`array`的数组中。

请注意，这段代码的目的是将`/proc/interrupts`文件中包含特定`$nicname`值的行的第一个字段（去除冒号后的值）存储在一个Bash数组中。

#### 开机自启动

**创建启动脚本**

```
sudo vim /etc/init.d/irqbalance_start.sh

```

##### 编辑脚本文件

```
#!/bin/bash
/usr/local/sbin/irqbalance

```

**授权脚本文件**

```
sudo chmod +x /etc/init.d/irqbalance_start.sh

```

##### 配置开机自启动

```
sudo update-rc.d irqbalance_start.sh defaults
```

##### 重启

```
sudo reboot
```

##### 验证 `irqbalance` 是否正在运行

```
ps aux | grep irqbalance
```







##### 重启网络设置

```
sudo systemctl restart NetworkManager.service
```

```


```

```


```

删除网口

```
sudo ip link set dev bond0 down
sudo ip link set dev bond1 down
sudo ip link delete bond0
sudo ip link delete bond1
sudo nmcli connection delete bond0
sudo nmcli connection delete bond1
```

### shc

#### 编译Shell脚本

```

shc -f script.sh
```

#### 运行编译后的二进制文件

```
./script.sh.x

```

### 链路聚合

#### **创建bond1**

```
nmcli connection add type bond con-name bond1 ifname bond1 mode 4 ipv4.method manual ipv4.addresses 192.168.174.160/24 ipv4.gateway 192.168.174.2 ipv4.dns 8.8.8.8
```

#### 查看bond1网卡连接状态

```
nmcli connection show
```

#### **添加物理网卡连接到bond1**　

```
nmcli connection add type bond-slave con-name slave1 ifname enp1s0 master bond1
nmcli connection add type bond-slave con-name slave2 ifname enp1s0d1 master bond1
```

#### **查看bond的配置信息**

```
cat /proc/net/bonding/bond1
```

#### 停掉eth1，查看配置文件状态

```
nmcli device disconnect eth1
```



### 配置网卡ip转发

```
echo 1 > /proc/sys/net/ipv4/ip_forward
```



抖动脚本

```
#!/bin/bash

# 从文本中提取每个 latency= 行后的数字
latency_values=$(grep -oP 'latency\s*=\s*\K\d+(\.\d+)?' tcp_performance_test.txt)

# 检查是否有足够的数据点
if [ -z "$latency_values" ]; then
    echo "没有找到合适的 latency 数据"
    exit 1
fi

# 计算平均值
sum=0
count=0
for latency in $latency_values; do
    sum=$(echo "$sum + $latency" | bc -l)
    count=$((count + 1))
done

average_latency=$(echo "scale=2; $sum / $count" | bc -l)

# 计算方差
variance_sum=0
for latency in $latency_values; do
    diff=$(echo "$latency - $average_latency" | bc -l)
    squared_diff=$(echo "$diff * $diff" | bc -l)
    variance_sum=$(echo "$variance_sum + $squared_diff" | bc -l)
done

variance=$(echo "scale=2; $variance_sum / $count" | bc -l)

# 输出平均值和方差值到原始文件
echo "平均值: $average_latency us" >> tcp_performance_test.txt
echo "抖动: $variance us" >> tcp_performance_test.txt
```

杀进程脚本

```
#!/bin/bash
exec &> /dev/null

# 要终止的进程名称列表
process_names=("rdma_s.sh" "04_tcp_rdma_001" "rdma_c.sh" "tcp_c_64.sh" "out1.00" "sleep" "ethperf" "tee")

# 查找并终止匹配进程
for name in "${process_names[@]}"; do
    pids=$(ps aux | grep "$name" | grep -v grep | awk '{print $2}')
    if [ -z "$pids" ]; then
        echo "没有找到匹配的进程: $name"
    else
        echo "正在终止进程: $name"
        for pid in $pids; do
            kill -9 "$pid"
            echo "已终止进程PID: $pid"
        done
    fi
done
exit 0

```

```
function on_interrupt {
   cd src
    ./kill.sh
    exit 1
}

# 捕获Ctrl+C信号
trap on_interrupt SIGINT

```





```
pid=$!
```

`$!`是一个特殊变量，它代表了最近一个在后台运行的进程的PID



nmcli配置ip

```
 nmcli con mod enp0s3 ipv4.addresses 192.168.1.4/24
 nmcli con mod enp0s3 ipv4.gateway 192.168.1.1
 nmcli con mod enp0s3 ipv4.method manual
 nmcli con mod enp0s3 ipv4.dns "8.8.8.8"
```

