---
layout: post
title:  "Phabricator Development Environment"
date:   2017-11-25 10:57:59
author: Darwin Li
categories: Wiki
tags:	phabricator
cover:  "/assets/welcome.jpg"
---

# Phabricator 环境配置

## Before You Start

* 请勿使用任何家庭版Windows系统（如Windows 10家庭中文版），包括依托于此系统的任何虚拟机
* 推荐使用Linux的任一发行版及Mac OS，不推荐使用Windows系统配置该项目所需环境
* 该教程以Ubuntu 16.04 LTS为基础，不同发行版本的Linux命令有细微差别
* 推荐IDE: VS Code、Sublime 3、Webstorm（收费）

## Step 1 - 安装Docker及Docker-Compose

在终端输入

```
sudo apt-get install docker.io
sudo apt-get install docker-compose
```

为了加快镜像拉取速度，我们修改（或新建） `/etc/docker/daemon.json` 文件并添加上 `registry-mirrors` 键值。

```json
{
    "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

修改保存后使用
```
sudo service docker restart
```

命令重启Docker以使设置生效。

## Step 2 - 安装Phabricator与MySQL

在终端输入

```
sudo docker pull redpointgames/phabricator
sudo docker pull mysql:5.7.14
```

### Step 3 - 下载工程所需代码

* Facebook Phabricator源代码

```
git clone https://github.com/phacility/phabricator
```

* 

```
git clone https://github.com/RedpointGames/phabricator
```

* 

```
git clone http://gitlab.sjtudoit.com/glory/phabricator
```

### Step 4 - 配置 `docker-compose.yml`文件

假设第二份代码的存储路径为 `/home/username/phabricator2/phabricator` 。打开第二份代码中的 `docker-compose.yml` 文件。

将代码第10、11行修改为

```yml
 - /home/username/phabricator2/phabricator/repos:/repos
 - /home/username/phabricator2/phabricator/extensions:/srv/phabricator/phabricator/src/extensions
```

在第11行下新增

```yml
 - /home/username/phabricator2/phabricator/scripts:/scripts
```

在第17行下新增

```yml
 - MYSQL_LINKED_CONTAINER=MYSQL
 - MYSQL_PORT_3306_TCP_ADDR=mysql
 - MYSQL_PORT_3306_TCP_PORT=3306
```

在21行下新增

```yml
 - MYSQL_PORT=3306
```

修改第26行为

```yml
 - PHABRICATOR_HOST=127.0.0.1:62080
```

在第26行下新增

```yml
 - SCRIPT_BEFORE_DAEMONS=/scripts/set_config.sh
```

修改第32行为

```yml
 - /home/username/phabricator2/phabricator/mysql:/var/lib/mysql
```

保存修改。在终端中输入

```
sudo docker-compose up
```

测试是否正常工作。

### Step 5 - To Be Continued