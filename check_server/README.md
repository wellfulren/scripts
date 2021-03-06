# 一键检测各服务器是否存活

## 说明
本脚本用于一键检测各宿主机、虚拟机是否存活。<br>
适用于vmware、kvm、openstack等宿主机、虚拟机检测。也可以将脚本稍作改造，用于检测物理服务器。<br>

## 脚本结构
本脚本包含2个文件，即：配置文件(server.txt)、执行脚本(check.sh)，如下：<br>
check_servers <br>
├── check.sh <br>
└── server.txt <br>
脚本执行前，请先编辑配置文件，书写格式为：vm宿主机、宿主机管理端口、虚拟机、虚拟机管理端口共4项，每项用空格分开，每行代表一台宿主机和其上面对应的一台虚拟机，宿主机可以重复录入。<br>
配置文件我已经给出了模板。<br>

## 脚本原理
* 脚本自动在/tmp/目录下创建check_host_net目录;
* 脚本从上往下读取配置文件，每次读一行，先对服务器ping2次，每次间隔0.02秒，然后将ping通的IP写入一个文件中，将ping不通的IP写入另外一个文件中；
* 通过telnet对ping不通的ip文件逐行检测，每次telnet检测过期时间为1秒，超过1秒自动断开telnet，然后继续telnet下一个IP的端口，将所有的telnet结果统一写入一个文件中；
* 将telnet通的IP提取出来追加到ping通的文件中；
* 如果有telnet产生的文件，则将配置文件中的提取出的所有IP和已经存活的IP文件进行匹配，将差异IP导入到一个后缀叫die.txt的文件。

## 执行脚本
```
chmod +x check.sh
./check.sh [super|vm|help]
```
或者：<br>
```
sh check.sh [super|vm|help]
```
super选项代表检测宿主机;vm选项代表检测虚拟机。<br>

## 备注
脚本执行结果，正确且重要的信息以绿色字体显示；错误的信息以红色字体显示；重要的提示信息以蓝底展示。<br>
将终端设置为黑底白字再执行脚本将会得到更好的体验。<br>

