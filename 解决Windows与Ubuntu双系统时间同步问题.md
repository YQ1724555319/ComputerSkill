## 解决Windows与Ubuntu双系统时间同步问题    
[点击阅读原文](http://mtoou.info/windows-ubuntu-shijian/)     
---
Ubuntu和Windows默认的时间管理方式不同，所以双系统发生时间错乱是正常的。Ubuntu默认时间是把BIOS时间当成GMT+0时间，也就是世界标准时，而我国在东八区（GMT+8），所以如果你的Ubuntu位置是中国的话你系统显示的时间就是BIOS时间+8小时。假如现在是早上8点，那么你Ubuntu会显示8点，这时BIOS中的时间是0点。

而当你切换到Windows系统时就会发生时间错乱，因为Windows会认为BIOS时间就是你的本地时间，结果就是Windows显示时间为0点……而假如你在Windows下同步时间，恢复显示为8点，这时BIOS时间也会被Windows改写成8点，再次进入Ubuntu时显示时间又变成了8+8=16点……

解决的办法有两个，一个是让Windows使用Ubuntu的时间管理方式，就是启用UTC（世界协调时）另一个就是让Ubuntu按照Windows的方式管理时间，就是让Ubuntu禁用（世界协调时）。++个人建议第二种++，因为通常Windows是主系统，++不推荐对Windows进行这种修改++，不过我还是都介绍一下：

* **在Windows下启用UTC**

打开运行窗口（快捷键Win+R），然后输入regedit启动注册表编辑器，并找到一下目录位置：


```
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Control/TimeZoneInformation/
```

添加一项类型为REG_DWORD的键值，命名为RealTimeIsUniversal，值为1然后重启后时间即回复正常。

* **在Ubuntu下关闭UTC**

这个用这个方法是我比较推荐的：按Ctrl+Alt+T调出终端，输入：


```
sudo gedit /etc/default/rcS
```

按Ctrl+F调出查找，找到UTC=yes这一行，改成UTC=no，保存即可，时间修改立即生效。这样就可以解决Windows与Ubuntu双系统时间同步问题了

---  
以上方法如果无法查找到utc项，如ubuntu16.04，（其他版本没有试过，但16.04亲测可用）就直接在终端输入：  
`sudo hwclock --systohc --localtime`
