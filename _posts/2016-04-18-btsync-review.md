---
layout: post
title: "btsync 体验"
tagline: "同步好助手"
description: ""
category: 产品体验
tags: [btsync, 产品体验]
last_updated: 
---

全称 BitTorrent Sync , 我习惯了叫他 btsync 了。想要了解他的前世今生直接去看维基[百科](https://zh.wikipedia.org/wiki/BitTorrent_Sync)就好了。一句话概括，他就是一个同步工具，类Dropbox，但是利用 P2P等等 bt 种子的技术。当然私人使用当成网盘工具也好，当成分享工具也好，看个人使用了。不过黑客提醒，虽然是去中心化的，但是安全性依然[存在问题](http://www.networkworld.com/article/2848723/microsoft-subnet/hackers-claim-bittorrent-sync-should-not-be-used-for-sensitive-data.html)，最好不要传输私人信息。

官网地址： <https://www.getsync.com/>

## 全平台

现在使用任何一个工具或者服务，我首要考虑的问题离不开跨平台了，最好是Windows , Linux, Mac 下全部都有，不然在平台间来回切换不同的服务和工具时间成本，学习成本太高了。也正是因为这个原因我放弃了 Google Drive 而转用 Dropbox，作为主力同步工具。当然 btsync 在全平台都有客户端，甚至连一些 NAS，路由器设备都有。


## 安装

安装非常简单，去官网下载，下一步下一步，OK。当然 Linux 下，如果不想使用 下一步下一步安装法，也可以使用命令从 PPA 里拖。

### PPA

	sudo add-apt-repository ppa:tuxpoldo/btsync
	sudo apt-get update

For normal desktop use, you only need to install `btsync-user`:

	sudo apt-get install btsync-user

Alternatively, if you're setting up your BTSync server, install `btsync`:

	sudo apt-get install btsync

### btsync client

在官网根据自己的机器选择合适的 client 下载并解压。并运行：

	./btsync

即可。

默认的Web GUI地址是 ： <http://127.0.0.1:8888>

更加详细的安装指南可以参考[这篇](https://www.digitalocean.com/community/tutorials/how-to-use-bittorrent-sync-to-synchronize-directories-in-ubuntu-12-04)


### VPS上架设

类似 Linux 下安装，官网下载并解压 btsync 文件。

	tar -zxvf BitTorrent-Sync_x64.tar.gz

然后执行：

	./btsync --dump-sample-config > btsync.conf

创建配置文件，然后修改 `btsync.conf` 配置文件中的：

	"listen" : "0.0.0.0:8888" 

还有 `login` 和 `password` , 端口默认是8888，可修改成其他没有冲突的。`login` 和 `password` 是登陆用户名和密码。其他配置看注释修改即可。参考官网 config [文章](http://help.getsync.com/hc/en-us/articles/204762689-Running-Sync-in-configuration-mode).

然后保存配置文件，启动：

	./btsync --config btsync.conf

在浏览器中就能够在 http://ip:port/ 访问 Web GUI。

然后在本地获取同步 key ，和VPS 上同步即可。

## 技巧 {#skills}

### 移动同步后的文件夹

如果你已经同步了一个文件夹，比如在 `~/books`，现在想要将该同步的目录移动到 `~/btsync/books` 目录下。 就像Dropbox [同步已经存在的文件夹](/post/2015/07/dropbox-sync-with-exist-folder.html)一样，如果单纯的再重新下载一边太麻烦了。所以幸好 btsync 和 dropbox 都有这样的性质，同步的内容都有文件记录，将文件重新加入索引，等索引完之后就可以继续和其他的文件同步了。

1. 拷贝该文件夹的"共享秘钥"
2. 从 btsync 中移除该文件夹
3. 在本地硬盘移动文件夹到新的位置
4. 重新在 btsync 用之前的”共享秘钥“，添加该文件夹

### VPS 上启用 https

默认 btsync 的 web gui 是没有启用加密的，如果想要使用 https://ip:port/gui 来访问，则需要使用 config 配置，并在config 配置中设置 `force_https`, `ssl_certificate`,`ssl_private_key` ，然后重启 btsync 。

如果觉得这样让 btsync 直接获取证书不安全，[这里](http://askubuntu.com/a/538312/407870) 还有另外一种配置，利用 nginx 的代理。


### 分享秘钥的网站

- <http://btsynckeys.com/>



## reference

- <https://www.digitalocean.com/community/tutorials/how-to-use-bittorrent-sync-to-synchronize-directories-in-ubuntu-12-04>
- <http://askubuntu.com/a/296130/407870>
- <https://program-think.blogspot.com/2015/01/BitTorrent-Sync.html>
- <http://dafacto.com/2013/how-to-move-folders-that-are-synced-with-bittorrent-sync/>
- <http://askubuntu.com/a/538312/407870>
