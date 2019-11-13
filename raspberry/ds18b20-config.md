#### Ds18b20在树莓派配置
1. 进入cd /boot/目录，打开nano config.txt文件
2. 配置dtoverlay=w1-gpio-pullup,gpiopin=4
3. 重启系统reboot
4. 重启完成进入：cd /sys/bus/w1/devices/ 查看传感器数据
