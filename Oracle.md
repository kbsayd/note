# 第一章 数据库

## 相关概念

​		**数据库**：DataBase(DB)顾名思义就是数据的仓库.它是长期存储在计算机内、有组织的、可共享的数据的集合.

​		**数据库管理系统**： Database Management System(DBMS)管理数据库的软件。具有对数据存储、安全、一致性、并发操作、恢复和访问等功能。

​		**数据库系统管理员**：Database Administrator(DBA)数据库管理人员。

## 数据库发展史

![img](Oracle.assets/clipboard.png)

## 主流关系型数据库

- Oracle：甲骨文公司，全称甲骨文股份有限公司(甲骨文软件系统有限公司)，是全球最大的企业级软件公司，总部位于美国加利福尼亚州的红木滩。
- MySql：瑞典Mysql AB公司开发,2008年被Sun公司收购，而Sun在次年被Oracle收购,现在Mysql也属于Oracle数据库。
- SQL Server：微软的产品。最初版本适用于中小型企业，但是应用范围不断扩展，已经触及到大型、跨国企业的数据库管理。
- DB2：IBM公司研发的关系型数据库。DB2是Oracle的主要竞争对手。

## 关系型数据库

​		关系型数据库：指采用了关系模型来组织数据的数据库。

​		关系模型指的就是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。

![img](Oracle.assets/clipboard-1618809748326.png)

​		表是由行和列组成：

- 列包含一组命名的属性（也称字段）。
- 行包含一组记录，每行包含一条记录。
- 行和列的交集称为数据项，指出了某列对应的属性在某行上的值，也称为字段值。
- 列需定义数据类型，比如整数或者字符型的数据。
- 数据库主键，指的是一个列或多列的组合，其值能唯一地标识表中的每一行，通过它可强制表的实体完整性。

## Oracle数据库

​		平常说的Oracle或Oracle数据库指的是Oracle数据库管理系统。它由二部分组成：

- Oracle数据库：相关的数据库操作系统文件的集合，这些文件组织在一起，成为一个逻辑整体，即为Oracle数据库。
- Oracle实例：位于物理内存里的数据结构，它由操作系统的多个后台进行和一个共享内存池所组成，共享的内存池可以被所有进程访问。

![img](Oracle.assets/clipboard-1618810069799.png)

## 安装数据库

> 注意事项：安装路径不要中文和空格。
>
> 如果计算机改名或计算机名为中文时，可能出现listener启动不了的问题。
>
> 修改：Oracle安装目录下的product\11.1.0\db_1\NETWORK\ADMIN中的listener.ora和tnsnames.ora将计算机名改为localhost即可

​		验证是否安装成功

- sqlplus：sqlplus 用户名/密码 [as sysdba;]
- SQL Developer



​		Oracle的服务

- OracleVssWriterORCL：该服务是Oracle对VSS的支持服务，Oracle卷映射拷贝写入服务。(非必须启动)
- OracleDBConsoleorcl： 该服务是Oracle控制台服务，即企业管理器(OEM)。(非必须启动)
- OracleJobSchedulerORCL：该服务是Oracle定时器服务。(非必须启动)
- OracleMTSRecoveryService：服务端控制。该服务允许数据库充当一个微软事务服务器MTS、COM/COM+对象和分布式环境下的事务的资源管理器。（非必须启动）
- OracleOraDb11g_home1ClrAgent：Oracle数据库对.NET扩展服务的一部分。 （非必须启动）
- OracleOraDb11g_home1TNSListener：该服务是服务器端为客户端提供的监听服务，只有该服务启动，客户端才能连接到服务器。(非必须启动)**要是使用PL/SQL Developer等第三方工具的话，此服务要开启。**
- ​	OracleServiceORCL：数据库服务(数据库实例)，是Oracle核心服务该服务是数据库启动的基础， 只有该服务启动，Oracle数据库才能正常启动。(必须启动)



# 第二章 查询和排序

## SQL

### SQL语句标准

​		SQL称结构化查询语言 (Structured Query Language)

​		美国国家标准局（ANSI）和国际化标准组织（ISO）制定了SQL标准。

- SQL89
- SQL92
- SQL99

### SQL语句组成

- 数据查询语言：SELECT
- 数据操作语言：INSERT、UPDATE、DELETE
- 数据定义语言：CREATE、ALTER、DROP
- 数据控制语言：GRANT、REVOKE
- 事务处理语言：COMMIT、ROLLBACK

### 练习库中表

![img](Oracle.assets/clipboard-1618810486046.png)

```sql
--查询当前用户是谁 		
show user;

--查询scott用户下的所有对象，使用tab表，tab表每个用户都有
select * from tab;

--查询emp表的结构
desc emp;
```

## 基本查询

```sql
SELECT *|{[DISTINCT] 列名|表达式 [别名][,...]}
	FROM 表名
```

​		SQL语句书写规则

- 不区分大小写。
- 可以单行来书写，也可以书写多行。
- 关键字不可以缩写、分开以及跨行书写。
- 每条语句需要以分号（；）结尾



​		练习：

- 查询公司所有部门的信息(*)。
- 查询公司所有部门的信息(列名)。
- 那种查询方式的效率更高(用多少，取多少)？
- 查询公司所有部门名称。

### 列别名

- 第一种方式：列名 列别名
- 第二种方式：列名 AS 列别名	

> 以下三种情况，列别名两侧需要添加双引号（""）：
>
> – 列别名中包含有空格
>
> – 列别名中要求区分大小写
>
> – 列别名中包含有特殊字符

### 连接运算符(||)

​		查询出以下格式数据：员工姓名：XXX    员工部门：XXX，注意：字符串使用单引号。

### **算术表达式：**+，-，*，/

- 查询所有员工一年的工资
- 查询所有员工提示10%后的工资。

### 空值（NULL）

​		NULL：表示未定义的，未知的。

​		空值不等于零或空格。

​		任意类型都可以支持空值。

​		

​		空值（NULL）在算术表达式中的使用

- 包括空值的任何算术表达式都等于空
- 包括空值的连接表达式等于与空字符串连接，也就是原来的字符串



​		查询所有员工一个月的工资(工资+提成)

### DISTINCT

​		取消重复行。

- 查询所有员工的工作
- 查询所有员工所在的部门

### 练习

- 查询员工表中员工的员工号、姓名、每个员工涨工资100元以后的年工资（按12个月计算）。
- 查询所有部门经理的员工编号（要求去掉重复值）。

## 限制

```sql
SELECT *|{[DISTINCT] 列名|表达式 [别名][,...]}
	FROM 表名
	[WHERE 条件]
```

- 使用WHERE子句限定返回的记录

- WHERE子句在 FROM 子句后

### 比较运算符

​		=	>	>=	<	<=	<>或!=

- 查询公司月薪高于2200 的员工信息。
- 查询在1981 年1 月1日以后进入公司的雇员信息。

### 特殊比较运算符

#### BETWEEN...AND...

- 查询员工工资在1500-3000的员工信息

> 注意：between and 对数值和日期包含两边的边界值。但是对于字符包含第一个边界值不包含第二个边界值。
>
> SELECT * FROM emp WHERE ename  BETWEEN 'A' AND 'C';

#### IN

- 查询部门编号为10 20 90的员工

#### LIKE

- %可以代替任意长度字符（包括长度为0）
- _可以代替一个字符
- ESCAPE 



- 查询姓名中包含A的员工信息
- 查询姓名中第二个字母为A的员工信息

#### IS NULL

- 查询所有没有补助的员工信息。

### 逻辑运算符

#### AND

- 查询工资1500-3000的员工信息
- 查询10和20号部门工资大于2500的员工信息

#### OR

- 查询工资大于3500或在10部门的员工信息

#### NOT

- 查询有补助的员工信息
- 查询姓名中不包含C的员工信息

### 运算符优先级

![img](Oracle.assets/clipboard-1618811731661.png)

## 排序

```sql
SELECT *|{[DISTINCT]  列名 | 表达式 [ 别名 ][,...]}
	FROM 表名
	[WHERE 条件 ]
	[ORDER BY { 列名 | 表达式 | 别名 } [ASC|DESC],…];
```

- ORDER BY子句可以出现在SELECT子句中没有出现过的列。
- ORDER BY子句后的列名，可以用数字来代替。这个数字是SELECT语句后列的顺序号。



- 查看公司员工信息，按照员工部门降序排列。
- 查看员工信息，结果按照年薪升序排列(使用别名)。
- 查看员工信息，结果按照按照job升序排列，月薪按照降序排列。
- 查看员工信息，按照月薪由高到低排列，而具体的工资数不显示。

## 练习

1. 查询姓名最后为S的员工的信息。

2. 查询参加工作时间在1981-7-9之后，并且不从事CLERK工作的员工的信息。

3. 查询员工姓名的第三个字母是A的员工的信息。

4. 查询除了10、20、110号部门以外的员工的信息。

5. 查询部门号为20号员工信息，先按工资降序，再按姓名升序排序。

6. 查询没有上级管理的员工(经理号为空)的信息。

7. 查询员工表中工资大于等于2500并且部门为10或者20的员工的姓名, 工资，部门号。



# 第三章 单行函数

## 单行函数

```sql
函数名[(参数1，参数2,…)]
```

​		特点：

- 单行函数对单行操作
- 每行返回一个结果
- 单行函数可以写在SELECT、WHERE、ORDER BY子句中
- 函数可以嵌套

![img](Oracle.assets/clipboard-1618814901140.png)

## 字符函数

> dual 确实是一张表.是一张只有一个字段,一行记录的表.。习惯上,我们称之为'伪表'.因为他不存储主题数据。

​		常用字符函数：

- LOWER( 列名 | 表达式 )：全小写
- UPPER( 列名 | 表达式 ) ：全大写
- INITCAP( 列名 | 表达式 ) ：首字母大写
- CONCAT( 列 1| 表达式 1, 列 2| 表达式 2)：字符串连接
- SUBSTR( 列名 | 表达式 ,m[,n])：字符串截取
- LENGTH( 列名 | 表达式 )：返回字符串长度
- INSTR( 列名 | 表达式 ,’string’, [,m], [n] ) ：返回一个字符串在另一个字符串中的位置。
- REPLACE ( 文本 ,  查找字符串 ,  替换字符串 )：替换字符串
- TRIM([leading|trailing|both]trim_character FROM trim_source) ：去掉左右两边指定字符。

## 数字函数

​		常用数字函数：

- ROUND( 列名 | 表达式 , n)：四舍五入函数
- TRUNC( 列名 | 表达式 ,n)：截断函数。
- MOD(m,n)：取余函数。

```sql
SELECT ROUND(65.654,2),ROUND(65.654,0),ROUND(65.654,-1)	FROM DUAL;
```

## 日期函数

​		**注意：日期可以加减。**

​		常用日期函数：

- SYSDATE：返回系统日期
- ROUND(date[,'fmt'])对日期进行指定格式的四舍五入操作。按照YEAR、MONTH、DAY等进行四舍五入。
- TRUNC(date[,'fmt'])对日期进行指定格式的截断操作。按照YEAR、MONTH、DAY等进行截断。
- MONTHS_BETWEEN：返回两个日期间隔的月数
- ​	ADD_MONTHS：在指定日期基础上加上相应的月数
- ​	NEXT_DAY：返回某一日期(星期几)的下一个指定日期

 ```sql
SELECT NEXT_DAY('02-2月-06','星期一')  NEXT_DAY   FROM DUAL;
--注意:也可以使用1表示周日2表示周1以此类推
 ```
- LAST_DAY：返回指定日期当月最后一天的日期
- EXTRACT：返回从日期类型中取出指定年、月、日

```sql
 EXTRACT ([YEAR] [MONTH][DAY] FROM [日期类型表达式])
```

​		练习：

- 查询.公司员工服务的月数。
- 81年公司员工转正日期(试用期3个月)。
- 入职员工入职日期按月截断。
- 部门编号是20的部门中所有员工入职月份

## 转换函数

​		常用转换函数：

- TO_CHAR
- TO_NUMBER
- TO_DATE

![img](Oracle.assets/clipboard-1618873700677.png)

## 其他函数

### 空值处理函数

-  NVL：NVL (表达式1, 表达式2)函数功能是空值转换，把空值转换为其他值，解决空值问题。表达式1是需要转换的列或表达式，表达式2是如果第一个参数为空时，需要转换的值。
- NVL2：NVL2( 表达式 1,  表达式 2,  表达式 3)函数是对第一个参数进行检查。如果第一个参数不为空，则输出第二个参数；如果第一个参数为空，则输出第三个参数。
- NULLIF：NULLIF ( 表达式 1,  表达式 2)函数主要是完成两个参数的比较。当两个参数不相等时，返回值是第一个参数值；当两个参数相等时，返回值是空值。
- COALESCE：COALESCE ( 表达式 1,  表达式 2, ...  表达式 n)函数是对NVL函数的扩展。COALESCE函数的功能是返回第一个不为空的参数，参数个数不受限制。

### 条件处理函数

- CASE表达式

```sql
CASE expr
WHEN comparison_expr1 THEN return_expr1
[WHEN comparison_expr2 THEN return_expr2
WHEN comparison_exprn THEN return_exprn
ELSE else_expr]
END
```

​		查询所有员工信息，部门显示名称(一张表部门名写死)。

- DECODE

```sql
DECODE(字段| 表达式 ,  条件 1, 结果 1[, 条件 2, 结果2… ， ][, 缺省值 ])
```

​		按工种调节工资(SALESMAN 0.15 CLERK 0.25 ANALYST 0.5)打印员工信息和调节前后的工资。

## 练习

1. 计算2000年1月1日到现在有多少月，多少周（四舍五入）。

2. 将员工工资按如下格式显示：123,234.00 RMB

3. 查询员工的姓名及其经理（mgr），要求对于没有经理的显示“No Manager”字符串。

4. 将员工的参加工作日期按如下格式显示：月份/年份。

5. 有年终奖comm的员工过节费是月薪的30%,	没有年终奖的员工过节费是月薪的50%

6. 查询员工姓名和工资阶级(0-1999：屌丝,2000-3999:白领,4000+高富帅)

7. 查询每个月倒数第三天受雇的员工信息。

# 第四章 多表查询

```sql
SELECT table1.column, table2.column
    FROM table1
    [CROSS JOIN table2] |
    [NATURAL JOIN table2] |
    [JOIN table2 USING (column_name)] |
    [JOIN table2
    ON(table1.column_name = table2.column_name)] |
    [LEFT|RIGHT|FULL OUTER JOIN table2
    ON (table1.column_name = table2.column_name)];
```

## 交叉连接

​		CROSS JOIN，生成笛卡尔积。

## 内连接

### NATURAL JOIN

```sql
SELECT * FROM emp NATURAL JOIN dept;
```

- 以左右两表相同名称的字段 作为连接条件
- 去掉重复的字段(即名称相同、类型也相同的字段)
- 如果列名相同，类型不同则出错。
- 同名列只保留一个
- 不允许在查询列前面使用表明或别名作为前缀。

### USING

​		USING子句，通过名字来具体指定连接。可以较灵活的完成在多表连接，多列列名相同时，使用其中的一列同名列连接，而不需写连接条件的功能。

```sql
SELECT * FROM emp JOIN dept USING(deptno);
```

### INNER JOIN

​		INNER JOIN，on后面写连接条件。

## 外连接

### LEFT OUTER JOIN

​		左外连接：在内连接的基础上,保证左表的数据都有 (右表的字段用空补全)

### RIGHT OUTER JOIN

- 右外连接：在内连接的基础上,保证右表的数据都有 (左表的字段用空补全)

### FULL OUTER JOIN

- 全外连接：在内连接的基础上保证左右表的数据都有 (左连接和右连接的并集)

## 自连接

​		连接查询时,左右两张表是同一张表为一张表取不同的别名，当成两张表来用。

## 总结

![img](Oracle.assets/src=http___image.bubuko.com_info_201412_20180919144224021501.jpg&refer=http___image.bubuko.jfif)

## 练习

1. 查询员工的编号，姓名，以及部门名称

2. 查询部门名称为SALES的员工的编号、姓名及所从事的工作。

3. 查询员工的编号，姓名，以及部门名称，包括没有员工的部门。

4. 查询员工的编号，姓名，以及部门名称，包括不属于任何部门的员工。

5. 查询员工编号、姓名、工资以及部门名称和所在地和工资等级。

6. 查询工作地点在芝加哥并且奖金不为空的员工姓名、部门名称、工资、奖金。

# 第五章 分组

## 分组函数

​		分组函数：

- MIN
- MAX
- SUM
- AVG
- COUNT

分组函数是对表中一组记录进行操作，每组只返回一个结果。

即首先要对表记录进行分组，然后再进行操作汇总，每组返回一个结果。

分组时可能是整个表分为一组，也可能根据条件分成多组。

**注意：多行函数会忽略空值！！！**

```tex
1.查询员工最低工资
2.查询员工最高工资的。
3.查询员工总工资
4.查询员工平均工资。
5.查询工作为CLERK的员工数量
6.查询所有工作种类数量
7.查询所有员工平均奖金
```

## 分组

```sql
SELECT 列名 ,  组函数 ( 列名 )
    FROM 表名
    [WHERE 条件 ]
    [GROUP BY 分组列 ]
    [ORDER BY 列名]
```

​		GROUP BY子句后的列可以不在SELECT语句中出现。

​		SELECT子句之后，只能出现分组的字段和统计函数，其他的字段不能出现。

```tex
1.每个部门的总工资。
2.相同职位且经理相同的员工平均工资
3.公司每个职位的平价工资，职位列不显示，结果按照平均工资排序。
```

## HAVING子句

```sql
--按部门编号进行分组，分组之后求每一个部门的平均薪水，要求显示平均薪水大于2000的部门的部门编号和平均薪水
select deptno,avg(sal) from emp
	where  avg(sal)>2000 group by deptno;
--问题在哪里？
```

```sql
SELECT 列名 ,  组函数
    FROM 表名
    [WHERE 条件 ]
    [GROUP BY 分组列 ]
    [HAVING 组函数表达式 ]
    [ORDER BY 列名 ];
```

​		HAVING子句是分组后的"where"

​		WHREE过滤行，HAVING过滤组。

## SELECT执行过程

​		总结SELECT语句执行过程：

- 通过FROM子句中找到需要查询的表；
- 通过WHERE子句进行非分组函数筛选判断；
- 通过GROUP BY子句完成分组操作；
- 通过HAVING子句完成组函数筛选判断；
- 通过SELECT子句选择显示的列或表达式及组函数；
- 通过ORDER BY子句进行排序操作。

## 四、练习

1. 查询部门平均工资在2500元以上的部门名称及平均工资。

2. 查询员工编号大于7500并且平均工资在2000元以上的工种及平均工资，并按平均工资降序排序。

3. 查询部门人数在4人以上的部门的部门名称及最低工资和最高工资。

4. 每个部门同一个职位的最大工资。

5. 查询工作不为CLERK，工资的和大于等于2500的工作编号和每种工作工资的和。

6. 薪水大于1200的雇员，按照部门编号进行分组，分组之后平均薪水必须大于1500,求分组内的平均工资，平均工资按倒序排列

7. 显示经理号码，这个经理所管理员工的最低工资，不包括经理号为空的，不包括最低工资小于2000的，按最低工资由高到低排序。

8. 查询各工种平均工资的最大值和最小值。

# 第六章 子查询

## 子查询

​		子查询(也叫嵌套查询)：查多次，多个select嵌套出现,第一次的查询结果可以作为第二次的查询条件或表名。

```sql
SELECT 查询列
    FROM 表名
    WHERE 列名 操作符
    (SELECT 查询列 FROM 表名 );
```

​		括号内的查询叫做子查询（Subquery）或者内部查询（Inner Query），外面的查询叫做主查询（Mainquery）或外部查询（Outer query）。

​		子查询 (内查询) 在主查询之前一次执行完成，子查询的结果被主查询使用 (外查询)。

​		子查询注意事项

- 子查询需要写在括号中；
- 子查询需要写在运算符的右端；
- 子查询可以写在WHERE，HAVING，FROM子句中；
- 子查询中通常不写ORDER BY子句。

```tex
1.查询和KING员工在相同部门的员工信息。
2.查JAMES的上司叫什么。
```

## 子查询分类

### 单行子查询

​		单行子查询，子查询返回的记录有且只有一条。

```sql
1.查询所有工资比KING低的员工信息
2.查询最低工资员工信息
```

### 多行子查询

​		子查询返回记录的条数可以是一条或多条。

- IN：在什么范围内。
- ANY：表示任意的。

​			= ANY 和子查询中任意一个结果相等即可，相当于IN。

​			< ANY 比子查询返回的任意一个结果小即可，即小于返回结果的最大值。

​			 > ANY比子查询返回的任意一个结果大即可，即大于返回结果的最小值。

```tex
1.查询任何比20部门员工工资高的员工信息。
2.查询经理信息
```

- ALL：表示所有的。

​			< ALL 比子查询返回的所有的结果都小，即小于返回结果的最小值。

​			 > ALL比子查询返回的所有的结果都大，即大于返回结果的最大值。

​			 = ALL 无意义，逻辑上也不成立。

```tex
1.查询比所有部门20员工工资高的员工信息。
```

### 多列子查询

​		多列子查询多用于in操作。

```sql
--1.查询那些员工的工资为所在工种最高。
select ename,sal,job 
from emp
where (sal,job) in (
	select max(sal),job
	from emp	
    group by job
)
--2.查询和SMITH一个部门一个工种的员工信息。
```

### 相关子查询

​		相关子查询中，内部查询需引用外部查询的列，进行交互判断。相关子查询的执行方式是一行行操作。外部查询每执行一行操作，内部查询都要执行一次。

```sql
--1.查询比本职位平均工资高的员工的信息。
SELECT ename, sal, job FROM emp e
    WHERE sal >(SELECT AVG(sal)
			FROM emp
			WHERE job = e.job);
```

### 练习

1. 查询工资高于编号为7499的员工工资，并且和7782号员工从事相同工作的员工的编号、姓名及工资。

2. 工资最高的员工姓名和工资。

3. 查询部门最低工资高于20号部门最低工资的部门的编号、名称及部门最低工资。

4. 查询员工工资为其部门最低工资的员工的编号和姓名及工资。

5. 显示经理是KING的员工姓名，工资。

6. 显示比员工KING参加工作时间晚的员工姓名，工资，参加工作时间。

# 第七章 数据操作和事务

## 数据操作

​		数据操作语言（DML： Data Manipulation Language）：

- INSERT
- UPDATE
- DELETE

> 复制表操作：CREATE TABLE myemp AS SELECT * FROM emp;

### 插入

```sql
INSERT INTO 表名[(列名1[,列名2，…，列名n])]
    VALUES ( 值 1[, 值 2，… ，值 n]);
```

```tex
1.将一个新成立部门的信息写入部门表。
2.插入一个新员工信息。
3.将受雇日期在“1981-4-1” 之前的员工信息复制到员工表中。
```

> **INSERT INTO SELECT** 语句从一个表复制数据，然后把数据插入到一个已存在的表中。
>
> 语句形式为：Insert into Table2(field1,field2,...) select value1,value2,... from Table1
>
> 或者：Insert into Table2 select  *  from Table1

### 修改

```sql
UPDATE  表名 SET  列名 = 表达式 [， 列名 = 表达式 ，···]
    [WHERE  条件表达式];
```

### 删除

```sql
DELETE [FROM]  表名
    [WHERE  条件表达式];
```

## 事务处理

### 问题

> 例子：转账
>
> 
>
> 账户表
>
> id  name  money 
>
>  1  zs    500
>
>  2  ls    2000
>
> 
>
> 转账功能
>
>  ls--转1500--zs
>
>  操作数据库中的数据，执行SQL语句。
>
>  update 账户表 set money=money-1500 where id = 2;//A
>
>  update 账户表 set money=money+1500 where id = 1;//B
>
> 
>
> 情况一：
>
>   A 成功  B 成功   转账成功    银行和客户都满意。
>
>   A 成功  B 失败   转账失败    结果ls钱消失了。客户不干
>
>   A 失败  B 成功   转账失败    结果zs钱多了。 银行不干
>
>   A 失败  B 失败   转账失败    银行和客户都满意。
>
> 
>
> 总结：有些业务操作需要执行多条SQL语句，而且必须SQL要么全部执行成功, 要么全部执行失败。就需要使用到事务。

### 事务

> ​		事务（Transaction）也称工作单元，是一个或多个SQL语句所组成的序列，这些SQL操作作为一个完整的工作单元，要么全部执行，要么全部不执行。通过事务的使用，能够使一系列相关操作关联起来，防止出现数据不一致现象。
>
> ​		**注意：只有DML语句才有必须要使用事务。一个DDL语句或者一个DCL语句就是一个完整的事务。**
>
> ​		在事务进行过程中，未结束之前都是只操作内存中的数据，当事务结束时才会修改底层硬盘文件中的内容。
>

### 特性

​		**特性:ACID**

- 原子性（Atomicity）：事务必须是一个自动工作的单元，要么全部执行，要么全部不执行。
- 一致性（Consistency）：事务把数据库从一个一致状态带入到另一个一致状态，事务结束的时候，所有的内部数据都是正确的。
- 隔离性（Isolation）：并发多个事务时，一个事务的执行不受其他事务的影响。
- 持久性（Durability）：事务提交之后，数据是永久性的，不可再回滚，不受关机等事件的影响。

### 控制

- 事务提交：COMMIT
- 事务回滚：ROLLBACK

### 开始和结束

​		事务开始于上一个事务结束后执行的第一个SQL语句。

​		事务结束：

- 执行了COMMIT 或者ROLLBACK命令
- 隐式提交（单个的DDL或DCL语句）
- 自动提交:SET AUTOCOMMIT [ON|OFF]; SHOW AUTOCOMMIT; 
- 用户退出
- 系统崩溃

### 并发问题

#### 三个读取

​		脏读：如果第二个事务查询到第一个事务还未提交的更新数据，形成脏读。		

![image-20210516115015423](Oracle.assets/image-20210516115015423.png)

​		虚读(幻读)：一个事务执行两次查询，第二次结果集包含第一次中没有或者某些行已被删除，造成两次结果不一致，只是另一个事务在这两次查询中间插入或者删除了数据造成的。

![image-20210516115215699](Oracle.assets/image-20210516115215699.png)

​		不可重复读：一个事务两次读取同一行数据，结果得到不同状态结果，如中间正好另一个事务更新了该数据，两次结果相异，不可信任。

![image-20210516115630450](Oracle.assets/image-20210516115630450.png)

#### 二个更新

​		第一类丢失更新：在完全未隔离事务的情况下，两个事物更新同一条数据资源，某一事物异常终止，回滚造成第一个完成的更新也同时丢失。	

![image-20210516120603554](Oracle.assets/image-20210516120603554.png)

​		第二类丢失更新：是不可重复读的特殊情况，如果两个事务都读取同一行，然后两个都进行写操作，并提交，第一个事务所做的改变就会丢失。	

![image-20210516120740268](Oracle.assets/image-20210516120740268.png)

### 隔离级别

- Read Uncommited 可读未提交
- Read Commited 可读已提交
- Repeatable Read 可重复读
- Serializable 串行化

![image-20210516121042867](Oracle.assets/image-20210516121042867.png)

​		**注意：Oracle数据库支持READ COMMITTED 和 SERIALIZABLE这两种事务隔离级别。**

# 第八章 表和约束

## 用户管理

### 用户

- 创建：Create User username  Identified by password

- 修改：alter User username  Identified by password

- 删除：drop user user_name [cascade]

  cascade：级联删除选项，如果用户包含数据库对象，则必须加 CASCADE选项，此时连同该用户所拥有的对象一起删除。

### 权限

#### 分类		

​		系统权限：系统规定用户使用数据库的权限。（系统权限是对用户而言)。	

​		实体权限：某种权限用户对其它用户的表或视图的存取权限。（是针对表或视图而言的）。

#### 分配

```sql
grant connect, resource, dba to 用户名1 [,用户名2]...;
```

​		注意：系统权限只能由DBA用户授出：sys, system

#### 收回

```sql
Revoke connect, resource from 用户名;
```

### 角色

​		角色是一组权限的集合，将角色赋给一个用户，这个用户就拥有了这个角色中的所有权限。

![image-20210516121649070](Oracle.assets/image-20210516121649070.png)

![image-20210516121725456](Oracle.assets/image-20210516121725456.png)

## 表管理

### 创建表

#### 数据类型

##### 字符型

- CHAR(size)：固定长度字符型数据，长度以字节为单位。范围1-2000
- VARCHAR2(size)：可变长度字符数据，范围1-4000
- CLOB：可变长度字符数据，最大可存储4G数据

##### 图片型

- BOLB：最大存储4G二级制的数据，可以存放图片、音频、视频文件。

##### 数字型

- NUBMER：数字型，可以表示整数，也可以表示小数。
- NUMBER(n)：整型。
- NUMBER(p,s)：浮点型。p表示整个位数，s表示小数位数。

##### 日期型

- DATE：包括年月日时分秒。
- TIMESTAMP：精度比DATE高一些，可以精确到毫秒。

#### 语法

```sql
CREATE TABLE [schema.]table
    (column datatype [DEFAULT expr][, ...])
```

### 修改表

#### 添加列

```sql
ALTER TABLE table
    ADD (column datatype[DEFAULT expr]
    [, column datatype]...);
```

#### 修改列

```sql
ALTER TABLE table
    MODIFY(column datatype[DEFAULT expr]
    [, column datatype]...);
```

#### 删除列

```sql
ALTER TABLE table
    DROP (column);	
```

#### 改表名

```sql
rename 旧表名 to 新表名
```

### 删除表

```sql
DROP TABLE table；
```

> **回收站**：新增加了回收站功能，drop table并不是真正的将表删除，实际上只是将表改一个名字。仍然占用用户的表空间。
>
> Oracle 10g提供的flashback drop 新特性为了加快用户错误操作的恢复。
>
> - 查看：select * from recyclebin;
> - 恢复：FLASHBACK TABLE 表名 TO BEFORE DROP
> - 清空：purge recyclebin;
> - 彻底删除：DROP TABLE table PURGE；

### 截断表

```sql
TRUNCATE TABLE table;
```

> drop table 和 truncate table 和 delete from 区别：
>
> **drop table**
>
> 1. 属于DDL
> 2. 不可回滚
> 3. 不可带where
> 4. 表内容和结构删除
> 5. 删除速度快
>
> **truncate table**
>
> 1. 属于DDL
> 2. 不可回滚
> 3. 不可带where
> 4. 表内容删除
> 5. 删除速度快
>
> **delete from**
>
> 1. 属于DML
> 2. 可回滚
> 3. 可带where
> 4. 表结构在，表内容要看where执行的情况
> 5. 删除速度慢,需要逐行删除

## 约束

​		保证数据完整性的规则，设置在单个字段或多个字段组合上，写入这些字段的数据必须符合约束的限制。

​		约束和表一样也是数据库对象之一，也需要有名字。如果不指定名字，系统会默认生成一个约束名字。

​		约束级别：

- 列级
- 表级:CONSTRAINT

```sql
CREATE TABLE [schema.] table
    (column datatype [ DEFAULTexpr][column_constraint],
    ...[table_constraint[s]][,...]);
```

​		约束类型：

![image-20210517100633262](Oracle.assets/image-20210517100633262.png)

### CHECK

​		Check约束:在check中定义检查的条件表达式，数据需要符合设置的条件。

```sql
CREATE TABLE member(
    mid NUMBER,
    name VARCHAR2(50) NOT NULL,
    sex VARCHAR2(10) NOT NULL,
    age NUMBER(3),
    CONSTRAINT pk_mid PRIMARY KEY(mid),
    CONSTRAINT ck_sex CHECK(sex IN('男','女')),
    CONSTRAINT ck_age CHECK(age BETWEEN 0 AND 100)
);
```

### FOREIGN KEY

​		FOREIGN KEY约束:外键确保了相关的两个字段的两个关系：

- 子表外键列的值必须在主表参照列值的范围内，或者为空
- 外键参照的是主表的主键或者唯一键。
- 作为主键的表称为“主表”，作为外键的关系称为“从表”

```sql
--列级
department_id number REFERENCES departments(department_id)
--表级
CONSTRAINT emp_dept_fk FOREIGN KEY (department_id) REFERENCES departments(department_id)
```

​		外键删除时：

- 主表外键值被子表参照时，主表记录不允许被删除。
- ON DELETE CASCADE:当父表中的列被删除时，子表中相对应的列也被删除
- ON DELETE SET NULL:子表中相应的列置空

## 练习

1. 创建表date_test,包含列d，类型为date型。试向date_test表中插入两条记录，一条当前系统日期记录，一条记录为“1998-08-18”。

2. 试创建student表，要包含以下信息：

   学生编号（sno）：字符型（定长）4位 主键

   学生姓名（sname）：字符型（变长）8位 唯一

   学生年龄（sage）：数值型 非空

3. 试创建sc表（成绩表），要包含以下信息：

   学生编号（sno）：字符型（定长）4位 主键 外键

   课程编号（cno）：字符型（变长）8位 主键

   选课成绩（grade）：数值型

4. 试修改学生姓名列数据类型为定长字符型10位。

# 第九章 其他数据库对象

## 序列

​		序列是一种用于产生唯一数字列值的数据库对象。

​		一般使用序列自动地生成主码值或唯一键值。序列可以是升序或降序。

​		**只能保证唯一，不能保证连续。**

### 创建

```sql
CREATE SEQUENCE [schema.] 序列名
[INCREMENT BY n]
[START WITH n]
[MAXVALUE n | NOMAXVALUE]
[MINVALUE n | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE n | NOCACHE];
```

### 使用

```sql
sequence_name.CURRVAL
sequence_name.NEXTVAL
```

### 修改

```sql
ALTER SEQUENCE [schema.] 序列名
[INCREMENT BY n]
[MAXVALUE n | NOMAXVALUE]
[MINVALUE n | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE n | NOCACHE];
```

### 删除

```sql
DROP SEQUENCE [schema.] 序列名 
```

## 索引

​		一种用于提升查询效率的数据库对象通过快速定位数据的方法，减少磁盘I/O操作索引信息独立与表存放。

​		Oracle数据库自动使用和维护索引(类似字典的目录)。



​		建立索引的优点：

1. 大大加快数据的检索速度;
2. 创建唯一性索引，保证数据库表中每一行数据的唯一性;
3. 加速表和表之间的连接;
4. 在使用分组和排序子句进行数据检索时，可以显著减少查询中分组和排序的时间。



​		索引的缺点：

1. 索引需要占物理空间。
2. 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，降低了数据的维护速度。



​		以下情况可以创建索引:

1. 列中数据值分布范围很广
2. 列经常在 WHERE 子句或连接条件中出现
3. 表经常被访问而且数据量很大

### 创建

- 自动：主键约束或唯一约束
- 手动

```sql
 CREATE INDEX 索引名 ON 表名 (列名[,列名]... )  
```

### 删除

```sql
DROP INDEX 索引名
```

## ROWNUM

​		ROWNUM是一个伪列，类似表中的列，但是没有实际存储在表中。

​		ROWNUM是返回结果集的顺序号，这个序列号是在记录输出时才一步一步产生的。



​		TonN查询：

```sql
select * from emp where rownum  <= 5  order by sal desc
select * from (select * from emp order by sal desc) where rownum <=5
--区别？
```



​		分页查询：

```sql
SELECT * FROM 
  ( SELECT A.*, ROWNUM RN   FROM 
        (SELECT * FROM Emp) A 
    WHERE ROWNUM <= page*pageSize )    
 WHERE RN > (page-1)*pageSize
```

## ROWID

​		代表记录的地址。显示为18位的字符串。用于定位数据库中一条记录的一个相对唯一地址值。通常情况下，该值在该行数据插入到数据库表时即被确定且唯一。

```sql
select rowid dname from dept;     
```

> 删除表中重复行？

##  视图

​		视图是虚表。是一个命名的查询，用于改变基表数据的显示，简化查询。

​		视图的访问方式与表的访问方式相同。

​		视图本质上就是一个SELECT语句。视图没有存储真正的数据，真正的数据还是存储在基表中。

​		预定义的查询，作为表一样的查询使用，是一张虚拟表。



​		视图好处：

- 可以限制对基表数据的访问，只允许用户通过视图看到表中的一部分数据
- 可以使复杂的查询变的简单
-  提供了数据的独立性，用户并不知道数据来自于何处
- 提供了对相同数据的不同显示



​		分类：

- 简单视图：只涉及到一个表，而且SELECT子句中不包含函数表达式列（包括单行函数和分组函数）。
- 复杂视图：涉及到一个或多个表，SELECT子句中包含函数表达式列（单行函数或分组函数）。                 

### 创建

```sql
CREATE [OR REPLACE] VIEW view
    [(alias[, alias]...)]
    AS subquery
    [WITH READ ONLY]

--赋权
grant create  view to scott; 
```

### 删除

```sql
DROP VIEW 视图名
```

## 同义词

​		是指向数据库对象（如：表、视图、序列、存储过程等）的数据库指针。

​		就是给数据库对象一个别名.



​		类型：

- 私有
- 公有

### 创建

```sql
CREATE [PUBLIC] SYNONYM  同义词
    FOR [schema.] 对象名 ；

--赋权
grant create synonym to scott;
```

### 删除

```sql
DROP SYNONYM s_emp;
```

# 第十章 PL SQL

## PL/SQL简介

​		PL/SQL也是一种程序语言。PL 是Procedural Language的缩写。

​		PL/SQL是Oracle数据库对SQL语句的扩展，增加了编程语言的特点。

​		数据操作和查询语句被包含在PL/SQL代码的过程性单元中，经过逻辑判断、循环等操作完成复杂的功能或者计算

​		优点：

- 改善了性能
- 可重用性
- 模块化

```plsql
DECLARE -- 可选部分
	• 变量、常量、游标、用户定义异常声明
BEGIN -- 必要部分
	• SQL语句
	• PL/SQL语句
EXCEPTION --可选部分
	• 程序出现异常时，捕捉异常并处理异常
END; -- 必要部分
```

```plsql
 BEGIN
    DBMS_OUTPUT.PUT_LINE(‘Hello’);
 END;

--sqlplus
SET SERVEROUTPUT ON(如果使用sqlplus需要设置)
在sqlplus中需要以/结尾！！！
```

## 变量声明

​		PL/SQL中可使用标识符来声明变量，常量，游标，用户定义的异常等，并在SQL语句或过程化的语句中使用。

```plsql
identifier [CONSTANT] datatype [NOT NULL] [:= |DEFAULT expr]
```

### 变量类型

#### 简单变量

​		简单变量不包括任何组合，只能保存一个值。

- v_sal NUMBER(9,2) := 0;
- %TYPE 属性：v_ename emp.ename%TYPE;				

> ```sql
> --变量赋值
> select sal into x from emp where empno = 7369;
> ```

​		使用%TYPE 属性的好处：

- 在编程时，可以不去查询数据库中字段的数据类型
- 数据库中字段的数据类型可能被改变
- 为了和前面的变量的类型始终保持一致

#### 复合（组合）变量

​		一个复合变量可以存放多个值。

- %ROWTYPE属性：%ROWTYPE的前缀是数据库表名。RECORD中的域，与表的字段的名称和数据类型完全相同

## 操作符

![image-20210517110712557](Oracle.assets/image-20210517110712557.png)

## 语句

### if

```plsql
IF condition THEN
statements；
[ELSIF condition THEN
statements；]
[ELSE
statements；]
END IF；
```

### loop

```plsql
LOOP
语句体;
[EXIT | EXIT WHEN 条件;]
END LOOP；
```

​		简单循环的特点，循环体至少执行一次.

​		在使用LOOP语句时必须使用EXIT语句，强制循环结束，否则将死循环。

### while

```plsql
WHILE 条件 LOOP
语句体;
END LOOP;
```

### for

```plsql
FOR counter IN [REVERSE] start_range..end_range LOOP
语句体;
END LOOP;
```

​		REVERSE:正常计数器从小到大递增，使用REVERSE将使计数器从大到小递减。

## PL/SQL与Oracle交互

### SELECT语句

​		必须使用INTO子句。

​		查询必须并且只能返回一行。

​		可以使用完整的SELECT 语法。

```sql
SELECT [DISTICT|ALL]{*|column[,column,...]}
    INTO (variable[,variable,...] |record)
    FROM {table|(sub-query)}[alias]
    [WHERE 子句]
```

### DML语句

​		通过使用DML 命令，可对数据库中表的数据实现下列操作：

- INSERT
- UPDATE
- DELETE

### 事务语句

- commit
- rollback

### 游标

​		游标的作用就是用于临时存储从数据库中提取的数据。

#### 声明游标

```plsql
--在DECLARE部分按以下格式声明游标： 
CURSOR 游标名[(参数1 数据类型[，参数2 数据类型...])] 
	IS SELECT语句; 

--参数是可选部分，所定义的参数可以出现在SELECT语句的WHERE子句中。
--如果定义了参数，则必须在打开游标时传递相应的实际参数。 
```

#### 打开游标

```plsql
--在可执行部分，按以下格式打开游标： 
OPEN 游标名[(实际参数1[，实际参数2...])]; 

--打开游标时，SELECT语句的查询结果就被传送到了游标工作区。 

```
#### 提取数据

​		在可执行部分，按以下格式将游标工作区中的数据取到变量中。提取操作必须在打开游标之后进行。 

​		游标打开后有一个指针指向数据区，FETCH语句一次返回指针所指的一行数据，要返回多行需重复执行，可以使用循环语句来实现。控制循环可以通过判断游标的属性来进行。 

- FETCH 游标名 INTO 变量名1[，变量名2...]; 第一种格式中的变量名是用来从游标中接收数据的变量，需要事先定义。变量的个数和类型应与SELECT语句中的字段变量的个数和类型一致。
- FETCH 游标名 INTO 记录变量;
#### 关闭游标

```plsql
CLOSE 游标名; 
--显式游标打开后，必须显式地关闭。游标一旦关闭，游标占用的资源就被释放，游标变成无效，必须重新打开才能使用。
```

#### 游标例子

> 游标属性：
>
> - %ROWCOUNT   整型  获得FETCH语句返回的数据行数    
> - %FOUND  布尔型 最近的FETCH语句返回一行数据则为真，否则为假    
> - %NOTFOUND   布尔型 与%FOUND属性返回值相反    
> - %ISOPEN 布尔型 游标已经打开时值为真，否则为假
```plsql
--使用loop遍历游标
declare
 cursor my is select * from dept;
 v_dept  my%rowtype; 
begin
    open my;
    loop
    	fetch my into v_dept;
    	exit when my%notfound;
      	dbms_output.put_line(v_dept.dname);    
    end loop;
    close my;
end;
```

```plsql
--使用for in时不用手动打开和关闭游标。
declare
 cursor my is select * from dept;
 v_dept  my%rowtype; 
begin  
    for v_dept in my loop
       dbms_output.put_line(v_dept.dname);
    end loop;     
end; 
```

```plsql
--使用带参光标cursor，查询10号部门的员工姓名和工资。
declare
    cursor cemp(pdeptno emp.deptno%type) is select ename,sal from emp where deptno=pdeptno;
    pename emp.ename%type;
    psal emp.sal%type; 
begin 
    open cemp(&deptno);
    loop
        fetch cemp into pename,psal;	 
        exit when cemp%notfound;
        dbms_output.put_line(pename||'的薪水是'||psal);
    end loop;
    close cemp;
end;
```

## 异常处理

​		PL/SQL用异常和异常处理器来实现错误处理。

```plsql
EXCEPTION
WHEN exception1 [OR exception2 . . .] THEN
语句体1;
. . .
[WHEN exceptionN] THEN
语句体n
. . .]
[WHEN OTHERS THEN
语句体n+1
. . .]
```

​		预定义异常：

- NO_DATA_FOUND --没有找到数据
- TOO_MANY_ROWS --找到多行数据
- INVALID_CURSOR --失效的游标
- ZERO_DIVIDE --除数为零
- DUP_VAL_ON_INDEX –唯一索引中插入了重复值

```plsql
EXCEPTION
	WHEN NO_DATA_FOUND THEN
		ROLLBACK;
		DBMS_OUTPUT.PUT_LINE(’没有50号部门记录 ’);
	WHEN TOO_MANY_ROWS THEN
		ROLLBACK;
		DBMS_OUTPUT.PUT_LINE(‘返回多条记录.’);
	WHEN OTHERS THEN
		ROLLBACK;
		DBMS_OUTPUT.PUT_LINE (’ 出现其他错误.’);
```

## 练习

1. 从部门表中找到最大的部门号，将其输出到屏幕

2. 在部门表中插入一个新部门

3. 将练习2中的部门从部门表中删除

4. 定义变量代表员工表中的员工号，根据员工号获得员工工资，查询某员工工资，如果工资小于4000，输出到屏幕上的内容为员工姓名和增涨10％以后的工资，否则输出到屏幕上的内容为员工姓名和增涨5％以后的工资

5. 根据员工号，获得员工到目前为止参加工作年限（保留到整数），员工号不存在时提示“此员工号不存在”

# 第十一章 存储过程、函数、触发器

## 命名PL/SQL块

​		命名块可被独立编译并且存储在数据库中，可以有参数。

- 存储过程
- 函数
- 触发器
- 包

## 存储过程

​		存储过程就是命了名的PL/SQL块，**可以有零个或多个参数，没有返回值**，以编译后的形式存放在数据库中，然后由开发语言调用或PL/SQL块中调用。	

```plsql
CREATE [OR REPLACE] PROCEDURE
[schema.]procedure_name [(argument [in|out|in out] type…)]
IS | AS
[本地变量声明]
BEGIN
执行语句部分
[EXCEPTION]
错误处理部分
END[procedure_name];

--赋权
GRANT CREATE ANY PROCEDURE TO SCOTT;
```

### 参数

​		参数可以为任何合法的PL/SQL类型。

​		参数模式：

- in：就是从调用环境通过参数传入值，在过程中只能被读取，不能改变。
- out：由过程赋值并传递给调用环境。不能是具有默认值的变量，也不能是常量，过程中要给OUT参数传递返回值。
- in out：具有IN 参数和OUT 参数两者的特性，在过程中即可传入值，也可传出值

### 调用

​		Sql*Plus： exec 存储过程名(参数列表)

​		过程中调用：没有输出参数：call 存储过程名(参数列表)

​		注意：有输出参数：则要在PL/SQL语句中调用

### 删除

```sql
DROP PROCEDURE procedure_name
```

## 函数

​		函数是有返回值的命名的PL/SQL块。

### 创建

```sql
CREATE [OR REPLACE] FUNCTION
[schema.] function_name [(argument [in|out|in out] type…)]
RETURN returning_datatype
IS | AS
[本地变量声明]
BEGIN
执行语句部分
[EXCEPTION]
错误处理部分
END[function_name];
```

### 调用

​		按正常函数调用即可。

### 删除

```sql
DROP FUNCTION function_name              
```

> 过程和函数的区别？
>
> 1、存储过程用户在数据库中完成特定操作或者任务（如插入，删除等），函数用于返回特定的数据。
>
> 2、存储过程声明用procedure，函数用function。
>
> 3、存储过程不需要返回类型，函数必须要返回类型。
>
> 4、存储过程可作为独立的plsql执行，函数不能作为独立的plsql执行，必须作为表达式的一部分。
>
> 5、存储过程只能通过out和in/out来返回值，函数除了可以使用out，in/out以外，还可以使用return返回值。
>
> 6、sql语句（DML或SELECT)中不可用调用存储过程，而函数可以。

##  触发器

### 组成

- 触发对象：包括表、视图、模式、数据库。只有在这些对象上发生了符合触发条件的触发事件，才会执行触发操作.。

- 触发事件：引起触发器被触发的事件。

- 触发时间：即该TRIGGER 是在触发事件发生之前（BEFORE）还是之后(AFTER)触发，也就是触发事件和该TRIGGER 的操作顺序。

- 触发操作：即该TRIGGER 被触发之后的目的和意图，正是触发器本身要做的事情。 例如：PL/SQL 块。

- 触发频率：说明触发器内定义的动作被执行的次数。即语句级(STATEMENT)触发器和行级(ROW)触发器。

  语句级(STATEMENT)触发器：是指当某触发事件发生时，该触发器只执行一次；

  行级(ROW)触发器：是指当某触发事件发生时，对受到该操作影响的每一行数据，触发器都单独执行一次。        

### 事件分类

- DML事件
- DDL事件
- 数据库事件

### 创建

```plsql
CREATE [OR REPLACE] TRIGGER 触发器名
[before|after] --触发时间
[insert|update|delete] --触发事件
ON 表名
[FOR EACH ROW]
BEGIN
    pl/sql语句
END;
```

​		当省略FOR EACH ROW 选项时，BEFORE 和AFTER 触发器为语句触发器。

​		在行触发器的PL/SQL块和WHEN 子句中可以使用相关名称参照当前的新、旧列值，默认的相关名称分别为OLD和NEW。

​		eg:    :old.ename   :new.ename

| 触发语句 | :old                 | :new                 |
| -------- | -------------------- | -------------------- |
| Insert   | 所有字段都是空(null) | 将要插入的数据       |
| Update   | 更新以前该行的值     | 更新后行的值         |
| Delete   | 删除以前该行的值     | 所有字段都是空(null) |

### 实例

#### DML触发器

1. 在更新操作时打印更新前和更新后的工资。
2. 不允许删除编号为7369的员工。

> RAISE_APPLICATION_ERROR 是将应用程序专有的错误从服务器端转达到客户端应用程序。
>
> RAISE_APPLICATION_ERROR 的声明：
>
> **PROCEDURE RAISE_APPLICATION_ERROR( error_number_in IN NUMBER, error_msg_in IN VARCHAR2);**
>
> 里面的错误代码和内容，都是自定义的，说明是自定义，当然就不是系统中已经命名存在的错误类别，是属于一种自定义事务错误类型，才调用此函数。
>
> error_number_in 之容许从 -20000 到 -20999 之间，这样就不会与 ORACLE 的任何错误代码发生冲突。
>
> error_msg_in 的长度不能超过 2k，否则截取 2k。

#### DDL触发器

```plsql
CREATE OR REPLACE TRIGGER NODROP_EMP
    BEFORE
 DROP ON SCOTT.SCHEMA 
    BEGIN
        IF Sys.Dictionary_obj_name='EMP' THEN
				RAISE_APPLICATION_ERROR(-20005,'错误信息：不能删除emp表！');
        END IF; 
END;
```

#### 数据库事件触发器

```plsql
--系统登录日志
CREATE OR REPLACE TRIGGER tr_logon
AFTER LOGON ON DATABASE
BEGIN
   INSERT INTO log_event (user_name, address, logon_date)
   VALUES (ora_login_user, ora_client_ip_address, systimestamp);
END tr_logon;

--系统登出日志
CREATE OR REPLACE TRIGGER tr_logoff
BEFORE LOGOFF ON DATABASE
BEGIN
   INSERT INTO log_event (user_name, address, logoff_date)
   VALUES (ora_login_user, ora_client_ip_address, systimestamp);
END tr_logoff;
```

### 删除

```sql
DROP TRIGGER trigger_name;
```

## 练习

1. 创建存储过程PrintEmp，将emp表中所有员工编号和姓名显示出来。

2. 创建存储过程PTEST，接受两个数相除并且显示结果，如果第二个数是0，则显示消息“not to DIVIDEBYZERO！”，不为0则显示结果。

3. 创建函数Emp_Avg：根据员工号，返回员工所在部门的平均工资。

4. 创建一个过程Update_SAL，通过调用上题3中的函数，实现对每个员工工资的修改：如果该员工所在部门的平均工资小于1000，则该员工工资增加500；大于等于1000而小于5000，增加300；大于等于5000，增加100。

5. 编写一个函数，根据员工编号计算该员工进入公司的月数。

6. 使用触发器实现主键自增长

# 第十二章 其他

## 三大范式

​		范式就是符合某种设计要求的总结。

### 第一范式

​		**第一范式：字段是原子性的，不可分。**

​		**1NF的定义为：符合1NF的关系中的每个属性都不可再分。**下表所示的情况，就不符合1NF的要求。

![image-20210519112954140](Oracle.assets/image-20210519112954140.png)

​		1NF是所有关系型数据库的最基本要求，也就是说，只要在RDBMS中已经存在的数据表，一定是符合1NF的。如下表所示：

### 第二方式

​		**第二范式：有主键，非主键字段依赖主键。**

​		例如： 对于下仅仅符合1NF的下表

![image-20210519113316619](Oracle.assets/image-20210519113316619.png)

​		仍会存在一些问题：

​		**数据冗余过大：** 学号、姓名、系名、系主任这些数据重复多次。每个系与对应的系主任的数据也重复多次

​		**插入异常:** 假如学校3月份新建了一个系，等到8月份才招生，那么无法将系名与系主任的数据单独地添加到数据表中去

​		**删除异常:** 假如将某个系中所有学生相关的记录都删除，那么所有系与系主任的数据也就随之消失了

​		**修改异常:** 假如李小明转系到法律系，那么为了保证数据库中数据的一致性，需要修改三条记录中系与系主任的数据

​		改进后如下：

![image-20210519113428921](Oracle.assets/image-20210519113428921.png)

### 第三方式

​		仅仅符合2NF的要求，很多情况下还是不够的，原因在于仍然存在非主属性(系主任)对于码(学号)的传递依赖。改进后如下：

![image-20210519123911683](Oracle.assets/image-20210519123911683.png)

## 表关系

### 一对一

![image-20210519123957254](Oracle.assets/image-20210519123957254.png)

### 一对多

![image-20210519124119334](Oracle.assets/image-20210519124119334.png)

### 多对多

![image-20210519124202879](Oracle.assets/image-20210519124202879.png)

### 例子

![image-20210519124240234](Oracle.assets/image-20210519124240234.png)

# 第十三章 JDBC

## JDBC简介

​		JDBC（Java Data Base Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。

## 数据库驱动

![image-20210701152203169](Oracle.assets/image-20210701152203169.png)

## 常用接口

### Driver

​		代表数据库驱动

### Connection

- createStatement()
- prepareStatement(sql)
- prepareCall(sql)
- setAutoCommit(boolean autoCommit)
- commit()
- rollback()

### Statement

- execute(String sql)
- executeQuery(String sql)
- executeUpdate(String sql)
- addBatch(String sql)
- executeBatch()
- clearBatch()

### PreparedStatement 

- clearParameters()
- executeQuery() 
- executeUpdate() 
- setXxx()	

### CallableStatement

### ResultSet

- next()
- getXxx()

## JDBC操作步骤

1. 导入对应数据库驱动包
2. 注册数据库驱动
3. 获得数据库连接
4. 获取数据库操作对象
5. 执行SQL语句
6. 关闭资源

## SQL注入

​		所谓SQL注入，就是通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

## 事务

​		特点(ACID)：

- 原子性(Atomicity)
- 一致性(Constistency)
- 隔离性(Isolation)
- 持久性(Durability)

## 批处理

​		批量处理允许将相关的SQL语句分组到批处理中，并通过对数据库的一次调用来提交它们，一次执行完成与数据库之间的交互。

​		一次向数据库发送多个SQL语句时，可以减少通信开销，从而提高性能。

​		步骤：

1. 创建PrepareStatement对象。
2. 设置事务不要自动提交
3. 设置参数值后，添加到批处理中
4. 执行批处理
5. 提交事务

## 自写工具类

​		提出常量到xx.properties文件中。

​		使用ResourceBundle rb=new ResourceBundle.getBundel("xx");

```java
String driver=rb.getString("driver");
```