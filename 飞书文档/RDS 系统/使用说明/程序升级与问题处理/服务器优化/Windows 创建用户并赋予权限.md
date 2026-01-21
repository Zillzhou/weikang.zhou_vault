# Windows 创建用户并赋予权限

## Windows 创建用户并赋予权限
## 进入数据库界面
## 1.打开rds安装目录并进入到 mariadb 的 bin 目录下输入 cmd，点击回车
例如： D:\RDS\data\mariadb-10.3.31-winx64\bin
### 2.输入进入数据库命令：mysql -uroot -p，点击回车
## 说明：
root -指 mariadb 数据库的默认用户名。
### 3.输入数据库密码，点击回车。 ps：数据库默认密码：mysql
## 动图示例：
1667544391726-cb277f69-1200-405e-947b-e5b01521d6fc.gif

## 创建用户
## 命令:
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
## 说明:
username - 你将创建的数据库用户名,
host - 指定该用户在哪个主机上可以登陆,如果是本地用户可用localhost, 如果想让该用户可以从任意远程主机登陆,可以使用通配符%.
password - 该用户的登陆密码,密码可以为空,如果为空则该用户可以不需要密码登陆服务器.
## 举例：
 CREATE USER 'rdsReader'@'%' IDENTIFIED BY 'rdsReader123';
## 分配权限
## 命令：
GRANT privileges ON databasename.tablename TO 'username'@'host'
## 说明:
privileges - 用户的操作权限,如SELECT , INSERT , UPDATE 等.如果要授予所的权限则使用ALL.;
databasename - 数据库名,
tablename-表名,如果要授予该用户对所有数据库和表的相应操作权限则可用 * 表示, 如 *.*
## 举例：
### 分配只读权限给rdsReader用户，命令如下：
GRANT SELECT ON *.* TO 'rdsReader'@'%';
## 刷新权限：
flush privileges;
## 说明：
配置完成后需要刷新，否则配置不生效；
## 查看权限：
SHOW GRANTS FOR 'rdsReader'@'%';
## 视频实例：

## 📎新增用户.mp4
