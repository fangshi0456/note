# Index

优化：减少IO次数 

为什么索引选择自动递增 --> 减少索引的更改成本、、



OLTP 联机分析处理

OLDP 联机事务处理



show profile for sql 

show process list 

Explain select * from xx



索引字段要尽量小



Innodb 必须有主键 通过主键构建索引 索引和数据存在一起 

有其他字段再建立索引 是索引字段和主键id 如果信息较多 需要回主键索引二次查询



Myisam 存储的是索引加数据的地址



![image-20210104133317642](https://raw.githubusercontent.com/fangshi0456/pic/main/pic/db_point.png)





Sharding sphere 分库分表工具





ACID



原子性 undolog

一致性

隔离性  lock

持久性 redolog





innodb中  一条记录 有三个隐藏字段 隐式主键

b tree 是多路平衡树

![image-20210104175353589](https://raw.githubusercontent.com/fangshi0456/pic/main/pic/20210104175353.png)



b+ tree

![image-20210104175831695](/Users/brian/Library/Application Support/typora-user-images/image-20210104175831695.png)





myisam

![image-20210104184333765](https://raw.githubusercontent.com/fangshi0456/pic/main/pic/20210104184333.png)





Innodb

![image-20210104191537591](https://raw.githubusercontent.com/fangshi0456/pic/main/pic/20210104191537.png)



联合索引

![image-20210104193051133](https://raw.githubusercontent.com/fangshi0456/pic/main/pic/20210104193051.png)







RLU mysq 内存管理