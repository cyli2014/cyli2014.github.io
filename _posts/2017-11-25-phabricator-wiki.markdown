---
layout: post
title:  "Phabricator Wiki"
date:   2017-11-25 10:57:59
author: Darwin Li
categories: Wiki
tags:	Phabricator
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

## Step 3 - 下载工程所需代码

* Facebook Phabricator源代码

```
git clone https://github.com/phacility/phabricator
```

* docker-compose脚本

```
git clone https://github.com/RedpointGames/phabricator
```

* 本项目所需代码（注意递归clone）

```
git clone --recursive http://gitlab.sjtudoit.com/glory/phabricator
```

## Step 4 - 配置 `docker-compose.yml`文件

假设docker-compose脚本（Code #2）的存储路径为 `/home/username/phabricator2/phabricator` 。打开Code #2中的 `docker-compose.yml` 文件。假设本项目所需代码（Code #3）的存储路径为 `/home/username/phabricator3/phabricator` 。

将代码第10、11行修改为

```yml
 - /home/username/phabricator3/phabricator/repos:/repos
 - /home/username/phabricator3/phabricator/extensions:/srv/phabricator/phabricator/src/extensions
```

在第11行下新增

```yml
 - /home/username/phabricator3/phabricator/scripts:/scripts
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
 - /home/username/phabricator3/phabricator/mysql:/var/lib/mysql
```

保存修改。在终端中输入

```
sudo docker-compose up
```

当出现

```
+ set +e
+ set +x
```

并停滞不动时，表明工作正常。

## Step 5 - 安装配置nginx

在终端输入

```
sudo apt-get install nginx
```

安装完成后，在 `/etc/nginx/sites-enabled/` 文件夹下新建 `config` 文件，复制下列内容：

```
upstream pha_server {
    server 127.0.0.1:62080 fail_timeout=0;
}

upstream pha_rails_server {
    server 127.0.0.1:3000 fail_timeout=0;
}

server {
  listen       10000;
  server_name  localhost;
    
  location ^~ /glory/api {
      proxy_set_header Host $host;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto https;
      proxy_redirect off;
      proxy_pass http://pha_rails_server;
  }

  location / {
      proxy_set_header Host $host;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto https;
      proxy_redirect off;
      proxy_pass http://pha_server;
  }
}
```

完成后，在终端输入

```
sudo nginx -t
```

出现

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

之后重启nginx。

```
sudo nginx -s reload
```

## Step 6 - 安装配置RVM

在终端输入

```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
```

重启终端以使设置生效，在终端输入

```
rvm -v
```

测试RVM是否正常安装。

## Step 7 - 安装配置Ruby

在终端输入

```
rvm install 2.4
```

安装完成后，打开 `~/.bashrc` 文件，在最后添加

```
# Add RVM to PATH for scripting. Make sure this is the last PATH variable change.
export PATH="$PATH:$HOME/.rvm/bin"

[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" 
```

重启终端以使设置生效，之后在终端输入

```
rvm use 2.4 --default
ruby -v
```

测试Ruby是否正常安装。若能正常显示版本号，我们将Ruby调整为国内的镜像。在终端中输入

```
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
gem sources -l
```

确保只有 `https://gems.ruby-china.org/` 一个源。

之后在终端输入

```
gem install bundler
bundle config mirror.https://rubygems.org https://gems.ruby-china.org
```

以安装Ruby的包管理器Bundler并修改为国内镜像。之后转到本项目代码（Code #3)下安装所需的包。假设Code #3的路径为 `/home/username/phabricator3/glory`，在终端输入

```
cd /home/username/phabricator3/glory
bundle
```

即可。

## Step 8 - Test Your Environment

To be continued.
