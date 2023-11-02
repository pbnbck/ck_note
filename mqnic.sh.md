```
#!/bin/bash

current_path="$PWD"

if [ $# -ne 1 ]; then
  echo "请提供一个文件夹名称作为参数"
  exit 1
fi

folder_name="$1"
search_path="$current_path/$folder_name"

if [ -d "$search_path" ]; then
  cd "$search_path"

  if [ "$folder_name" == "corundum-master" ]; then
    cd modules/mqnic
    sudo rmmod mqnic
    make clean
    make
    sudo insmod mqnic.ko
    echo "已进入 $folder_name 文件夹，当前路径: $PWD"
  else
    sudo rmmod mqnic
    make clean
    make
    sudo insmod mqnic.ko
    echo "已进入 $folder_name 文件夹，当前路径: $PWD"
  fi

else
  echo "$folder_name 文件夹不存在于当前路径中"
  exit 1
fi


```

`$#` 表示传递给脚本的参数数量，`-ne` 表示不等于。因此，`if [ $# -ne 1 ]; then` 表示如果传递给脚本的参数数量不等于1（也就是没有传递文件夹名称作为参数），则执行后续的错误消息和退出操作。这有助于避免在没有足够参数的情况下执行脚本，从而减少可能的错误。

`-d` 表示检查路径是否为目录。



```
#!/bin/bash

current_path="$PWD"

if [ $# -ne 1 ]; then
  echo "请提供一个参数"
  exit 1
fi
base_ip1="100.1.1."
base_ip2="100.1.2."

name="$1"
search_path="$current_path/mqnic"
cd $search_path
  if [ "$name" == "single" ]; then
    sed -i '/EXTRA_CFLAGS += -DFIXED_MAC_ADDRESS/s/^/#/' "$search_path/Makefile"
    echo "单路"
  elif [ "$name" == "mult" ]; then
    sed -i '/EXTRA_CFLAGS += -DFIXED_MAC_ADDRESS/s/^#//' "$search_path/Makefile"
fi
  sudo rmmod mqnic
  make clean
  make
  sudo insmod mqnic.ko
  lspci_output=$(lspci)
  target_line=$(echo "$lspci_output" | grep "Ethernet controller: Device 1234")
   	if [ -n "$target_line" ]; then
		last_four_numbers=$(echo "$target_line" | grep -oE '[0-9]{4}$')
	    	first_seven_chars=$(echo "$target_line" | awk '{print substr($0, 1, 7)}')
	        last_digit="${target_line: -1}"
		ip_address1="$base_ip1$last_digit"
		ip_address2="$base_ip2$last_digit"
          
		nic_names1=$(ls /sys/devices/pci0000:00/0000:00:00.0/0000:$first_seven_chars/net/ | head -n 1)
		nic_names2=$(ls /sys/devices/pci0000:00/0000:00:00.0/0000:$first_seven_chars/net/ | sed -n '2p')
		sudo ifconfig $nic_names1 $ip_address1 netmask 255.255.255.0 up
		sudo ifconfig $nic_names2 $ip_address2 netmask 255.255.255.0 up

        fi


    echo "最后四个数字：$last_four_numbers"
    echo "last:$last_digit"
    echo "ip_address1:$ip_address1"
    echo "ip_address2:$ip_address2"

    echo "nic_names:$nic_names1"
    echo "nic_names:$nic_names2"

    echo "first_seven_chars:$first_seven_chars"
```

- `grep`: 这是用于查找文本的命令。

- `-oE`: 这是`grep`的选项。 `-o` 表示只输出匹配的部分，而 `-E` 表示使用扩展正则表达式进行匹配。

- ```
  '[0-9]{4}$'
  ```

  : 这是正则表达式模式。它匹配文本中以4个连续数字结尾的字符串。在正则表达式中：

  - `[0-9]` 表示匹配任意一个数字（0到9之间的任何数字）。
  - `{4}` 表示前面的模式（即数字）必须恰好重复4次。
  - `$` 表示匹配字符串的末尾。

因此，命令 `grep -oE '[0-9]{4}$'` 会从输入文本中查找并提取出以4个数字结尾的子字符串。



```
#!/bin/bash

current_path="$PWD"

if [ $# -ne 1 ]; then
  echo "请提供一个参数"
  exit 1
fi
base_ip1="100.1.1."
base_ip2="100.1.2."

name="$1"
search_path="$current_path/mqnic"
cd $search_path
  if [ "$name" == "single" ]; then
    sed -i '/EXTRA_CFLAGS += -DFIXED_MAC_ADDRESS/s/^/#/' "$search_path/Makefile"
    echo "单路"
  elif [ "$name" == "mult" ]; then
    sed -i '/EXTRA_CFLAGS += -DFIXED_MAC_ADDRESS/s/^#//' "$search_path/Makefile"
  fi
  sudo rmmod mqnic
  make clean
  make
  sudo insmod mqnic.ko
  lspci_output=$(lspci)
  target_line=$(echo "$lspci_output" | grep "Ethernet controller: Device 1234")
   	if [ -n "$target_line" ]; then
		last_four_numbers=$(echo "$target_line" | grep -oE '[0-9]{4}$')
	    	first_seven_chars=$(echo "$target_line" | awk '{print substr($0, 1, 7)}')
	        last_digit="${target_line: -1}"
		ip_address1="$base_ip1$last_digit"
		ip_address2="$base_ip2$last_digit"
          
		nic_names1=$(ls /sys/devices/pci0000:00/0000:00:00.0/0000:$first_seven_chars/net/ | head -n 1)
		nic_names2=$(ls /sys/devices/pci0000:00/0000:00:00.0/0000:$first_seven_chars/net/ | sed -n '2p')
		sudo ifconfig $nic_names1 $ip_address1 netmask 255.255.255.0 up
		sudo ifconfig $nic_names2 $ip_address2 netmask 255.255.255.0 up

    fi


    echo "最后四个数字：$last_four_numbers"
    echo "last:$last_digit"
    echo "ip_address1:$ip_address1"
    echo "ip_address2:$ip_address2"

    echo "nic_names:$nic_names1"
    echo "nic_names:$nic_names2"

    echo "first_seven_chars:$first_seven_chars"
    #问题：只有一个网卡能配上ip
```











```
if ps aux | grep -q '[t]cp_c_64.sh'; then
        echo "64"
else


    ./tcp_c_1024.sh
fi
```

`grep -q '[t]cp_c_64.sh'`：这个部分使用 `grep` 命令来在进程列表中查找包含字符串 `[t]cp_c_64.sh` 的进程。注意，正则表达式中的方括号 `[t]` 是一种技巧，用于确保 `grep` 不会匹配到自身。`-q` 选项使得 `grep` 命令在找到匹配项时不输出任何信息

```
substr($0, 1, 7)
```

表示从每一行($0表示整行)的第一个字符开始提取7个字符。

`$0`代表整个文本行，而`$1`、`$2`、`$3`等表示文本行中的第一个、第二个、第三个字段



`substr`是Awk编程语言中的一个内置函数，用于提取字符串的子串。它的基本语法如下

```
substr(string, start, length)
```

