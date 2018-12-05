# Mysql C API Data Structures

说明：

本文内容翻译自Mysql官方说明文档[https://dev.mysql.com/doc/refman/8.0/en/c-api-data-structures.html](https://dev.mysql.com/doc/refman/8.0/en/c-api-data-structures.html)

---

- [MYSQL](#MYSQL)
- [MYSQL_RES](#MYSQL_RES)
- [MYSQL_ROW](#MYSQL_ROW)
- [MYSQL_FIELD](#MYSQL_FIELD)
- [MYSQL_FIELD_OFFSET](#MYSQL_FIELD_OFFSET)
- [my_ulonglong](#my_ulonglong)
- [my_bool](#my_bool)
- [char * name](#char\ \*\ name)
- [char * org_name](#char\ \*\ org_name)
- [char * table](#char\ \*\ table)
- [char * org_table](#char\ \*\ org_table)
- [char * db](#char\ \*\ db)
- [char * catalog](#char\ \*\ catalog)
- [char * def](#char\ \*\ def)
- [unsigned long length](#unsigned\ long\ length)
- [unsigned long max_length](#unsigned\ long\ max_length)
- [unsigned int name_length](#unsigned\ int\ name_length)
- [unsigned int org_name_length](#unsigned\ int\ org_name_length)
- [unsigned int table_length](#unsigned\ int\ table_length)
- [unsigned int org_table_length](#unsigned\ int\ org_table_length)
- [unsigned int db_length](#unsigned\ int\ db_length)
- [unsigned int catalog_length](#unsigned\ int\ catalog_length)
- [unsigned int def_length](#unsigned\ int\ def_length)
- [unsigned int flags](#unsigned\ int\ flags)
- [unsigned int decimals](#unsigned\ int\ decimals)
- [unsigned int charsetnr](#unsigned\ int\ charsetnr)
- [enum enum_field_types type](#enum\ enum_field_types\ type)

---

#### MYSQL
此结构表示一个数据库连接的处理程序，它几乎用于所有的Mysql函数中。不要尝试复制本结构，不保证这种拷贝是否可用。

#### MYSQL_RES
此结构表示一个有行信息返回的查询（SELECT，SHOW，DESCRIBE，EXPLAIN），从查询返回的信息叫做“结果集”。

#### MYSQL_ROW
这是一行数据的类型安全的表示。它目前实现为计数字节字符串数组。（如果字段值可能包含二进制数据，则不能将这些视为以空值终止的字符串，因为此类值可能在内部包含空字节。）通过调用mysql_fetch_row()获得行数据。
#### MYSQL_FIELD

#### MYSQL_FIELD_OFFSET
#### my_ulonglong
#### my_bool
#### char * name
#### char * org_name
#### char * table
#### char * org_table
#### char * db
#### char * catalog
#### char * def
#### unsigned long length
#### unsigned long max_length
#### unsigned int name_length
#### unsigned int org_name_length
#### unsigned int table_length
#### unsigned int org_table_length
#### unsigned int db_length
#### unsigned int catalog_length
#### unsigned int def_length
#### unsigned int flags
#### unsigned int decimals
#### unsigned int charsetnr
#### enum enum_field_types type
