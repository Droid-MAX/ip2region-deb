# ip2region-deb

本项目用于打包 `ip2region` 为可被Debian/Armbian/Ubuntu等Linux发行版系统直接安装的deb软件包，且在原有的 `ip2region` 基础上，加入了兼容系统服务的管理脚本，极大的提高的软件的易用性。

## 服务管理

软件包在安装完毕之后，默认将 `ip2region` 服务设为开机自启。可以手动切换 `ip2region` 服务的开机自启状态。

1. 将 `ip2region` 服务设为开机自启，操作如下：

`sudo systemctl enable ip2region.service`

2. 将 `ip2region` 服务取消开机自启，操作如下：

`sudo systemctl disable ip2region.service`

3. 启动/停止/重启/查看状态，操作如下：

`sudo systemctl start ip2region.service`、 `sudo systemctl stop ip2region.service`、 `sudo systemctl restart ip2region.service`、 `sudo systemctl status ip2region.service`

4. 默认配置文件： `/etc/ip2region.conf`

其中 `DBPATH` 为默认数据库路径，`HOST` 为默认监听地址，`PORT` 为默认监听端口。

5. 日志查看：

使用命令 `journalctl -xe | grep ip2region` 来查看并过滤由 `ip2region` 产生的日志。

## 如何构建

在开始构建deb软件包之前请务必安装 `fakeroot` 工具(推荐)，或切换到root管理员身份操作

首先将本项目克隆到本地环境当中(注意在打包前删除项目目录下的.git文件夹，README.md以及LICENSE文件，以免将不必要的文件打包到安装包当中!)，根据需要自行从网上下载对应版本的 `ip2region` 预编译发布应用包，并将其解压到本地目录当中，然后将可执行程序 `ip2region` 复制到 `ip2region-deb/usr/bin/` 目录下(注意赋予可执行权限). 最后切换到 `ip2region-deb` 所在的目录下，执行 `fakeroot dpkg-deb -b ip2region-deb ip2region-1.0-arm64.deb` (注意这里的架构请依据实际情况加以修改) 或切换到root管理员身份执行 `dpkg-deb -b ip2region-deb ip2region-1.0-arm64.deb` . 稍等片刻即可生成最终的deb安装包文件。

## 查询接口

查询访问者的IP地区:访问http://{服务器IP}:{指定的端口，默认8080}/ip

查询任意IP地区:访问http://{服务器IP}:{指定的端口，默认8080}/ip?ip={待查询的IP}
