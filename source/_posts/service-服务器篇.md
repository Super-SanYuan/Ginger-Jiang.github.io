---
title: service-服务器篇
date: 2020-04-25 12:54:24
tags:
  - CI&CD
categories:
  - CI&CD
---

# 云服务器

每个程序员都应该有一台自己的云服务器

> [知乎-自己拥有一台服务器可以做哪些很酷的事情](https://www.zhihu.com/question/40854395)

服务器厂商:

- [腾讯云](https://cloud.tencent.com/)
- [阿里云](https://cn.aliyun.com/)
- [新浪云](https://www.sinacloud.com/)
- [华为云](https://www.huaweicloud.com/)

## 购买服务器

> 注: 下面流程以我自己购买的腾讯云服务器为例

点击[腾讯云链接](https://cloud.tencent.com/)进入腾讯云首页, 点击右上角登录进行登录

在首页轮播图常年有活动, 新用户与学生一般是活动优惠力度最大的

点击轮播图活动进入后, 按照需求选择我们需要的服务器配置, 如果是个人学习、博客等使用, 最基本的即可

![服务器](../images/service-ci-cd/01/01-01.png)

点击立即抢购之后, 跳入一个配置选择界面, 选择离自己所在地最近的城市, 选择机型配置, 选择 CentOS 系统

![服务器](../images/service-ci-cd/01/01-02.png)

点击**立即购买**即可跳转付款页面, 核对无误后付款即可

## 控制台

在腾讯云首页右上角有一个控制台按钮, 点击即可进入控制台

![服务器](../images/service-ci-cd/01/01-02.png)

点击服务器后将会跳转至实例界面(如果没有实例就创建一个)

![实例](../images/service-ci-cd/01/01-04.png)

> 首次购买后会自动跳转至服务器控制台

## 登录远程服务器

> 以 MacOS 为例

打开终端, 输入 `ssh root@xx.xx.xx.xx` 回车

```shell
ssh root@xx.xx.xx.xx
```

> xx.xx.xx.xx 替换为自己服务器公网 ip 地址

弹出提示要求输入密码

> 输入密码不会显示, 输入完成后直接回车即可

![ssh](../images/service-ci-cd/01/01-05.png)

如果看到如下界面就表示密码输入成功

![ssh](../images/service-ci-cd/01/01-06.png)

输入 `cat /etc/redhat-release` 查看系统信息

```shell
cat /etc/redhat-release
```

![ssh](../images/service-ci-cd/01/01-07.png)

## 配置免密验证

在使用上面的命令登录远程服务器的时候, 是需要输入密码的, 而且以后每次访问都需要输入, 这样就很繁琐.

我们可以通过---ssh 免密验证

### 在本地生成秘钥

通过如下命令在本地机器生成秘钥

> 如果你使用过 git 配置, 就无需重复生成

```shell
# -t 指定加密算法； -b 指定生成的密钥长度； -C 一句话，一般都填邮箱地址。
# 更多参数说明可以在终端输入：ssh-keygen --help 查看
ssh-keygen -t rsa -b 4096 -C "你的邮箱地址"
```

这条命令执行完之后, 会提示指定公钥和私钥在本地机器上储存的位置, 因为我本地已经有秘钥了, 就从网上找了一张图

![ssh](../images/service-ci-cd/01/01-08.png)

### 将公钥储存到远程服务器主机

输入以下命令, 然后回车, 将上一步我们在本地机器上生成的公钥写入服务器

```shell
# i后面接的参数是保存你公钥的文件(我们这里是.ssh/id_rsa.pub)；
# root是远程主机的用户，这条指令会往root这个用户的主目录下的
# .ssh/authorized_keys文件写入id_rsa.pub保存的公钥
ssh-copy-id -i .ssh/id_rsa.pub root@xx.xx.xx.xx
```

![ssh](../images/service-ci-cd/01/01-09.png)

根据提示, 输入服务器主机登录密码

![ssh](../images/service-ci-cd/01/01-10.png)

如果看到如下提示, 表示添加成功

![ssh](../images/service-ci-cd/01/01-11.png)

此时, 我们再使用 `ssh root@xx.xx.xx.xx` 对服务器进行访问, 就能直接登录, 不需要再输入密码了

![ssh](../images/service-ci-cd/01/01-12.png)
