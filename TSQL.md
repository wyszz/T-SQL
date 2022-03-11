# SQL Server 数据库高级编程期末复习

## 分数分布

1. 选择（40个$\times$1'=40分）
2. $编程（4个\times15'=60分)
   \begin{cases}
   游标\\函数\\过程\\触发器
   \end{cases}$

## SQL基础知识总结

​	代表本机：（  **.**  ,  计算机名  ,  Localhost  ,  (Local)  ,  127.0.0.1）

1. SQL Server Management Studio$\longrightarrow$ 启动程序

2. 5种状态（选择）

   1. 启动
   2. 停止
   3. 暂停
   4. 恢复
   5. 重启

3. 3种验证方式

   1. win验证
   2. SQL验证
   3. 混合验证

4. 5个表

   |  表名   | PRIMARY KEY（主键约束） | 主从表 |
   | :-----: | :---------------------: | :----: |
   | student |           sno           |  主表  |
   | course  |           cno           |  主表  |
   |   sc    |         sno,cno         |  从表  |
   |  dept   |         deptno          |  主表  |
   |   emp   |          empno          |  从表  |

   其中**sc表**中，通过sno,cno**主外键关联**与student表，course表产生联系$\longrightarrow$三表之间

   **主表**不可以随便更改

### 一、服务（名字，状态）、登陆方式、服务器

- SQL Server(SQLEXPRESS)      $$\longrightarrow$$服务器名称(**.**\SQLEXPRESS)
- SQL Server(MSSQLSERVER)  $$\longrightarrow$$服务器名称(**.**)

### 二、SQL(结构化查询语言)

- **DDL**：数据定义$$\longrightarrow$$create,alter,drop$$\longrightarrow$$结构上
- **DML**：数据操纵$$\longrightarrow$$insert,update,delete
- **DQL**：数据查询$$\longrightarrow$$select
- **DCL**：数据控制$$\longrightarrow$$grant(授权),revoke(回收授权),deny(拒绝)

### 三、增删改

1. insert into$$\longrightarrow$$**表**(列，列，列$$\cdots$$)$$\longrightarrow$$values(**值，值，值**$$\cdots$$) $$\Longrightarrow$$行操作

   - **※※**用**null**来代替不知道的值
   - 或指定**表的列**

2. update$$\longrightarrow$$**表**

   set      $$\longrightarrow$$**列**=新值，列=新值，列=新值$$\cdots$$

   where$$\longrightarrow$$**行**（行的过滤） $$\Longrightarrow$$为空时判定用is

3. delete from $$\longrightarrow$$ **表**

   where          $$\longrightarrow$$**行**（行的过滤）

   drop table  $$\longrightarrow$$**表名**

### 四、数据查询 select

​	select$$\longrightarrow$$**列**（列，列的别名，列的表达式，列的函数）

​	from$$\longrightarrow$$**数据集**（表，视图，子查询）

​	where$$\longrightarrow$$**行的选择**（<,=,>,is,between$$\cdots$$and$$\cdots$$,in,or,like）

​	group by$$\longrightarrow$$**分组**

​	having$$\longrightarrow$$**组的选择和过滤**

​	order by$$\longrightarrow$$**排序**，asc升序，desc降序

$select
\begin{cases}
distinct\longrightarrow消除重复键\\
avg求平均，sum求和,max求最大值,min求最小值,count(*)查有几行\cdots\longrightarrow聚簇函数\\\qquad聚簇函数会从第一条数据查到最后一行才输出结果\\
as\quad'  \quad '\longrightarrow起别名
\end{cases}$

#### 1.简单查询

1. between$$\cdots$$and$$\cdots$$$$\longrightarrow$$连续的闭区间
2. and$$\longrightarrow$$与
3. or$$\longrightarrow$$或
4. in/not in$$\longrightarrow$$是否在集合内（可进行离散值的查询）
5. where$$\longrightarrow$$选择行
6. is/is not null$$\longrightarrow$$空不空使用
7. like '%   %'，ike '\_   \_ '$$\longrightarrow$$模糊查询, '**%**' 表示任意数量字符，'**_**' 表示仅有一个字符

#### 2.分组查询

- Order by$$\longrightarrow$$只能用在最后一行，且只能出现一次

- select中除聚簇函数外其他属性列必须放在group by 中

  - ```sql
    select deptno,avg(sal) group by deptno
    ```

#### 3.非相关子查询 in

```sql
select sname from student where sno in(select sno from sc where cno='c1')
```

#### 4.相关子查询 exist

​	

#### 5.连接查询

```sql
//自然连接
where student.sno=sc.sno
```

$外连接(信息是全的)
\begin{cases}
left\ join\longrightarrow从左表返回所有的行，即使右表中没有匹配。如果右表中没有匹配，则结果为 NULL。\\
right\ join\longrightarrow从右表返回所有的行，即使左表中没有匹配。如果左表中没有匹配，则结果为 NULL。\\
full\ join\longrightarrow在其中一个表中存在匹配项时返回行
\end{cases}$

- **INNER JOIN** -当两个表中都有匹配项时返回行。
- **LEFT JOIN** -返回左侧表中的所有行，即使右表中没有匹配项。
- **RIGHT JOIN** -返回右表中的所有行，即使左表中没有匹配项。
- **FULL JOIN** -在其中一个表中存在匹配项时返回行。
- **SELF JOIN** -这用于将表连接到自身，就像该表是两个表，临时重命名MS SQL Server语句中的至少一个表。
- **CARTESIAN JOIN** -返回两个或多个联接表中的记录集的笛卡尔乘积。

<img src="join.jpg"  />

## T-SQL基础（SQL高级编程语言）

### 一、数据类型、常量、变量

#### 	1.自定义数据类型：exec(调用执行 )

​				sp_$\longrightarrow$不是真正的数据类型，不需要约束条件

​				$exec
\begin{cases}
sp\_addtype\quad's\_sno','char(8)','not\quad null'\longrightarrow定义自定义类型\\sp\_droptype\quad's\_sno'\longrightarrow删除自定义类型\end{cases}$

#### 2.常量

- 又称为字面值或标量值,程序运行过程中**值不变**

-  **'**O**''**Bbaar**’**，如果单引号中的字符串包含引号，可以使用两个单引号表示嵌入的单引号。

$常量
\begin{cases}
字符串常量\\数值型常量\\日期型常量\longrightarrow单引号包起来，中间不能有汉字\begin{cases}'April\ 20,2000'\rightarrow字母日期格式\\'4/15/1998'\rightarrow数字日期格式\\'20011207'\rightarrow未分隔的字符串格式\end{cases}\\Money(货币)常量\rightarrow前缀为\$\end{cases}$

#### 3.变量

​	变量名不能与系统变量相同

**※※※※**TSQL中无string类型$\longrightarrow$$
\begin{cases}char\\varchar\\nchar\\\cdots
\end{cases}$

$变量
\begin{cases}
@a\longrightarrow(可)自定义，局部变量\\@@\longrightarrow系统定义（不可操控），全局变量,作为函数引用\end{cases}$

#####  	变量的声明赋值与使用

​		**declare**   @i  int$\longrightarrow$声明变量 

​		**set**   @i=10$\longrightarrow$变量赋值，同时给多个变量赋值时使用**select**

​		**print**  @a$\longrightarrow$变量打印输出，同时输出多个变量时使用**select**

- print  '和为'+str(@c)$\longrightarrow$str把变量转型成字符串
- ltrim$\longrightarrow$去掉左边空格同时把变量转型成字符串
- rtrim$\longrightarrow$去掉右边空格同时把变量转型成字符串
- @a+@b$\longrightarrow$'+'连接作用$\begin{cases}@a,@b都为数值型\longrightarrow加法\\@a,@b都为字符串\longrightarrow连接\\@a为数值型,@b为字符型\longrightarrow ltrim(),str(),rtrim()\end{cases}$

#### 4.注释

$注释
\begin{cases}
--\quad\longrightarrow单行\\/*\quad*/\longrightarrow多行注释\end{cases}$

### 二、流程控制

|                  控制语句                   |                             说明                             |
| :-----------------------------------------: | :----------------------------------------------------------: |
| begin$\cdots$end$\longrightarrow${$\cdots$} |                       语句块，允许嵌套                       |
|               if$\cdots$else                | 条件语句，表达式用不用括号都可以，语句结束后加不加**';'**都可以 |
|                    case                     |                   分支语句，**多**条件判定                   |
|                    while                    |               循环语句$\rightarrow$多用于游标                |
|                  continue                   |                    用于重新开始下一次循环                    |
|                    break                    |                       用于退出本次循环                       |
|                   return                    |                          无条件返回                          |

#### case※※

​	case语句上边通常为半句话

​	**※※※※**使用**case**语句给变量赋值

```sql
--查询05880101学生选课的平均成绩，并根据平均成绩的范围输出等级A、B、C、D、E五个字母
use demo
go
declare @avg int,@flag varchar(20)
select @avg=avg(grade)
from sc
where sno='05880101'
select @flag=
case  
     when @avg>=90 and @avg<=100 then 'A'
     when @avg>=80 and @avg<90   then 'B'
     when @avg>=70 and @avg<80   then 'C'
     when @avg>=60 and @avg<70   then 'D'
     when @avg<60 then 'E'
     else 'No such grade'
end
select @flag as '等级'

```

### 三、游标

#### 	1.两种题型

​		2种题型$\begin{cases}1.结果遍历输出\\2.指向某一行，操纵数据，对行进行修改\end{cases}$

#### 	2.游标作用

​		**使用游标可以在查询数据的同时对数据进行处理(删、改)**

#### 	3.游标种类

游标种类$\begin{cases}普通游标\\游标变量\begin{cases}局部游标变量\\全局游标变量\end{cases}\end{cases}$

#### 4.5个步骤（普通游标的使用）

|  步骤  |   关键词   |   说明   |
| :----: | :--------: | :------: |
| 1.声明 |  declare   |   绑定   |
| 2.打开 |    open    |   游动   |
| 3.读取 |   fetch    |   取值   |
| 4.关闭 |   close    |   关闭   |
| 5.删除 | deallocate | 删除游标 |

#### 5.语句格式

1. daclare 游标名 cursor
2. for select * from student where dept='系名'
3. open 游标名
4. fetch (next|prior|frist|last|absosute|relative) from 游标名/into@t
5. close 游标名
6. deallocate 游标名

#### 6.游标基础代码（3~4分）

```sql
declare c1 cursor for select/*update/delete*/
open c1
fetch next from c1
	while @@fetch_status=0
		begin
			fetch next from c1
			/*游标遍历结果集全部输出*/
			
			/*delete/update where current of c1当前行的数据*/
		end
close c1
deallocate c1
```

#### 7.全局游标变量(选择)

1. **@@cursor_rows：返回最后打开的游标中当前存在的满足条件的行数。**

   返回**0**：表示游标**未打开**;

   返回**-1**：表示游标为**动态**游标；

   返回值（**n**）：是游标中的**总行数**。

2. **@@fetch_status：返回fetch语句执行后游标的状态。**

   返回**0**：表示fetch语句执行**成功**；

   返回**-1**：表示fetch语句执行**失败**；

   返回**-2**：表示被读取的记录**不存在**。

#### 8.声明游标

```sql
declare c1 cursor
[ local | global ]                  /*游标作用域*/
[ forward_only | scroll ]           /*游标移动方向*/
[dynamic |static | fast_forward ]	/*游标类型*/
[ read_only]                        /*访问属性*/
for select                       	 /*SELECT查询语句*/
for update of 列名					/*可修改的列*/

```

##### （1）游标作用域

- **scroll**：说明所声明的游标可以前滚、后滚，可使用所有的提取选项（frist、last、prior、next）。

##### （2）游标移动方向

- **forward_only**：表示游标只能从第一行滚动到最后一行，即该游标只能支持fetch的next提取选项。

##### （3）游标类型

- **dynamic**：为动态游标。它能够反映对结果集中所做的更改(update、delete)，并且如果使用 where current  of子句通过游标进行更新，则它们也立即在游标中反映出来。
- **static**：为静态游标。数据库中所做的任何影响结果集成员的更改（增加、修改或删除数据），都不会反映到游标中，新的数据值不会显示在静态游标中。
- **fast_forward**：为只进游标。只支持游标从头到尾顺序提取数据。在结果集行提取后对行所做的更改对游标是不可见的。

##### （4）访问属性

- **read_only**：说明游标为只读的，不能通过该游标更新数据库中的数据。

##### （5）目的

- **for update** **of 某一列**：指出游标中可以更新的列。

##### （6）游标类型与移动方向之间的关系

- `fast_forward`不能与`scroll`一起使用，且`fast_forward`与`forward_only`只能选用一个。
- 若指定了移动方向为`forward_only`，而没有用`static`或`dynamic`关键字指定游标类型，则默认所定义的游标为动态游标。
- 若移动方向`forward_only`和`scroll`都没有指定，那么移动方向关键字的默认由以下条件决定：
  1. 如果指定了游标移动类型为`static`或`dynamic`，则移动方向默认为`scroll`；
  2. 如果没有用`static`或`dynamic`关键字指定游标类型，则移动方向默认值为`forward_only`。

#### 9.打开游标

`global`说明打开的是**全局游标**，否则打开局部游标。游标变量名可以引用要进行提取操作的已打开的游标。

#### 10.读取数据

打开游标后，可以使用`fetch`语句从中读取数据说明读取数据的位置

fetch [next,prior,first,last,absosute,relative] $\longrightarrow$说明读取数据的**位置**

from  (global)   游标名/@<游标变量名> 

into  @<游标变量名>  (n)

- **next**
  紧跟当前行返回结果行，并且当前行递增为返回行。 如果fetch next 为对游标的第一次提取操作，则返回结果集中的第一行。 next 为默认的游标提取选项。
- **prior**
  返回紧邻当前行前面的结果行，并且当前行递减为返回行。 如果 fetch prior 为对游标的第一次提取操作，则没有行返回并且游标置于第一行之前。
- **frist**
  返回游标中的**第一行**并将其作为当前行。
- **last**
  返回游标中的**最后一行**并将其作为当前行。

- **absosute**

​       n为正数读取从游标头开始的后第n行，并且使其位置为当前行。

​       n为负数读取从游标尾开始的前第n行，并且使其位置为当前行。 

- **relative**

​       n为正数读取从当前行之后的第n行，并且使其位置为当前行。

​       n为负数读取从当前行之前的第n行，并且使其位置为当前行。

#### 11.关闭游标

​	游标使用完以后，要及时关闭。关闭游标使用`close`语句

​	close (global)   游标名/@<游标变量名> 

#### 12.删除游标

游标关闭后，其定义仍在，需要时可用`open`语句打开，再次使用。若确认游标不再需要，就要释放其定义占用的系统空间，即删除游标。删除游标使用`deallocate`语句

deallocate  (global)   游标名/@<游标变量名> 

### 四、函数

##### 1.函数分类

函数分为系统函数和用户自定义函数

##### 2.系统函数

$系统函数\begin{cases}聚组函数\begin{cases}avg\\count\\count(*)\\max\\min\\sum\end{cases}\\数学函数\begin{cases}abs\rightarrow绝对值\\ceiling\rightarrow取大于等于指定值的最小整数\\floor\rightarrow小于等于指定值得最大整数\\power\\round\\square\rightarrow返回指定浮点值的平方\\sqrt\end{cases}\\字符串函数\begin{cases}char\longrightarrow获取ASCII码对应的字符Char\\str\\ltrim/trim\\left/right\longrightarrow截取左/右边字符串\\charindex\rightarrow在指定的字符串中搜索特定的字符串，并可以指定开始搜索的位置，返回第一次找到目标字符串的字符数\\len\\lower/upper\end{cases}\\数据类型转换函数\\日期时间函数\begin{cases}getdate\\year/month/day\\dateadd\end{cases}\\\cdots\end{cases}$

##### 3.用户定义函数

$自定义函数\begin{cases}标量函数\\表值函数\begin{cases}内嵌表值函数\\多语句表值函数\end{cases}\end{cases}$

- 标量函数：只能返回单个确定类型的标量值。
- 内嵌表值函数：以表的形式返回一个返回值，即返回的是一个表，相当于一个参数化的视图。
- 多语句表值函数：可以看作标量型和内嵌表值型函数的结合体。返回值是一个表，但它和标量型函数一样，有一个begin$\cdots$end语句括起来的函数体，该函数体包含插入语句，向返回表中插入数据。

###### 1.标量函数的定义

语法格式：

1. create function 架构名.函数名$\longrightarrow$函数名部分（demo数据库中架构名为dbo）

2.  （@参数名 as  数据类型 =default  readonly)  $\longrightarrow$形参定义部分

3. returns 返回值类型$\longrightarrow$返回参数的类型

4. as

5. begin

    函数体         $\longrightarrow$函数体部分

   return 标量表达式 $\longrightarrow$返回语句

   end

说明：参数和返回值的数据类型可以是任何有效的SQL标量数据类型，不能是用户自定义的数据类型、timestamp或

​       cursor游标。

```sql
/*使用标量函数average实现以下功能：
1.查询全体学生选修某门课程（课程号）的平均成绩。
2.调用上述函数，输出选修“c2”课程的平均成绩。*/
create function average(@cno char(8))
returns int
as
begin
   declare @avg int
   select @avg=avg(grade)
   from sc
   where cno=@cno
   return @avg
End
/*函数调用*/
declare @aver int
select @aver=dbo.average('c2')
print 'c2号课程的平均分为：'+ltrim(@aver)
print 'c2号课程的平均分为：'+ltrim(dbo.average('c2'))
```

###### 2.标量函数的调用

- 当调用用户定义的标量函数时，必须提供至少由两部分组成的名称（架构名.函数名）。
- 可以在select、set 和print语句中调用标量函数。

######  3.表值函数

$表值函数\begin{cases}内嵌(内联)表值函数\\多语句表值函数\end{cases}$

内嵌函数可用于实现参数化视图。例如，视图如下：

```sql
create view view1
as
select sno,sname
from student where dept='计算机系'
```

###### 3.1	内嵌表值函数的定义

1. create function (架构名.) 函数名$\longrightarrow$函数名部分
2. (@参数名 as 数据类型=（default)) readonly$\longrightarrow$形参定义部分
3. returns table$\longrightarrow$返回值为表类型
4. as
5. return select语句$\longrightarrow$通过select语句返回内嵌表

- 说明：在内嵌表值函数中，通过单个select语句定义table返回值，因此内联函数没有相关联的返回变量。

```sql
create function getinfo(@dept varchar(15))
returns table
as
  return
     (
        select sno,sname
        from student
        where dept=@dept
    )
select * from getinfo('计算机系')

```

###### 3.2	内嵌表值函数的调用

- 内嵌表值函数只能通过select语句调用
- select   \*  from   函数名（实参）内嵌表值函数调用时，可以仅使用函数名。

```sql
/*创建内嵌表值函数Course_Student完成以下功能：
（1）查询student表中选修了某门课程（课程号）的所有学生的信息。
（2）调用上述函数，输出选修“c4”课程的所有学生的信息。*/
create function course_student1(@cno char(8))
returns table
as
  return 
        select * from student
        where sno in(select sno from sc
                     where cno=@cno)
select * from course_student1('c4')

create function course_student(@cno char(8))
returns table
as
  return 
        select student.* from student,sc
        where student.sno=sc.sno
              and cno=@cno
select * from course_student('c4')
```

###### 3.3	多语句表值函数的定义

语法格式：

1. create function (架构名.) 函数名$\longrightarrow$ 定义函数名部分

2. ( @参数名 as 数据类型 (=default) ) (n)) $\longrightarrow$形参定义部分

3. returns @return_variable table $\longrightarrow$返回值为表类型

4. (列名 数据类型 列选项(n) )   $\longrightarrow$定义表的内容

5. as

6. begin

7. insert @return_variable

   函数体         $\longrightarrow$   定义函数体

8. return

9. end

###### 3.4 	多语句表值函数的调用。

多语句表值函数的调用与内嵌表值函数的调用方法相同。

select  \*  from   函数名（实参）

```sql
/*在数据库中创建返回表的多语句表值函数，完成以下功能：
（1）显示某个学生的学号，姓名及其选修课程的名称，学分和成绩。
（2）通过以学号“05880103”作为实参，调用该函数输出*/
create function score_table(@sno char(8))
returns @score table
(
 s_sno char(8),
 s_sname char(20),
 c_cname char(20),
 c_grade int,
 c_credit int)
as
 begin 
     insert @score
         select student.sno,sname,cname,grade,credit
         from student,course,sc
         where student.sno=sc.sno and course.cno=sc.cno
               and student.sno=@sno
         return
end
```

###### 4.内嵌表值函数和多语句表值函数不同之处

1. 内嵌表值函数没有函数主体，返回的表是单个SELECT语句的结果集；
2. 而多语句表值函数在begin$\cdots$end块中定义的函数主体包含T-SQL语句，这些语句可生成行并将行插入到表中，最后返回表。

###### 5形式参数为空的函数定义和调用

```sql
/*使用多语句表值函数返回“计算机系”和“信息系”学生的学号、姓名和院系。*/
create function dept_table()
returns @dept table
(
  s_sno char(8),
  s_sname char(20),
  d_dept varchar(15)
)
as
begin
   insert @dept
     select sno,sname,dept
     from student
     where dept='计算机系' or dept='信息系'
  return
end
select * from dept_table()
/*内嵌表值函数：*/
create function dept_table1()
returns table
as
return
    select sno,sname,dept
     from student
     where dept='计算机系' or dept='信息系'
     
select * from dept_table1()

```

###### 6.用户定义函数的删除

使用界面方式定义与修改用户自定义函数。

对于一个已创建的用户定义函数，可有2种方法删除：

1. 通过对象资源管理器删除，此方法非常简单。

2.  利用T-SQL语句`drop function`删除，语法格式：

   ​	drop function (架构名.) 函数名(n))

### 五、存储过程和触发器

- 存储过程和触发器都是SQL Server的数据库对象。
- 存储过程由一组预先编译好的SQL语句组成。存储过程的存在独立于表，存放在服务器上，供客户端调用。
- 触发器是一种特殊类型的存储过程，他不是由用户直接调用的，而是当用户对数据进行某种操作（包括数据的insert、update或delete操作）时自动执行。因此触发器的使用和表的更新操作紧密结合。
- 使用触发器可以大大提高数据库应用程序的灵活性和健壮性，可以利用触发器来实现复杂的业务规则，更有效地实施数据完整性。

##### 1.存储过程

- 可以接受**输入参数**、**返回表格**或**标量结果和消息**，调用“数据定义语言（**DDL**）”和“数据操作语言（**DML**）”语句，然后返回输出参数。（**DDL**：数据定义$$\longrightarrow$$create,alter,drop$$\longrightarrow$$结构上，**DML**：数据操纵$$\longrightarrow$$insert,update,delete）

###### 1.1存储过程的类型

$\begin{cases}(1)系统存储过程\rightarrow系统存储过程是由SQL\ Server提供的存储过程，可以作为命令执行。\\(2) 扩展存储过程\rightarrow扩展存储过程是指在SQL\ Server环境之外，使用编程语言（例如C\#语言）创建的外部例程形成的动态链接库（DLL）。\\(3)用户存储过程\rightarrow是用户根据需要，在自己的普通数据库中创建的存储过程，用户的存储过程可以使用T-SQL语言编写。\end{cases}$

###### 1.2存储过程的创建

- 基本语法：
  1. create proc/procedure 存储过程名
  2. @参数名 数据类型$\longrightarrow$定义参数的类型
  3. varying (=default) (out/out put)$\longrightarrow$定义参数的属性
  4. (with encryption)$\longrightarrow$对语句文本加密
  5. as{T-SQL语句执行的操作}$\longrightarrow$执行的操作
- 说明
  1. 不要以sp\_为前缀创建任何存储过程。sp_前缀是SQL Server用来命名系统存储过程的，使用这样的名称可能会与以后的某些系统存储过程发生冲突。
  2. varying：指定作为输出参数支持的结果集。该参数由存储过程动态构造，其内容可能发生改变，仅适用于定义cursor输出参数。
  3. default: 指定存储过程输入参数的默认值，默认值必须是常量或NULL。如果定义了默认值，执行存储过程时可以不提供实参。
  4. 如果希望其他用户无法查看存储过程的定义，则可以使用WITH ENCRYPTION子句创建存储过程。这样，存储过程定义将以不可读的形式存储。
  5. out 或 output：指定参数为输出参数类型，用于从存储过程返回结果。

###### 1.3存储过程的执行

- 通过`execute`或`exec`命令可以执行一个已定义的存储过程，exec是execute的简写。

- 语法格式：

  1. exec

  2. **@return_status =** 存储过程名

  3. @参数名=值/变量名 output/default (n)

  4. 设计简单的存储过程，不带任何参数的存储过程。

     - ```sql
       /*返回05880101号学生的成绩情况，该存储过程不使用任何参数。
       存储过程定义后，执行存储过程student_info：
       EXECUTE   student_info 或者Exec  student_info
       */
       create procedure student_info
       as
         select *
         from student
         where sno='05880101'
         /*执行存储过程student_info*/
         execute student_info
       ```

  5. 使用带输入参数的存储过程

     ```sql
     /*从学生选课数据库的三个表中查询某人指定课程的成绩和学分。（该存储过程接收与传递参数精确匹配的值）*/
     create proc student_info1 @sname char(20),@cname char(20)
     as
       select student.sno,sname,cname,grade,credit
       from student,course,sc
       where student.sno=sc.sno and course.cno=sc.cno
             and sname=@sname and cname=@cname
     /*执行存储过程student_info1*/        
     exec student_info1'周一','java'
     ```

  6. 使用带输入和输出（OUTPUT）参数的存储过程。

     ```sql
     /*创建一个存储过程do_action，根据条件处理相应数据，处理后输出相应的信息。*/
     create proc do_action @flag bit,@str char(8) output
     as
       if @flag=0
          begin
               update student
               set sname='qqq'
               where sno='05880105'
               set @str='修改成功'
             end
          else
              if @flag=1
                   begin
                       delete from student
                       where sno='05880105'
                       set @str='删除成功'
                   end
        /* 执行存储过程do_action，并查看结果*/
       declare @s_str char(8)
       exec do_action 0,@s_str output
       print @s_str 
     ```

  7. 使用带有通配符参数的存储过程，实现模糊查询

     ```sql
     /*从三个表的连接中返回指定学生、指定课程的成绩。若没有传递输入参数的值，姓名默认为“周%”，课程默认为“java”(该存储过程在参数中使用了模式匹配，如果没有提供参数，则使用预设的默认值。 )*/
     create proc st_info @sname varchar(20)='周%',@cname char(20)='java'   
     as
       begin
            select grade
            from student,course,sc
            where student.sno=sc.sno and course.cno=sc.cno
                  and sname like @sname and cname=@cname
     end  
     
     exec st_info '张%'
     ```

  8. 使用`output`游标参数的存储过程。`output`游标用于返回存储过程的局部游标。**游标类型只能用于定义输出参数**

     ```sql
     /*创建一个带有OUTPUT游标参数的存储过程，在student表中声明一个scroll类型的游标并打开。*/create proc p13(@stu_cur cursor varying output)
     as
       begin
            declare stu_cur1 cursor
            scroll
            for
            select * from student
            set @stu_cur=stu_cur1
            open @stu_cur
       end
     /*声明一个局部游标变量，执行上述存储过程，并将执行结果中返回的游标赋值给该局部游标变量，然后通过该局部游标变量读取student中最后一行记录。 */
     declare @my_cur cursor
     exec p13 @my_cur output
     fetch last from @my_cur
     close @my_cur
     deallocate @my_cur
     
     ```

###### 1.4存储过程的创建与执行

- 使用界面方式定义与修改存储过程。
- 对于一个已创建的存储过程，可有2种方法删除：
  1. 通过对象资源管理器删除，此方法非常简单，请读者自己练习。
  2. 利用T-SQL语句drop proc删除
  3. 语法格式：drop proc 存储过程名

##### 2.触发器

- 触发器是一类**特殊的存储过程**，与表的关系密切，用于**保护表中的数据**。
-  触发器是一个被指定关联到一个表的数据对象，**触发器是不需要调用**的，当对**一个表的特别事件出现**时，如对表执行插入、更新或删除操作时，**它就会被激活**。
-  触发器代码也是由T-SQL语句组成的，因此**用在存储过程中的语句也可以在触发器的定义中**。
- ![image-20211212170609021](image-20211212170609021.png)

###### 2.1触发器作用

-  触发器的主要作用是能实现由主键和外键所不能保证的、复杂的参照完整性和数据的一致性，除此之外，触发器还有其他许多不同的功能。
-  跟踪变化
  -  触发器可以侦测数据库内的操作，从而禁止了数据库未经许可的更新和变化，使数据库的修改、更新操作更安全，数据库运行更稳定。
-  可以强化数据条件约束
  - 触发器能够实现比`check`语句更为复杂的约束，更适合在大型数据库管理系统中用来约束数据的完整性。（**rollback**回滚）
- 级联和并行运行
  -  触发器可以侦测数据库内的操作，并自动地级联影响整个数据库的各项内容。例如，某个表的触发器中包含有对另外一个表的数据操作，如删除、更新、插入，而该操作又导致该表上的触发器被触发。
- 由此可见，触发器可以实现高级形式的业务规则、复杂行为限制和定制记录等功能。

2.触发器的类型

$触发器\begin{cases}DML触发器\\DDL触发器\end{cases}$

1. DML触发器。当数据库中发生数据操纵语言（DML）事件时将调用(激活)DML触发器。DML事件包括对表或视图的insert语句、update语句和delete语句，因而DML触发器可分为3种类型：insert、update和delete。
2. DDL触发器。DDL触发器是SQL Server 2005新增的功能，也是由相应的事件触发，但DDL触发器触发的事件是数据定义语句（DDL）。这些语句主要是以create、alter、drop等关键字开头的语句。

###### 2. 2	创建DML触发器(编程题只出DML)

语法格式：

1. create trigger (架构名.)触发器名
2. on 表名或视图名$\longrightarrow$指定操作对象
3. (with encryption)$\longrightarrow$说明是否采用加密方式
4. after|instead of
5. insert|update|delete
6. as
7. begin
8. <T-SQL语句>
9. end

说明：

- after：用于说明触发器在指定操作成功执行后触发，如`after Insert`表示向表中插入数据时激活触发器。
- Instead of：指定用DML触发器中的T-SQL语句代替触发语句。
- Insert|Update|Delete：指定激活触发器的语句类型，必须**至少指定一个**选项。在触发器定义中允许使用上述选项的任意顺序组合。
- 如果触发器表存在约束，则在after触发器执行之前检查这些约束。如果违反了约束，则不执行after触发器。

###### 2.2.1	对student表创建一个insert触发器

```sql
/*对student表创建一个insert触发器。*/
create trigger tg_insert
on student after insert
as
begin
print 'insert student...'
end
insert into student
values('05880209','刘同学','男',23,'计算机系')
```

###### 2.2.2	对SC表创建一个delete触发器

```sql
/*对SC表创建一个DELETE触发器。*/
create trigger tg_delete
on sc after delete
as
begin
print 'delete sc _'
end

delete from sc
where grade<70
```

###### 2.2.3	对course表创建一个update触发器

```sql
/*对course表创建一个update触发器*/
create trigger tg_delete
on sc after delete
as
begin
print 'delete sc _'
end

delete from sc
where grade<70
```

###### 2.3	创建DML触发器时通常使用的两个特殊表

在执行触发器时，系统创建了两个特殊的临时表inserted表和deleted表，这两个表的内容如下

- Inserted表：当向表中插入数据时，INSERT触发器触发执行，新的记录插入到触发器表和inserted表中
- Deleted表：用于保存已从表中删除的记录，当触发一个DELETED触发器时，被删除的记录存放到deleted表中。
- 修改一条记录等于插入一条新记录，同时删除旧记录。当对定义了`update`触发器的表记录修改时，表中原记录移到deleted表中，而修改过的新记录插入到inserted表
- 对**行**影响

**注意**：由于inserted表和deleted表都是临时表，它们在触发器执行时被创建，触发器执行完后就消失了，所以只可以在本触发器的语句中使用SELECT语句查询这两个表。

```sql
/*对student表创建一个UPDATE触发器，并输出某个学生修改前与修改后的年龄值。*/
create trigger tg_update
on student after update
as
begin
declare @name char(8),@oldage int,@newage int
select @name=sname,@oldage=age from deleted
select @newage=age from inserted
print @name
print @oldage
print @newage
end

update student
set age=20
where sno='05880102'
```

######  2.4	Update(column)函数

​    Update()函数返回一个布尔值，指示是否对表的指定列进行了insert或update操作。其中insert操作可是看做是把原来的null值修改成了新值。

​    例：在student表中创建update和delete触发器，当修改或删除student表中的学号时，同时修改或删除sc表中的该学号。

```sql
/*在course表中创建update和delete触发器，当修改或删除course表中的课程号时，同时修改或删除sc表中的该课程号。*/
create trigger tg_course
on course after update,delete
as 
  begin
  if update(cno) 
      update sc 
      set cno=(select cno from inserted)
      where cno=(select cno from deleted)
  else 
      delete from sc
      where cno=(select cno from deleted)
   end
Delete from course
Where cname='java‘

update course 
set cno='c1new'
where cno='c1'
```

###### 2.5	Instead of 触发器

- `After`触发器是在触发语句执行后触发的。与`after`触发器不同的是，`instead of` 触发器触发时只执行触发器内部的SQL语句，而不执行激活该触发器的SQL语句

-   在一个表或视图上，每个insert、update或delete语句最多可以定义一个instead of触发器。

  ```sql
  /*创建表table_2，属性只有一个a，整数类型，在表中创建instead of insert触发器，当向表中插入记录时显示相应消息。*/
  create trigger table_insert
  on table_2 instead of insert
  as
    begin
       print 'instead of trigger is working'
    end
  ```

###### 2.5.1	Instead of 触发器的主要作用

是用于不可更新的视图，支持更新。如果视图不可更新，则必须使用`instead of`触发器支持基表中数据的插入、更新和删除操作。

- 视图更新操作的限制

1. 如果视图是从多个基本表使用连接操作导出的，则不允许更新。

2. 如果定义视图的select语句包含group by、distinct、聚组函数（虚列），则不允许更新。

3.  如果建立视图时带with read only选项，则不能对该视图进行任何插入、更新和删除操作。

   ```sql
   /*创建一个基于多表的视图，通过instead of触发器来实现对视图的更新操作。*/
   create view stu_sc
   as
     select student.sno,dept,cno,grade
     from student,sc
     where student.sno=sc.sno
   
   insert into stu_sc
   values('05880909','英语系','c2',79)
   ```

###### 2.6	创建DDL触发器

语法格式：

1. create trigger 触发器名
2. on all server|database
3. (with encryption)
4. after event_group |event_type
5. as
6. begin
7. T-SQL语句
8. end

 DDL触发要用于：防止对数据库架构进行某些修改；希望数据库中发生某些变化以利于相应数据库架构中的更改；记录数据库架构中的更改或事件。

DDL触发器只在响应由T-SQL语法所指定的DDL事件时才会触发。

all server |database：`all server `是指当前DDL触发器的作用于当前服务器。database指DDL触发器作用于当前数据库。

event_type： on关键字后面为`database`，此选项包括create_table、alter_table、drop_table、create_view等。

event_group： on关键字后面为`all server`，此选项包括create_database、alter_database、drop_database 等。

###### 2.6.1	创建数据库作用域的DDL触发器

```sql
/*创建数据库作用域的DDL触发器，当删除一个表时，提示禁止该操作，然后回滚删除表的操作。*/
create trigger satety
on database after drop_table
as
  begin
       print '不能删除表'
       rollback
  end
```

###### 2.6.2	创建服务器作用域的DDL触发器

```sql
/*创建服务器作用域的DDL触发器，当删除一个数据库时，提示禁止该操作并回滚删除数据库的操作。	*/
create trigger satety_server
on all server after drop_database
as
  begin
       print '不能删除该数据库'
       rollback
  end
```

###### 2.6.3	删除触发器

- 使用界面方式可以定义、修改、查看和删除DML触发器，但是对于DDL触发器，则只能查看和删除。
- 触发器本身是存在表中的，因此，当表被删除时，表中的触发器也将被删除。删除触发器使用drop trigger语句。
- 语法格式：
  1. drop trigger 架构名.触发器名$\longrightarrow$删除DML触发器
  2. drop trigger 触发器名 on all server |database$\longrightarrow$ 删除DDL触发器

**注意：**

如果是删除DDL触发器，则要使用on关键字指定在数据库作域还是服务器作用域。
