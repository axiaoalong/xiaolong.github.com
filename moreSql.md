多表查询分类：
http://blog.csdn.net/jintao_ma/article/details/51260458
1、首先先列举本篇用到的分类(内连接，外连接，交叉连接)和连接方法(如下)：
A）内连接：join，inner join
B）外连接：left join，left outer join，right join，right outer join，union
C）交叉连接：cross join
1、内连接：inner join 或者join 
下面这个例子是错的，如果a表与b表中数据的个数对不上，应该增加一个条件：
a.id=b.id




2.2 外连接（六种场景）


2.2.1 left join 或者left outer join(等同于left join)


[java] view plain copy
  1. select a.*, b.* from tablea a  
  2. left join tableb b  
  3. on a.id = b.id  
或者
[java] view plain copy
  1. select a.*, b.* from tablea a  
  2. left outer join tableb b  
  3. on a.id = b.id  
结果如下，TableB中更不存在的记录填充Null:


2.2.2  [left   join 或者left outer join(等同于left join)]  +  [where B.column is null]
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. left join tableb b  
  3. on a.id = b.id  
  4. Where b.id is null  


2.2.3  right join 或者right outer join(等同于right join)
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. right join tableb b  
  3. on a.id = b.id  


2.2.4 [right   join 或者right outer join(等同于left join)]  +  [where A.column is null]
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. right join tableb b  
  3. on a.id = b.id  
  4. where a.id is null  


2.2.5 full join （mysql不支持，但是可以用 left join  union right join代替）
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. left join tableb b  
  3. on a.id = b.id  
  4. union  
  5. select a.id aid,a.age,b.id bid,b.name from tablea a  
  6. right join tableb b  
  7. on a.id = b.id  



2.2.6 full join + is null（mysql不支持，但是可以用 （left join + is null） union （right join+isnull代替）
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. left join tableb b  
  3. on a.id = b.id  
  4. where b.id is null  
  5. union  
  6. select a.id aid,a.age,b.id bid,b.name from tablea a  
  7. right join tableb b  
  8. on a.id = b.id  
  9. where a.id is null  


union表示连接上下两条查询数据，要求这两条数据的列数必须相同，并且每一列的数据类型必须相同
UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。
2.3 交叉连接 （cross join）
2.3.1 实际应用中还有这样一种情形，想得到A，B记录的排列组合，即笛卡儿积，这个就不好用集合和元素来表示了。需要用到cross join：
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. cross join tableb b  
2.3.2 还可以为cross  join指定条件 （where）：
  1. select a.id aid,a.age,b.id bid,b.name from tablea a  
  2. cross join tableb b  
  3. where a.id = b.id  
三 注意事项
上面仍然存在遗漏，那就是mysql对sql语句的容错问题，即在sql语句不完全符合书写建议的情况，mysql会允许这种情况，尽可能地解释它:
3.1 一般cross join后面加上where条件，但是用cross join+on也是被解释为cross join+where；
3.2 一般内连接都需要加上on限定条件，如上面场景2.1；如果不加会被解释为交叉连接；
3.3 如果连接表格使用的是逗号，会被解释为交叉连接；
注:sql标准中还有union join和natural  inner join，mysql不支持，而且本身也没有多大意义，其结果可以用上面的几种连接方式得到
总结：总结了mysql所有连接方法，其中有一些是之前没有注意到的问题，平时开发也都不外乎这些。
