# 执行 SQL 语句

## 执行 SQL 语句
## 方法说明
通过 SQL 命令操作 RDS 系统数据库，可用于创建自定义数据表。
function jdbcExecuteSql(sql: string): Boolean

## 输入参数
sql：string 类型，创建表的 SQL 语句。示例如下：
CREATE TABLE `test` (`id` bigint NOT NULL AUTO_INCREMENT, PRIMARY KEY (`id`)) ENGINE=InnoDB AUTO_INCREMENT=30 DEFAULT CHARSET=utf8mb3

## 输出参数：
true， SQL 命令执行成功。
false， SQL 命令执行失败。
## 异常
本方法不抛出异常，方法内捕获到异常后，按执行失败的信息返回处理。
