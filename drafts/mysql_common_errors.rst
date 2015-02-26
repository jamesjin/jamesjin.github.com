Mysql 一些与直觉不一致的地方
===================================

.. _sql_mode:

SQL Mode
----------------

.. _case_sensitive:

大小写敏感问题
----------------

1. 表名大小写

2. 文本大小写问题

3. 不同字符被认为相等的问题

.. _supplementary_unicode:

Supplementary Unicode characters
--------------------------------------

.. _mysql_database_and_grant_tables:

innodb_table_monitor, page_parser
-----------------------------------
CREATE TABLE innodb_table_monitor (id int) ENGINE=InnoDB;
这样，在 mysql 日志中会打印出每个索引的 Root page 页码

------------------

mysql database and grant tables
-----------------------------------

mysqld --print-defaults
---------------------------------

--explicit_defaults_for_timestamp
-----------------------------------

--character-set-client-handshake
----------------------------------

character_set_server
--------------------------------

SQL 语句最大 size
-----------------------

varchar(85) vs. varchar(255)
--------------------------------


innodb_sort_buffer_size vs. sort_buffer_size
-----------------------------------------------
http://bugs.mysql.com/bug.php?id=37359
http://www.mysqlperformanceblog.com/2010/10/25/impact-of-the-sort-buffer-size-in-mysql/
http://venublog.com/2010/05/11/mysql-query-engine-scalability-issues/

Mysql Innodb Locks
------------------------------------

# row lock
主要看索引怎么走。索引走过的记录都会被加锁，不管事务隔离级别是 READ-COMMITTED, 还是REAPEATABLE-READ。
IN查询走的索引比较精确，比 between 和 >=, <= 走过的索引记录数要少。如，
select * from t1 where id in(3, 4), 不会锁id为2的记录，
而 select * from t1 where id >= 3 and id <= 4 会锁定 id 小于 3 中的 id最大的那一条记录。 
# next key lock
row lock + gap lock
# Gap lock
READ-COMMITTED 没有 Gap lock, 而 REAPEATABLE-READ 有 Gap Lock。
Gap Lock 主要是为了防止出现幻读现象，为了避免别的事物插入记录

注意：查询是否走了期望中的索引，很多时候用小表做测试时，Mysql会倾向于使用全表扫描

group_concat reverse
--------------------------------------

SET @rank=0;
select user_id, sr.orders, SUBSTRING_INDEX(SUBSTRING_INDEX(sr.orders, ',',n),',',-1) as remark_id, n
from `shortcut_remark_order` sr, (select @rank:=@rank+1 as n from shortcut_remark) as tmp 
where sr.orders is not null and sr.orders!='' and n <= char_length(sr.orders) - char_length(replace(orders, ',','')) + 1 and sr.wangwang_nick=':'

有一个好办法，就是利用一个数据量比较大的表（一般用引用到的数据表就可以），select出n（n为最大的逗号数+1）条数据

innodb_sort_buffer 和 mysq sort buffer，后者调大了反而慢?
--------------------------------------------------------------------



.. author:: default
.. categories:: none
.. tags:: none
.. comments::
