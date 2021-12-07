\---

title: Ubantu下webd网盘的安装

toc: True

tag: webd, 网盘

category: Linux

date: 2021-12-07

\---

# Ubantu下webd网盘的安装

[详细网址](http://www.webd.cf)

### 1. 下载

基于操作系统下载相应的压缩包，linux下下载linux_x86-64.tar.gz即可

### 2. 解压

tar -zxvf 文件名

###  3. 配置

```bash
  cp -fv webd/webd /usr/bin/
  cp -fv webd/webd.conf /etc/
  rm -rf /tmp/webd /tmp/${pkg} # 删除无用的残留文件,可不删
```

```bash
 # 假设要把硬盘挂载目录 /mnt/sda1 当作网盘目录
  mkdir -pv mkdir -p /mnt/sda1/.Trash # 创建回收站文件夹，否则不能删除文件
```

对webd.conf文件进行配置，配置文件介绍如下：

```bash
配置文件：
  webd 启动时会在当前目录和 /etc 下查找并加载 webd.conf 文件。
  编辑 webd.conf 去掉行首的 # 可让改行配置生效。
  含有空格的路径需用英文双引号包起来。

  Webd.Root 指定网盘文件的路径
    更改后需移动原 web 目录下的 .Trash 文件夹到新路径下，否则无法删除文件
  Webd.Listen 监听端口或特定的地址，支持多个，可配置成 [::]:9212 来同时监听 IPv6 和 IPv4
  Webd.Hide 隐藏托盘图标, 无参数，该项仅支持 Windows
  Webd.User 设置用户的权限、用户名和密码，支持两个用户，但使用同一目录
    比如 Webd.User rlumS user1 pass1 表示设置 user1 的密码为 pass1 ，具有 r、l、u、m、S 五种权限。
    其中 r 表示访问文件，l 表示获取文件列表，u 表示上传文件，m 表示删除移动重命名文件，S 表示显示隐藏文件。
    可赋于用户任意单个或多个权限，任意组合，灵活配置。
  Webd.Guest 设置无需登录的访客权限，参考上面的权限组合；设置成 0 表示禁用访客。
  Webd.Browser 用于指定自定义的浏览器路径；该项还能解决双击托盘图标无法弹出界面的问题。
```

### 4. 启动

命令：

```bash
 ./webd -w /mnt/sda1 -u rlum:user:pass
```

PS：开机启动的脚本设置后无法运行，所以就暂时跳过。