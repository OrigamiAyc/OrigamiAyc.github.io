---
layout:		default
title:		快速上手安装 Ubuntu 或者其他操作系统（尤其是在操作系统开课的时候）
date:		03/20/2020
author:		上官凝
catalog:	true
tags:
	- Tutorials & Tricks
---

## 准备工作
### 对于 Ubuntu
首先你得对你要安装的系统有一些了解对吧

对于 Ubuntu，其实有很多种选择的
- win 用户其实可以考虑 win 自带的子系统（如果你真的习惯命令行的话）（不过那上面也可以安装xfce，但是为什么不直接用一个现成的呢···）
	- 优点是轻量，启动、安装什么的速度快，配置要求低，而且天然和 win 的宿主系统有着共享文件的联系，对宿主系统的资源占用很小（因为其实很多情况下都是你那个重量级的图形化界面占用了很多CPU和内存）
- 如果要选发行版的话，第一个编号是偶数的是正式版（换句话说，不要下载那个 19.10，下载 18.04.4，如果以后更新了同理选择）
- 虚拟机安装 [Xubuntu](https://101.ustclug.org/Ch01/#get-vm-images)（而不是默认的 Ubuntu，用的桌面是 GNOME）可以节约超过 0.5 GB 的内存，对于配置较低的电脑还是一个不错的选项
### 还需要什么
- 一台有 4GB 以上**内存**的电脑（内存不是你C盘什么的）（如果你打LOL没问题那基本就OK）
- 一个虚拟机软件
	- win 上面用 vmware workstation pro 即可，官网下载~~然后搜一个注册码~~就可以了（支持正版，前面那个搜码当我没写啊(划掉)）
	- macOS 有经济条件的话首推 parallels desktop，**尤其是要用 win 虚拟机**的亲们
	- 替代的是 vmware fusion，或者 virtualbox（这个的教程看[这里](https://blog.taoky.moe/2019-02-23/installing-os-on-vm.html)）
	- 如果有蜗壳的亲们嫌官网下载太慢的，可以 mail 我（astark@mail.ustc.edu.cn），我rec上分享
- 一个 Ubuntu 的 ISO 光盘映像，可以在[蜗壳开源站](http://mirrors.ustc.edu.cn)或者隔壁[清华tuna](https://mirrors.tuna.tsinghua.edu.cn)找到
- 稳定的网络（因为你安装的时候是需要额外下载的）
## 开始安装
### 安装vmware
- macOS 上，需要注意在系统偏好设置中来同意内核驱动的安装
- win 上，需要重启才能看到 vmware 的图标
### 安装虚拟机
#### macOS
1. 打开之后，顶栏的图标点开，选 “创建新虚拟机”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320163232363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70#pic_center)
2. 选中 “从磁盘或镜像安装”，点 “继续”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320163533201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70#pic_center)
3. 按照提示操作即可（这图是盗的我没有自己下载镜像）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032016383076.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70)
4. 用户名什么的其实随便填，后期可以修改的，不过密码将是你的 `sudo` 密码，一定要记住的
5. 然后他会自动打开 Ubuntu，一路继续就好，地区选上海就行（其实无所谓），注意如果下载过慢，就关网络，安装结束后在系统内重新安装即可
6. 还有不明白的[看这里](https://blog.taoky.moe/2019-02-23/installing-os-on-vm.html)
#### Windows
1. 讲真我不想详细写了（因为有学长写的[很详细](https://ibugone.com/blog/2019/02/setup-ubuntu-in-vmware-cn/)）
2. 如果要说呢，就打开 vmware workstation，选择 “典型”，剩下的跟前面差不多
3. 注意点是磁盘容量最少选 16GB，用单个文件存储，去掉打印机什么的用不到的设备以提高效能
### 进入 Ubuntu
Ubuntu 的界面真的有点丑啊···

先试试 terminal 能不能用。。。真的很关键

Enjoy your Linux life!
## 写在后面
其实···你都有 GNOME 了，为什么不充分利用一下呢。。。

完成之后的界面如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200320171758791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70)

教程我就不写了吧···太长了
看[这个](https://www.cnblogs.com/feipeng8848/p/8970556.html)吧
至于美化的来源，参考这个[org](https://www.pling.com/s/Gnome)
