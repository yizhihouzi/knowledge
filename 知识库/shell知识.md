## Shell知识

1.  Shell 编程跟java、php编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。常称的shell脚本是指bash脚本，原因是大多数Linux系统默认就包含bash脚本的执行环境，所以使用广泛。

2.  shell脚本第一行

```
#!/bin/bash
echo "Hello World !"
```

"#!" 是一个约定的标记，在通过./bash.sh执行脚本时它告诉系统这个脚本需要什么解释器来执行，即使用哪一种Shell。而如果通过/bin/bash  /path/to/bash.sh执行脚本，那么第一行将会作为注释被忽略。

3.  shell变量名

* 变量名不加美元符号$
* 首个字符必须为字母（a-z，A-Z）
* 中间不能有空格，可以使用下划线（_）
* 不能使用标点符号
* 不能使用bash里的关键字（可用help命令查看保留关键字）

4.  变量使用

使用一个定义过的变量，只要在变量名前面加美元符号即可。

```bash
your_name="qinjx"
echo $your_name
echo ${your_name}
for skill in Ada Coffe Action Java; 
do
    echo "I am good at ${skill}Script"
done
```

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界。如果不给skill变量加花括号，写成`echo "I am good at $skillScript"`，解释器就会把`$skillScript`当成一个变量（其值为空），代码执行结果就不是我们期望的样子了.

```bash
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```

变量在第二次赋值时也不用加`$`，使用变量的时候才加美元符`$`

5.   只读变量

```bash
#!/bin/bash
myUrl="http://www.w3cschool.cc"
readonly myUrl
myUrl="http://www.runoob.com"
#执行结果
#/bin/sh: NAME: This variable is read only.
```

6.  删除变量

```bash
unset variable_name
```

变量被删除后不能再次使用。unset 命令不能删除只读变量。

7. shell脚本的变量类型

* 局部变量 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
* 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
* shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行。

8. shell字符串

* 单双引号

字符串可以用单引号，也可以用双引号，也可以不用引号。单双引号的区别跟PHP类似。

> 单引号字符串的限制
    单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
    单引号字串中不能出现单引号（对单引号使用转义符后也不行）
> 双引号的优点
    双引号里可以有变量
    双引号里可以出现转义字符

```bash
your_name='qinjx'
str="Hello, I know you are \"$your_name\"! \n"
```

* 拼接字符串

```bash
your_name="qinjx"
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting $greeting_1
```

* 获取字符串长度

```bash
string="abcd"
echo ${#string} 
#输出 4
```

* 提取子字符串

以下实例从字符串第 2 个字符开始截取 4 个字符:

```bash
string="runoob is a great site"
echo ${string:1:4} 
#输出 unoo
```

* 查找子字符串

查找字符 "i 或 o" 的位置：

```bash
string="runoob is a great company"
echo `expr index "$string" oi` 
# 输出 4
```

以上脚本中 "`" 是反引号，而不是单引号 "'"，不要看错了哦。

9.  shell数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。类似与C语言，数组元素的下标由0开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于0。

* 定义数组

```bash
#1. 用括号来表示数组，数组元素用"空格"符号分割开。
array_name=(value0 value1 value2 value3)
#2. 
array_name=(
  value0
  value1
  value2
  value3
  )
#3. 可以不使用连续的下标，而且下标的范围没有限制。
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

* 读取数组

```bash
#一般格式
valuen=${array_name[n]}
#使用@ 或 *符号可以获取数组中的所有元素
echo ${array_name[@]}
echo ${array_name[*]}
```

* 获取数组的长度

```bash
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

10.  shell传递参数

在执行shell脚本时，可以向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数...

* 以下是shell脚本中的几个特殊参数：

```
$#	传递到脚本的参数个数
$*	以一个单字符串显示所有向脚本传递的参数。如"$*"用"括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
```

11.  shell运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如awk和expr,expr最常用。expr是一款表达式计算工具，使用它能完成表达式的求值操作。

* 算术运算符

```
+	加法	`expr $a + $b`
-	减法	`expr $a - $b`
*	乘法	`expr $a \* $b`
/	除法	`expr $b / $a`
%	取余	`expr $b % $a`
=	赋值	a=$b (将把变量b的值赋给a)
==	相等。用于比较两个数字，相同则返回 true。
!=	不相等。用于比较两个数字，不相同则返回 true。
```

条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。

```bash
#!/bin/bash
a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
#1、乘号"*"前边必须加反斜杠"\"才能实现乘法运算；
#2、在MAC中shell的expr语法是:$((表达式)),此处表达式中的"*"不需要转义符号"\"。
```

* 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

```
-eq	检测两个数是否相等，相等返回 true。
-ne	检测两个数是否相等，不相等返回 true。
-gt	检测左边的数是否大于右边的，如果是，则返回 true。
-lt	检测左边的数是否小于右边的，如果是，则返回 true。
-ge	检测左边的数是否大于等于右边的，如果是，则返回 true。
-le	检测左边的数是否小于等于右边的，如果是，则返回 true。
```

```bash
#!/bin/bash
a=10
b=20

if [ $a -eq $b ]
then
   echo "$a -eq $b : a 等于 b"
else
   echo "$a -eq $b: a 不等于 b"
fi
if [ $a -ne $b ]
then
   echo "$a -ne $b: a 不等于 b"
else
   echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi
```

* 布尔运算符

```
!	非运算，表达式为 true 则返回 false，否则返回 true。
-o	或运算，有一个表达式为 true 则返回 true。
-a	与运算，两个表达式都为 true 才返回 true。
```

```
#!/bin/bash
a=10
b=20

if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a -lt 100 -a $b -gt 15 : 返回 true"
else
   echo "$a -lt 100 -a $b -gt 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a -lt 100 -o $b -gt 100 : 返回 true"
else
   echo "$a -lt 100 -o $b -gt 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a -lt 100 -o $b -gt 100 : 返回 true"
else
   echo "$a -lt 100 -o $b -gt 100 : 返回 false"
fi
```

* 逻辑运算符

```
&&	逻辑  AND
||	逻辑  OR
```

```bash
#!/bin/bash
a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi
```

* 字符串运算符

```
=	检测两个字符串是否相等，相等返回 true。
!=	检测两个字符串是否相等，不相等返回 true。
-z	检测字符串长度是否为0，为0返回 true。
-n	检测字符串长度是否为0，不为0返回 true。
str	检测字符串是否为空，不为空返回 true。
```

```bash
#!/bin/bash
a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a 等于 b"
else
   echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
if [ -n $a ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi
```

* 文件测试运算符

```
-b file	检测文件是否是块设备文件，如果是，则返回 true。
-c file	检测文件是否是字符设备文件，如果是，则返回 true。
-d file	检测文件是否是目录，如果是，则返回 true。
-f file	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。
-g file	检测文件是否设置了 SGID 位，如果是，则返回 true。
-k file	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。
-p file	检测文件是否是有名管道，如果是，则返回 true。
-u file	检测文件是否设置了 SUID 位，如果是，则返回 true。
-r file	检测文件是否可读，如果是，则返回 true。
-w file	检测文件是否可写，如果是，则返回 true。
-x file	检测文件是否可执行，如果是，则返回 true。
-s file	检测文件是否为空（文件大小是否大于0），不为空返回 true。
-e file	检测文件（包括目录）是否存在，如果是，则返回 true。
```

```bash
#!/bin/bash
file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

12.  printf命令

printf 命令模仿 C 程序库（library）里的 printf()程序。标准所定义，因此使用printf的脚本比使用echo移植性好。printf 使用引用文本或空格分隔的参数，外面可以在printf中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认printf不会像 echo 自动添加换行符，我们可以手动添加 \n。

```bash
#!/bin/bash

printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 

#执行脚本，输出结果如下所示：
#姓名     性别   体重kg
#郭靖     男      66.12
#杨过     男      48.65
#郭芙     女      47.99
```

```
%s %c %d %f都是格式替代符
%-10s 指一个宽度至少为10个字符（-表示左对齐，没有则表示右对齐），如果不足则自动以空格填充，超过就将内容全部显示出来。
%-4.2f 指格式化为小数，其中.2指保留2位小数。
```

```bash
#!/bin/bash
 
# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样 
printf '%d %s\n' 1 "abc" 

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n" 
```

* printf转移字符

```
\a	警告字符，通常为ASCII的BEL字符
\b	后退
\c	抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略
\f	换页（formfeed）
\n	换行
\r	回车（Carriage return）
\t	水平制表符
\v	垂直制表符
\\	一个字面上的反斜杠字符
\ddd	表示1到3位数八进制值的字符。仅在格式字符串中有效
\0ddd	表示1到3位的八进制值字符
```

13) test命令

```bash
#!/bin/bash
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

```bash
#!/bin/bash
num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi
```

```bash
#!/bin/bash
cd /bin
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```

shell还提供了与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来，其优先级为："!"最高，"-a"次之，"-o"最低

```bash
cd /bin
if test -e ./notFile -o -e ./bash
then
    echo '有一个文件存在!'
else
    echo '两个文件都不存在'
fi
```

14.  shell流程控制

* if else

语法格式

```bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

```bash
#!/bin/bash
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```

* for循环

语法格式

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

```bash
#!/bin/bash
for value in 1 2 3 4 5
do
    echo "The value is: $value"
done
#输出结果：
#The value is: 1
#The value is: 2
#The value is: 3
#The value is: 4
#The value is: 5
```

* while语句

语法格式

```bash
while condition
do
    command
done
```

```bash
#!/bin/sh
int=1
while(( $int<=5 ))
do
        echo $int
        let "int++"
done
```

* until循环

until循环执行一系列命令直至条件为真时停止。until循环与while循环在处理方式上刚好相反。

```bash
until condition
do
    command
done
```

* case

```bash
#!/bin/bash
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

* break 与 continue

与c、java、php等其他语言类似

* 无限循环

```bash
while :
do
    command
done
#或者
while true
do
    command
done
#或者
for (( ; ; ))
do
    command
done
```

15. 函数

```bash
[ function ] funname [()]

{

    action;

    [return int;]

}
#中括号的部分代表这部分可选
```

参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。return后跟数值n(0-255)

* 函数传参

```bash
#!/bin/bash

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

特殊字符

```
$#	传递到脚本的参数个数
$*	以一个单字符串显示所有向脚本传递的参数
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
```

16. 输入/输出重定向

```
标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。
```

```
command > file	将输出重定向到 file。
command < file	将输入重定向到 file。
command >> file	将输出以追加的方式重定向到 file。
n > file	将文件描述符为 n 的文件重定向到 file。
n >> file	将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m	将输出文件 m 和 n 合并。
n <& m	将输入文件 m 和 n 合并。
<< tag	将开始标记 tag 和结束标记 tag 之间的内容作为输入。
```

17.  文件包含

语法格式

```bash
. filename
# 注意点号(.)和文件名中间有一空格
#或
source filename
```
### TIP 

1.  只修改文件夹权限

    ```bash
    #path指具体路径，如~/Documents
    #1.
    chmod 0755 `find path -type d`
    #2.
    find path . -type d -exec chmod 0755 \{\} \;

    ```

2.  只修改文件权限

    ```bash
    #1.
    chmod 0644 `find path -type f`
    #2.
    find path . -type f -exec chmod -644 \{\} \;
    ```

    ​