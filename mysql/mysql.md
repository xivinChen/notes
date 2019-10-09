#  mysql 索引笔记

## 1. 前导模糊查询不能使用索引

## 2 union in or 都能命中索引，但建议使用in

## 3 联合索引遵循最左前缀原则

## 4 负向条件不能使用索引  ！=  <> not in not exists not like 等等

## 5 范围列可以用到索引（且索引只能是最左前缀）

## 6 计算放到业务处理

## 7 强制类型转换会导致全表扫描

## 8 更新频率频繁/ 数据区分度不高的字段上不建议建立索引   count(distinct(列名)/count(*)) 来计算

## 9 建立索引的列 不允许为null

## 10 


## explain 分析

```aidl

mysql> EXPLAIN SELECT `birday` FROM `user` WHERE `birthday` < "1990/2/2"; 
-- 结果： 
id: 1 

select_type: SIMPLE -- 查询类型（简单查询、联合查询、子查询） 

table: user -- 显示这一行的数据是关于哪张表的 。

type: range -- 区间索引（在小于1990/2/2区间的数据)，这是重要的列，显示连接使用了何种类型。从最好到最差的连接类型为system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL,const代表一次就命中，ALL代表扫描了全表才确定结果。一般来说，得保证查询至少达到range级别,最好能达到ref。 

possible_keys: birthday  -- 指出MySQL能使用哪个索引在该表中找到行。如果是空的，没有相关的索引。这时要提高性能，可通过检验WHERE子句，看是否引用某些字段，或者检查字段不是适合索引。  

key: birthday -- 实际使用到的索引。如果为NULL，则没有使用索引。如果为primary的话，表示使用了主键。 

key_len: 4 -- 最长的索引宽度。如果键是NULL，长度就是NULL。在不损失精确性的情况下，长度越短越好。

ref: const -- 显示哪个字段或常数与key一起被使用。  

rows: 1 -- 这个数表示mysql要遍历多少数据才能找到，在innodb上是不准确的。 

Extra: Using where; Using index -- 执行状态说明，这里可以看到的坏的例子是Using temporary和Using

```


## PROCEDURE analyse() 分析
``select * from user procedure analyse()``




### https://blog.csdn.net/xuanwugang/article/details/80955841