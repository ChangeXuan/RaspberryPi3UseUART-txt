# 关于如何在树莓派3上使用外接蓝牙
## 因为树莓派3在自带有一颗板载蓝牙，所以把UART给占用了，要想让外接蓝牙使用UART，就必须把板载蓝牙的占用给取消调
## 不过不知道可不可以使用软串口......

- 打开Terminal,输入$ sudo systemctl disable hciuart(没有任何提示说明执行成功)
- 输入$ sudo nano /lib/systemd/system/hciuart.service
```
把打开的文档中所以的ttyAMA0改为ttyS0,如果没有,直接退出就可以了
```

- 去http://ukonline2000.com/?attachment_id=881下载pi3-miniuart-bt-overlay
```
拷贝到/boot/overlays目录下(要使用sudo)
```

- 输入$ sudo nano /boot/config.txt
```
在打开的文档的末尾添加
dtoverlay=pi3-miniuart-bt-overlay
force_turbo=1
```

- 输入$ sudo nano /boot/cmdline.txt
```
把里边的内容替换成
dwc_otg.lpm_enable=0  console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4  elevator=deadline fsck.repair=yes   rootwait
(注意！！其中的root=/dev/mmcblk0p?不能修改,否则可能造成不能开机)
```

- 最后更新重启
```
sudo apt-get update
sudo apt-get upgrade
sudo rebot
```
