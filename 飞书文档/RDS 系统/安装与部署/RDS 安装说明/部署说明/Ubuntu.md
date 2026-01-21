# Ubuntu

## Ubuntu
## 一. 系统介绍
服务器选择 Ubuntu24.04 LTS 版本，安装 RDS & Core 的运行环境。
### 二、软件环境介绍
## 服务器内需安装如下环境：
## openjdk-11
### mariadb-server (10.6.8)
### 2.1 openjdk-11
## 推荐离线安装
### 2.1.1 离线安装 openJDK-11（推荐）
下载 deb 安装包：sbs 软件库 openjdk11_linux_x86_64_deb 包下载
### 此安装包支持 Ubuntu18、20、22、24
## image.png

## 任意方法上传至服务器
## 方式一：可视化界面安装
## 双击 deb 包，输入密码等待片刻即可
## 方式二：无界面命令行安装

sudo dpkg -i /上传位置完整路径/openjdk11auto_11.0.2_linux_x86_64.deb
### 2.1.2 在线安装 openJDK-11
## 安装 openjdk-11

sudo apt-get install openjdk-11-jdk openjdk-11-jre
## 查看 Java 版本

## java -version
### 2.1.3 检测JDK-11环境

## java -version
### 2.2 MariaDB Server
如果现场服务器是全新的 Ubuntu 24.04 操作系统，此前未安装过数据库，安装 MariaDB 数据库建议采用离线安装方式，离线安装流程较简单，省略大部分配置步骤，文档参照 2.2.1 节。
如果此前该服务器已经安装过数据库，需要将此前安装的数据库卸载完全，才能继续采用离线安装方式。
### 2.2.1 离线安装 MariaDB Server 10.6.8（推荐）
### 2.2.2 章节是在线安装 Mariadb Server 的方式，已弃用，离线安装的方式，如下:
下载 deb 安装包：sbs 软件库 mariadb_ubuntu_x86_64_deb 包下载
### 此安装包支持 Ubuntu18、20、22、24
## image.png

## 任意方法上传至服务器
## 方式一：可视化界面安装
## 双击 deb 包，输入密码等待片刻即可
## 方式二：无界面命令行安装

sudo dpkg -i /上传位置完整路径/mariadbauto_10.6.8_ubuntu_x86_64.deb
## 检测数据库环境

### sudo mysql -u root -p
## 输入密码: mysql
### 2.2.2 在线安装 MariaDB Server（弃用，当前仅推荐 2.2.1 离线安装方式）
### MariaDB Server 在线安装的方式
## 安装步骤如下：
## 安装相关依赖
sudo apt-get install software-properties-common dirmngr apt-transport-https
## 导入密钥
sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
## 导入下载源
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] https://ftp.ubuntu-tw.org/mirror/mariadb/repo/10.7/ubuntu bionic main'

## 注意：
有时会出现下载源连接超时的现象，请访问 mariadb官网获取新的下载源
mariadb官网： Download MariaDB Server - MariaDB.org
## 更新下载源列表，安装mariaDB
## sudo apt update
sudo apt install mariadb-server
### 2.2.2.1 配置 MariaDB
### 安装完成后需要进行数据库初始化配置，具体如下：
登录 root 权限（首次设置数据库必须用root权限登录）
## su root
## 初始化配置
### mysql_secure_installation
## 具体配置如下
[root@server1 ~]# mysql_secure_installation
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MySQL
SERVERS IN PRODUCTION USE! PLEASE READ EACH STEP CAREFULLY!
In order to log into MySQL to secure it, we'll need the current
password for the root user. If you've just installed MySQL, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.
Enter current password for root (enter for none):<–初次运行直接回车
OK, successfully used password, moving on…
Setting the root password ensures that nobody can log into the MySQL
root user without the proper authorisation.
Set root password? [Y/n] <– 是否设置root用户密码，输入y并回车或直接回车
New password: <– 设置root用户的密码,这里配置为 mysql
Re-enter new password: <– 再输入一次你设置的密码, mysql
Password updated successfully!
Reloading privilege tables..
## … Success!
By default, a MySQL installation has an anonymous user, allowing anyone
to log into MySQL without having to have a user account created for
them. This is intended only for testing, and to make the installation
go a bit smoother. You should remove them before moving into a
production environment.
Remove anonymous users? [Y/n] <– 是否删除匿名用户,生产环境建议删除，所以直接回车
## … Success!
Normally, root should only be allowed to connect from 'localhost'. This
ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? [Y/n] <–是否禁止root远程登录,根据自己的需求选择Y/n并回车,建议禁止
## … Success!
By default, MySQL comes with a database named 'test' that anyone can
access. This is also intended only for testing, and should be removed
before moving into a production environment.
Remove test database and access to it? [Y/n] <– 是否删除test数据库,直接回车
### - Dropping test database…
## … Success!
- Removing privileges on test database…
## … Success!
Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.
Reload privilege tables now? [Y/n] <– 是否重新加载权限表，直接回车
## … Success!
## Cleaning up…
All done! If you've completed all of the above steps, your MySQL
installation should now be secure.
### Thanks for using MySQL!
## 可以验证下是否配置成功，方法如下：
## 进入普通用户
## su sr
### 尝试使用 mysqld 连接 mysql
## mysql -u root -p
## 默认密码为空，直接回车即可
### 依次输入下方命令并回车设置密码为 mysql
use mysql;
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('mysql');
FLUSH PRIVILEGES;
## quit
## 测试是否成功
## mysql -u root -p
### 直接回车，观察是否报错（没有输入密码，报错是正常的）
## mysql -u root -p
输入密码：mysql 后回车，此时应该成功连接，输入 quit 并回车退出连接；
### 2.2.2.2 修改数据库大小写不敏感
查看大小写是否敏感。RDS 需要将数据库设置为大小写不敏感，Ubuntu 系统中安装 MariaDB 之后默认大小写是敏感的。登录数据库之后，使用 show variables like "%case%"; 命令查询结果如下图，lower_case_table_names = 0，意味着大小写敏感，如果等于1，则不需要执行第2，3步。
MariaDB [(none)]> show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 0     |
+------------------------+-------+
### 2 rows in set (0.001 sec)

## MariaDB [(none)]>
修改为大小写不敏感，这里我们采用修改配置文件的方式。使用以下命令修改配置文件：

sudo vim /etc/mysql/conf.d/my.cnf
### 接着点击键盘 i 键进入编辑模式，输入以下文本
## [mysqld]
### lower_case_table_names=1
使用 Esc + :wq 命令保存文件。
检查是否修改成功。使用 sudo systemctl restart mariadb 命令重新启动数据库。然后再次登录数据库，输入以下命令查询结果，显示如下。
MariaDB [(none)]> show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 1     |
+------------------------+-------+
### 2 rows in set (0.001 sec)

## MariaDB [(none)]>
lower_case_table_names = 1 ，修改成功。
### 2.2.3 数据库常用操作指令
这部分命令是数据库使用的一些常用指令，只是作为科普。安装数据库过程中不需要特意操作以下指令。
## 查看数据库
show databases;
## 创建数据库
create database 数据库名;
## 选择数据库
use 数据库名;
## 查看数据库的表
show tables;
## 删除数据库
drop database 数据库名;
### 2.2.4 常用 linux 系统关于 mariadb 的指令
## 开启/关闭数据库服务
### sudo systemctl start mariadb
### sudo systemctl stop mariadb
## 查看数据库服务状态
### sudo systemctl status mariadb
## 设置开机默认启动数据库服务
### sudo systemctl enable mariadb
## 关闭数据库服务开机自启
sudo systemctl disable mariadb

### 2.2.5 数据库可视化软件（选装）
服务器内安装了dbeaver工具，用于可视化修改数据库信息。
下载链接： https://dbeaver.io/download/
## 启动方法为终端输入
## dbeaver
更多使用信息请自行百度。
### 2.3 远程工具（选装）
### 为方便问题处理，可选装相关远程工具，推荐如下几种：
## Todesk
## 向日葵
### 通过上述超链接下载对应 deb 安装包后，通过如下命令安装
### sudo dpkg -i xxx.deb
### 2.4 wireshark（选装）
服务器可安装网络抓包工具 wireshark ，打开方式为终端输入
## sudo wireshark
注意：用普通用户开启 wireshark 会检测不到网卡信息。
### 2.5 Roboshop
再制品库找到最新的 roboshop 安装包，名称以 .deb 结尾，如 Roboshop-ubuntu-2.4.1.179.deb
将下载的文件上传至服务器，在此文件路径下打开终端执行如下命令安装
sudo dpkg -i Roboshop-ubuntu-2.4.1.179.deb

# 命令中的 Roboshop-ubuntu-2.4.1.179.deb 需要替换成你要安装包的名字
### 在左下角菜单搜索即可找到安装的 roboshop
短期内没有linux版本的 roboshop ,在 ubuntu 上运行 roboshop 需要借助 wine 。
### 2.5.1 安装 wine
服务器内已安装了 wine ,如在客户提供的服务器上安装 wine ，使用如下命令：
### sudo apt install wine64

注意：要安装64位版本的 wine ，不要直接安装 wine-stable ，否则会导致安装大量32位的依赖和 gnome 桌面冲突。
### 2.5.2 运行 Roboshop
将 roboshop 的zip版本拷贝到服务器的 /home 文件夹下，解压缩，进入 Roboshop 文件夹下，输入如下命令启动 Roboshop
### wine RoboshopPro.exe
### 2.5.3 更新 Roboshop
替换原来的 Roboshop 文件夹即可，最好不改变路径。
### 2.5.4 创建桌面快捷方式
在文件目录搜索 .desktop ,选择 terminal 复制到桌面
1655176195989-ad729e72-9cf3-43aa-b722-191ca1b27db0.png

## 右键编辑属性
1655176196380-b2f0445b-4095-48a0-a4e2-2c6f05cb115b.png

## 输入如下命令
wine /home/sr/RoboshopPro/RoboshopPro.exe
将快捷方式选择为可执行命令，即可。
### 三、RDS系统
服务器已内置 rds 和 rds-core ，请勿重复安装。 robod 接管 rds 的升级和开机自启，通过 roboshop 更新和激活 rds 相关程序。
### 3.1 目录说明
## # 程序目录,各目录下自己管理
## /opt/data/wms
## /opt/data/rds
## /opt/data/rdscore
## /opt/data/srd
## /opt/data/roboshop

## # 程序数据目录
## /opt/.data/wms
## /opt/.data/rds
## /opt/.data/rdscore
## /opt/.data/srd
## /opt/.data/roboshop

## # 工具目录
## /opt/data/tools/

## # 备份目录
## /opt/backups/

### 3.2 安装说明
## 安装包如下：
注意：此处安装包非最新，可用此处安装包完成安装后，到软件库获取最新的 RDS，Robod，Core 程序在 Roboshop 上进行升级。
### 3.2.1 安装 robod
sudo dpkg -i linux-base-rdscore-robod-4.XXXX.XXX-x86.deb
### 3.2.2 安装 core
打开 roboshop - 【添加设备】，输入服务器ip (可在远程主机访问，也可在服务器本机访问），然后点击【辅助程序】
1655178627760-0e88caa4-4cb8-4bc8-a582-cc3bc898e114.png

## 选择【升级/备份】
1655178742324-bb2a88e8-f35d-47a4-b42a-d62af1aab3f9.png

### 将上述 core 的程序包拖入升级区，点击【开始升级】
## 等待安装完成
### 3.2.3 安装 rds
保持上一步界面，将 rds 程序包拖入升级区，点击【开始升级】
## 等待安装完成

### 3.2.4 激活
点击【版本信息】，选择【导出信息】将导出的文件发送给技术人员获取激活文件。
1655178992178-60bdae55-28dc-45c3-929c-e2ade011a32c.png

获取激活文件后，点击【离线激活】，导入获得的激活文件，等待激活完成。
1655179056473-25a1580c-652a-4cb1-b2b4-d2a62ddafa85.png

### 3.3 更新说明
同3.2.2，将升级包拖入升级区，点击【开始升级】，等待升级完成。
### 四、配置网卡地址
点击右上角网络图标，配置IP地址。
1655176201658-fc6391ac-1c1b-4d4b-b1f2-681782b29e8f.png

注意：目前出现更改后静态IP未生效的情况，需要网卡禁用再启动后生效。
### 五、常见问题
### 5.1 更新 RDS 后，程序无法启动
服务器内置的 Robod 版本较老，更新到最新的 4.3.3 之后版本重新更新RDS程序，重启后即可。
