

系统运维常用

备份
mysqldump -u root -p nm@401 --default-character-set=utf-8 oldboy > oldboy.sql
mysqldump -u root -p nm@401 oldboy  --default-character-set=utf-8 | gzip > /root/oldboy.sql.gz
-----------

------------
全量备份
mysqldump -u root -p nm@401 --default-character-set=utf-8 oldboy > oldboy.sql
mysqldump -u root -p nm@401 oldboy  --default-character-set=utf-8 | gzip > /root/oldboy.sql.gz

导出表数据
mysqldump -uroot -ppasswd dbname tabtest>db_test.sql;
mysqldump -u root -p nm@401 oldboy tabtest -F -B  --default-character-set=utf-8 | gzip > /root/tab_test_$(date+%F).sql.gz


-B
此参数用于指定多个数据库，同-A参数，生成创建库的语句及use语句。

-F
同参数--flush-logs，在dump之前刷新日志，即生成一个新的二进制日志。一次dump多个库时，每个库都会刷新一次。但使用--master-data或--lock-all-tables只会刷新一次。


------
加上日期
mysqldump -u root -p nm@401 oldboy -F -B  --default-character-set=utf-8 | gzip > /root/oldboy_$(date +%F).sql.gz

mysqldump -u root -p oldboy -S /data/3306/mysql.sock -F -B  --default-character-set=utf8 | gzip > /root/oldboy_$(date +%F).sql.gz

导出整个数据库结构（不包含数据）
mysqldump -uroot -d entrym> dump.sql

导出单个数据表结构（不包含数据）
mysqldump -h localhost -uroot -p123456  -d database table > dump.sql
-----
增量备份
-------
恢复命令
mysql -u root -p -S data/3306/mysql.sock < /tmp/oldboy_bak.sql
mysql -u root -p -S data/3306/mysql.sock -e "select * from oldboy.test;"    # 这种方式也可以直接操作sql 查看是否已恢复
--------------
＝＝＝＝＝＝＝＝
操作库、表结构

创建数据库
create database oldboy_default;
或 create database `oldboy_default`;
指定编码格式
CREATE DATABASE db_name DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

删除数据库
drop database oldboy_default;
－－
进入数据库
use oldboy_default;
显示库中的表
show tables;

显示表的结构
describe 表名;
－－－
数据表的创建与更新

创建表
create table test(
id int(4) not null primary key auto_increment,
name char(20) not null
);
＝＝＝＝＝
条件约束有
唯一约束：UNIQUE
主键约束：PRIMARY KEY
外键约束：FOREIGN KEY
检查约束：CHECK
非空约束：NOT NULL
＝＝＝＝＝

SQL语句添加字段说明语法：EXECUTE sp_addextendedproperty N'MS_Description','列属性说明',N'user',N'dbo',N'table',N'table_name(表名)',N'column',N'column_name(列名)'

--以下示例是给Card表的字段CardID添加注释/说明为“卡号”
EXECUTE sp_addextendedproperty N'MS_Description','卡号',N'user',N'dbo',N'table',N'Card',N'column',N'CardID'

＊＊＊
创建与删除索引
CREATE [UNIQUE] [CLUSTER] INDEX 索引名 ON 表名(列名[排序方式]....)

CREATE INDEX i_profession ON T_teacher(profession);
为T_teacher的profession列创建了名为i_profession的单列索引

CREATE INDEX i_dept_profession ON T_teacher(dept,profession);
为T_teacher的dept 和profession列创建了名为i_dept_profession的复合索引

DROP INDEX 索引名;
DROP INDEX i_profession;
删除名为i_profession的索引

－－－－
修改数据表 （关键字 ALTER TABLE）

向表中增加一新列
ALTER TABLE table_name ADD (column_name datatype [constraint_condition]);

向 T_teacher表增加了名为salary 的新列
ALTER TABLE T_teacher ADD salary INT NOT NULL;

增加一个约束条件
ALTER TABLE table_name ADD constraint_type(column_name);

为 T_dept表的deptID列字段增加了主键约束
ALTER TABLE T_dept ADD PRIMARY KEY(deptID);

增加一个索引
ALTER TABLE table_name ADD INDEX (column_name1[, column_name2]...);

为T_curriculum 表的credit列字段增加一个名为i_credit的索引
ALTER TABLE T_curriculum ADD INDEX i_credit(credit);

修改表中的其中一列
ALTER TABLE table_name MODIFY column_name datatype;

修改T_student表中sex字段列的数据类型修改为 CHAR长度为2；
ALTER TABLE T_student MODIFY sex CHAR(2);

删除表中一列
ALTER TABLE table_name DROP column_name;

删除T_teacher表中的deptID列
ALTER TABLE T_teacher DROP deptID;

删除一个约束
ALTER TABLE table_name DROP constraint_type;

删除T_dept表中的PRIMARY KEY（主键约束）
ALTER TABLE T_dept DROP PRIMARY KEY;

删除数据中的一个表
DROP TABLE table_name[CASCADE CONSTRAINTS];        // CASCADE CONSTRAINTS表示删除表中的外键约束

删除T_teacher 表，同时删除T_curriculum的外键约束
DROP TABLE T_teacher;
DROP TABLE T_curriculum CASCADE CONSTRAINTS;


－－－－－－
操作数据

查询数据

SELECT stuID FROM T_result;
查询T_result表中的stuID列数据，并且不输出重复数据 （关键字DISTINCT）
SELECT DISTINCT stuID FROM T_result;

使用别名查询
SELECT 目标列 [AS] 列别名[, 目标列 [AS] 列别名...] FROM 表名或者视图名 [, 表名或者视图];

SELECT stuID AS 学生编号, stuName AS 学生姓名, age AS 年龄, sex AS 性别, birth AS 出生日期 FROM T_student;
如果使用的别名太敏感，造成sql语句执行报错，就要在使用的别名用单引号或双引号包住
SELECT stuID AS '学生编号', stuName AS '学生姓名' FROM T_stude;

查询多个 id号
select * from 表名 where id in (1,2,5);

select * from 表名 where id=1 or id=2 or id=5;

select * from `business`.`admin_user` where id=2 or id=3 or id=4;
select * from `business`.`admin_user` where admin_id in (2,3,4);

--
连表查询

带条件的UNION查询，也可以查询同一张表，查询年龄为18，23岁的学生信息

SELECT ID,Name FROM Student WHERE Age=18
UNION
SELECT ID,Name FROM Student WHERE Age=23;

--
SELECT ID,Name FROM Students
UNION
SELECT ID,Name FROM Teachers;

－－－－－－－
插入
insert into test (id,name) values(1, 'dfa');
insert into test (name) values('affw');  # 自增生成id
select * from test;


---------
更新

update table1 set field1=value1 where ki=2;
update students set age=21,city="gz" where id=103;

UPDATE categories
SET display_order = CASE id
WHEN 1 THEN 3
WHEN 2 THEN 4
WHEN 3 THEN 5
END,
title = CASE id
WHEN 1 THEN 'New Title 1'
WHEN 2 THEN 'New Title 2'
WHEN 3 THEN 'New Title 3'
END
WHERE id IN (1,2,3);

MySql中INSERT语法具有一个条件DUPLICATE KEY UPDATE，这个语法和适合用在需要判断记录是否存在，不存在则插入存在则更新的记录。
基于上面这种情况，针对更新记录，仍然使用insert语句，不过限制主键重复时，更新字段。如下：
复制代码 代码如下:

INSERT INTO t_member (id, name, email) VALUES
(1, 'nick', 'nick@126.com'),
(4, 'angel','angel@163.com'),
(7, 'brank','ba198@126.com')
ON DUPLICATE KEY UPDATE name=VALUES(name), email=VALUES(email);

用一个表的数据更新另一个表中的数据
Update T1
set dc=(select dc1 from T2 where T1.A=T2.A1 AND T1.B=T2.B1)
WHERE T1.A = (
    SELECT T2.A1 FROM T2
    WHERE T1.A=T2.A1 AND T1.B=T2.B1
);

Update student Set student.stu_name = ( Select lag.lag_name from lag where lag.lag_id = student.stu_id )
Where student.stu_id in ( Select lag.lag_id from lag );
---
删除
delete from stude where id=2;
－－－－－
统计分析

总数：select count as totalcount from table1;
求和：select sum(field1) as sumvalue from table1;
平均：select avg(field1) as avgvalue from table1;
最大：select max(field1) as maxvalue from table1;
最小：select min(field1) as minvalue from table1;

－－－－－－－

创建用户
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';

CREATE USER 'pig'@'192.168.1.101' IDENTIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '123456';

GRANT privileges ON databasename.tablename TO 'username'@'host' ;
GRANT ALL ON *.* TO 'pig'@'%';


权限
企业应用中的常规MySQL用户
• 企业生产系统中MySQL用户的创建通常由DBA统一协调创建，而且按需
创建
• DBA通常直接使用root用户来管理数据库
• 通常会创建指定业务数据库上的增删改查、临时表、执行存储过程的权限给应 用程序来连接数据库

Create user app_full identified by ‘mysql’;
Grant select,update,insert,delete,create temporary tables,execute on esn.* to
app_full@’10.0.0.%’;
mysql>show grants for app_full@'10.0.0.%';
+------------------------------------------------------------------------------------------------------------+
|Grantsforapp_full@10.0.0.% |
+------------------------------------------------------------------------------------------------------------+
|GRANTUSAGEON*.*TO'app_full'@'10.0.0.%' |
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE TEMPORARY TABLES, EXECUTE ON `esn`.* TO 'app_full'@'10.0.0.%' |
• 通常也会创建指定业务数据库上的只读权限给特定应用程序或某些高级别人员 来查询数据，防止数据被修改

Create user app_readonly identified by ‘mysql’;
Grant select on esn.* to app_readonly identified by ‘mysq’;
-------------
