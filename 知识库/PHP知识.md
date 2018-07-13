## PHP知识







### TIP

1)  PDO对象的lastInsertId始终返回0    (待完善)

lastInsertId函数底层依赖于mysql_insert_id函数,mysql_insert_id函数返回的是储存在有AUTO_INCREMENT约束的字段的值，如果表中的字段不使用AUTO_INCREMENT约束或者使用自己生成的唯一值插入，那么该函数不会返回你所存储的值，而是返回NULL或0。因此，在没有使用AUTO_INCREMENT约束的表中，或者ID是自己生成的唯一ID，lastInsertId函数返回的都是0。