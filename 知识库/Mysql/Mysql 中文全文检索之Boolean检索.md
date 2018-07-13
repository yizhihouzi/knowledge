## Mysql 中文全文检索之Boolean检索

>   背景：使用ngram parser 作为索引创建插件，innodb作为存储引擎。ngram_token_size值设定为2。

### 特征

1.  BOOLEAN检索的结果`不会`按照相关性递减的顺序自动排序；

2.  停止词列表通过以下表格控制：

    ```
    innodb_ft_enable_stopword
    innodb_ft_server_stopword_table
    and innodb_ft_user_stopword_table
    ```

3.  不要再一个关键词前使用多个操作符，否则会输出语法错误。如：`"+*"`,`"++"`,`"+-"`都会导致语法错误；

4.  只能支持`"+"`或`"-"`前置，不支持后置，否则会引起语法错误。如：支持`'+apple'` 而不支持`'apple+'`；

5.  不支持@操作符；

6.  不受50%阈值限制，即超过50%匹配，仍会返回结果；

### 支持的操作符

1.  `"+"` 操作符，表示关键词一定存在；

2.  `"-"`操作符，表示关键词一定不存在。需要注意的是，它只是对其它关键词匹配结果的排除。一个只包含`"-"`匹配的bool检索，只会返回一个空结果；即它的含义不是”所有不含某个关键词的行“，而是”由其它匹配项匹配的结果中不包含指定项的结果“；

3.  空操作符，表示包含该关键词的行，相关性更高；

4.  `"@distance"`操作符用于匹配两个或多个关键词之间的距离。示例：`MATCH(col1) AGAINST('"word1 word2 word3" @8' IN BOOLEAN MODE)`;

5.  `">"`操作符，增大关键词的相关性；

6.  `"<"`操作符，减小关键词的相关性；

7.  `"()"`操作符，将关键词进行分组，支持括号操作符嵌套。示例：`+(>turnover <strudel)`；

8.  `"~"`操作符，降低关键词的相关性，但并不排除匹配行（区别于`"-"`）；

9.  `"*"`操作符，称为通配符，即可以实现关键词的部分匹配。示例：`apple* 可以匹配到 “apple”, “apples”, “applesauce”, or “applet”`;

    >   通配符所在关键词长度若小于ngram_token_size，则匹配以关键词开始的短语；若所在关键词长度大于ngram_token_size,则通配符被忽略。如：`a*`匹配所有以`a`开始的短语，而`abc*`被转换为`ab bc`;

10.  `"""`操作符，双引号操作符表示短语匹配，但非字字符不影响短语匹配。示例：`"test phrase"`  匹配 `"test, phrase"`，但不匹配`"test the phrase"`。

    >    `"abc def"`被转换为`"ab bc de ef"`,若一个文档中包含`"abc def"` 或`"ab bc de ef"`则被返回，但`"abcdef"`不被返回。

### 搜索示例及解读

1.  'apple banana'

    至少匹配apple或banana中的一个；

2.  '+apple +juice'

    既包含apple有包含juice的行；

3.  '+apple macintosh'

    apple必须被包含，macintosh可增加相关性，但不是必须存在；

4.  '+apple -macintosh'

    必须包含apple，但macintosh必须不包含；



4.  '+apple ~macintosh'

    必须包含apple，如果同时包含macintosh，则降低它的相关性；

5.  '+apple +(>turnover <strudel)'

    必须包含apple、turnover或apple、strudel，但是apple、turnover组合相关性更高；

6.  'apple*'

    查找以apple开头的短语。如：“apple”, “apples”, “applesauce”, “applet”；

7.  '"some words"'

    匹配存在`"some words"`的行，如包含`"some words of wisdom"`的行被匹配，但`"some noise words"`不被匹配。

###  InnoDB布尔模式搜索的相关性排名

InnoDB的全文搜索是仿造Sphinx实现的，它的相关性排序算法基于`BM25`、`TF-IDF`。因此，InnoDB的布尔搜索结果相关性与MyISAM存在不同。

InnoDB的相关性算法更近似于`"term frequency-inverse document frequency"(`TF-IDF`)`,它的主要特点是：

关键词在同一篇文档中出现的频率越高，同时在所有文档中出现的频率越低，那么它的相关性越高。

#### 相关性计算公式

`${rank} = ${TF} * ${IDF} * ${IDF}`

其中：

1.  `${TF} `即关键词在一篇文章中出现的次数;

2.  `${IDF}`称为逆文档频率，它的计算方法为：

    `${IDF} = log10( ${total_records} / ${matching_records} )`

    如果一篇文章中关键词出现`${TF}`次，这这篇文章的逆文档频率为：`${TF} * ${IDF}`

对于一次检索包含多个关键词的，则它的相关行为各关键词相关性的简单相加。

