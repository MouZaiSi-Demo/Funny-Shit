## 利用netsh在windows上寻找已连过的wifi密码

首先得知道netsh是什么



**netsh简介**

是windows系统本身提供的功能强大的网络配置命令行工具,常用命令如下:

查看ip配置信息:

```
netsh interface ip show config 
```

查看网络配置文件:

```
netsh -c interface dump
```

开/关网卡:

```
netsh int set int name="ethernet" admin=enabled 

netsh int set int name="ethernet"admin=disabled
```

查看所有tcp连接:

```
netsh interface ip show tcpconnections 
```

设置本机ip、子网掩码、网关ip:

```
netsh interface ip set address "Local Area Connection"static 192.168.1.2 255.255.255.0 192.168.1.1
```

开/关防火墙:

```text
netsh firewall set opmode enable 

netsh firewall set opmode disable 
```

记不住也完全没关系,可以直接在命令行内输入netsh ?来获取接下来的命令

```
下列指令有效:

此上下文中的命令:
..             - 移到上一层上下文级。
?              - 显示命令列表。
abort          - 丢弃在脱机模式下所做的更改。
add            - 在项目列表上添加一个配置项目。
advfirewall    - 更改到 `netsh advfirewall' 上下文。
alias          - 添加一个别名
bridge         - 更改到 `netsh bridge' 上下文。
bye            - 退出程序。
commit         - 提交在脱机模式中所做的更改。
delete         - 在项目列表上删除一个配置项目。
dhcpclient     - 更改到 `netsh dhcpclient' 上下文。
dnsclient      - 更改到 `netsh dnsclient' 上下文。
dump           - 显示一个配置脚本。
exec           - 运行一个脚本文件。
exit           - 退出程序。
firewall       - 更改到 `netsh firewall' 上下文。
help           - 显示命令列表。
http           - 更改到 `netsh http' 上下文。
interface      - 更改到 `netsh interface' 上下文。
ipsec          - 更改到 `netsh ipsec' 上下文。
lan            - 更改到 `netsh lan' 上下文。
mbn            - 更改到 `netsh mbn' 上下文。
namespace      - 更改到 `netsh namespace' 上下文。
netio          - 更改到 `netsh netio' 上下文。
offline        - 将当前模式设置成脱机。
online         - 将当前模式设置成联机。
p2p            - 更改到 `netsh p2p' 上下文。
popd           - 从堆栈上打开一个上下文。
pushd          - 将当前上下文放入堆栈。
quit           - 退出程序。
ras            - 更改到 `netsh ras' 上下文。
rpc            - 更改到 `netsh rpc' 上下文。
set            - 更新配置设置。
show           - 显示信息。
trace          - 更改到 `netsh trace' 上下文。
unalias        - 删除一个别名。
wcn            - 更改到 `netsh wcn' 上下文。
wfp            - 更改到 `netsh wfp' 上下文。
winhttp        - 更改到 `netsh winhttp' 上下文。
winsock        - 更改到 `netsh winsock' 上下文。
wlan           - 更改到 `netsh wlan' 上下文。

下列的子上下文可用:
 advfirewall bridge dhcpclient dnsclient firewall http interface ipsec lan mbn namespace netio p2p ras rpc trace wcn wfp winhttp winsock wlan
```

------

现在回到主题

我们要利用cmd来获取已连接过的widows密码

1.Windows键+R

2.输入cmd 打开命令窗口

3.输入

- netsh wlan show profiles //这时候会出现连接过的WiFi名字

- netsh wlan show profiles name="***具体的WiFi名称***" key=clear //这样就显示了WiFi密码

  

------

以我的wifi来做例子

输入第一条命令后

```
C:\Users\17956>netsh wlan show profiles

接口 WLAN 上的配置文件:


组策略配置文件(只读)
---------------------------------
    <无>

用户配置文件
-------------
    所有用户配置文件 : mzs
    所有用户配置文件 : i-WZU
    所有用户配置文件 : WZU-free
    所有用户配置文件 : CMCC-pun2
    所有用户配置文件 : zhazha
    所有用户配置文件 : CMCC-YfQX
    所有用户配置文件 : 数zhazha
    所有用户配置文件 : CMCC-7gmf

```

输入第二条命令

```
C:\Users\17956>netsh wlan show profiles name="mzs" key=clear

接口 WLAN 上的配置文件 mzs:
=======================================================================

已应用: 所有用户配置文件

配置文件信息
-------------------
    版本                   : 1
    类型                   : 无线局域网
    名称                   : mzs
    控制选项               :
        连接模式           : 自动连接
        网络广播           : 只在网络广播时连接
        AutoSwitch         : 请勿切换到其他网络
        MAC 随机化: 禁用

连接设置
---------------------
    SSID 数目              : 1
    SSID 名称              :“mzs”
    网络类型               : 结构
    无线电类型             : [ 任何无线电类型 ]
    供应商扩展名           : 不存在

安全设置
-----------------
    身份验证         : WPA2 - 个人
    密码                 : CCMP
    身份验证         : WPA2 - 个人
    密码                 : GCMP
    安全密钥               : 存在
    关键内容            : baibai123

费用设置
-------------
    费用                : 无限制
    阻塞                : 否
    接近数据限制        : 否
    过量数据限制        : 否
    漫游                : 否
    费用来源            : 默认

```

上面代码块中的关键内容就是这个wifi的wifi密码啦!

------

注意:如果输入netsh的命令说 "netsh不是内外部命令..." 说明你的环境变量已经遭到改变(有可能是某些软件,或者你在配置编程环境变量的时候不小心改变). 

拯救的方法很简单:

1.右键我的电脑

2.点高级系统设置(以Windows 10 为例)

3.点环境变量(N)...

4.找到名为path的变量 并双击它进入编辑环境变量界面

5.新建 输入%SystemRoot%\system32 按确定即可关闭所有界面

6.然后重新启动cmd再次输入netsh试试

