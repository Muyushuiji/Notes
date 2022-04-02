## 变量

### 命名规则

1. 不以数字开头，只能包含英文、数字、下划线
2. 不能使用bash里的关键字

```shell
#正确命名
R_DB_NAME
_var
_var1
variable
#错误命名
?var
2var
```

### 运用

```shell
my_name="test"
echo $my_name
echo ${my_name} //推荐写法
```

![image-20220307091444101](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307091444101.png)

echo命令用于在终端输出字符串，参数设定

```shell
-n 不换行输出
-e 输出转义字符
#转义字符
\b 退格键
\c 不换行输出，\c后面存在的字符不会输出
\n 换行，后面存在的字符另起一行
\f 换行 新行开头位置连接上一行的行尾
\v 和\f相同
\t 插入tab 制表符
\r 光标移至首行，不换行。\r后面存在的字符同等替换从头开始额字符
\\ 插入"\"本身
```
>变量类型

1. 局部变量：`脚本或命令中定义`，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2. 环境变量：`所有的程序都能访问`，shell脚本也可以定义环境变量。
3. shell变量：shell程序设置的特殊变量。`shell变量中一部分是环境变量，一部分是局部变量。`

> 循环输出

```shell
for space in Adia Libarary School Company; do
	echo "This book is in ${space}Go got it"
done
```

![image-20220307092617522](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307092617522.png)

> 只读变量

```shell
varible="baidu.com"
readOnly variable
variable="google.com"
```

![image-20220307093524273](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307093524273.png)

>删除变量

```shell
varible="baidu.com"
unset varible
echo ${varible}
```

![image-20220307093743196](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307093743196.png)

### Shell字符串

字符串可以单引号、双引号、不用引号。

```shell
#单引号，单引号的任何字符都会原样输出，且其中的变量无效。
str='this is a string'
#双引号，可以有优点，可以出现转义字符
my_name="Robbie"
str="Hello,\"${my_name}\"! \n"
echo -e ${str}
```

![image-20220307094820846](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307094820846.png)

>字符串拼接

```shell
my_name="robbie"
# 双引号拼接
variable="hello, "${my_name}" !"
variable_1="hello, ${my_name} !"
echo $variable $variable_1
# 单引号拼接
variable_2='hello, ${my_name} !'
variable_3='hello, '$my_name' !'
echo $variable_2 $variable_3
```

![image-20220307100853327](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307100853327.png)

>获取字符串长度

```shell
string="abcd"
echo ${#string}
#变量为数组，${#variable}等价于${#variable[0]}
string_1="12345" 
echo ${#string_1[0]}

# 提取从第2个字符开始截取4个字符
string_2="this is a test"
echo ${string_2:1:4}
```

![image-20220307105049789](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307105049789.png)

>查找子字符串

```shell
#查找字符i或o的位置（计算先出现的字符）
string="this is a test"
echo `expr index "${string}" io`
```

![image-20220307105424949](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307105424949.png)

```shell
# expr表达式
expr length [string] 计算字符串长度
expr substr "this is a test" [3] [5] 从第三个字符开始，抓取5个字符
expr index [string] [char] 抓取char第一个出现的位置
expr [运算表达式]
```

>Shell 数组

```shell
array_name=(value1 value2 value3)
echo ${array_name[0]}
# 遍历数组中的所有元素
echo ${array_name[@]}
```

![image-20220307110816325](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307110816325.png)

```shell
# 取得数组元素的个数
length=${#array_name[@]}
length_1=${#array_name[*]}
# 取得单个元素的长度
length_2=${#array_name[0]}
```

## Shell 传递参数

```shell
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

![image-20220307113411283](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307113411283.png)

```shell
$# 传递到脚本的参数个数
$* 以一个但字符串显示所有向脚本传递的参数
$$ 脚本运行的当前进程ID号
$! 后台运行的最后一个进程的ID号
$@ 与$*相同，使用时加引号

echo "Shell 传递参数实例！";
echo "第一个参数为：$1";
echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
echo "脚本的进程ID号为：$$"
echo "传递的参数作为一个字符串显示：$@"
```

![image-20220307114450376](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307114450376.png)

>$*和$@区别

区别：在双引号中体现出来，脚本运行时三个参数1、2、3，"*"等价于 "1 2 3"（传递了一个参数），"@"等价于 "1" "2" "3"（传递了三个参数）

```shell
for i in "$*"; do
    echo $i
done

for i in "$@"; do
    echo $i
done
```

![image-20220307114922786](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307114922786.png)

## Shell运算符

原生bash不支持简单的数学运算，可以通过其他命令实现，`awk`和`expr`。

### 基本运算符

1. 表达式和运算符之间要有空格，如`2+2`应写成`2 + 2`
2. 完整的表达式要被` 单引号` `` 包含

```shell
a=10
b=20
# + 加法
add=`expr $a + $b`
echo ${add}
# - 减法
subtracte=`expr $a - $b`
echo ${subtracte}
# * 乘法
multiply=`expr $a \* $b`
echo ${multiply}
# / 除法
remove=`expr $b / $a`
echo ${remove}
# % 取余
remainder=`expr $b % $a`
echo ${remainder}
# = 赋值
a=${b}
# 是否相等
if [ $a == $b ]
then
    echo "a 等于 b"
fi
if [ $a != $b ]
then
    echo "a 不等于 b"
fi
```

![image-20220307140952936](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307140952936.png)

>(*)前必须加(\)

### 关系运算符

```shell
a=10
b=20

# 检测两个数是否相等，相等返回true
if [ $a -eq $b ]
then
    echo "$a -eq $b : a 等于 b"
else
    echo "$a -eq $b : a 不等于 b"
fi
# 检测两个数是否不相等，不相等返回true
if [ $a -ne $b ]
then
    echo "$a -ne $b : a 不等于 b"
else
    echo "$a -ne $b : a 等于 b"
fi
# 检测左边的数是否大于右边的，如果是，返回true
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
# 检测左边的数是否小于右边的，如果是，返回true
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
# 检测左边的数是否大于等于右边的，如果是，返回true
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
# 检测左边的数是否小于等于右边的，如果是，返回true
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi
```

![image-20220307150616327](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307150616327.png)

### 布尔运算符

```shell
a=10
b=20

# ! 非运算，表达式为true则返回false，否则返回true
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a == $b: a 等于 b"
fi
# -o 或运算，有一个表达式为true则返回true
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
# -a 与运算，两个表达式都为true才返回true
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
   echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
```

![image-20220307151547042](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307151547042.png)

### 逻辑运算符

```shell
a=10
b=20

# && 逻辑的AND
if [[ $a -lt 100 && $b -gt 100 ]]
then
    echo "返回 true"
else
    echo "返回 false"
fi
#  || 逻辑的OR
if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi
```

![image-20220307152000688](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307152000688.png)

### 字符串运算符

```shell
a="abc"
b="def"

# = 检测两个字符串是否相等，相等返回true
if [ $a = $b ]
then
    echo "$a = $b : a 等于 b"
else
    echo "$a = $b: a不等于 b"
fi
# ！= 检测两个字符串是否不想打，不相等返回true
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
# -z 检测字符串长度是否为0
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
# -n 检测字符串长度是否不为0，不为0返回true
if [ -n "$a" ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
# $ 检测字符串是否为空，不为空返回true
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi
```

![image-20220307154510216](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307154510216.png)

### 文件测试运算符

```shell
file="d:\downloads\a.sh"
# -r 是否可读，可读返回true
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
# -w 是否可写，可写返回true
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
# -x 是否可执行，可执行返回true
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
# -f 是否为普通文件，普通文件返回true
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
# -d 是否是目录，是目录返回true
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
# -s 是否为空，为空则返回false，不为空返回true
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
# -e 文件是否存在，存在返回true
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

![image-20220307154952736](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307154952736.png)

## Shell test命令

test用于检查某个条件是否成立，可以进行数值、字符和文件三个方面的测试

>数值测试

```shell
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

![image-20220307174105532](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307174105532.png)

>字符串测试

```shell
num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi

# -z 字符串的长度为零则为真
if test -z $num1 
then
    echo '字符串长度为0'
else 
    echo '字符串长度不为0'
fi

# -n 字符串的长度不为零则为真
if test -n $num1 
then
    echo '字符串长度不为0'
else 
    echo '字符串长度为0'
fi
```

>文件测试

```shell
cd /bin
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```

![image-20220307175236402](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307175236402.png)

文件测试还支持逻辑运算符

## Shell流程控制

### if

sh的流程控制不可为空

```java
if(expression){
    find(a);
}else{
    // nothing to do
}
```

`java`的流程控制可以这样写，`shell`不行。

>if 写法

```shell
if condition
then
	command1
	command2
	command3
	...
fi
```

写成一行（适用于终端命令提示符）：

```shell
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```

>if else 写法

```shell
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

>if else-if else

```shell
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

>实例

```shell
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

![image-20220307180017744](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307180017744.png)

### for循环

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

```shell
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

![image-20220307180251088](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220307180251088.png)

### while 语句

```shell
while condition
do
    command
done
```

```shell
a=1
while((${a}<=6))
do
    echo ${a}
    let a++
done
```

![image-20220308094543822](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308094543822.png)

### until 循环

```shell
until condition
do
	command
done
```

```shell
a=0

until [ ! ${a} -lt 10 ]
do 
    echo ${a}
    a=`expr ${a} + 1`
done
```

![image-20220308095857199](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308095857199.png)

### case ... esac

类似于switch...case语句,取值后面必须为`in`，model必须以`)`结束。取值可以为`常量和变量`

```shell
case var in
model1) 
	command1
	command2
	...
	;;
model2)
	command1
	command2
	...
	;;
*)
	command1
	command2
	...
	;;
esac
```

```shell
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

*捕获匹配模式，如果之前的模式都没有匹配，则执行*后面的命令。

```shell
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

![image-20220308102723776](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308102723776.png)

## Shell 函数

```shell
[ function ] funname [()]

{

    action;

    [return int;]

}
```

1. 可以带function fun()定义，也可以直接fun()定义，不带任何参数。

2. return返回如果不加，以最后一条命令运行结果作为返回值。

```shell
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"

funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

![image-20220308104522960](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308104522960.png)

函数在使用前必须定义，函数返回值在调用该函数后通过`$?`来获得。

>调用函数时，可以向其传递参数。在函数体内部，通过$n的形式获取参数的值。

```shell
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

![image-20220308105609786](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308105609786.png)

`n>=10时，需要使用${n}虎丘参数`

### /dev/null文件

希望执行某个命令不在屏幕显示输出结果，可以将输出重定向到/dev/null

```shell
command > /dev/null
```

写入到`/dev/null`文件中的内容都会呗丢弃，将命令的输出重定向到它，会起到“禁止输出”的效果。

如果想要屏蔽stdout和stderr

```shell
command > /dev/null 2>&1
```

`2> 不可以有空格，合一起表示错误输出。`

## Shell文件包含

```shell
. filename
# (.)和文件名中间有一空格 或者
source filename
```

>b.sh

```shell
url="http://www.baidu.com"
```

>a.sh

```shell
. ./b.sh
# 或者
# source ./b.sh

echo "搜索网站：$url"
```

![image-20220308112928693](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220308112928693.png)

被包含的`b.sh`文件不需要可执行权限。

