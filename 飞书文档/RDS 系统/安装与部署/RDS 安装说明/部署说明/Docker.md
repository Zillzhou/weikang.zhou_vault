# Docker

## Docker

文档所需文件请至仙工制品库 SBS 360 中 RDS 目录下载。
对于 Windows 和 Ubuntu 系统，Docker 的命令是一致的，唯一区别是，Linux 系统需要使用 sudo 命令执行 Docker 相关操作。 以下所有命令基于 Ubuntu18.04，如果服务器是 Windows，请自行去除命令中的 sudo 关键字，而且 Windows Server 这种对用户权限有要求的系统还需要管理员权限安装使用 Docker 应用。
## 1. 安装环境
对于 Windows 系统，需满足更多系统条件约束 ，本文档不做详细解释，通过 在 Windows 上使用 Docker 里面的“系统要求”查看是否满足。
## 1.1. 在线安装 Docker 程序
服务器需要安装 Docker 应用程序，以下提供中英版本的安装链接。
Windows 系统： 安装 Docker 桌面版  英语版：Install Docker Desktop on Windows
Ubuntu 系统： 安装 Docker Engine   英语版：Install Docker Engine on Ubuntu
## 1.2. 离线安装 Docker 程序
请根据服务器系统选择对应的离线安装包。
## 1.2.1. Ubuntu 系统 Docker 离线安装包
Ubuntu Docker 程序下载路径： docker-Ubuntu-offline-installer.zip
### 下载并解压之后，进入该文件夹通过以下命令安装：
sudo dpkg -i containerd.io_1.6.21-1_amd64.deb \
  docker-ce_24.0.2-1~ubuntu.18.04~bionic_amd64.deb \
  docker-ce-cli_24.0.2-1~ubuntu.18.04~bionic_amd64.deb \
  docker-buildx-plugin_0.10.5-1~ubuntu.18.04~bionic_amd64.deb \
  docker-compose-plugin_2.18.1-1~ubuntu.18.04~bionic_amd64.deb

## 1.2.2. Windows 系统 Docker 离线安装包
需要先满足 Docker 对于 Windows 系统的上述 安装环境 要求才可以安装以下离线安装包。
Windows Docker exe 程序请至官方下载。
## 1.2.3. SLES SP4 系统(x86_64)
## 下载 Docker 安装包
下载 Docker 相关程序 👉：docker-rpm-x86_64.zip，下载解压后，通过以下命令安装：
sudo rpm -i runc-1.1.5-150000.41.1.x86_64.rpm \
        containerd-1.6.19-150000.87.1.x86_64.rpm \
        catatonit-0.1.7-150500.1.3.x86_64.rpm \
        docker-20.10.23_ce-150000.175.1.x86_64.rpm \
        docker-compose-switch-1.0.5-bp155.1.10.x86_64.rpm \
        docker-compose-2.14.2-bp155.1.6.x86_64.rpm
## 设置开机启动
### sudo systemctl enable docker
## 启动 docker
### sudo systemctl start docker
## 1.3. 部署 RDS
## 1.3.1. 数据库镜像
在线安装 Docker 程序可跳过本小节，执行 1.3.2。
### 下载路径： MariaDB 10.7 Docker 镜像
### 下载后使用以下命令将 MariaDB 镜像导入服务器：
sudo tar zxvf docker-mariadb-10.7.tar.gz
sudo docker load -i mariadb-10.7.tar
1692090695658-f0d02a31-ed32-49bb-a78d-0d84171ed4ce.png

## 1.3.2. RDS 镜像
下载路径： RDS Docker 镜像 docker-rds-robod-4.4.12.tar.gz
下载了 RDS Docker 镜像之后，使用以下命令导入镜像到服务器中，下文中 installer.tar.gz 是下载的 RDS Docker 镜像压缩包的全名，请根据实际下载结果修改。
sudo docker load -i installer.tar.gz
1690430457268-0f6272c1-d271-4161-a3ce-52cdf6ed1e8b.png

### 2. 执行
附件中 docker-compose.yml 是 Docker 的启动文件 ， my.cnf application.yml application-biz.yml 是 RDS 的配置文件。
将附件下载后放到服务器一个自定义文件夹中备用。
### 2.1. 编辑 Docker 配置
### 编辑 docker-compose.yml 如下：
## version: '3'
## services:
## rds:
## container_name: rds
### image: rds:slim-robod-4.4.8
## privileged: true
## ports:
## - 8080:8080
## - 8090:8090
## - 502:502
## - 8088:8088
## - 8089:8089
## - 19204:19204
## - 19207:19207
## - 19208:19208
## - 20204:20204
## - 20206:20206
## - 20207:20207
## environment:
## - TZ=Asia/Shanghai
### command: /bin/bash init.sh
## depends_on:
## - mariadb
## restart: always
## networks:
## - rds-net
## volumes:
      - /opt/docker/.data:/opt/.data
### #- /opt/docker/data:/opt/data
      - /opt/docker/application.yml:/opt/data/rds/app/application.yml
## mariadb:
### container_name: mariadb
## privileged: true
## image: mariadb:10.7
## ports:
## - 3306:3306
## environment:
### - MYSQL_ROOT_PASSWORD=mysql
## - TZ=Asia/Shanghai
## restart: always
## networks:
## - rds-net
## volumes:
      - /opt/docker/mariadb/my.cnf:/etc/mysql/conf.d/my.cnf
      - /opt/docker/mariadb/log:/var/log/mysql
      - /opt/docker/mariadb/data:/var/lib/mysql
## networks:
## rds-net:
请确保该文件中 ports 相关端口在服务器中没有被占用，否则会无法启动 RDS。
在 docker-compose.yml 文件中，有两处需要注意。
## 其一：
- /opt/docker/application.yml:/opt/data/rds/app/application.yml
上述配置冒号":"前是自定义的路径，然后将附件中的 application.yml 放到 /opt/docker/ 文件夹下面（没有 docker 文件夹则新建文件夹，路径可自定义） 。Windows 系统可以写成如： /d/docker/ ，即自定义文件夹是 D 盘中的 docker 文件夹，没有则新建该 docker 文件夹。
## 其二：
- /opt/docker/mariadb/my.cnf:/etc/mysql/conf.d/my.cnf
上述配置冒号":"前是自定义的路径，然后将附件中的 my.cnf 放到 /opt/docker/mariadb/ 文件夹下面（没有该 mariadb 文件夹则新建文件夹，路径可自定义） 。Windows 系统可以修改文件/opt/docker/mariadb/路径为： /d/docker/mariadb/ ，即 D 盘中的 docker 文件夹下的 mariadb 子文件夹，没有则新建该 mariadb 文件夹。
### 2.2. 启动 Docker 容器
在终端进入启动文件 docker-compose.yml 所在的目录，执行以下命令：
### sudo docker compose up -d
第一次启动如下，如果是入网服务器，docker 将会通过互联网拉取 MariaDB 数据库：
1690514204399-7a3cb48c-1083-4d85-bfb1-1e051b179933.png

1690531761106-72eb6a13-5868-4907-909e-c1b643c8fe08.png

### 2.3. 复制容器文件(!!重要!!)
等待1分钟左右，后台 Docker RDS 相关容器就会完全启动，此时执行以下命令（表示将 Docker 内 /opt/data 文件夹复制到本地 /opt/docker/data）：
sudo docker cp rds:/opt/data  /opt/docker/data 
### 当拷贝完成之后，需要执行以下命令关闭容器：
### sudo docker compose down
关闭容器后，然后修改配置文件 docker-compose.yml，将 29 行 #- /opt/docker/data:/opt/data的井号#去掉，改为 - /opt/docker/data:/opt/data ，保存 docker-compose.yml 文件，使用 2.2 节的命令 sudo docker compose up -d 重启 Docker 容器。
重启后，等待 1 分钟左右 Docker 完全启动，即可使用 Roboshop 升级 rds 和 rdscore， 升级后然后便可以浏览器访问 rds 了。
### 2.4. 关闭 Docker 容器
当需要关闭 RDS 程序时，在终端进入到配置文件 docker-compose.yml 所在的目录，执行以下命令关闭 RDS：
### sudo docker compose stop
1690514529679-5eeb97f0-e74f-4152-b2b0-9c94d8ffbf71.png

除上述 docker compose stop 命令外，2.3 节使用的 sudo docker compose down ，这是 RDS docker 关闭并删除容器命令 ， 当需要修改 docker-compose.yml 文件时，需要执行 sudo docker compose down 这个命令，仅关闭程序不需要修改配置则使用 sudo docker compose stop 命令。
### 3. 附件

## my.cnf

## application.yml

## docker-compose.yml
