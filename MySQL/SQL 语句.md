#### SQL 语句

##### insert 插入

*如果可以相互转换的数字和字符串会自动转换*

```mysql
# 这个方式可以自己指定值  推荐这个写法
insert into table_name (字段，字段) values (值，值)  
# 这种方式默认要添加所有的字段值
insert into table_name values (值，值，值，值)    

```

*批量插入*

```mysql
# 后面接多个 values ，注意主键不能重复
insert into dept (deptno,dname,loc) values(1,'stefa','aa'),values(2,'stefa','aa'),values(3,'stefa','aa'),values(4,'stefa','aa')
```

---



##### update 更新

```mysql
# update table_name set column1 = value1, column2 = value2 where ......
update emp set deptno=1111 where deptno=1000;
```

*更新多个字段*

```mysql
update emp set job='job',sal = sal + 1000 where deptno=1111 
```

---



##### delete 删除

*物理删除*

```mysql
delete from emp where ename = 'stefan'
```

*逻辑删除*  

自己设置一个特定的字段  用来标识  1表示存在 0表示不存在。没有真正的删除

```mysql
select * from emp where is_vaild = 1
update emp set is_vaild = 0 where is_vaild = 1;
```



---

#### select  查询语法

##### 基础查询

```mysql
select [distinct] column1, column2 as 字段别名 from 表名 where 过滤条件 order by column
```

> 查询列

```mysql
# 查询 mysql 库中的 user 表的 user,host
select user, host from mysql.user;        # 或者是查询之前指定哪个数据库
```

> 查询表中所有的字段

```
select * from user;   # 企业中会少用 * 进行统配查询，会有性能问题
```

> 增加查询条件

**where 后面只能是表达式或者是原始的字段 ，不能是别名**

```mysql
# where 后面是查询条件 
select user,host from user where user='root';
# select user as u,host from where u='root'    这是错误的
```

针对复杂的业务，查询表中的数据时候的顺序

1. 确定查询的表名
2. 确定查询的字段
3. 过滤条件

> 去重 distinct 

```mysql
select distinct host from mysql.user;
```

> 别名 as  方便记忆

**别名可以省略 as**

```mysql
select user as 用户名,host as 主机 from mysql.user;
select user as 用户名,host as '主  机' from mysql.user;   # 加了引号后就可以保留原来的空格
select user myuser,host from mysql.user; # 省略了 as 的别名 myuser 是 user 的别名
```

> 字符串

```mysql
select 'str',user from mysql.user;    # 字符串
select 'I\'am root' from mysql.user;   # 转义
select "I'am root" from mysql.user;    # 双引号
```

> 嵌套查询 子查询

```mysql
# 后面的哪个表一定要用别名，否则会报错
select myuser from (select user as myuser,host from user) as T;
# 后面的字段使用了别名后，前面的就一定要用字段的别名
# 错误的示范 select user from (select user as myuser,host from user) as T;
```

>dual 虚表

```
select 1+1 from dual;
select 1+1; # 可以省略 dual;
```

> NULL 

**在设置字段的值时，尽量不用使用 NULL**

**在使用 null 做比较的时候 ，不能使用 != ,=     要使用 is , not is** 

```mysql
select 1+null ; # NULL
select 'str'+null; # NULL
select ifnull(column,0);  # 判断是否是 NULL
```

- 查询行(记录)

> 条件比较 <   >   <=   >=   <>   !=   =  between small and big

```mysql
select user,host from mysql.user where user!='root';   # 查询 user 不等于 'root' 的记录
select user,host from mysql.user where user='root';
select age from mysql.user where age between 18 and 25; # 查询 age 在 [18,25] 之间
select age from mysql.user where age>=18 and age<=25;  # 同上
```

> 且或非  and or !=

优先级 not > and > or

```python
select user from user where user='root' or host='localhost';
select user from user where user='root' and host='%';
select user from user where user!='root';
```

> 查询某个字段不为空的记录

```mysql
# 查询 当 Password_reuse_history 不为 null 的记录
select user,host from user where Password_reuse_history is not null;
# 相反
select user,host from user where Password_reuse_history is null;
```

> union （联合查询）

**union左右的字段数要一样**  **union all 不去除重复**

**返回的是之前的字段数   字段会合并**

```mysql
# 字段数要一样， 默认时去除重复
select host from user union select user from user;

select ename from emp union select deptno from dept;
结果
SMITH
ALLEN
WARD
JONES
MARTIN
BLAKE
CLARK
SCOTT
KING
TURNER
ADAMS
JAMES
FORD
MILLER
10
20
30
40
```

> like 模糊匹配

模糊查询,使用通配符

> %  （任意个数的字符）
>
> _      (一个字符)
>
> 遇到内容包含 % _ 的使用 \ 转义

```mysql
select user,host from user where host like '%host%';
```

> in 和 exists

**in 相当于使用 or 的多个等值，定值集合,如果存在子查询，确保类型相同、字段数为 1**

```mysql
select user,host from user where in ('%','localhost');   
# 如果有子查询 ，后面查询的字段和前面的字段要一样
select user,host from user where host in (select host from user);
```

**exists() 返回 true 或者 false**

```python
# 效果是一样的
select user,host from user where 1;
select user,host from user where exists(select * from user where user='root');
```

**exists() 原理**

1. exists对外表用loop逐条查询，每次查询都会查看exists的条件语句，当 exists里的条件语句能够返回记录行时(无论记录行是的多少，只要能返回)，条件就为真，返回当前loop到的这条记录，反之如果exists里的条 件语句不能返回记录行，则当前loop到的这条记录被丢弃，exists的条件就像一个bool条件，当能返回结果集则为true，不能返回结果集则为 false 

```mysql
select * from A where exists (select * from B where B.id = A.id);
# 可以用下面的代码解释
for key,value in A.items():
	if B.id ** A.id:
		print(value)
```



> order by name asc (默认是 asc 升序)

```python
select name,age from users order by name;
select name,age form users order by name desc,age asc  # (asc 是升序）
```

---

##### 分组函数

**组信息 与单条记录不能同时查询**

**select max(sal), ename, sal from emp; #错误**

1. 确定表
2. 确定条件
3. 确定组函数的字段
4. 确定最终结果

> count() 统计记录数

```mysql
select count(*) from emp; # 返回所有的行
select count(1) from emp; # 返回所有的行

select count(column) from emp； # 返回值不为null的数量
```

> 最大值 max    最小值  min  自动过滤 null

```mysql
#查询所有员工的 最高薪水 ，最低薪水，员工总数 
#>组信息 
select max(sal) maxSal , min(sal) minSal , count(1) from emp; 

#查询 最高薪水的员工名称 及薪水 #组信息 与单条记录不能同时查询 
select max(sal), ename, sal from emp; #错误 
select ename, sal from emp where sal=(select max(sal) from emp ); 
```

> 求和 sum   平均值 avg ()  自动过滤 null

```mysql
#查询 10 部门的所有员工的工资总和 
select sum(sal) from emp where deptno=10

# 查询工资低于平均工资的员工编号，姓名及工资 
select empno, ename,sal from emp where sal<(select avg(sal)from emp);
```

##### 分组

> 分组: group by，将符合条件的记录 进一步的分组

**group by 默认返回每一组的第一条数据**

**group by 一般配合 min()  max()  avg() 使用**

```mysql
# 返回了每一组的第一条数据
select * from emp group by deptno;
7369	SMITH	CLERK	7902	1980-12-17	800		20	1
7499	ALLEN	SALESMAN	7698	1981-02-20	1600	300	30	1
7782	CLARK	MANAGER	7839	1981-06-09	2450		10	0

select max(sal) from emp group by deptno;
```

> 过滤组 having by 分组的同时 过滤

```mysql
# 查询平均工资大于2000的部门的平均工资
select avg(sal), deptno from emp group by deptno having avg(sal)>2000; 

# 查询最低平均工资的部门编号 
```

##### mysql 解析步骤

**表结构**

```mysql
select distinct 字段
from 表名
where 过滤行记录条件
group by 分组字段列表
having 过滤组
order by 字段列表 asc | desc
```

**解析步骤**

1.  from
2.  where
3.  group by
4.  having 
5.  select 
6.  order by

##### 多表查询

就是把多张表连接到一起来 ，和 union 不一样，

**多表查询是指基于两张表或者是两个视图以及以上的查询操作。在实际开发工作往往单表操作不能满足业务需求**

###### 一、笛卡儿积现象

> 笛卡尔积是指在数学中，两个集合 X 和 Y 的笛卡尓积（Cartesian product），又称直积，表示为 X×Y，第一个对象是 X 的成员而第二个对象是 Y 的所有可能有序对的其中一个成员。 假设集合 A={a, b}，集合 B={0, 1, 2}，则两个集合的笛卡尔积为{(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}。 

- 交叉连接 （直接笛卡尔积 不做筛选）

```mysql
select ename, dname from emp, dept;
```

- 等值连接 

```mysql
# 查询 用户名以及用户所在部门名select ename, dname from emp,dept where emp.deptno = dept.deptno;
```

- 非等值连接

```mysql
# 查询员工姓名，工资及等级# 900  属于哪个等级 select grade from salgrade where 900 between losal and hisal;# 查询员工名称，工资和等级select ename, sal, grade from emp, salgrade where sal between losal and hisal;
```

- 自连接 (特殊的等值连接  来源于同一张表)

```mysql

```

##### 二、子查询

##### 三、连接方式查询 （内置的 类似于传统笛卡儿积方式）

- 内连接 inner into   (等值，非等值，自连接)

```
select ename,dname from emp join dept on emp.deptno=dept.deptno
```

- 左连接

left join 一定要加 on

**在满足后面的条件的情况下，以左边为主，把左边这个字段，所有的值都写出来，右边没有的写null**

```mysql
select ename,dname,emp.deptno, dept.deptno from emp  left join dept on emp.deptno=dept.deptno;
```

- 右连接

一定要加 on

**在满足后面的条件的情况下，以右边为主，把右边这个字段，所有的值都写出来，左边没有的写null**

```mysql
select ename,dname,emp.deptno, dept.deptno from emp  right join dept on emp.deptno=dept.deptno;
```











