# 使用 SQL 查询数据

## 使用 SQL 查询数据
## 方法说明
本方法可以使用自定义的 SQL 查询数据，并返回指定字段。
function jdbcQuery(sql: string): string

## 输入参数
sql：string 类型，SQL 语句。
输出参数是 JSON 格式的字符串。
假设查询 sql 参数为：sql="select  id, name, age from person_table"
### 则返回的结果，经 JSON 反序列化后，数据如下：
## [
## {
            "id": "1",
            "name": "lilei",
## "age": 14
## }
## ]

## 异常
本方法不抛出异常。
