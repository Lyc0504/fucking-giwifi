# Fuck-giwifi
自动多拨Giwifi和账号认证

## 感谢 giwifi-gear

[giwifi-gear](https://github.com/icepie/giwifi-gear)由`icepie`编写

----------

自动多拨Giwifi和账号认证

这只是一个简单的shell脚本

```sh
ifup vwan1 && ifup vwan2 && ifup vwan3
```
上面这行代码是重启vwan接口，已便让这些接口从禁用变为离线，同时认证后重启这些接口能让这些接口被认证
多拨的时候设置的多少拨这里就写多少拨，比如6拨就是vwan1 - vwan6，3拨就是vwan1-vwan3

```sh
giwifi -g 10.254.0.1 -u 11111111111 -p 23456
```
其中的以下参数需改为你自己的数据

- 网关：10.254.0.1。
- 账号：11111111111。
- 密码：23456。
- 记得修改为你自己的账号和密码，以及正确的网关

而giwifi这个则是 https://github.com/icepie/giwifi-gear/tree/sh 的脚本。下载这个脚本，然后将其giwifi-gear.sh改为giwifi,同时在linux或MacOS终端输入`chmod +x giwifi`将其变为可执行文件

----------

### 完整代码：需修改网关和账号密码
```sh
#!/bin/bash

echo "Start running FuckGiwifi!"

echo "Waitting for 10s,let my network server init"
sleep 10;

echo "Start auth!"
giwifi -g 10.254.0.1 -u 11111111111 -p 23456

echo "Waitting for 5s. Start reboot vwan interface!"
sleep 5;
ifup vwan1 && ifup vwan2 && ifup vwan3

echo "Waitting for 10s, Reboot vwan interface again"
sleep 10;
ifup vwan1 && ifup vwan2 && ifup vwan3

echo "Start deamo running giwifi"
# if down, will be reboot again!
for ((i=1;i<=2;i)) do {
    sleep 2;
    giwifi -g 10.254.0.1 -u 11111111111 -p 23456 -d
}

done

echo "Fuck !"
```
