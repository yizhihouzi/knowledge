### Yaconf使用

Yaconf是鸟哥(laruence)写的一个高性能的配置管理扩展.

#### API

```
mixed Yaconf::get(string $name, mixed $default = NULL)
     这个是获取一个配置, 名字是配置的名字, 一般来说如果你有一个ini文件叫做foo.ini,
     那么$name使用foo的话就会获取到这个文件内的所有内容, 以数组形式返回. 
     default是当配置不存在的时候返回的默认值.
     
bool Yaconf::has(string $name)
     这个是检测一个配置是否存在.
```

#### Yaconf的配置项

```
yaconf.directory
配置文件目录, 这个配置不能通过ini_set指定, 因为必须在PHP启动的时候就确定好.

yaconf.check_delay
多久(秒)检测一次文件变动, 如果是0就是不检测, 也就是说如果是0的时候, 文件变更只能通过重启PHP重新加载
```

#### 示例

文件名称 test.ini

```
[parent]
name = "yaconf"
year = 2015
features[] = "fast"
features[] = "slow"
features.2 = "light"
features.plus = "zero-copy"
features.constant = PHP_VERSION

[children : parent]
year = 2016
```

获取配置

```
Yaconf::get("test.children.year");
#2016
Yaconf::get("test.children.features");
#array(5) {
#  [0]=>
#  string(4) "fast"
#  [1]=>
#  string(4) "slow"
#  [2]=>
#  string(5) "light"
#  ["plus"]=>
#  string(9) "zero-copy"
#  ["constant"]=>
#  string(6) "7.0.13"
#}
Yaconf::get("test.parent.features.0");
#fast
```

即Yaconf支持继承/Map/数组/下标定义配置

     
