# MySQL默认隔离级别为什么是repeatable-read

#### 1. 兼容MySQL5.1版本及其之前，bin log 为 statement格式，带来的主从数据不一致问题；
#### 2. 解决RC所不能解决的[不可重复读]问题；

> 基于next key lock 算法实现的可重复读隔离级别，具备 基于 record lock 实现的 读已提交 读未提交 隔离级别所不具备的解决[不可重复读]的能力。
