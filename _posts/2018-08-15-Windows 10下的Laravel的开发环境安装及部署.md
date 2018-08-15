---
title: Windows 10下的Laravel的开发环境安装及部署
date: 2018-08-15 12:00:00
description: 对于一个偏爱Windows的人来说，能够在Windows操作环境下进行程序的开发，无异于一大享受。那么今天我们一起来看一下如何在Windows 10操作系统中进行Laravel的开发。
categories:
 - PHP
tags:
 - php
 - windows
 - laravel
 - vagrant
 - homestead
---



> 对于一个偏爱Windows的人来说，能够在Windows操作环境下进行程序的开发，无异于一大享受。那么今天我们一起来看一下如何在Windows 10操作系统中进行Laravel的开发。
当然，方法不止一种，如果你的电脑上已经下载了PHP，安装了Apache或nginx，并且有偏爱的数据库系统，那么基本上你已经可以开始写代码了。而我们今天要介绍的是使用Vagrant，在Homestead中进行Laravel的开发。
Vagrant是一款基于Virtual Box的虚拟机环境，可以安装多种Box（即预先配置好的开发环境），达到多种开发环境可以快速切换并且不需要担心Package Dependencies。

> Homestead是Laravel官方创建的Vagrant Box，已经集成了Laravel所需的开发环境及工具。是不是感觉发现了新大陆？

那么我们需要做的几个步骤如下：
 - 下载并安装 `Vagrant` 及 `Virtual Box` 
 - 下载 `Homestead` 并配置
 - 创建一个新的 `Laravel` 项目。
 - 我们需要的：
	- `Git Bash`
	- `Vagrant`
	- `Virtual Box`
	- `Homestead`

---

> 注意： Laravel官方推荐的Shell是Git Bash。因为git Bash自动将 ~（tilde）映射到用户根目录下（例如，C:\Users\MyUserName），而如果使用Windows自带的cmd，那么需要使用%HOMEDRIVE%%HOMEPATH%环境变量来进入正确的文件夹。

---

好了那么我们开始行动！首先需要我们先下载 [Git Bash](https://git-for-windows.github.io/)

直接下载可能很慢，我们可以右键复制下载链接，使用迅雷等下载工具下载。

安装 `Git` 很简单，按默认选项安装就可了： 

安装完成后，我们就可以进行下一步了：
 
安装 [Vagrant](https://www.vagrantup.com/downloads.html) 及 [Virtual Box](https://www.virtualbox.org/wiki/Downloads)

---

完成之后，我们就可以开始安装 `Homestead` 了。
如果你在国外，或者你所在的地区对于CLI方式下载安装 `Homestead` 网速没有问题，那么可以打开 `Git Bash` ,输入

```
$ vagrant box add laravel/homestead
```

会自动开始下载安装最新版本的 `Homestead`

然而国内可能部分地区用这条命令下载网速非常的慢。所以如果发现半天下载不下来，那么我们可以采取手动下载安装的办法：
首先先去 `Vagrant` 官网上查询一下 [最新的Homestead版本](https://atlas.hashicorp.com/laravel/boxes/homestead)

然后根据版本号使用以下链接下载 `Homestead` ：

[下载链接](https://atlas.hashicorp.com/laravel/boxes/homestead/versions/2.0.0/providers/virtualbox.box)

可以看到，目前最新的版本是 `2.0.0` ,各位可以根据需要在 `versions/` 之后把需要下载的版本号替换上去。

温馨提示：上面的链接可以迅雷喔，是不是很方便呢？
当然，这个下载地址就是在 `Git Bash` 中输入 `vagrant box add laravel/homestead` 后自动下载的地址，只是我把它复制出来了。

---

好了，那么如果以上步骤是自动下载的，那么可以直接跳过此步，如果是手动下载的 `Homestead` ,那么请继续跟我一起操作。

在以上手动步骤里，我们下载了最新的 `Homestead` 版本，请到下载文件夹中，将下载的文件 `hc-download` 重命名为 `homestead.box` （前缀不重要，但是一定要加上 `.box` 后缀）。

接下来打开 `Git Bash` 输入

```
$ vagrant box add laravel/homestead file:///c:/users/(UserName)/downloads/homestead.box
```

其中请用你系统的用户名替换 `UserName` ,用自己的文件下载路径替换 `/downloads/homestead.box` ,然后等待添加完成。 

完成后，你会发现自己的用户文件夹里多出了一个 `.vagrant.d` 的文件夹 `C:\Users\(UserName).vagrant.d`

接下来，非常重要的一个步骤：

我们打开这个文件夹，找到以下路径：

```
C:\Users\(UserName)\.vagrant.d\boxes\laravel-VAGRANTSLASH-homestead
```

在该文件夹下新建一个叫 `metadata_url` 的文件： 

文件的内容里，添加以下链接：

```
https://atlas.hashicorp.com/laravel/homestead
```

注意，不要留任何空白字符，保存文件。
完成之后，我们就可以开始配置 `Homestead` 了。

---

在 `Git Bash` 里输入

```
$ git clone https://github.com/laravel/homestead.git ~/Homestead
```

下载好了 `Homestead` 之后，我们就可以建立 `Homestead.yaml` 配置文件了：

进入 `Git Bash`

```
# 先cd到刚才下载的Homestead文件夹
$ cd ~/Homestead

# 开始初始化Homestead
$ bash init.sh
```

然后在 `~/Homestead` 文件夹中，我们可以看到多出了一个叫 `Homestead.yaml` 的文件。使用 `notepad++` 打开它，可以看到内容如下：

```
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/Code
      to: /home/vagrant/Code

sites:
    - map: homestead.app
      to: /home/vagrant/Code/laravel/public

databases:
    - homestead

# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```

我们一条一条来配置。 
首先我们需要建立 `SSH Key` 并且填入路径。在 `Git Bash` 中，输入：

```
$ ssh-keygen -t rsa
```

完成后，我们会在用户根目录下看到一个 `.ssh` 文件夹，里面分别有

```
id_rsa.pub
id_rsa
```

两个文件，分别对应配置文件中

```
authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

```

---

接下来我们看到这两段配置

```
folders:
    - map: ~/Code
      to: /home/vagrant/Code

sites:
    - map: homestead.app
      to: /home/vagrant/Code/laravel/public

```

其中 `folders` 中，将 `map` 后面的文件夹 `~/Code` 映射到 `Homestead` 中的 `/home/vagrant/Code` 。这就像我们常见到的和虚拟机中系统共享文件夹类似, 我们把本机的 `~/Code` 文件夹分享给 `Homestead`, 所以我们可以很方便地使用自己喜欢的 `IDE` (比如 `PhpStorm` ) 进行开发，在 `~/Code` 中所进行的修改会如实反应在 `Homestead` 对应的文件夹中。在 `sites` 中，我们定义了 `homestead.app` 指向 `/home/vagrant/Code/laravel/public` 这个文件夹，即 `Laravel` 项目的 `public` 文件夹。这样在浏览器中输入 `homestead.app`, 我们就可以直接看到项目主页了。


> 注意：以上的文件夹及映射是可以根据用户喜好更改的，比如我喜欢把我的开发文件夹叫做 WebDev，在里面我新建了一个叫 teahouse 的项目，那么以上的配置，我就可以做出相应修改：

```
folders:
    - map: ~/Webdev
      to: /home/vagrant/Webdev

sites:
    - map: teahouse.app
      to: /home/vagrant/Webdev/teahouse/public

```
是不是很方便呢？

---

下一步我们要在 `Windows` 的 `C:\Windows\System32\drivers\etc\hosts` 文件中添加一行：

```
...

192.168.10.10 homestead.app

...
```

用 `notepad++` 打开即可：

```
# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host
# localhost name resolution is handled within DNS itself.
#   127.0.0.1       localhost
#   ::1             localhost

# 添加上这一行，将homestead.app替换成自己的项目名称
192.168.10.10 homestead.app
```

---

接下来我们来看

```
databases:
    - homestead
```

在这里，我们每添加一个数据库名字，那么 `Homestead` 将会自动创建一个该名字的数据库来供我们操作。

在这里，我添加了一个新的数据库：

```
databases:
    - homestead
    - teahouse
```

> 注意：不要使用tab来留空，否则会报错（可以是用空格对齐）。默认的MySQL服务器用户名为homestead，密码是secret。

---

将 `Homestead.yaml` 保存，我们就可以启动 `Vagrant` 了！激动吗？

以管理员的身份打开 `Git Bash`, 输入

```
$ cd ~/Homestead
$ vagrant up
```

`Vagrant` 已经顺利运行了！ 

接下来，要进入 `Homestead`, 我们输入

```
$ vagrant ssh
```

---

还差一步，我们就大工告成：

```
# cd进入我们共享的文件夹
$ cd /home/vagrant/Code

# 创建一个新的laravel项目
$ laravel new new_project
```

现在开始，我们可以开始 `sLaravel` 的开发了！

---

另外，如果在安装过程中出现以下错误：

```
Vagrant was unable to mount VirtualBox shared folders. This is usually because the filesystem “vboxsf” is not available. This filesystem is made available via the VirtualBox Guest Additions and kernel module. Please verify that these guest additions are properly installed in the guest. This is not a bug in Vagrant and is usually caused by a faulty Vagrant box. For context, the command attempted was:
mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant
```

解决办法如下（已测试）：

降级 `VirtualBox` 或者修改以下目录下的文件中的第 `206` 行： 

`C:\HashiCorp\Vagrant\embedded\gems\gems\vagrant-1.9.2\lib\vagrant\util\platform.rb`

在我的配置文件中，第 `206` 行为 

```
“\\?\” + path.gsub(“/”, “\”) 
```

我们只需要把前面的代码删掉，留下 

```
path.gsub(“/”, “\”) 
```

再运行 `vagrant up` 即可。

在 `stackoverflow` 上有人说自己的配置为：

```
Before edit: “\?\” + path.gsub(“/”, “\”) 
After edit: path.gsub(“/”, “\”)
```

如果有一样的配置，按照上面的方法操作即可。