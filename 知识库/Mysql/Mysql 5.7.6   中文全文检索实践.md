# Mysql 5.7.6   中文全文检索实践

## 参考链接
1. [mysql中文分词插件ngram相关博客文章](https://mysqlserverteam.com/innodb全文索引：n-gram-parser/)
2. [Innodb全文索引官方用户手册](https://dev.mysql.com/doc/refman/5.7/en/innodb-fulltext-index.html)
3. [mysql分词解析器ngram官方用户手册](https://dev.mysql.com/doc/refman/5.7/en/fulltext-search-ngram.html)
4. [MySQL中InnoDB全文检索](https://www.cnblogs.com/olinux/p/5169282.html)
   ​

Mysql 主要通过ngram插件实现中文检索。支持InnoDB与MyISAM存储引擎。

##   环境配置

当使用ngram作为全文索引的分词插件时，mysql的全文索引配置除以下四项无效外，其他的都依然有效；

### 无效配置

`innodb_ft_min_token_size, innodb_ft_max_token_size, ft_min_word_len, ft_max_word_len`

### ngram 通过 ngram\_token\_size 参数定义分词的长度

ngram\_token\_size 的取值范围为1～10

> ngram的 ngram\_token\_size 值默认为2，此时如“我爱中国”就被解析为“我爱”，“爱中”，“中国”三个词组；

ngram\_token\_size 参数在mysql运行起来以后就只作为一个只读参数，不可修改。 因此应该在mysql启动前通过配置文件指定或者在启动mysql时作为参数设置：

* 配置文件/path/my.cnf

```
[mysqld]
ngram_token_size=2
```

* 启动参数

```
mysqld --ngram_token_size=2
```

> 阿里云RDS修改参数需要到RDS控制台进行设置，修改完成后，重启即可生效

## 创建全文索引列

创建一个使用ngram解析分词的全文索引，需要在创建时指定使用的分词插件。

全文索引可在INFORMATION\_SCHEMA.INNODB\_FT\_INDEX\_CACHE中查看。

示例：

1. 创建table时指定

```Mysql
 CREATE TABLE articles (
      id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
      title VARCHAR(200),
      body TEXT,
      FULLTEXT (title,body) WITH PARSER ngram
    ) ENGINE=InnoDB CHARACTER SET utf8mb4;
```

2. 在已有table中添加

```Mysql
CREATE TABLE articles (
      id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
      title VARCHAR(200),
      body TEXT
     ) ENGINE=InnoDB CHARACTER SET utf8;
ALTER TABLE articles ADD FULLTEXT INDEX ft_index (title,body) WITH PARSER ngram;
# Or:
CREATE FULLTEXT INDEX ft_index ON articles (title,body) WITH PARSER ngram;
```

InnoDB存储引擎允许用户查看指定倒排索引的Auxiliary Table分词的信息，可以通过设置innodb_ft_aux_table来观察倒排索引的Auxiliary Table 下面的SQL 语句设置查看test架构下表articles的Auxiliary Table：

```mysql
SET GLOBAL innodb_ft_aux_table="test/articles";
```

设置后就可以在information_schema.INNODB_FT_INDEX_TABLE中得到表articles中的分词信息。
