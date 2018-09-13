## Git安装

### Linux系统

在有 yum 的系统上（比如 Fedora）或者有 apt-get 的系统上（比如 Debian 体系）,可以用如下命令安装：

**Debian/Ubuntu**

```shell
# 安装git
apt-get install git

# 检查git版本号
git --version
``` 

**Centos/RedHat**

```shell
# 安装git
yum -y install git-core

# 检查git版本号
git --version
```

* **源码安装**

Git的工作需要调用 curl, zlib, openssl, expat, libiconv 等库的代码，需要先安装这些依赖工具。

可以从 [Kernel.org](https://www.kernel.org/pub/software/scm/git) 获取git源码或从[GitHub 网站](https://github.com/git/git/releases)上的镜像来获取。

```shell
# CentOS/RedHat
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel

# Debian/Ubuntu
apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev

# 编译安装
tar -zxf git-2.0.0.tar.gz
cd git-2.0.0
make configure
./configure --prefix=/usr
```

git升级
```shell
git clone git://git.kernel.org/pub/scm/git/git.git
```

### Windows系统

[下载安装包](https://git-scm.com/)



### Mac系统