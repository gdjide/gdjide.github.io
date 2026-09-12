---
title: Centos7的详细网络配置
categories: 
    - [环境搭建]
single_column: true
cover: https://pic-bed-6vd.pages.dev/img/fm4.webp
banner:
  type: img
  bgurl: https://pic-bed-6vd.pages.dev/img/bg7.webp
  banner_text: 
---
## 第一种连接模式：桥接模式（操作简单）
### 1. 在虚拟机开机前进行设置
在虚拟机设置中<span style="color: red;">网络适配器</span>下选择桥接模式，选中<span style="color: red;">复制物理网络连接状态</span>。
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net1.png)

### 2. 也可在虚拟机开机后进行设置
有时候在开机前设置完之后打开虚拟机网络又会变成NAT模式，很奇怪，所以可以在
开机后重新设置一下网络。
*虚拟机右下角也可以找到网络设置*
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net2.png)
选择桥接模式即可。
## 第二种连接模式：NAT模式（较复杂）
在运行虚拟机之后，打开虚拟机网络编辑器。
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net2.png)
选择<span style="color: red;">VMnet8</span>网络，没有的话创建一个。
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net3.png)
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net4.png)
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net5.png)
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net6.png)
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net7.png)
在虚拟机中输入
```shell
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```
进去后按 i 进入编辑模式
将BOOTPROTO=dhcp 更改为   BOOTPROTO=static
  ONBOOT=no      更改为   ONBOOT=yes

添加以下内容
```shell
IPADDR=192.168.100.1 与虚拟机在相同网段
NETMASK=255.255.255.0 固定值
GATEWAY=192.168.100.2 必须与前面的“网关IP”相同
DNS1=114.114.114.114
DNS2=8.8.8.8
```
按Esc键退出编辑模式，输入 :wq 保存退出。
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net8.png)
重启网络服务
```shell
systemctl restart network
```
查看网络状态，查看IP地址
```shell
ip addr
```
可以ping一下百度看看网络连接有无成功
```shell
ping www.baidu.com

使用ctrl + z 退出ping
```
![虚拟机设置](https://pic-bed-6vd.pages.dev/img/net9.png)

<script src="https://giscus.app/client.js"
        data-repo="gdjide/TheDiscussions"
        data-repo-id="R_kgDONP3u_g"
        data-category="Announcements"
        data-category-id="DIC_kwDONP3u_s4CkTPz"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme="transparent_dark"
        data-lang="zh-CN"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>