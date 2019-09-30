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




### https://blog.csdn.net/xuanwugang/article/details/80955841