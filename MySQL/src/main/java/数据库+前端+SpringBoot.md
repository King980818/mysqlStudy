# 数据库

### SQL语句

- 定义 : 结构化查询语言(Structured Query Language) 简称SQL . 是一种特殊目的的		   编程语言 , 是一种数据库查询和程序设计语言 , 用于存取数据以及查询 , 更新		   和管理数据库系统 ; 同时也是数据库脚本文件的扩展名 . 
- 分类 : 
  - DML (Data Manipulation Language) 数据操纵语言
    - 如 : insert , delete , update , select 简称CRUD 新增Create , 查询 Retrieve , 修改Update , 删除Delete
  - DDL(Data Definition Language)数据库定义语言
    - 如 : create table之类
  - DCL(Data Control Language) 数据库控制语言
    - 如 : grant , deny , revoke等 , 只有管理员才有相应的权限
  - DQL(Data Query Language) 数据查询语言
    - 注意 : SQL不区分大小写

### 数据库相关SQL

| 数据库操作             | 指令                                       |
| ---------------------- | :----------------------------------------- |
| 创建数据库             | create database 数据库名;                  |
| 创建数据库并指定字符集 | create database 数据库名 charset=UTF8/gbk; |
| 查询所有数据库         | show databases;                            |
| 查询数据库信息         | show create database 数据库名;             |
| 使用数据库             | use 数据库名;                              |
| 删除数据库             | drop database 数据库名;                    |
|                        |                                            |

### 表相关的SQL语句

| 表相关操作                    | 指令                                                         |
| ----------------------------- | :----------------------------------------------------------- |
| 创建表                        | create table 表名 (字段1名  类型 , 字段2名  类型)charset=UTF8; |
| 查询所有表                    | show  tables;                                                |
| 查询表信息                    | show create table 表名;                                      |
| 查询表字段                    | desc 表名;                                                   |
| 删除表                        | drop table 表名;                                             |
| 修改表名                      | rename table 原名 to 新名                                    |
| 添加表字段(最后添加)          | alter table 表名 add 字段名  类型 ;                          |
| 添加表字段(最前面添加)        | alter table 表名 add 字段名  类型 first;                     |
| 添加表字段(在xxx字段后面添加) | alter table 表名 add 字段名  类型 after  xxx;                |
| 删除表字段                    | alter table  表名 drop 字段名;                               |
| 修改表字段                    | alter table 表名 change 原名  新名 新类型;                   |
|                               |                                                              |

### 数据相关SQL

| 数据操作                       | 指令                                                 |
| ------------------------------ | ---------------------------------------------------- |
| 插入数据(全表插入)             | insert into 表名 values (值1 , 值2 , 值3)            |
| 插入数据(指定字段插入)         | insert into 表名(字段1名 , 字段2名)values(值1 , 值2) |
| 批量插入数据                   | 在values后面写多组值                                 |
| 查询数据(查询表中所有的名字)   | select  字段信息  from  表名;                        |
| 查询数据                       | select  字段信息  from  表名  where  条件;           |
| 查询表中所有数据的左右字段信息 | select*from 表名;                                    |
| 修改数据                       | update 表名 set 字段名=值 , 字段名=值where 条件;     |
| 删除数据                       | delete from 表名 where 条件 ;                        |
|                                |                                                      |

### 中文乱码

​		如果执行上面包含中文的SQL语句出现十六进制错误的提示 , 执行set names GBK;

或者set names utf8; .

### 字段约束

- primary key     主键约束 , 自动创建唯一索引
- Foregin key      外键约束 , 外键字段的内容是引用另一表的字段内容 , 不能瞎写
- Unique Index   唯一索引 , 唯一值但不是主键
- 对于约束的好处是 , 数据库会进行检查 , 违反约束会报错 , 操作失败 . 数据库提供了丰富的约束检查 , 还有其他约束 , 但现今弱化关系型数据库的前提下 ,基本已经很少使用 . 

#### 主键约束 primary key

​		主键 : 表示数据唯一性的字段 ,  一个表中只能有一个主键约束 , 主键约束可以用				   修改表字段进行添加 , 但不能通过修改表字段去除

​		约束 : 创建表时给表字段添加的限制条件

​		主键约束 :  唯一且非空

​		格式 : 

​				 create table t1(id int primary key,name varchar(50))charset=utf8;

​				 insert into t1 values(1,'aaa');

​				 insert into t1 values(1,'bbb');	//报错 : 不能插入重复的值

​				 insert into t1 values(null,'ccc');   //报错 : 不能为null

#### 主键约束+自增 primary key auto_increment

​		自增规则 : 从历史最大值 +1 , 自增可以用修改表字段进行添加和去除 , 

​		格式 :

​				  create table t2 (id int primary key auto_increment,name varchar(50));

​				  insert into t2 values (null,'aaa');			id=1

​				  insert into t2 values (null,'bbb');			id=2

​				  insert into t2 values(10,'ccc');				id=10

​				  insert into t2 values(null,'ddd');			id=11

​				  delete from t2 where id>=10;

​				  insert into t2 values(null,'eee');			id=12

#### 非空约束

- 非空约束 : 如果为一个列添加了非空约束 , 那么这个列的值就不能为空 , 但可以重复
- 添加非空约束 , 例如为password添加非空约束 :

```sql
create table user(
	id in primary key auto_increment,
	password varchar(50) not null);
show tables;
insert into users values(null,null);//不符合非空约束
insert into users values(null,'123');//OK
```

#### 唯一约束

- 唯一约束 : 如果为一个列添加了唯一约束 , 那么这个列的值就必须是唯一的(即不能重复),但可以为空 . 添加唯一约束 , 例如为username添加唯一及非空约束 : 

- ```sql
  create table test(
  	id int primary key auto_increment,
  	username varchar(50) unique--唯一约束);
  show tables;
  insert into test values(null,'lisi');
  insert into test values(null,'lisi');//username的值要唯一,重复会报错
  select * from test;
  ```

### 数据类型 

#### 1 整数

- tinyint , int 整数类型
- float , double 小数类型
- numeric(5 , 2) decimal(5 , 2)--也可以表示小数 , 表示总共5位 , 其中小数可以有两位 . 
- decimal 和 numeric 表示精确的整数数字

​				int(m) 和 bigint(m)  bigint等效java中的long   m代表显示长度

​				m = 5 存 18 查出来00018 用来补零  需要结合zerofill关键字使用

​				create table t3(age int(5) zerofill);

​				insert into t3 values(18);

​				select * from t3;

#### 2 浮点数

​				double(m,d) 和 float(m,d) m代表总长度 , d 代表小数长度

​				create table t4(salary double(5,3));

​				insert into t4 values(56.789);

​				insert into t4 values(34.56789);		//小数部分四舍五入

​				insert into t4 values(345.678);		//报错 超出范围

#### 3 字符串

​				char(m) : 固定长度 m=10 存abc  占10个字符长度  好处 : 执行效率略高 , 最大长度255 , 最多容纳2000个字符 .

​				varchar(m) : 可变长度 m=10 存abc 占3个字符长度 好处 : 节省空间 , 最大长度65535 , 最多容纳4000个字符 , 建议保存长度较小的数据时使用(低于255时使用)

​				text(m) : 可变长度 最大长度65535 , 建议保存长度较大的数据时使用(不推荐使用)

#### 4 日期

​				date : 只能保存年月日

​				time : 只能保存时分秒

​				datetime : 年月日时分秒 , 最大值9999-12-31 , 默认值为null

​				timestamp (时间戳 , 从1970年1月1日到指定日期的毫秒数)

​				举例 : 

​						create table t5(t1 date,t2 time,t3 datetime,t4 timestamp);

​						insert into t5 values('2020-11-20',null,null,null);

​						insert into t5 values(null,'10:58:20','2019-10-20 10:20:30',null);

#### 5 图片

- blob 二进制数据 可以存放图片 ,声音 , 容量4g . 早就有这样的设计 , 但是缺点非常明显 , 数据库庞大 , 备份缓慢 , 这些内容去备份多份价值不大 . 同时数据库迁移时过大 , 迁移时间过久 . 所以目前主流都不会直接存储这样的数据 , 而只存放访问路径 , 文件则存放在磁盘上 .

### 导入*.sql文件

​				通过以下指令 : 

​				格式 : source 路径;

​							source D:/emp.sql;

​				导入完成后  测试查询

​				show tables;     查询两个表  emp和dept

​				select * from emp; 里面会有一堆数据

### 条件查询

#### 去重  distinct

​				1.查询员工表中出现了哪几种不同的工作

​					select distinct job from emp;

​				2.查询员工表里面有哪几个部门id

​					select distinct deptId from emp;

#### is null 和 is not null

​				当查询字段的值为空值时,不能用等号进行判断,使用is

​				1.查询没有上级领导的员工信息;

​					select * from emp where manager is null;

​				2.查询有上级领导的员工信息;

​					select * from emp where manager is not null;

#### 比较运算符 > < >= <= = != 和 <>

​				1.查询工资大于等于3000的员工姓名和工资

​					select name,sal from emp where sal>=3000;

​				2.查询1号部门的员工姓名和工作

​					select name,job from emp where deptId=1;

​				3.查询不是程序员的员工姓名 , 工资和工作(用到上面两种不同的写法)

​					select name,sal,job from emp where job!='程序员';

​					select name,sal,job from emp where job<>'程序员';

​				4.查询有奖金的的员工姓名和奖金

​					select name,comm from emp where comm>0;

#### and和or

- and 类似Java中的&&
- or 类似Java中的||
- not 类似Java中的 !

1. ```sql
   1. 查询1号部门工资高于2000的员工信息
      select * from emp where deptId=1 and sal>2000;
   2. 查询是程序员或者工资等于5000的员工信息
      select * from emp where job='程序员' or sal=5000;
   3. 查询出CEO和项目经理的名字
      select name from emp where job='项目经理' or job='CEO';
   4. 查询出奖金为500并且是销售的员工信息
      select * from emp where comm=500 and job='销售';
   
      查询出工资为3000 , 1500 和 5000的员工信息
      select * from emp where sal=3000 or sal=1500 or sal=5000;
   ```


#### in关键字

- 当查询某个字段的值为多个值的时候使用

1. ```sql
   1. 查询出工资为3000,1500和5000的员工信息
      select * from emp where sal in(3000,1500,5000);
   2. 查询工资不是3000,1500和5000的员工信息
      select * from emp where sal not in(3000,1500,5000);
   3. 查询1号和2号部门工资大于2000的员工信息
      select * from emp where deptId in(1,2) and sal>2000;
   
      查询工资在2000到3000之间的员工信息
      select * from emp where sal>2000 and sal<3000;
   ```


#### between x and y

- 查询数据在两者之间使用 , 包含 x 和 y

1. 查询工资在2000到3000之间的员工信息

   select * from emp where sal between 2000 and 3000;

2. 查询工资在2000到3000之外的员工信息

   select  * from emp where sal not between 2000 and 3000;

   ##### 综合练习题

```sql
1.查询3号部门中有上级领导的员工信息
	select * from emp where deptId=3 and manager is not null;
2.查询2号部门工资在2000-3000之间的员工姓名,工资和部门编号
	select name,sal,deptId from emp where deptId=2 and (sal between 2000 and 3000);
3.查询1号部门工资为800和1600的员工信息
	select * from emp where deptId=1 and sal in(800,1600);
4.查询1号部门中出现了哪几种不同的工作 
	select distinct job from emp where deptId=1;
5.查询1号和2号部门工资低于1000的员工信息
	select * from emp where deptId in(1,2) and sal<1000;
```

#### 模糊查询 like

- _ : 代表一个未知字符
- % : 代表0或多个未知字符
- 举例 : 
  - 以x开头		x%
  - 以x结尾        %x
  - 包含x           %x%
  - 第二个字符是x   _x%
  - 第三个是x倒数第二个是y    _ _ x %y _

1. ```sql
   1. 查询姓孙的员工信息
      select * from emp where name like "孙%";
   2. 查询工作中第二个字是售的员工信息
      select * from emp where job like "_售%";
   3. 查询名字中以精结尾的员工姓名
      select name from emp where name like "%精";
   4. 查询名字中包含僧的员工并且工资高于2000的员工信息
      select * from emp where name like "%僧%" and sal>2000;
   5. 查询1号和2号部门中工作以市开头的员工信息
      select * from emp where deptId in(1,2) and job like "市%";
   6. 查询有领导的员工中是经理的员工姓名
      select name from emp where manager is not null and job like "%经理%";
   ```

#### 排序 order by

- 格式 : order by  排序字段名 asc 升序(默认)或desc降序

```sql
1.查询所有员工的姓名和工资并按照工资升序排序
	select name,sal from emp order by sal;
2.查询所有员工
	select name,sal from emp order by sal desc;
3.查询所有员工姓名,工资和部门编号,按照部门编号升序,如果部门编号一致则按照工资降序	 排序
	select name,sal,deptId from emp order by deptId,sal desc;

```

#### 分页查询 limit

- 格式 : limit 跳过的条数 , 请求的条数(每页的条数)
- 跳过的条数 = (请求的页数-1)*每页的条数
- 举例 :
  - 查询第一页的10条数据    limit 0 , 10
  - 查询第二页的10条数据    limit 10 , 10
  - 查询第5页的10条数据      limit 40,10
  - 查询第8页的5条数据        limit 35,5
  - 查询第7页的9条数据        limit 54,9

```sql
1.工资升序排序 查询前三名
	select * from emp order by sal limit 0,3;
2.查询员工表中工资降序排序 第二页的3条数据
	select * from emp order by sal desc limit 3,3;
3.查询1号部门中工资最高的员工信息
	select * from emp where deptId=1 order by sal desc limit 0,1;
4.查询销售相关工作里面赚钱最少的员工姓名和工资
	select name,sal from emp where job like '%销售%' order by sal limit 0,1;
5.按照工资降序排序查询工资高于1000的所有员工姓名和工资, 查询第三页的两条数据
	select name,sal from emp where sal>1000 order by sal desc limit 4,2;

```

### 数值计算 + - * / %

```sql
1. 查询每个员工的姓名 , 工资和年终奖(年终奖=5*月工资)
   select name,sal,5*sal from emp;
2. 查询2号部门中的员工姓名 , 工资和涨薪5块钱之后的工资
   select name,sal,sal+5 from emp;
3.让员工表中3号部门的员工每人涨薪5块钱
	update emp set sal=sal+5 where deptId=3;

```

### 别名

```sql
select name as '姓名' from emp;
select name '姓名' from emp;
select name 姓名 from emp;
```

```sql
1.
	select * from emp where manager is not null and sal between 1000 and 3000;
2.
	select name,job from emp where name like '%精%' or job like '%序%';
3.
	select * from emp where deptId in(1,2) order by sal limit 4,2;
4.
	select * from emp where manager=8 order by sal desc limit 0,3;

```

### 基础函数

- SELECT  **lower**(dname) from dept; -----数据转小写
- select **upper**(dname) from dept; -----数据转大写 
- select **length**(dname) from dept; -----数据的字节长度
- select **substr(dname , 1 , 3)** from dept; -----截取[1,3]不能写0 , 截取第一个字母到第三个字母的数据
- select **concat(dname,'123')** from dept; -----拼接数据
- select **replace (dname , 'a' , '666')** from dept;----把字符串dname中的a替换成666
- select **ifnull(comm , 10)** from dept; -----判断 , 如果是comm是null , 用10替换
- 
- select comm , **round(comm)** from emp; -----round四舍五入,直接四舍五入取整
- select comm , **round(comm , 1)**  from emp; -----四舍五入并保留一位小数
- select comm , **ceil(comm)** , **floor(comm)** from emp; -----ceil 向上取整;floor 向下取整
- now() 获取当前时间的年月日时分秒
- curdate() 获取当前时间的年月日
- curtime()获取当前时间的时分秒
- select year(now()) , month(now()) , day(now()) , hour(now()) , minute(now()) , second(now()) ; -----每个单独的函数分别获取年月日时分秒

### 聚合函数

- 可以对查询的多条数据进行统计查询
- 包括的统计方式有 : 
  - 平均值  avg(字段名)
  - 最大值  max(字段名)
  - 最小值  min(字段名)
  - 求和      sum(字段名)
  - 计数      count(字段名或*)

1. 平均值 avg (字段名)

   - 查询1号部门的平均工资

     select avg (sal) from emp where deptId=1;

2. 最大值 max (字段名)

   - 查询程序员的最高工资

     select max(sal) from emp where job='程序员';

3. 最小值 min(字段名)

   - 查询2号部门的最低工资

     select min(sal) from emp where deptId=2;

4. 求和 sum(字段名) 

   - 查询3号部门的工资总和

     select sum(sal) from emp where deptId=3;

5. 计数 count(字段名或*)

   - 查询1号部门人数

     select count(*) from emp where deptId=1;

```sql
1.
	select sum(sal) from emp where job like '%销售%';
2.
	select count(*) from emp where deptId=1 and sal>1500;
3.
	select count(*) 人数,avg(sal) 平均工资 from emp where deptId=1 and name like '%僧%';
4.
	select max(sal) from emp where manager=4;

```

### 分组 group

#### 分组查询 group by

- 分组查询可以将某个字段相同值得数据划分为一组 , 以组为单位进行统计查询

```sql
1. 查询每一种工作的平均工资
   select job , avg(sal) from emp group by job;
2. 查询每个部门的平均工资
   select deptId 部门编号,avg(sal) 平均工资 from emp group by deptId;
3. 查询每种工作的人数
   select job,count(*) from emp group by job;
4. 查询每个部门工资大于2000的人数
   select  deptId 部门 , count(*) from emp where sal>2000 group by deptId;
5. 查询平均工资最高的部门编号
   select deptId 部门编号,avg(sal) 平均工资 from emp group by deptId order by avg(sal) desc limit 0,1;
6. 查询人数最多的工作名称
   select job , count(job) from emp group by job order by count(job) desc limit 0,1;
	
```

#### having

- where 后面只能写普通字段的条件 , 不能写聚合函数的条件
- having 关键字和group by分组查询 

```sql
1.查询每个部门的平均工资 , 只查询平均工资高于2000的数据
	select deptId ,avg(sal) from emp group by deptId having avg(sal)>2000;
	select deptId ,avg(sal) a from emp group by deptId having a>2000;
	别名可以起到代码复用的作用
2.查询每种工作的人数 , 只查询人数大于1的工作名称和人数
	select job 工作,count(*) 人数 from emp group by job having 人数>1;
3.查询每个部门的工资总和,只查询有领导的员工,并且要求
	select deptId 部门, sum(sal) 总和 from emp where manager is not null group by deptId having 总和>5400;
4.查询每个部门的平均工资,只查询工资在1000到3000之间的,并且过滤掉平均工资低于2000的部门信息
	select deptId 部门,avg(sal) 平均工资 from emp where sal between 1000 and 3000 group by deptId having 平均工资>=2000;
5.查询每种工作的人数大于1个,并且只查询1号部门和2号部门
	select job,count(*) 人数 from emp where deptId in(1,2) group by job having 人数>1 order by 人数 desc;
6.查询高于2000工资人数最多的工作
	select job from emp where sal>2000 group by job order by count(*) desc limit 0,1;

```

### 子查询(嵌套查询)

- 将一条sql语句嵌入到另外一条sql语句中,当作查询条件的值

```sql
1.查询工资高于1号部门平均工资的员工信息
	select * from emp where sal>(select avg(sal) from emp where deptId=1);
查询1号部门的平均工资
	select avg(sal) from emp where deptId=1;
查询工资高于上面结果的员工信息
	select * from emp where sal>(select avg(sal) from emp where deptId=1);
```

```sql
2.查询工资最高的员工信息
	select * from emp where sal=(select max(sal) from emp);
3.查询工资高于2号部门最低工资的员工信息
	select * from emp where sal>(select min(sal) from emp where deptId=2);
4.查询和孙悟空相同工作的员工信息
	select * from emp where job=(select job from emp where name='孙悟空') and name!='孙悟空';
5.查询最低工资员工的同事们的信息(同事指同一部门)
	select * from emp where deptId=(select deptId from emp where sal=(select min(sal) from emp)) and sal!=(select min(sal) from emp);

```

### 并联关系

- 创建表时 , 表和表之间存在的业务关系
- 有哪几种关系
  - 一对一 : 有A B两张表 , A 表中的一条数据对应B表中的一条数据 , 同时B表中的一条数据也对应A表中的一条数据
  - 一对多 : 有A B两张表 , A 表中的一条数据对应B表中的多条数据 , 同时B表中的一条数据对应A表中的一条数据
  - 多对多 : 有A B两张表 , A 表中的一条数据对应B表中的多条数据 , 同时B表中的一条数据也对应A表中的多条数据
- 表与表之间如何建立关系?
  - 通过一个单独的字段指向另一张表的主键
  - 一对一 : 有AB两张表,在任意一张表中添加字段指向另外一个表的主键
  - 一对多 : 有AB两张表,在一对多的关系中,多的一端添加一个单独字段指向另   外一张表的主键
  - 多对多 : 有AB两张表 还需要创建一个单独的关系表,里面两个字段分别指向      另外两张表的主键

### 关联查询

- 同时查询多张表数据的查询方式称为关联查询
- 有三种关联查询的方式 :
  - 等值连接
  - 内连接
  - 外连接

#### 等值连接

- 格式 : select  字段信息  from A , B where 关联关系 and 条件 ;

```sql
1.查询工资高于2000的员工的姓名 , 工资 以及对应的部门名
	select e.name , sal , d.name from emp e, dept d where e.deptId = d.id and sal>2000;
2.查询有领导并且和销售相关工作的员工姓名,工作,部门名和部门地点
	select e.name,e.job,d.name,d.loc from emp e,dept d where e.deptId=d.id and e.manager is not null and e.job like '%销售%';

```

#### 内连接

- 等值连接和内连接查询到的数据是一样的 都是两个表的交集数据,只是书写格式不一样
- 格式: select 字段信息 from A join B on 关联关系 where 条件

```sql
1.查询工资高于2000的员工的姓名,工资以及对应的部门名
select e.name,sal,d.name
from emp e join dept d on e.deptId=d.id
where sal>2000;
2.查询 有领导并且和销售相关工作的员工姓名,工作,部门名和部门地点
select e.name,job,d.name,loc
from emp e join dept d on e.deptId=d.id
where manager is not null and job like '%销售%';

```

#### 外连接

- 查询一张表的全部和另外一张表的交集数据,使用外连接
- 格式: select 字段信息 from A left/right join B on 关联关系 where 条件

```sql
1.查询所有员工姓名和对应的部门名
insert into emp(name,sal) values('灭霸',88);
select e.name,d.name
from emp e left join dept d
on e.deptId=d.id;
2.查询所有部门名对应的员工姓名和工资
select d.name,e.name,sal
from emp e right join dept d
on e.deptId=d.id;

```

#### 关联查询总结

- 如果需要查询的数据时两个表的交集数据,使用等值连接或内连接(推荐)
- 如果查询的是一张表的全部和另外一张表的交集使用外连接

### 表设计

举例: 实现一个微博网站

- 需要保存的数据: 用户名,密码,昵称,注册时间,注册地,性别,手机号,微博正文,微博附件,点赞量,浏览量,发布时间,发布地址,评论正文,点赞量,评论时间
- 分析上面的数据需要创建几张表,并且考虑表和表之间的关系
  - 用户表:id,用户名,密码,昵称,注册时间,注册地,性别,手机号
  - 微博表:id,微博正文,微博附件,点赞量,浏览量,发布时间,发布地址,用户id
  - 评论表:id,评论正文,点赞量,评论时间,用户id, 微博id

### JDBC

- Java DataBase Connectivity: Java数据库连接, 学习JDBC主要学习的就是如何在Java代码中执行SQL语句
- JDBC是Sun公司提供的一套通过Java连接数据库的API(Application Programma Interface 应用程序编程接口)
- 为什么使用JDBC
- 如果没有JDBC接口, 每个数据库厂商都有可能定义自己一套全新的方法(做的事儿是一样的但是方法名可能不一样),这样对于Java程序员而言需要学习几套不同的方法, 为了避免这种情况出现,Sun公司定义了JDBC接口,将方法名固定, 让各个数据库厂商根据此方法名写各自的实现类, 这样java程序员只需要遵循JDBC的标准写代码,就算将来换数据库,代码也不用变 ,因为各个数据库厂商提供的方法名是一样的.

### 如何使用JDBC

1. 创建maven工程
2. 在pom.xml中添加mysql驱动的依赖(驱动实际上就是JDBC方法的实现类)
3. 通过以下代码 连接数据库并执行SQL语句

```sql
//获取数据库连接  异常抛出  http://
Connection conn =
        DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/newdb3?" +
                        "characterEncoding=utf8&" +
                        "serverTimezone=Asia/Shanghai",
                "root","root");
System.out.println(conn);
//创建执行SQL语句的对象
Statement s = conn.createStatement();
//执行SQL语句
s.execute("create table jdbct1(id int)");
//关闭资源
conn.close();
```

### 执行SQL语句对象Statement

- execute(sql) 可以执行任意SQL语句, 但是推荐执行表相关的SQL
- executeUpdate(sql) 执行增删改相关SQL语句
- ResultSet rs = executeQuery (sql) 执行查询相关SQL语句











# Web前端













# Spring Boot





