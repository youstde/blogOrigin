---
title: linux常用命令
date: 2018-05-25 16:01:43
tags:
---

#### linux代码压缩上传

假如项目在Downloads文件夹下

项目目录为test

1.cd到项目文件夹的上层目录

```
cd Downloads
```

2.查看文件列表

```
ls
```

3.删除之前压缩过的文件（如果有的话）

```
rm test.tar
```

4.同2

5.

```
tar -cvf test.tar  test     (将项目打包)
```

6.同2

7.

```
scp test.tar root@adbaitai.com:/home/admin/lgweb/ssp
```

8.

```
ssh root@adbaitai.com
```

9.

```
cd /home/admin/
ls
```

11.

```
cd lgweb/ssp/
ls
```

12.

```
cp test.tar ../    (复制文件到指定路径) 待定！
cd ../
ls
cd ssp/
ls
rm -rf *			(将线上项目目录下的所有文件都删除)
cd ../
ls
tar -xvf test.tar  (解压项目压缩包)
ls
rm test.tar        (删除项目的压缩包)
y
ls
cd ../
ls
cp -R test/* ssp   (将项目文件夹下的文件全部放到ssp文件夹下)
ls
cd ssp/
ls
cd ../
ls
rm -rf test/     (删除项目的解压包)
```



先将项目打包，然后发到服务器上，然后解压到项目存放的上层目录中，将压缩包删除，把解压的文件附到项目目录中，最后删除项目解压的文件，完事





cd /etc/nginx/conf.d/

 vim default.conf













```
scp -r * admin@robot.adbaitai.com:/home/admin/wx/
```
