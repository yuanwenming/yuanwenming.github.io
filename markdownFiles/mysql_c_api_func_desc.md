# Mysql C API Function 说明
说明：
本文内容翻译自Mysql官方说明文档[https://dev.mysql.com/doc/refman/8.0/en/c-api-functions.html](https://dev.mysql.com/doc/refman/8.0/en/c-api-functions.html)

---

- [mysql_affected_rows](#mysql_affected_rows)
- [mysql_autocommit](#mysql_autocommit)
- [mysql_change_user](#mysql_change_user)
- [mysql_character_set_name](#mysql_character_set_name)
- [mysql_close](#mysql_close)
- [mysql_commit](#mysql_commit)
- [mysql_connect](#mysql_connect)
- [mysql_create_db](#mysql_create_db)
- [mysql_data_seek](#mysql_data_seek)
- [mysql_debug](#mysql_debug)
- [mysql_drop_db](#mysql_drop_db)
- [mysql_dump_debug_info](#mysql_dump_debug_info)
- [mysql_eof](#mysql_eof)
- [mysql_errno](#mysql_errno)
- [mysql_error](#mysql_error)
- [mysql_escape_string](#mysql_escape_string)
- [mysql_fetch_field](#mysql_fetch_field)
- [mysql_fetch_field_direct](#mysql_fetch_field_direct)
- [mysql_fetch_fields](#mysql_fetch_fields)
- [mysql_fetch_lengths](#mysql_fetch_lengths)
- [mysql_fetch_row](#mysql_fetch_row)
- [mysql_field_count](#mysql_field_count)
- [mysql_field_seek](#mysql_field_seek)
- [mysql_field_tell](#mysql_field_tell)
- [mysql_free_result](#mysql_free_result)
- [mysql_get_character_set_info](#mysql_get_character_set_info)
- [mysql_get_client_info](#mysql_get_client_info)
- [mysql_get_client_version](#mysql_get_client_version)
- [mysql_get_host_info](#mysql_get_host_info)
- [mysql_get_option](#mysql_get_option)
- [mysql_get_proto_info](#mysql_get_proto_info)
- [mysql_get_server_info](#mysql_get_server_info)
- [mysql_get_server_version](#mysql_get_server_version)
- [mysql_get_ssl_cipher](#mysql_get_ssl_cipher)
- [mysql_hex_string](#mysql_hex_string)
- [mysql_info](#mysql_info)
- [mysql_init](#mysql_init)
- [mysql_insert_id](#mysql_insert_id)
- [mysql_kill](#mysql_kill)
- [mysql_library_end](#mysql_library_end)
- [mysql_library_init](#mysql_library_init)
- [mysql_list_dbs](#mysql_list_dbs)
- [mysql_list_fields](#mysql_list_fields)
- [mysql_list_processes](#mysql_list_processes)
- [mysql_list_tables](#mysql_list_tables)
- [mysql_more_results](#mysql_more_results)
- [mysql_next_result](#mysql_next_result)
- [mysql_num_fields](#mysql_num_fields)
- [mysql_num_rows](#mysql_num_rows)
- [mysql_options](#mysql_options)
- [mysql_options4](#mysql_options4)
- [mysql_ping](#mysql_ping)
- [mysql_query](#mysql_query)
- [mysql_real_connect](#mysql_real_connect)
- [mysql_real_escape_string](#mysql_real_escape_string)
- [mysql_real_escape_string_quote](#mysql_real_escape_string_quote)
- [mysql_real_query](#mysql_real_query)
- [mysql_refresh](#mysql_refresh)
- [mysql_reload](#mysql_reload)
- [mysql_reset_connection](#mysql_reset_connection)
- [mysql_reset_server_public_key](#mysql_reset_server_public_key)
- [mysql_result_metadata](#mysql_result_metadata)
- [mysql_rollback](#mysql_rollback)
- [mysql_row_seek](#mysql_row_seek)
- [mysql_row_tell](#mysql_row_tell)
- [mysql_select_db](#mysql_select_db)
- [mysql_server_end](#mysql_server_end)
- [mysql_server_init](#mysql_server_init)
- [mysql_session_track_get_first](#mysql_session_track_get_first)
- [mysql_session_track_get_next](#mysql_session_track_get_next)
- [mysql_set_character_set](#mysql_set_character_set)
- [mysql_set_local_infile_default](#mysql_set_local_infile_default)
- [mysql_set_local_infile_handler](#mysql_set_local_infile_handler)
- [mysql_set_server_option](#mysql_set_server_option)
- [mysql_shutdown](#mysql_shutdown)
- [mysql_sqlstate](#mysql_sqlstate)
- [mysql_ssl_set](#mysql_ssl_set)
- [mysql_stat](#mysql_stat)
- [mysql_store_result](#mysql_store_result)
- [mysql_thread_id](#mysql_thread_id)
- [mysql_use_result](#mysql_use_result)
- [mysql_warning_count](#mysql_warning_count)

---

## mysql_affected_rows()
my_ulonglong mysql_affected_rows(MYSQL *mysql);

#### **说明**
[mysql_affected_rows()](#mysql_affected_rows())可能会在使用[mysql_query()](#mysql_query())或[mysql_real_query()](#mysql_real_query())执行语句后立刻被调用。如果最后一次执行的sql操作是UPDATE、DELETE、INSERT，则本函数返回更新、删除、插入的行数。对于SELECT语句来说，[mysql_affected_rows()](#mysql_affected_rows)就像[mysql_num_rows()](#mysql_num_rows)一样。

在UPDATE语句中，受影响行数的默认值就是实际影响的行数。当连接mysqld时，如果[mysql_real_connect()](#mysql_real_connect())指定了CLIENT_FOUND_ROWS标志，受影响行数的值就是WHERE语句匹配的行数。

在REPLACE语句中，如果新行替换掉了旧行，则受影响行数为2，因为在删除了旧行之后新插入了一行。

对于INSERT ... ON DUPLICATE KEY UPDATE这种语句，如果将行作为新行插入，则每行的受影响行值为1;如果更新现有行，则为2;如果现有行设置为其当前值，则为0。如果指定 CLIENT_FOUND_ROWS标志，则如果将现有行设置为其当前值，则affected-rows值为1（不为0）。

在CALL存储过程的语句之后， mysql_affected_rows()返回它将为过程中执行的最后一个语句返回的值，或者0-如果那个语句返回结果为-1。在该过程中，您可以使用ROW_COUNT()为个别语句在SQL级别获取受影响的值。

mysql_affected_rows()为大量语句返回有意义的值。有关详细信息，请参阅用于描述 ROW_COUNT()在 第12.14节，“信息函数”。


#### **返回值**
大于零的整数表示受影响或检索的行数。零表示UPDATE语句没有更新记录，WHERE查询没有匹配到数据或者尚未执行任何查询。-1表示查询返回错误，或者对于SELECT查询， mysql_affected_rows()在调用之前先错误的调用了mysql_store_result()。

因为mysql_affected_rows() 返回无符号值，所以可以通过将返回值与(my_ulonglong)-1（或 (my_ulonglong)~0）进行比较来检查-1 。
#### **错误信息**
没有
#### **示例**
```c
char *stmt = "UPDATE products SET cost=cost*1.25
              WHERE group=10";
mysql_query(&mysql,stmt);
printf("%ld products updated",
       (long) mysql_affected_rows(&mysql));
```

---


## mysql_autocommit()
bool mysql_autocommit(MYSQL *mysql, bool mode);

#### **说明**
如果mode是1，则打开为自动提交模式；如果为0，则关闭自动提交模式。
#### **返回值**
成功返回0，如果失败，则返回非0。
#### **错误信息**
没有
#### **示例**
没有

---

## mysql_change_user()
bool mysql_change_user(MYSQL *mysql, const char *user, const char *password, const char *db);

#### **说明**
更改用户并使指定的数据库db成为mysql指定的连接上默认（当前）数据库。在后续查询中，此数据库是不包含显式数据库说明符的表引用的缺省值。

mysql_change_user()如果连接的用户无法通过身份验证或无权使用数据库，则会失败。在这种情况下，用户和数据库不会更改。

如果您不想拥有默认数据库，请传递参数NULL给db。

此函数会重置会话状态，就好像已完成新连接并重新进行身份验证一样。（请参见[第28.7.24节“C API自动重新连接控制”](#https://dev.mysql.com/doc/refman/8.0/en/c-api-auto-reconnect.html)。）它始终执行ROLLBACK任何活动事务，关闭和删除所有临时表，并解锁所有锁定表。会话系统变量重置为相应全局系统变量的值。准备好的报表已发布， HANDLER变量已关闭。获得的锁GET_LOCK() 被释放。即使用户没有改变，也会发生这些影响。

要在不更改用户的情况下以更轻量级的方式重置连接状态，请使用 mysql_reset_connection()。
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_character_set_name()
const char *mysql_character_set_name(MYSQL *mysql);

#### **说明**
返回当前连接的默认字符集名称
#### **返回值**
默认的字符集名称
#### **错误信息**
没有
#### **示例**
没有

---

## mysql_close()
void mysql_close(MYSQL *mysql);

#### **说明**
关闭前面已经打开的连接。mysql_close()也会取消由[mysql_init()](#mysql_init())或[mysql_connect()](#mysql_connect())自动分配的由mysql代指的连接句柄。
#### **返回值**
没有
#### **错误信息**
没有
#### **示例**
没有

---

## mysql_commit()
bool mysql_commit(MYSQL *mysql);

#### **说明**
提交当前事务。

此函数的操作取决于系统变量completion_type的值 。特别地，如果completion_type的值是RELEASE（或2）时，服务器结束交易之后执行释放和关闭客户端连接。客户端程序调用[mysql_close()](#mysql_close())以从客户端侧关闭连接。
#### **返回值**
成功为0，不成功则非零。
#### **错误信息**
没有
#### **示例**
没有

---

## mysql_connect()
MYSQL *mysql_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd);

#### **说明**
不推荐使用此函数,请改用[mysql_real_connect()](#mysql_real_connect())函数。

mysql_connect()尝试与运行在host上的MySQL数据库引擎建立连接。mysql_connect()必须先成功完成才能执行任何其他API函数，但以下情况除外 mysql_get_client_info()。

参数的含义与[mysql_real_connect()](#mysql_real_connect())的参数含义相同，除了连接参数有可能为NULL。在这种情况下，C API会自动为连接结构分配内存，并在调用[mysql_close()](#mysql_close())时释放该内存。此方法的缺点是，如果连接失败，则无法取到错误消息。（要从[mysql_errno()](#mysql_errno())或[mysql_error()](#mysql_error())获取错误信息，您必须提供有效的MYSQL指针。）
#### **返回值**
同[mysql_real_connect()](#mysql_real_connect())返回
#### **错误信息**
同[mysql_real_connect()](#mysql_real_connect())返回
#### **示例**
没有

---

## mysql_create_db()
int mysql_create_db(MYSQL *mysql, const char *db);

#### **说明**
创建名为参数db所指的数据库。
此函数不再推荐使用，使用[mysql_query()](#mysql_query())函数执行SQL语句"CREATE DATABASE"来代替。
#### **返回值**
成功返回零，不成功返回非零。
#### **错误信息**
- CR_COMMANDS_OUT_OF_SYNC

    命令以不正确的顺序执行。
- CR_SERVER_GONE_ERROR

    Mysql服务器已消失。
- CR_SERVER_LOST

    在查询期间，与服务器的连接丢失。
- CR_UNKNOWN_ERROR

    出现未知错误。

#### **示例**
```c
if(mysql_create_db(&mysql, "my_database"))
{
   fprintf(stderr, "Failed to create new database.  Error: %s\n",
           mysql_error(&mysql));
}
```

---


## mysql_data_seek()
void mysql_data_seek(MYSQL_RES *result, my_ulonglong offset);

#### **说明**
寻找查询结果集中的任意行。该offset值是行号,范围从0到mysql_num_rows(result)-1。

此函数要求结果集结构包含查询的整个结果，因此mysql_data_seek()只能与[mysql_store_result()](#mysql_store_result())结合使用，而不能与[mysql_use_result()](#mysql_use_result())结合使用 。
#### **返回值**
没有
#### **错误信息**
没有
#### **示例**
没有

---

## mysql_debug()
void mysql_debug(const char *debug);

#### **说明**
使用给定的debug字符串信息执行DEBUG_PUSH操作。mysql_debug()使用了Fred Fish库，要使用此功能，必须编译客户端以支持调试。
详细请参考[第29.5.3节"DEBUG包"](#https://dev.mysql.com/doc/refman/8.0/en/dbug-package.html)
#### **返回值**
没有
#### **错误信息**
没有
#### **示例**
此处显示的调用导致客户端库/tmp/client.trace在客户端计算机上生成跟踪文件：
```c
mysql_debug("d:t:O,/tmp/client.trace");
```

---


## mysql_drop_db()
int mysql_drop_db(MYSQL *mysql, const char *db);

#### **说明**
删除由参数db指定的数据库。
不推荐使用此功能，推荐使用mysql_query()来执行"DROP DATABASE"SQL语句。
#### **返回值**
成功返回零，不成功返回非零。
#### **错误信息**
- CR_COMMANDS_OUT_OF_SYNC

    命令以不正确的顺序执行。
- CR_SERVER_GONE_ERROR

    Mysql服务器已消失。
- CR_SERVER_LOST

    在查询期间，与服务器的连接丢失。
- CR_UNKNOWN_ERROR

    出现未知错误。
#### **示例**
```c
if(mysql_drop_db(&mysql, "my_database"))
  fprintf(stderr, "Failed to drop the database: Error: %s\n",
          mysql_error(&mysql));
```

---


## mysql_dump_debug_info()
int mysql_dump_debug_info(MYSQL *mysql);

#### **说明**
指示服务器将调试信息写入错误日志。已连接的用户必须具有该 SUPER权限。
#### **返回值**
成功返回零，不成功返回非零。
#### **错误信息**
- CR_COMMANDS_OUT_OF_SYNC

    命令以不正确的顺序执行。
- CR_SERVER_GONE_ERROR

    Mysql服务器已消失。
- CR_SERVER_LOST

    在查询期间，与服务器的连接丢失。
- CR_UNKNOWN_ERROR

    出现未知错误。
#### **示例**
没有

---


## mysql_eof()
bool mysql_eof(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_errno()
unsigned int mysql_errno(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_error()
const char *mysql_error(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_escape_string()


#### **说明**
    注意
    不要使用此功能。mysql_escape_string()没有参数使其能够遵从当前字符集或引用上下文。请改用[mysql_real_escape_string_quote()](#mysql_real_escape_string_quote())。
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_fetch_field()
MYSQL_FIELD *mysql_fetch_field(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_fetch_field_direct()
MYSQL_FIELD *mysql_fetch_field_direct(MYSQL_RES *result, unsigned int fieldnr);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_fetch_fields()
MYSQL_FIELD *mysql_fetch_fields(MYSQL_RES *result);

#### **说明**
返回一个包含结果集所有MYSQL_FIELD结构体的数组。每个结构体提供了结果集每一列字段定义信息。
对于元数据可选的连接，当系统变量result_metadata设置为NONE时，此函数返回NULL。
为了检查结果集是否包含元数据，可以使用[mysql_result_metadata()](#mysql_result_metadata())函数。
有关管理结果集元数据传输的详细信息，可以参考[第28.7.23节“C API可选结果集元数据”](#https://dev.mysql.com/doc/refman/8.0/en/c-api-optional-metadata.html)。
#### **返回值**
结果集所有列的MYSQL_FIELD结构体数组。如果结果集没有元数据则返回NULL。
#### **错误信息**
没有
#### **示例**
```c
unsigned int num_fields;
unsigned int i;
MYSQL_FIELD *fields;

num_fields = mysql_num_fields(result);
fields = mysql_fetch_fields(result);
for(i = 0; i < num_fields; i++)
{
   printf("Field %u is %s\n", i, fields[i].name);
}
```

---


## mysql_fetch_lengths()
unsigned long *mysql_fetch_lengths(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_fetch_row()
MYSQL_ROW mysql_fetch_row(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_field_count()
unsigned int mysql_field_count(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_field_seek()
MYSQL_FIELD_OFFSET mysql_field_seek(MYSQL_RES *result, MYSQL_FIELD_OFFSET offset);

#### **说明**
将字段游标设置为给定的offset，则下次调用[mysql_fetch_field()](#mysql_fetch_field())时检索与offset关联的列的字段定义。
要查找行的开头，请置offset为零。
#### **返回值**
字段游标的前一个值
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_field_tell()
MYSQL_FIELD_OFFSET mysql_field_tell(MYSQL_RES *result);

#### **说明**
返回最后一次调用[mysql_fetch_field()](#mysql_fetch_field())后字段游标的位置。返回值可以作为参数在[mysql_field_seek()](#mysql_field_seek())中使用。
#### **返回值**
当前字段游标的偏移量。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_free_result()
void mysql_free_result(MYSQL_RES *result);

#### **说明**
释放由[mysql_store_result()](#mysql_store_result()),[mysql_use_result()](#mysql_use_result()),[mysql_list_dbs()](#mysql_list_dbs())等等申请的存放结果集的内存空间。当处理完一个结果集之后，必须调用本函数来释放分配的内存空间。

注意：释放之后不要再尝试读取结果集。
#### **返回值**
没有
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_character_set_info()
void mysql_get_character_set_info(MYSQL *mysql, MY_CHARSET_INFO *cs);

#### **说明**
获取默认的客户端字符集信息。默认的字符集可能被函数[mysql_set_character_set()](#mysql_set_character_set())修改过。
#### **返回值**
没有
#### **错误信息**
没有
#### **示例**
此示例显示了MY_CHARSET_INFO结构体中可用的字段：
```c
if (!mysql_set_character_set(&mysql, "utf8"))
{
    MY_CHARSET_INFO cs;
    mysql_get_character_set_info(&mysql, &cs);
    printf("character set information:\n");
    printf("character set+collation number: %d\n", cs.number);
    printf("character set name: %s\n", cs.name);
    printf("collation name: %s\n", cs.csname);
    printf("comment: %s\n", cs.comment);
    printf("directory: %s\n", cs.dir);
    printf("multi byte character min. length: %d\n", cs.mbminlen);
    printf("multi byte character max. length: %d\n", cs.mbmaxlen);
}
```

---


## mysql_get_client_info()
const char *mysql_get_client_info(void);

#### **说明**
返回表示MySQL客户端库版本的字符串; 例如，"8.0.15"。

函数值是提供客户端库的MySQL或Connector/C的版本。有关更多信息，请参考[第28.7.4.5节“C API服务器和客户端库版本”](#https://dev.mysql.com/doc/refman/8.0/en/c-api-server-client-versions.html)。
#### **返回值**
表示Mysql客户端库版本信息的字符串。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_client_version()
unsigned long mysql_get_client_version(void);

#### **说明**
返回表示Mysql客户端库版本信息的整数值。返回值是XYYZZ格式，其中X代指主版本，YY是发行版本（或次要版本），而ZZ是发行版本的子版本。

    major_version*10000 + release_level*100 + sub_version

例如，"8.0.15"返回80015。
函数值是提供客户端库的Mysql或Connector/C的版本。更多信息请参考[第28.7.4.5节“C API服务器和客户端库版本”](#https://dev.mysql.com/doc/refman/8.0/en/c-api-server-client-versions.html)
#### **返回值**
一个表示MySQL客户端库版本的整数。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_host_info()
const char *mysql_get_host_info(MYSQL *mysql);

#### **说明**
返回描述正在使用的连接类型的字符串，包括服务器主机名。
#### **返回值**
表示服务器主机名和连接类型的字符串。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_option()
int mysql_get_option(MYSQL *mysql, enum mysql_option option, const void *arg);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_get_proto_info()
unsigned int mysql_get_proto_info(MYSQL *mysql);

#### **说明**
返回当前连接使用的协议版本。
#### **返回值**
一个表示当前连接协议版本的无符号整数。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_server_info()
const char *mysql_get_server_info(MYSQL *mysql);

#### **说明**
返回一个表示Mysql服务器版本的字符串，例如："8.0.15"
#### **返回值**
表示服务器版本的字符串。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_server_version()
unsigned long mysql_get_server_version(MYSQL *mysql);

#### **说明**
返回表示Mysql服务器版本信息的整数值。返回值是XYYZZ格式，其中X代指主版本，YY是发行版本（或次要版本），而ZZ是发行版本的子版本。

    major_version*10000 + release_level*100 + sub_version

例如，"8.0.15"返回80015。
这个函数在客户端程序决定服务器是否有一些版本特性功能时很有帮助。
#### **返回值**
一个表示服务器版本的整数值。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_get_ssl_cipher()
const char *mysql_get_ssl_cipher(MYSQL *mysql);

#### **说明**
返回连到服务器的指定连接的加密密码。mysql是[mysql_init()](#mysql_init())返回的一个连接句柄。
#### **返回值**
返回连接使用的加密密码字符串，或者连接未加密时返回NULL
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_hex_string()
unsigned long mysql_hex_string(char *to, const char *from, unsigned long length);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_info()
const char *mysql_info(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_init()
MYSQL *mysql_init(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_insert_id()
my_ulonglong mysql_insert_id(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_kill()
int mysql_kill(MYSQL *mysql, unsigned long pid);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_library_end()
void mysql_library_end(void);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_library_init()
int mysql_library_init(int argc, char **argv, char **groups);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_list_dbs()
MYSQL_RES *mysql_list_dbs(MYSQL *mysql, const char *wild);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_list_fields()
MYSQL_RES *mysql_list_fields(MYSQL *mysql, const char *table, const char *wild);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_list_processes()
MYSQL_RES *mysql_list_processes(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_list_tables()
MYSQL_RES *mysql_list_tables(MYSQL *mysql, const char *wild);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_more_results()
bool mysql_more_results(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_next_result()
int mysql_next_result(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_num_fields()
unsigned int mysql_num_fields(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_num_rows()
my_ulonglong mysql_num_rows(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_options()
int mysql_options(MYSQL *mysql, enum mysql_option option, const void *arg);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_options4()
int mysql_options4(MYSQL *mysql, enum mysql_option option, const void *arg1, const void *arg2);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_ping()
int mysql_ping(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_query()
int mysql_query(MYSQL *mysql, const char *stmt_str);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_real_connect()
MYSQL *mysql_real_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd, const char *db, unsigned int port, const char *unix_socket, unsigned long client_flag);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_real_escape_string()
unsigned long mysql_real_escape_string(MYSQL *mysql, char *to, const char *from, unsigned long length);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_real_escape_string_quote()
unsigned long mysql_real_escape_string_quote(MYSQL *mysql, char *to, const char *from, unsigned long length, char quote);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_real_query()
int mysql_real_query(MYSQL *mysql, const char *stmt_str, unsigned long length);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_refresh()
int mysql_refresh(MYSQL *mysql, unsigned int options);

#### **说明**

    注意
    mysql_refresh()已弃用，将在未来的Mysql版本中删除。可以使用mysql_query来执行FLUSH语句。

此函数刷新表或高速缓存，或重置复制服务器。已连接的用户必须具有RELOAD权限。

所述options参数是一个从下面的值中选的任意组合组成的位掩码。可以将多个值一起“或”运算，以通过一次调用执行多个操作。

- REFRESH_GRANT

刷新授权表，例如FLUSH PRIVILEGES。

- REFRESH_LOG

刷新日志，比如FLUSH LOGS。

- REFRESH_TABLES

刷新表缓存，就像FLUSH TABLES。

- REFRESH_HOSTS

刷新主机缓存，就像FLUSH HOSTS。

- REFRESH_STATUS

重置状态变量，如FLUSH STATUS。

- REFRESH_THREADS

刷新线程缓存。

- REFRESH_SLAVE

在从属复制服务器上，重置主服务器信息并重新启动从属服务器，如 RESET SLAVE。

- REFRESH_MASTER

在主复制服务器上，删除二进制日志索引中列出的二进制日志文件并截断​​索引文件，如RESET MASTER。

#### **返回值**
成功返回零，不成功返回非零。
#### **错误信息**
- CR_COMMANDS_OUT_OF_SYNC

    命令以不正确的顺序执行。
- CR_SERVER_GONE_ERROR

    Mysql服务器已消失。
- CR_SERVER_LOST

    在查询期间，与服务器的连接丢失。
- CR_UNKNOWN_ERROR

    出现未知错误。
#### **示例**
没有

---


## mysql_reload()
int mysql_reload(MYSQL *mysql);

#### **说明**
要求数据库服务器重新加载那些有权限的表，当前连接的用户必须具有RELOAD的权限。
本函数不再推荐使用，使用[mysql_query()](#mysql_query())来发出SQL FLUSH_PRIVILEGES语句。
#### **返回值**
成功返回零，不成功返回非零。
#### **错误信息**
- CR_COMMANDS_OUT_OF_SYNC

    命令以不正确的顺序执行。
- CR_SERVER_GONE_ERROR

    Mysql服务器已消失。
- CR_SERVER_LOST

    在查询期间，与服务器的连接丢失。
- CR_UNKNOWN_ERROR

    出现未知错误。
#### **示例**
没有

---


## mysql_reset_connection()
int mysql_reset_connection(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_reset_server_public_key()
void mysql_reset_server_public_key(void);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_result_metadata()
enum enum_resultset_metadata mysql_result_metadata(MYSQL_RES *result);

#### **说明**
返回一个值以表明结果集是否含有元数据。当客户端事先不知道特定结果集是否具有元数据时，它对于元数据可选的连接可能是有用的。例如，如果客户端执行返回多个结果集并可能更改系统变量resultset_metadata值的存储过程，则客户端可以为每个结果集调用mysql_result_metadata()以确定它是否具有元数据。
#### **返回值**
返回以下的其中之一：
```c
enum enum_resultset_metadata {
 RESULTSET_METADATA_NONE= 0,
 RESULTSET_METADATA_FULL= 1
};
```

#### **错误信息**
没有
#### **示例**
没有

---


## mysql_rollback()
bool mysql_rollback(MYSQL *mysql);

#### **说明**
回滚当前事务。

此函数的表现取决于系统变量completion_type的值 。特别地，如果completion_type的值是RELEASE（或2）时，服务器结束交易之后会执行释放和关闭客户端连接。客户端程序调用mysql_close()以从客户端侧关闭连接。
#### **返回值**
成功返回零，不成功返回非零。
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_row_seek()
MYSQL_ROW_OFFSET mysql_row_seek(MYSQL_RES *result, MYSQL_ROW_OFFSET offset);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_row_tell()
MYSQL_ROW_OFFSET mysql_row_tell(MYSQL_RES *result);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_select_db()
int mysql_select_db(MYSQL *mysql, const char *db);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_server_end()
void mysql_server_end(void);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_server_init()
int mysql_server_init(int argc, char **argv, char **groups);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_session_track_get_first()
int mysql_session_track_get_first(MYSQL *mysql, enum enum_session_state_type type, const char **data, size_t *length);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_session_track_get_next()
int mysql_session_track_get_next(MYSQL *mysql, enum enum_session_state_type type, const char **data, size_t *length);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_set_character_set()
int mysql_set_character_set(MYSQL *mysql, const char *csname);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_set_local_infile_default()
void mysql_set_local_infile_default(MYSQL *mysql);

#### **说明**
将LOAD DATA LOCAL INFILE回调函数设置为C客户端库内部使用的默认值。如果mysql_set_local_infile_handler() 尚未调用或未为每个回调提供有效函数，库将自动调用此函数 。
#### **返回值**
没有
#### **错误信息**
没有
#### **示例**
没有

---


## mysql_set_local_infile_handler()
void mysql_set_local_infile_handler(MYSQL *mysql, int (*local_infile_init)(void **, const char *, void *), int (*local_infile_read)(void *, char *, unsigned int), void (*local_infile_end)(void *), int (*local_infile_error)(void *, char*, unsigned int), void *userdata);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_set_server_option()
int mysql_set_server_option(MYSQL *mysql, enum enum_mysql_set_option option);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_shutdown()
int mysql_shutdown(MYSQL *mysql, enum mysql_enum_shutdown_level shutdown_level);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_sqlstate()
const char *mysql_sqlstate(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---


## mysql_ssl_set()
bool mysql_ssl_set(MYSQL *mysql, const char *key, const char *cert, const char *ca, const char *capath, const char *cipher);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---

## mysql_stat()
const char *mysql_stat(MYSQL *mysql);

#### **说明**
返回包含类似于mysqladmin status 命令提供的信息的字符串。包括以秒为单位的正常运行时间以及正在运行的线程，问题，重新加载和打开表的数量。
#### **返回值**
描述服务器状态的字符串，如果发生错误则返回NULL
#### **错误信息**
- CR_COMMANDS_OUT_OF_SYNC

    命令以不正确的顺序执行。
- CR_SERVER_GONE_ERROR

    Mysql服务器已消失。
- CR_SERVER_LOST

    在查询期间，与服务器的连接丢失。
- CR_UNKNOWN_ERROR

    出现未知错误。
#### **示例**
没有
---


## mysql_store_result()
MYSQL_RES *mysql_store_result(MYSQL *mysql);

#### **说明**
#### **返回值**
#### **错误信息**
#### **示例**
```c
```


---

## mysql_thread_id()
unsigned long mysql_thread_id(MYSQL *mysql);

#### **说明**
返回当前连接的线程ID，可以使用(mysql_kill()](#mysql_kill())来杀死该线程。

如果连接丢失后再使用[mysql_ping()](#mysql_ping())进行连接，则线程ID也会改变。这意味着应该在需要时才去获取线程ID，而不该是先获取然后再保存起来供以后使用。

    注意
    如果线程ID大于32位，则此功能在某些系统上可能无法使用，为了避免这种问题，请不要使用mysql_thread_id()，可以通过执行SELECT CONNECTION_ID()来获取线程ID。

#### **返回值**
当前连接的线程ID
#### **错误信息**
没有
#### **示例**
没有

---

## mysql_use_result()
MYSQL_RES *mysql_use_result(MYSQL *mysql);

#### **说明**

#### **返回值**
#### **错误信息**
#### **示例**
```c
```

---

## mysql_warning_count()
unsigned int mysql_warning_count(MYSQL *mysql);

#### **说明**
返回在执行上一个SQL语句期间生成的错误，警告和注释的数量。
#### **返回值**
警告的数量
#### **错误信息**
没有
#### **示例**
没有

---
