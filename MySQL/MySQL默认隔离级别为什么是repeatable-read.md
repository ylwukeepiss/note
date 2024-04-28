#MySQL默认隔离级别为什么是repeatable-read
###先说结论，基于next key lock 算法实现的可重复读隔离级别，具备 基于 record lock 实现的 读已提交 读未提交 隔离级别所不具备的解决幻读的能力。

###record lock 锁住的仅是某一行的主键，而next key lock先通过record lock 锁住主键，再通过gap lock 锁住一定范围的行记录

###首先明确一个概念，事务的隔离级别越高，事务所占用的锁越多，范围越大，且持有锁的时间越长，因此选择事务隔离级别，通常都要在事务并发性能跟安全之间做一个衡量与选择

###read-commit 跟 read-uncommit会出现幻读问题，而repeatable-read在next-key-lock算法的保护下，很好地解决了幻读的问题。

###在这点上，跟oracle的区别就是，oracle要将隔离级别设置为serializable才能保证不出现幻读

###nextKeyLock算法，针对索引列，采用的是record lock，对于非索引列，锁住的是一个范围，区间为闭合，其他事务若对范围内的值做修改，将会被阻塞

###这也是mysql默认隔离级别不选择 readcommit与read uncommit的原因，这两种隔离级别的锁都是record类型，会出现幻读

###举个例子，table(id, column) data = {(1, 1), (2, 2), (3, 3), (6, 6), (8, 8)}
####select * from table where column = 3,  next key lock 会对 id = 3 进行record锁，对于非主键索引 column，会锁住一个范围 [2, 6] ，也就是说，对于辅助索引，从该索引的前一个值2，到后一个值6，这个闭合区间，都会被锁住

####对于select * from table where column > 3, 锁住的则是id = 3 record锁，以及[3, +∞）的一个区间