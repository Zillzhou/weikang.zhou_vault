# RDS 服务启动失败

## RDS 服务启动失败
1666342144634-914055a3-d5c1-4276-bf5e-e68b87ae2888.png

RDS 目录结构不对导致服务启动失败。
### RDS 目录结构引起 RDS 服务无法启动
jdk 的版本不对，要求： OpenJDK 11 。
### JDK版本错误导致RDS无法启动（以win10为例）
1666342517910-d468719f-c655-471d-a675-344ce8b02f89.png

## mariadb 服务未启动
打开命令提示窗→以管理员身份运行→进入 MariaDB 安装目录（输入 MariaDB 所在盘符例 ： C: 回车进入对应的盘符;输入进入对应的 cd C:\RDS\data\mariadb-10.3.31-winx64\bin ）输入命令 mysqladmin -uroot -pmysql ping ( root 指用户名， mysql 指数据库密码)
1666342956930-4adf0ae5-4223-4a11-ba5d-205e4e84ee1f.png

如上图出现 mysql is alive 字样表示数据库服务已启动，否则数据库服务未启动，需要启动数据库服务。
### windows 下启动 MariaDB 服务：
## 临时解决方案：
找到 RDS 安装目录下的 data/mariadb-10.3.31-winx64/bin/ 下面的 mysqld.exe （注意不是 mysql.exe ），双击运行后重启 RDS 即可。(如： C:/RDS/data/mariadb-10.3.31-winx64/bin )
## 永久解决方案：
打开命令提示窗→以管理员身份运行→ 进入 MariaDB 安装目录（ cd C:\RDS\data\mariadb-10.3.31-winx64\bin ）→ 输入命令 mysqld -install MySQL（robod 4.3.8+ 使用mysqld --install MySQL） （如果出现 Install/Remove of the Service Denied! ，则说明不是以管理员身份运行的命令提示窗） →360会有相关提示，需要允许操作→打开任务管理器，进入到服务→找到 MySQL →右键启动→然后再右键属性，确定启动类型是自动，然后重启 RDS 即可(如果MySQL服务存在，先删除服务，命令sc delete MySQL)。
1666343030774-e354c204-1b29-48d3-835f-0638f34a3a2f.gif

Windows 下在老版本数据库 mariadb-10.3.31-winx64 中，可能会遇到重新注册服务仍无法启动的问题。重新启动如下图所示。此问题为数据库异常损坏。
## image.png

解决办法：这时需要更新数据库 mariadb-10.6-winx64 
## 1、从较新版本的 Base_RDSInstaller 包中获取 mariadb-10.6-winx64 。
### 2、将  mariadb-10.6-winx64 放到与 mariadb-10.3.31-winx64 平级目录。
### 3、卸载数据库服务。
### 4、在 mariadb-10.6-winx64 bin 目录中重新注册服务。
### 5、启动服务。启动 RDS。
### 6、此时 RDS 正常启动但天风任务丢失。
### 7、较新版本的 RDS 会有备份，直接导入即可。
## image.png

### 8、如果没有天风任务备份，在以前 RDS 正常运行日志里搜索 taskLabel=
## image.png

复制后面的整个 detail 从 { 开始 到对应 } 结束，定义天风任务，名字与 taskLabel= 后的任务名相同，点击确定。 
## image.png

 点击编辑，点击源码，粘贴刚才复制的 detail，保存。即可复原一个天风任务，其他任务同理复原。

## Ubuntu 系统环境
直接在终端窗口中输入命令 mysqladmin -u root -pmysql ping 如出现 mysql is alive 字样表示数据库服务已启动。否则输入命令 sudo systemctl start mariadb 启动数据库。

### `Ubuntu` 系统环境下数据库未忽略大小写
查看大小写是否敏感。 RDS 需要将数据库设置为大小写不敏感， `Ubuntu 系统中安装 MariaDB 之后默认大小写是敏感的。登录数据 库，命令： mysql -uroot -p 输入数据库密码： mysql ，登录数据库之后，使用 show variables like "%case%"
MariaDB [(none)]> show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 0     |
+------------------------+-------+
### 2 rows in set (0.001 sec)

## MariaDB [(none)]>
修改为大小写不敏感，这里我们采用修改配置文件的方式。使用命令修改配置文件 sudo vim /etc/mysql/conf.d/my.cnf ；
sudo vim /etc/mysql/conf.d/my.cnf
### 接着点击键盘 i 键进入编辑模式，输入以下文本
## [mysqld]
### lower_case_table_names=1
### 接着使用 Esc + :wq 命令保存文件
检查是否修改成功。使用 sudo systemctl restart mariadb 命令重新启动数据库。然后再次登录数据库，输入以下命令查询结果，显示如下:
MariaDB [(none)]> show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 1     |
+------------------------+-------+
### 2 rows in set (0.001 sec)

## MariaDB [(none)]>
## 升级 RDS 配置文件未替换
1666343638445-0c992754-672c-4b41-900e-4e7d2b2b9379.png

Robod 在4.3.4之前的版本升级 RDS 不会替换 RDS 的配置文件，在这个版本及以后版本会升级会替换 RDS 配置文件；解决配置文件不会替换的方法：升级 Robod-4.3.4 及以上版本，然后重新升级 RDS 即可。若升级失败可能会出现相同的问题
