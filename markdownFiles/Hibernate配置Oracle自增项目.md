### 在Oracle数据库中，通过对表项目建Sequence和Trigger实现了列的自增，但是在Hibernate中配置后，发现在插入数据时不能自增，经过一番尝试，发现表的配置文件里面的generator要设置为“assigned”，详细代码如下。


----------

----------


### Oracle Sequence代码：
```sql
	-- Create sequence
	create sequence SEQ_URLTBL
	minvalue 1
	maxvalue 9999999999999999999999999999
	start with 15
	increment by 1
	nocache;
```
------
### Oracle Trigger代码：
```sql
	create or replace trigger insert_urltbl
	  before insert
	  on urltbl
	  for each row
	declare
	  -- local variables here
	begin
	  select seq_urltbl.nextval into :new.urlid from dual;
	end insert_urltbl;
```

------

### Hibernate配置代码：
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
	 "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
	<hibernate-mapping>
    <class name="bean.UrlTbl" table="urltbl">
        <id name="urlId" type="java.lang.Integer">
            <column name="urlid"></column>
            <generator class="assigned">
                <!-- param name="sequence">SEQ_URLTBL</param> -->
            </generator>
        </id>
        <property name="folderName" type="java.lang.String">
            <column name="foldername" length="100" />
        </property>
        <property name="urlAddr" type="java.lang.String">
            <column name="urladdr" length="100" />
        </property>
        <property name="saveFlg" type="java.lang.String" insert="false" update="false">
            <column name="saveflg" length="1" />
        </property>
        <property name="updTime" type="java.sql.Date" insert="false" update="false">
            <column name="updtime" />
        </property>
    </class>
	</hibernate-mapping>
	```
