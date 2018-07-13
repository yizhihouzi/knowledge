Mysql join 相关测试及总结

##### 基础数据

tperson表:

```mysql
;建表
CREATE TABLE `torder` (
  `o_id` int(11) NOT NULL AUTO_INCREMENT,
  `what` varchar(45) NOT NULL,
  `p_id` varchar(45) NOT NULL,
  PRIMARY KEY (`o_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4;
;填充数据
INSERT INTO `tperson` (`pid`, `name`) VALUES (1, ' chenghao'),(2, 'zhuweijian'),(3, 'zhangwang');
```


| pid  | name       |
| ---- | ---------- |
| 1    | chenghao   |
| 2    | zhuweijian |
| 3    | zhangwang  |

torder表：

```mysql
;建表
CREATE TABLE `torder` (
  `o_id` int(11) NOT NULL AUTO_INCREMENT,
  `what` varchar(45) NOT NULL,
  `p_id` varchar(45) NOT NULL,
  PRIMARY KEY (`o_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4;
;填充数据
INSERT INTO `torder` (`o_id`, `what`, `p_id`) VALUES (1, '饮料', '1'),(2, '啤酒', '1'),(3, '花生', '2'),(4, '二葵', '4');
```

| oid  | what | pid  |
| ---- | ---- | ---- |
| 1    | 饮料   | 1    |
| 2    | 啤酒   | 1    |
| 3    | 花生   | 2    |
| 4    | 二葵   | 4    |

##### 测试

1.  `SELECT  *  FROM tperson LEFT JOIN torder ON 1`

  | pid  | name       | oid  | what | pid  |
  | ---- | ---------- | ---- | ---- | ---- |
  | 1    | chenghao   | 1    | 饮料   | 1    |
  | 2    | zhuweijian | 1    | 饮料   | 1    |
  | 3    | zhangwang  | 1    | 饮料   | 1    |
  | 1    | chenghao   | 2    | 啤酒   | 1    |
  | 2    | zhuweijian | 2    | 啤酒   | 1    |
  | 3    | zhangwang  | 2    | 啤酒   | 1    |
  | 1    | chenghao   | 3    | 花生   | 2    |
  | 2    | zhuweijian | 3    | 花生   | 2    |
  | 3    | zhangwang  | 3    | 花生   | 2    |
  | 1    | chenghao   | 4    | 二葵   | 4    |
  | 2    | zhuweijian | 4    | 二葵   | 4    |
  | 3    | zhangwang  | 4    | 二葵   | 4    |

>    这就是左连接的直接结果啦，也就是两个表格的笛卡尔积。
2.    `SELECT  *  FROM tperson LEFT JOIN torder ON tperson.pid = torder.pid;`

| pid  | name       | oid  | what | pid  |
| ---- | ---------- | ---- | ---- | ---- |
| 1    | chenghao   | 1    | 饮料   | 1    |
| 1    | chenghao   | 2    | 啤酒   | 1    |
| 2    | zhuweijian | 3    | 花生   | 2    |
| 3    | zhangwang  | null | null | null |

>   这就是左连接的标准用法啦。

3.  `SELECT  *  FROM tperson LEFT JOIN torder ON tperson.pid = torder.pid AND torder.pid=1;`

| pid  | name       | oid  | what | pid  |
| ---- | ---------- | ---- | ---- | ---- |
| 1    | chenghao   | 1    | 饮料   | 1    |
| 1    | chenghao   | 2    | 啤酒   | 1    |
| 2    | zhuweijian | null | null | null |
| 3    | zhangwang  | null | null | null |

>   注意这里我用的on condition1 and condition2。

4.  `SELECT  *  FROM tperson LEFT JOIN torder ON tperson.pid = torder.pid WHERE tperson.pid=1;`

| pid  | name     | oid  | what | pid  |
| ---- | -------- | ---- | ---- | ---- |
| 1    | chenghao | 1    | 饮料   | 1    |
| 1    | chenghao | 2    | 啤酒   | 1    |
> 注意这里我用的on condition1 where condition2。

##### 总结

1.  w3c上对左连接的结果的定义：即使右表中没有匹配，也从左表返回所有的行

```
在结果集中，左表中每行至少出现一次;
如果某行出现多个匹配行，则可能出现多次;
如果一个满足on condition（group）条件的匹配行也没有，那么就将右表中查询的行值设定为NULL（虽然此时就不满足测试2中tperson.pid = torder.pid的要求）。
```

2.  结合测试1与测试2，设想左连接实现的逻辑。

```
a)	做table1（左表）与table2的笛卡尔积;
b)	针对步骤a的结果，进行condition过滤;
c)	针对b的结果，检查出table1中所有的未存在于b结果集的行，并全部填充至b结果集，对应行中table2中的列值全部设定为NULL;
由此得出左连接最终结果。
```

3.  对比测试3与测试4的结果
```
测试3中：JOIN tableB ON conditionA AND conditionB
测试4中：JOIN tableB ON conditionA WHERE conditionB
测试3、4中都是条件A 、B，可是结果却有比较大的差距。
这里就是今天关注的重点。连接条件（ON 后面的条件）的执行时机是针对连接笛卡尔积结果进行过滤筛选，同时需要满足连接特性（如左连接与右连接具有不同的约束）约束。而查询条件（WHERE 条件）是针对经过连接条件筛选的链接结果的筛选，没有其余约束。这就是为什么测试三中的第3、4行在测试4中不存在的原因。
SELECT  *  FROM tperson LEFT JOIN torder ON tperson.pid = torder.pid WHERE tperson.pid=1;
等效于
SELECT  *  FROM tperson LEFT JOIN torder ON tperson.pid = torder.pid AND torder.pid=1 WHERE torder.pid IS NOT NULL
```