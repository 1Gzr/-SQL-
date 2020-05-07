





SQL操作大全及命令解读

本人采用oracle数据库进行练习

# 1.数据库基本概念

## 1.1数据库定义：

​		电子化的文件柜 ，一个有趣的比喻，将数据库看成了一个存放数据的物理位置，不管数据是什么，也不管数据是如何组织的。数据库是“按照数据结构来组织、存储和管理数据的仓库”。是一个长期存储在计算机内的、有组织的、可共享的、统一管理的大量数据的集合。

​		万物皆数据，无论是文本，还是图片，视频，音乐，这些都是数据，而数据库是一个按数据结构来存储和管理数据的计算机软件系统。数据库的概念实际包括两层意思：

（1）数据库是一个实体，它是能够合理保管数据的“仓库”，用户在该“仓库”中存放要管理的事务数据，“数据”和“库”两个概念结合成为数据库。

（2）数据库是数据管理的新方法和技术，它能更合适的组织数据、更方便的维护数据、更严密的控制数据和更有效的利用数据。

## 1.2一些术语

数据库：database，保存有组织的容器。

数据库管理系统：DBMS。数据库是通过DBMS创建和操纵的容器。

表：table。表是一种结构化的文件，可以用来存储某种特定类型的数据。

列：column

行：row

主键：primary key

SQL:结构化查询语言(Structured Query Language),一种专门和数据库进行交互的语言。

# 2.检索数据（Select）

以emp表为例：

## 查询所有，*代表通配符

select * from emp

![image-20200430095840859](sql操作.assets/image-20200430095840859.png)

## 单列查询

​	select  empno from emp

## 多列查询

​	select rownum ,empno,ename from emp

​	select empno,ename from emp



## distinct 指引查出不同的数据，

​		select distinct job from emp

![image-20200430100305913](sql操作.assets/image-20200430100305913.png)

## 查出前5行

​	select  *  from emp where rownum <=5

![image-20200430100348667](sql操作.assets/image-20200430100348667.png)

注意：当使用rownum应该要有明确的范围，例如

​	select * from emp where rownum>5返回为空

应该这样写

​	select * from  (select  e.* ,rownum rn from emp e ) where rn>5

​	![image-20200430101737179](sql操作.assets/image-20200430101737179.png)

## 查出第5到10行数据

​	select * from  (select  e.* ,rownum rn from emp e ) where rn>5 and rn<=10

其中采用了别名方式的写法，之所以用rownum rn，是把内部rownum转成实例，因为rownum本身只能用 <=的比较方式，容只有转成实列，才可做 >=的比较。

结果如下：

![image-20200430101543177](sql操作.assets/image-20200430101543177.png)



# 3.排序检索数据

order by 字段  对表进行升序排序，当字段为空时，排到后面，数字按大小，字母按升序排

order by 字段  desc 降序排列

## 单列排序：

  select * from emp order by MGR

![image-20200430103019219](sql操作.assets/image-20200430103019219.png)





select * from emp  order by job

![image-20200430103300628](sql操作.assets/image-20200430103300628.png)

## 按多个列排序

​	select * from emp order by ename,job

先按ename排相同之后按job排

![image-20200430103957332](sql操作.assets/image-20200430103957332.png)

还可以按列号排但在实际场合下不实用

​	 select  * order by 1,2

## 降序排列

​	select * from emp order by empno desc

![image-20200430104247017](sql操作.assets/image-20200430104247017.png)

## 多列组合

​	select * from emp order by deptno desc,job

  按deptno降序，job升序进行排列

![image-20200430104442957](sql操作.assets/image-20200430104442957.png)

# 4.过滤数据

##  使用where 过滤

   select * from emp where empno=7369

​	select * from emp where deptno=30 order by job

当where和order by共同使用时，应该让where在前

## 范围检查

select * from emp where sal<= 1500

## 不匹配检查 

检查所有job不是CLERK的

select * from emp where job<>'CLERK'

select * from emp where job !='CLERK'

![image-20200430105016226](sql操作.assets/image-20200430105016226.png)

## 空值检查

​	select * from emp where comm is null

![image-20200430105050574](sql操作.assets/image-20200430105050574.png)



# 5.高级数据过滤

##   and ，or ，not ，in 使用

and:	

​	select * from emp where job='CLERK' and deptno=30

![image-20200430105420574](sql操作.assets/image-20200430105420574.png)

or:

 	select * from emp where job='CLERK' or job='SALESMAN'

![image-20200430105621720](sql操作.assets/image-20200430105621720.png)

in:指定条件范围

​	select * from emp where JOB in('CLERK','ANALYST')

![image-20200430105942065](sql操作.assets/image-20200430105942065.png)

与or差不多



NOT:

​	select * from emp where  NOT job = 'CLERK'

![image-20200430110130377](sql操作.assets/image-20200430110130377.png)



## 优先级考虑

​		优先级：and>or，当组合在一起时，要加（）来明确分组

​		select * from emp where sal>2500 or (job='CLERK' AND sal <=1500 )

![image-20200430111644688](sql操作.assets/image-20200430111644688.png)

# 6.通配符过滤

%，_ , [ ] , 

## %通配符

​	 select * from emp where job like 'C%'

找到所有JOB以C开头

![image-20200430111904914](sql操作.assets/image-20200430111904914.png)

​	select * from emp where job like '%E%'

找到所有job中含E的字符

![image-20200430111941529](sql操作.assets/image-20200430111941529.png)

select * from emp where ENAME like 'A%S'

找到ename中以A开头以E结尾

![image-20200430112102283](sql操作.assets/image-20200430112102283.png)



## _匹配符

​	select * from emp where MGR like '_8__'

找到mgr中第二个是8的

![image-20200430112433191](sql操作.assets/image-20200430112433191.png)

注意: _只能是单个匹配，

## []通配符

 Oracle不支持

# 7.创建计算字段

​	select ename||'('||job||')' from emp

![image-20200430171959923](sql操作.assets/image-20200430171959923.png)

# 8.使用函数处理数据

# 9.汇总数据

# 10.分组数据

group by ，having



​	select job,count(*) as jobpersons from emp group by job

 按job进行分组，并统计人数

![image-20200430173228353](sql操作.assets/image-20200430173228353.png)

having：过滤分组，where过滤行

​	select job,count(*) as jobpersons from emp group by job having count(*)>2

![image-20200430173520659](sql操作.assets/image-20200430173520659.png)

# 11.使用子查询



# 18.使用视图

​		视图(view)，也称虚表, 不占用物理空间，这个也是相对概念，因为视图本身的定义语句还是要存储在数据字典里的。视图只有逻辑定义。每次使用的时候,只是重新执行SQL。

## **视图的作用**

​	1)提供各种数据表现形式, 可以使用各种不同的方式将基表的数据展现在用户面前, 以便符合用户的使用习惯(主要手段: 使用别名)；

​	2)隐藏数据的逻辑复杂性并简化查询语句, 多表查询语句一般是比较复杂的, 而且用户需要了解表之间的关系, 否则容易写错; 如果基于这样的查询语句创建一个视图, 用户就可以直接对这个视图进行"简单查询"而获得结果. 这样就隐藏了数据的复杂性并简化了查询语句.这也是oracle提供各种"数据字典视图"的原因之一,all_constraints就是一个含有2个子查询并连接了9个表的视图(在catalog.sql中定义)；

​	3)执行某些必须使用视图的查询. 某些查询必须借助视图的帮助才能完成. 比如, 有些查询需要连接一个分组统计后的表和另一表, 这时就可以先基于分组统计的结果创建一个视图, 然后在查询中连接这个视图和另一个表就可以了；

​	4)提供某些安全性保证. 视图提供了一种可以控制的方式, 即可以让不同的用户看见不同的列, 而不允许访问那些敏感的列, 这样就可以保证敏感数据不被用户看见；

​	5)简化用户权限的管理. 可以将视图的权限授予用户, 而不必将基表中某些列的权限授予用户, 这样就简化了用户权限的定义。



## 视图使用

## 创建视图

![image-20200501172349360](sql操作.assets/image-20200501172349360.png)



使用视图

select * from rn5_10

![image-20200501173012435](sql操作.assets/image-20200501173012435.png)

## 更新视图

/*更新视图*/
	CREATE OR REPLACE VIEW  rn5_10 AS
	select * from  (select  e.* ,rownum rn from emp e ) where rn>5 and rn<=15;

![image-20200501173752426](sql操作.assets/image-20200501173752426.png)





# 19.使用存储过程

 存储过程是为以后使用而保存的一条或多条sql语句，可将其视为批文件，虽然它们的作用不仅限于批处理。

## 使用原因：

​	1）允许模块化程序设计，就是说只需要创建一次过程，以后在程序中就可以调用该过程任意次。

   2）允许更快执行，如果某操作需要执行大量SQL语句或重复执行，存储过程比SQL语句执行的要快。

   3）减少网络流量，例如一个需要数百行的SQL代码的操作有一条执行语句完成，不需要在网络中发送数百行代码。

   4）更好的安全机制，对于没有权限执行存储过程的用户，也可授权他们执行存储过程。 

## 19.1PLSQL入门

### **plsql输出helloworld**

-- Created on 2020/5/1 by THINK 
declare
  -- Local variables here
  i integer;
begin
  -- Test statements here
  Dbms_Output.put_line('helloworld');
end;



### PLSQL变量类型

分为2种：

1.普通变量（char ，number，varchar2，date，boolean，long）

2.特殊变量：引用型，记录型

声明方式

变量名  变量类型   如： ListCount  integer

#### 普通变量

**PLSQL变量赋值：**

1. ：=    
2. 语句赋值  select  into

 --打印个人信息：姓名，薪水，地址

```plsql
 --打印个人信息：姓名，薪水，地址

declare
  -- Local variables here
  v_name varchar2(30):='张三';
  v_sal  number;
  v_add varchar2(50);
begin
  --直接赋值
  v_sal:=123;
  
  --语句赋值
  select '上海' into v_add from dual;  


  Dbms_Output.put_line('姓名'||v_name||'薪水'||v_sal||'v_add'||v_add);
end;

```



#### **引用型变量：**

​       变量类型和长度取决于表中字段的类型和长度

​       声明方式：

​     		 变量名  表名.列名%TYPE    例如：v_name  emp.ename%type

![image-20200501214738930](sql操作.assets/image-20200501214738930.png)

![image-20200502085940144](sql操作.assets/image-20200502085940144.png)



引用型好处

![image-20200502090351058](sql操作.assets/image-20200502090351058.png)



#### 记录型变量：



![image-20200502090533432](sql操作.assets/image-20200502090533432.png)

![image-20200502091917732](sql操作.assets/image-20200502091917732.png)

### 流程控制;

#### 条件分支if

![image-20200502092135287](sql操作.assets/image-20200502092135287.png)



**注意关键字：ELSIF**

示例：测试emp表中数据

```plsql

declare
  -- Local variables here
  v_num number;

begin

  SELECT count(1) into v_num from emp;

  if v_num >= 20 then
    Dbms_Output.put_line('emp>=20');
  elsif v_num > 10 then
    Dbms_Output.put_line('emp>10');
  else
    Dbms_Output.put_line('emp<10');
  end if;

end;
p
```



#### 循环

 ORACLE中有三种循环：FOR，while，loop

![image-20200502093051655](sql操作.assets/image-20200502093051655.png)



示例：打印输出1~10

```plsql

declare

  v_num number := 1;

begin

  loop
  
    exit when v_num > 10;
    dbms_output.put_line(v_num);
  
    v_num := v_num + 1;
  
  end loop;

end;

```





## 20游标

### 游标的概念: 

​        游标是SQL的一个内存工作区，由系统或用户以变量的形式定义。游标的作用就是用于临时存储从数据库中提取的数据块。在某些情况下，需要把数据从存放在磁盘的表中调到计算机内存中进行处理，最后将处理结果显示出来或最终写回数据库。这样数据处理的速度才会提高，否则频繁的磁盘数据交换会降低效率。 
​        游标有两种类型：**显式游标**和**隐式游标**。在前述程序中用到的SELECT...INTO...查询语句，==一次只能从数据库中提取一行数据==，对于这种形式的查询和DML操作，系统都会使用一个隐式游标==。但是如果要提取多行数据，就要由程序员定义一个显式游标==，并通过与游标有关的语句进行处理。==显式游标对应一个返回结果为多行多列的SELECT语句==

![image-20200502093716711](sql操作.assets/image-20200502093716711.png)



###  游标语法



![image-20200502093811941](sql操作.assets/image-20200502093811941.png)

### 游标属性



![image-20200502093917752](sql操作.assets/image-20200502093917752.png)



### 游标使用

```plsql
--使用游标查询 emp中所有的ename，sal
declare

  --声明游标
  cursor c_emp is
    select ename, sal from emp;
  --声明变量接收游标
  v_name emp.ename%type;
  v_sal  emp.sal%type;

begin

  --打开游标
  open c_emp;

  --遍历游标
  loop
  
    exit when c_emp%notfound;
  
    --获取游标中的数据
    fetch c_emp
      into v_name, v_sal;
  
    dbms_output.put_line(v_name || '-' || v_sal);
  
  end loop;

  --关闭游标
  close c_emp;

end;

```

结果：

![image-20200502095114127](sql操作.assets/image-20200502095114127.png)

### 带参游标示例

使用游标查询 emp中某部门的ename，sal，部门编号为手动输入

```plsql
--使用游标查询 emp中某部门的ename，sal，部门编号为手动输入
declare

  --声明游标
  cursor c_emp(v_deptno emp.deptno%type) is
    select ename, sal from emp where deptno = v_deptno;
  --声明变量接收游标
  v_name emp.ename%type;
  v_sal  emp.sal%type;

begin

  --打开游标
  open c_emp(10);

  --遍历游标
  loop
  
    --获取游标中的数据
    fetch c_emp
      into v_name, v_sal;
  
    exit when c_emp%notfound;
  
  
    dbms_output.put_line(v_name || '-' || v_sal);
  
  end loop;

  --关闭游标
  close c_emp;

end;

```



注意：%notfound：默认false

## 21.存储过程

### 定义：

​		存储过程是事先经过编译并存储在数据库中的一段SQL语句的集合，调用存储过程可以简化应用开发人员的很多工作，
​		减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

**优点：**

- 允许模块化程序设计，就是说只需要创建一次过程，以后在程序中就可以调用该过程任意次。
- 允许更快执行，如果某操作需要执行大量SQL语句或重复执行，存储过程比SQL语句执行的要快。
- 减少网络流量，例如一个需要数百行的SQL代码的操作有一条执行语句完成，不需要在网络中发送数百行代码。
- 更好的安全机制，对于没有权限执行存储过程的用户，也可授权他们执行存储过程。 

### 语法：

![image-20200502100208825](sql操作.assets/image-20200502100208825.png)



### 无参存储：

```plsql
create or replace procedure P_hello is
--声明变量
begin
  dbms_output.put_line('hello');
end P_hello;
```

```plsql

declare

 --调用存储
begin

 p_hello;
end;

```

![image-20200502101103355](sql操作.assets/image-20200502101103355.png)



### 带参数的存储过程：

![image-20200502101638137](sql操作.assets/image-20200502101638137.png)

****

****

![image-20200502101717532](sql操作.assets/image-20200502101717532.png)

![image-20200502102010859](sql操作.assets/image-20200502102010859.png)