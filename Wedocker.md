# Wedocker

Tags： Docker

---

## 本地php开发环境docker镜像步骤

### Windows 10 运行步骤
1. 首先开启windows 10 H-v（开启windows 自带虚拟机）
2. 安装 docker 软件（百度搜索docker for windows）
3. 安装 docker-compose（命令行工具）
4. 选择合适的项目文件夹（E：Wedocker）
5. 本地WeDocker文件夹下面新建sites文件夹
6. 修改nginx配置文件中的虚拟站点（域名）和项目文件夹地址
    > Wedocker/nginx/conf.d/nginx.conf

7.	修改docker-compose.yml文件中的项目目录。
    > **E:\WeDocker\sites**:/sites:rw 加粗部分为你的电脑路径

8.	回到有后缀为yml的文件夹中，执行如下命令，启动docker：
    > `Docker-compose up`

9.	单独启动nginx容器（如果需要的话）
    > docker-compose restart nginx

10.	进入相应的容器目录，
    > docker exec -it wedocker_php_1 /bin/sh

11.	打开浏览器，输入刚才修改的虚拟站点。验证是否可以正确访问项目首页。

# 深度说明

## 运行之前

### nginx

* `server_name` 选项改为自己设定的域名
* `root` 选项中 `/sites` 这个前缀是在 `Dockerfile` 中约定好的，如果你没有更改 `Dockerfile` 文件，那么 `/sites` 前缀就是固定的，不需要更改。

> 你需要在本机的 `hosts` 文件中增加你的域名
> 建议在目录 `WeDocker` 下创建一个文件夹 `sites` 以后就将项目放到此目录中，方便统一管理

### php
* `php.ini` 文件在 `php` 目录下，使用的是 `php.ini-development` 容器启动的时候会通过 `./php/php.ini-development:/usr/local/etc/php/php.ini` 挂载到容器内部
* `root` 文件是 `php` 容器内部的 `crontab`
* `script.sh` 文件是 `mp` 项目中用到的脚本
### docker-compose.yml
* 你需要修改 `nginx` 和 `php` 中的 `volumes`
  > 举例：
  > 你现在的项目目录是 `/wwwroot/sites`
  > 那么就将 `volumes` 中设置为：
  > `/wwwroot/sites:/sites:rw`
  > 你需要修改的就是 `：`  之前的内容
  > 它的意思是：将你本机的这个目录 `/wwwroot/sites` 映射到容器内部 `/sites` 目录
  > `windows` 同理 
* 如果你用到了 `xdebug` 并且 `9000` 端口已经在本机被占用，那么你需要修改 `php` 容器中的 `XDEBUG_CONFIG` 中的 `remote_prot`
## Windows
> 在 `windows_software` 目录中有 `docker-compose` 工具，另外你还需要下载 `windows docker` 这个太大了，就不放到这里，你需要先安装它们
> 下载地址：`https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe` 
## 启动
> 准备好运行之前配置，开始启动 `WeDocker`
> `docker-compose up `  如果是第一次 `up` ， `php`会有一个构建的过程，你需要耐心等待。
> 一切就绪之后，你会看到类似下面的信息：
> ```bash
> Starting wedocker_redis_1     ... done
> Starting wedocker_beanstalk_1 ... done
> Starting wedocker_php_1       ... done
> Starting wedocker_nginx_1     ... done

> 并且命令行处于占用的状态。
> 如果你想后台启动，你需要执行：
> docker-composer up -d 命令
```

## 附录
* 容器中 `php` 已经指定了内网 `dns` 服务器。你的本机可以不用再指定。
* 基础镜像使用的都是 `alpine` ，小巧好用。
* 为什么要在复制`php lib` 目录里面的包到镜像编译。因为实际情况中网络太差了，有些可能会被屏蔽。







