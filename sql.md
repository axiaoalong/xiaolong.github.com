mysql 命令大全：http://www.cnblogs.com/zhangzhu/archive/2013/07/04/3172486.html
isqlplus:

下面：使用超级管理员登录上oracle
首先：
然后：
了解当前用户下面表的结构是什么样的:


sql语言：分类：

查询语句里面只有select语句。
命令：desc emp     了解emp表里内容：

empno: emp:雇员：雇员编号
ename:雇员名称                                                               varchar2<10>:可变字符串10位
job:工种
mgr:雇员相关经理
hiredate:雇员入职日期
sal:雇员薪水                                                                      number<7,2>:7位数字，2位是小数
comm:津贴多少
deptno:所属部门编号
   
desc  dept:部门表：

deptno::部门编号
dname:名称
loc:部门所在的，位置
  
desc salgrade:工薪级别：


select*把里面内容全取出来，from后面跟表名。
查看工薪级别
查看部门：


select语句中可以进行运算：

也可以进行纯粹简单运算：

其中简单运算可以用dual:     .....from dual    空表
 取当前系统时间：SYSDATE

给字段取别名：

字符连接：


distinct关键字：将重复的字段去掉，其中，可以将两个字段的组合重复的去掉。


where：查找。字符串类型用    ' 内容 '  。 不等号：<>      between:包含边界。
显示空值：is null。显示不是空值：is not null



日期处理：必须按照一定的格式：

模糊查询：关键字。like关键字

第二个字母为A：

自己制定转义字符：



排序： order by 默认升序。desc降序
升序，可以不写asc

降序：desc

首先按照deptno升序，在deptno相同情况下，按照名字降序排列。


select lower<ename>from emp;
显示字段小写。


从第二个字符开始截，截3个字符：


将acs2码转化为字符：


 
四舍五入：


to  char：吧数字或者日期转换为某种特殊形式：
$美元，L人民币



to char表示时间：

to date:

 
to number  将数字转换为字符串类型。


nvl: 如果nvl里面的数字为空值，用0替代。


组函数：avg平均数，min max sum和 count(*)显示表里面有多少条记录。


count：凡是不是空值的一共有几个。

有多少个单独的唯一的部门编号。


group by: 按照字段分组：


having：where语句是过滤单挑语句的，不能再组函数里面用。过滤分组函数，用having：






自连接：


 cross join 交叉连接：

join  。。on：后面跟连接条件

等值连接：join。。 using



 
左外连接：会把左面的多余的数据，不能和另外那个表里不能连接的数据拿出来。 



insert语句：  
insert into 表名 values。添加行。




创建一个名字叫emp2的表，内容和emp一模一样。



创建一张表：
create table 表名 后面跟字段及数据类型（varchar2   char定长字符串占用8个字符  number    date    long）
非空字段：constraint 名字 not null 非空约束。
唯一约束，避免重复：unique关键字。空值不算重复。
default 添加默认值。
表级约束:    

主键约束：
唯一且非空。primary key  也可以：,名字可以重复!!!所以不太适合做主键。

创建约束参考：
refenrences 参考表

先创建外键，在创建。。被参考的字段必须是主键。



修改表结构：
alter：
增加表结构：

修改：修改之后必须容纳原有数据

修改：约束条件：
删除约束条件：

增加约束条件：

索引创建：

创建视图：

PLSQL:
1.declare:变量声明
变量名用（v_名字）来声明。数据类型：
char varchar2
date

number

text
                                                
2.begin:程序开始执行
3.exception：抓到错误时执行的
4.end

java连接oracle：
1、
2、
3、

4、其中，rs是个结果集，用来取结果。代表一个游标。

总结：

完美版：

从eclipse里面查看数据库。

可以灵活指定变量的sql语句：





批处理：


遇到必须两条语句同时加入的情况：首先关闭自动提交。




