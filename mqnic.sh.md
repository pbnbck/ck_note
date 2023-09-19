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
