# 天脉linux安装方法

```bash
df -h
```

查看磁盘挂载情况

```
mkfs.ext4 /dev/sda4
```

格式化磁盘

```
mount /dev/sda4 /mnt
```

将sda4挂载在mnt目录下

传输天脉压缩包至mnt目录下并且解压

```
cd /boot/efi/boot/grub
```

目录下

```
vim grub.cfg
```

复制以下代码

```
serial --unit=0 --speed=115200 --word=8 --parity=no --stop=1 

search --no-floppy --set=root -l 'boot' 

default=boot 

timeout=10

 

menuentry 'AcoreLinux Embedded version V1.0.0.F.2' {

​    insmod part_gpt

​    insmod ext2

​    set root='hd0,gpt4'

​    Echo  ‘Loading Linux 4.19 ACoreLinux-01 ...’

​    linux  /boot/Image root=/dev/sda4

​    console=ttyAMA1,115200 splash loglevel=7 rootdelay=5 KEYBOARDTYPE=pc KEYTABLE=us security=

}
```



重启

```
sudo reboot
```

