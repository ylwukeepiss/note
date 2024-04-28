# MySQL优化之group by 与 order by

### group by : 对于sql语句：select city ,count(*) as num from staff group by city; 使用了临时表（using tempoary）和排序（using file sort）
###  1. 创建内存表 temp -> city num；
###  2. 从主键索引树中取出所有的行，判断city（x）是否在temp中，在->所在行记录的nun值+1， 不在->插入一条记录 （x，1）
###  3. 遍历完成后，再根据city做排序，将结果返回

### 基于对原理的理解，当sql语句变成 select city, count(*) as num from staff where age > 15 group by city; 可以通过加索引 idx_age(age)去优化， 比上述流程多了走索引数idx_age 查询到 age > 30 的主键 id，再回表查这两个过程

### 当sql变成 select city, count(*) as num from staff group by city having num > 1; 跟基础流程，多了一个后置处理，将接过集过滤一遍，再返回

### 
