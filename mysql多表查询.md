	数据库的创建 : create database 数据库的名 character set 字符集  collate 校对规则

​	数据库的删除: drop database 数据库名

​	修改: alter database 数据库 character set 字符集(utf8)

​	查询:  show databases;

​		  show create database 数据库的名字

​		  select database();

​	切换数据库 :

​			use 数据库的名字	

​	表结构的操作:

​		创建:  create table 表名(	

​				列名 列的类型  列的约束,

​				列名 列的类型  列的约

​			)

​			列的类型: char / varchar   

​			列的约束: 

​					primary key 主键约束

​					unique : 唯一约束

​					not null 非空约束

​		      自动增长 : auto_increment

​	     删除 :  drop table 表名

​	    修改:   alter table 表名 (add, modify, change , drop)

​			rename table 旧表名  to 新表名

​			alter table 表名 character set 字符集

​	   查询表结构:

​			show tables; 查询出所有的表

​			show create table 表名: 表的创建语句, 表的定义

​			desc 表名:　表的结构


​	表中数据的操作

​		插入：　 insert into 表名(列名,列名) values(值1,值2);

​		删除:        delete from 表名 [where 条件]

​		修改:        update 表名 set 列名='值' ,列名='值' [where 条件];

​		查询:    select [distinct]  * [列名1,列名2] from 表名 [where 条件]

​			  as关键字: 别名

​			   where条件后面:

​		

​					关系运算符:  > >= < <= !=  <>

​						--判断某一列是否为空:  is null    is not null

​						in 在某范围内

​						between...and

​					逻辑运算符: and or not

​					模糊查询:  like    

​							_ : 代表单个字符

​							%: 代表的是多个字符

​					分组: group by 

​					分组之后条件过滤:  having

​					聚合函数: sum()  ,avg() , count()  ,max(), min()

​					排序: order by  (asc 升序, desc 降序)

​							

​

- 多表之间的关系如何维护: 外键约束 :   foreign key
- 添加一个外键: alter table product add foreign key(cno)  references category(cid);
  - ​		foreign key(cno) references category(cid)
  - 删除的时候, 先删除外键关联的所有数据,再才能删除分类的数据
- 建表原则:
  - 一对多:
    - 建表原则: 在多的一方增加一个外键,指向一的一方
  - 多对多:
    - 建表原则: 将多对多转成一对多的关系,创建一张中间表
  - 一对一: 不常用, 拆表操作
    - 建表原则:  将两张表合并成一张表
      - 将两张表的主键建立起关系
      - 将一对一的关系当作一对多的关系去处理




主键约束: 默认就是不能为空, 唯一

-  外键都是指向另外一张表的主键
-  主键一张表只能有一个

唯一约束:  列面的内容, 必须是唯一, 不能出现重复情况, 为空

- 唯一约束不可以作为其它表的外键
- 可以有多个唯一约束



一对多 : 建表原则: 在多的一方添加一个外键,指向一的一方

多对多: 建表原则:

​		拆成一对多

​		创建一张中间表, 至少要有两个外键, 指向原来的表

一对一: 建表原则: 合并一张表, 将主键建立关系 , 将它当作一对多的情况来处理




#### 多表查询

- 交叉连接查询  笛卡尔积

- 内连接查询----------------结果是2个表的交集
	区别：结果都一样
	-隐式内连接 ： 在查询出的结果的基础上去做的where条件过滤
 		select * from customers c,orders o where c.customer_id=o.customer_id;
	-显示内连接 ： 带着条件去查询结果，执行效率要高
		 select * from customers c inner join orders o on c.customer_id=o.customer_id;
	 
- 左外连接：------------结果是左表+交集
	以左表作为基准进行查询，左表数据会全部显示出来，右表如果和左表匹配的
数据则显示相应字段的数据，如果不匹配则显示为 null。

	select * from customers c left outer join orders o on c.customer_id=o.customer_id;


- 右外连接  ：---------------结果是右表+交集
	以右表作为基准进行查询，右表数据会全部显示出来，左表如果和右表匹配的
数据则显示相应字段的数据，如果不匹配则显示为 null。

	select * from customers c right outer join orders o on c.customer_id=o.customer_id;




#### 分页查询

- 每页数据数据3

- 起始索引从0 

- 第1页: 0

- 第2页: 3

  起始索引:  index 代表显示第几页 页数从1开始

  每页显示3条数据

  startIndex  = (index-1)*3

  ​

第一个参数是索引 

第二个参数显示的个数

select * from product limit 0,3;

select * from product limit 3,3;



#### 子查询(了解的内容,非常重要)

#索引
/*索引的概述和优缺点和种类*/
什么是索引？
索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分),它们包含着对数据表里所有记录的引用指针.
类比理解:数据库中的索引相当于书籍目录一样,能加快数据库的查询速度.
没有索引的情况,数据库会遍历全部数据后选择符合条件的选项.
创建相应的索引,数据库会直接在索引中查找符合条件的选项.

索引的性质分类：
索引分为聚集索引和非聚集索引两种,聚集索引是索引中键值的逻辑顺序决定了表中相应行的物理顺序,而非聚集索引是不一样;
聚集索引能提高多行检索的速度,而非聚集索引对于单行的检索很快.

索引的优点：
(1)加快数据检索速度 (创建索引主要原因)
(2)创建唯一性索引,保证数据库表中每一行数据的唯一性
(3)加速表和表之间的连接
(4)使用分组和排序子句对数据检索时,减少检索时间
(5)使用索引在查询的过程中,使用优化隐藏器,提高系统的性能

索引的缺点：
(1)创建索引和维护索引要耗费时间,时间随着数据量的增加而增加
(2)索引需要占用物理空间和数据空间
(3)表中的数据操作插入、删除、修改, 维护数据速度下降

索引种类
(1)普通索引: 仅加速查询
(2)唯一索引: 加速查询 + 列值唯一（可以有null）
(3)主键索引: 加速查询 + 列值唯一（不可以有null）+ 表中只有一个
(4)组合索引: 多列值组成一个索引,专门用于组合搜索,其效率大于索引合并
(5)全文索引: 对文本的内容进行分词,进行搜索 (注意:目前仅有MyISAM引擎支持)
