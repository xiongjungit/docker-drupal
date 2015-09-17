docker-drupal
=============

## 1. 简介

![](http://pic002.cnblogs.com/images/2012/307487/2012051501265445.jpg)

[Drupal](https://www.drupal.org/)是使用PHP语言编写的开源内容管理框架（CMF），它由内容管理系统（CMS）和PHP开发框架（Framework）共同构成。连续多年荣获全球最佳CMS大奖，是基于PHP语言最著名的WEB应用程序。截止2011年底，共有13,802位WEB专家参加了Drupal的开发工作；228个国家使用181种语言的729,791位网站设计工作者使用Drupal。著名案例包括：联合国、美国白宫、美国商务部、纽约时报、华纳、迪斯尼、联邦快递、索尼、美国哈佛大学、Ubuntu等。

## 2. 组件

> 
drupal版本

> 
* [7.39](http://drupalchina.cn/download)

> 
* [8.0](http://drupalchina.cn/download)

此仓库为创建Drupal的docker镜像, 使用ubuntu 14.04基础镜像创建, 包含以下必要组件

* Apache

* PHP

* MySQL

* APC

* memcache

## 3. 安装步骤

> 可以直接pull运行我制作好的镜像

* dockerhub上的镜像(速度慢)
```
docker run -itd -p 80:80 --name drupal dockerxman/docker-drupal
```

* 道客云上的镜像(速度快)
```
docker run -itd -p 80:80 --name drupal daocloud.io/xiongjun_dao/docker-drupal 
```

* 时速云上的镜像(速度快)
```
docker run -itd -p 80:80 --name drupal docker pull index.tenxcloud.com/dockerxman/docker-drupal
```

### 3.1. 结束所有正在运行的docker进程

```
sudo killall docker
```

### 3.2. 安装 docker:

```
curl get.docker.io | sudo sh -x
```

### 3.3. 克隆这个仓库到任意地方 

```
git clone https://github.com/ricardoamaro/docker-drupal.git
cd docker-drupal
```

### 3.4. 构建drupal的docker镜像:
```
sudo docker build -t <yourname>/drupal .
```

这可能需要一段时间,但最终应该返回一个命令提示符(`Successfully built {hash}`)。

### 3.5. 运行容器并连接80端口:

```
sudo docker run -d -t -p 80:80 <yourname>/drupal
```
在你的浏览器访问`http://localhost/`。

注意:如果80端口已经被占用容器将无法启动。
在这种情况下,你可以通过设置:`-p 8080:80`

**还可以直接从github构建drupl镜像**

```
sudo docker build -t <yourname>/drupal git://github.com/xiongjungit/docker-drupal.git
```


## 4. 初始用户和密码

* <font color=red>mysql数据库root用户密码保存在`/mysql-root-pw.txt`(随机生成)里面

* mysql数据库drupal用户密码保存在`/drupal-db-pw.txt`(随机生成)里面

* drupal 用户名=admin 密码=admin</font>


## 5. 更多docker技巧

这将创建一个ID,可以启动/停止/提交修改:

```
# sudo docker ps
ID                  IMAGE                   COMMAND               CREATED             STATUS              PORTS
538114c20d36        <yourname>/drupal:latest   /bin/bash /start.sh   3 minutes ago       Up 6 seconds        80->80  
```

**开始/停止**

```
sudo docker stop 538114c20d36
sudo docker start 538114c20d36
```

**提交修改到镜像**
```
sudo docker commit 538114c20d36 <yourname>/drupal
```

**提交修改后重新开始**

```
sudo docker run -d -t -p 80:80 <yourname>/drupal /start.sh
```

**一次封装，到处运行**

```
sudo docker push  <yourname>/drupal
```

**你可以在 [Docker Index][docker_index]找到更多的镜像**。

```
docker search ricardoamaro/drupal-lamp
```
## 6. 清理

使用下面的命令删除旧的实例

```
sudo docker ps -a | awk '{print $1}' | grep -v CONTAINER | xargs -n1 -I {} docker rm {}
``` 

## 7. 数据持久化

下面这条命令是显示挂载的mysql数据库文件夹路径

```
docker inspect -f {{.Volumes}} 538114c20d36
```
需要备份mysql数据库的话可以把此目录进行备份。

也可以[下载phpmyadmin](https://www.phpmyadmin.net/downloads/)进行数据库管理。

## 8. drupal配置

### 8.1 中文语言设置

可以先安装英文版，再导入中文包。

英文版的安装就不再赘述，下面介绍一下如何导入中文包。



1. 在英文版安装好了以后，点击顶部的菜单“Modules”，进入模块管理页面(admin/modules)，找到“Locale”模块，将其开启。

2. 下载drupal中文语言包[7.39](http://ftp.drupal.org/files/translations/7.x/drupal/drupal-7.39.zh-hans.po)或者[8.0](http://ftp.drupal.org/files/translations/8.x/drupal/drupal-8.0.0-beta14.zh-hans.po)

3. 接着点击“Configuration” > “Languages”，进入语言管理界面(admin/settings/language)。

4. 点击 Add Language 连接，选择 Chinese, Simplified（简体中文），接着点击“Configuration” > “Translate interface”，再点击“Import”，在“Language file”下选择本地已下载的drupal7中文包，接着点击“Import”按钮，即可开始导入中文包。

5. 到 admin/settings/language 目录下，将简体中文设为“Default”即可。


## 8. 贡献

创建分支步骤

1. fork仓库

2. 创建您的分支 (`git checkout -b my-new-feature`)

3. 提交您的更改 (`git commit -am 'Added some feature'`)

4. 推送到分支 (`git push origin my-new-feature`)

5. 创建新的pull请求

## 作者

创建和维护 [xiongjungit][author]

QQ: 479608797

邮箱: fenyunxx@163.com

## License
GPL v3

[author]:                 https://github.com/xiongjungit
[docker_index]:           https://index.docker.io/

## 演示地址

[灵雀云drupal演示网站](http://drupal-dockerxman.myalauda.cn)

