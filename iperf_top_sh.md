```
#!/bin/bash

# 定义目标服务器地址
server=192.168.187.129

# 定义测试时间（秒）
test_time=10

# 定义报告间隔（秒）
report_interval=1

# 定义数据包大小列表
packet_sizes=(64 128 512)

# 获取当前时间并格式化为可读的时间戳
current_time=$(date +"%Y-%m-%d %H:%M:%S")

# 运行iperf测试
for size in "${packet_sizes[@]}"; do
    echo "Testing with packet size: ${size} bytes - Time: ${current_time}"
    iperf -c $server -t $test_time -l ${size} -i $report_interval
    echo "-----------------------------------------"
done


```

