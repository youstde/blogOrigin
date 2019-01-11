---
title: 服务端安装git
date: 2017-03-19 21:57:21
tags:
---

#### (1)首先先更新系统

```
sudo yum update
```

#### (2)安装依赖的包

```
sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```

#### (3)下载git源码并解压缩

```
$ wget https://github.com/git/git/archive/v2.3.0.zip

$ unzip v2.3.0.zip

$ cd git-2.3.0

```

#### (4)编译安装

将其安装在“/usr/local/git”目录下。

```
make prefix=/usr/local/git all

sudo make prefix=/usr/local/git install

```

#### (5)此时你如果使用git --version 查看git版本的话，发现git仍然是1.7.1版本。这是因为它默认使用了"/usr/bin"下的git。

你可以用下面的命令查看git所在的路径：

```
$ whereis git

git: /usr/bin/git /usr/local/git /usr/share/man/man1/git.1.gz

```

#### (6)我们要把编译安装的git路径放到环境变量里，让它替换"/usr/bin"下的git。为此我们可以修改“/etc/profile”文件（或者/etc/bashrc文件）。

```
sudo vim /etc/profile
```

然后在文件的最后一行，添加下面的内容，然后保存退出。
export PATH=/usr/local/git/bin:$PATH

#### (7)使用source命令应用修改。

```
source /etc/profile
```

#### (8)然后再次使用git --version 查看git版本，发现输出2.3.0，表明安装成功。