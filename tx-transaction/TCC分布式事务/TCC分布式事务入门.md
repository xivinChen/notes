tcc （try-comfirm-cancal) 的实现方案有：tcc-transaction,byteTcc,spring-cloud-rest-transaction

tcc和xa 的区别
   tcc: 保证最终一致性，它先预定资源，再决定确认还是提交，保证了长事务。
   
   xa :保证强一致性，会一直占用锁，它是先 prepare,再commit 遇到问题则rollback
   
   
全局事务：强一致性
本地消息表（异步确保） 最终一致性
TCC 最终一致性（2PC）


