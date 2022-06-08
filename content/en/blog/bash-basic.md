---
title: bash_basic
encrypt: false
enc_pwd: askding
categories:
  - Coding
date: 2021-1-2 18:00:59
top:
tags:
layout:
---
编写进度
- [x] 
  
# 编程基本元素

## I/O

```zsh
VAR="string" # 声明变量之间不能带有空格
read MYVAR  # input
CMDOUT=$(pwd)  # 将pwd命令的输出保存到CMDOUT中

echo "$MYVAR"  # output
printf "$VAR"  # output
```

### 输入`read命令`
- a 后跟一个变量，该变量会被认为是个数组，然后给其赋值，默认是以空格为分割符。
- d 后面跟一个标志符，其实只有其后的第一个字符有用，作为结束的标志。
- p 后面跟提示信息，即在输入前打印提示信息。
- e 在输入的时候可以使用命令补全功能。
- n 后跟一个数字，定义输入文本的长度，很实用。
- r 屏蔽\，如果没有该选项，则\作为一个转义字符，有的话 \就是个正常的字符了。
- s 安静模式，在输入字符时不再屏幕上显示，例如login时输入密码。
- t 后面跟秒数，定义输入字符的等待时间。
- u 后面跟fd，从文件描述符中读入，该文件描述符可以是exec新开启的。 )

### 输出

#### echo命令

 echo显示颜色
![echo显示颜色
](http://c.biancheng.net/uploads/allimg/180926/3-1P926164539348.jpg)

```zsh
echo [-ne E] [String]

-E : (默认)转义,不解释参数中的转义符
-e : 不转义,解释参数中的转义符

-n : 打印内容不换行
```

- `String` 中的转义符(由echo命令解释)
  - `\a` : 告警
  - `\b` : 退格
  - `\c` : 忽略输出中最后的换行符
  - `\f` : 换页
  - `\n` : 回车换行(Newline)
  - `\r` : 回车
  - `\t` : 水平制表符
  - `\v` : 垂直制表符
  - `\\` : 反斜线


#### `printf`
```zsh
printf "<foramtString>" arg1 arg2 ...
```

- `formatString` : 待输出的字符串

- `格式规范 %[flags][width][.precision]<type>`
  1. `flags` : `+ - # <space>`
    - `+` : 在整数前加+/-
    - `-` : 使用`width`时,表示输出数值左对齐
    - `#` : 输出(八进制标识符)`0` 和(十六进制标识符)`0x`或`0X`
    - `<space>` : 空格,起对齐作用在打印正数前面加上一个空格,打印负数前面加上`-`。

  2. `width` 指定输出参数时最小字段宽度<num>, 对应的参数默认采用右对齐的形式 

  3. `.precision` : 表示整数的最小位数,字符串的最大字符数,`*`代表使用下一个参数作为精度

  4. `type` 
    - `%d` : 整数
    - `%u` : 无符号整数
    - `%o` : 八进制整数
    - `%x` : 十六进制整数(a-f)
    - `%X` : 十六进制整数(A~F)

    - `%c` : 单个字符
    - `%s` : 字符串字面量
    - `%b` : 包含转义字符的字符串
    - `%%` : 百分号


### 多行输出

- 不转义输出
```zsh
cat <<EOF     # date命令会执行
Line1
`date`
Line3
EOF
```


- 转义输出
```zsh
cat <<\EOF    # date命令原样输出
Line1
`date`
Line3
EOF
```




## 变量
变量名(字母、数字、下划线)由[a-z]、[A-Z]、`_`、[0-9]组合，且开头不能是[0-9]

> 变量本质上是存储数据的一个或多个计算机内存地址

### 变量操作

```zsh
var=value   # 等号两边不能有空格 
var=`command argument` # 变量名

var=1  # 设置变量
unset var  # 清除变量
var=2
readonly var #设置var为只读变量
```

### 变量间接引用`eval`
eval 使shell对args求值, 然后执行求职结果。常用于从变量中构造命令行

变量适用条件：
-  含有命令终止符: `;、|、&`
-  含有I/O重定向: `<、>`
-  引号: `'、"`

```zsh
x="askding"
askding_url="askding.github.io"
eval echo \$${x}_url
```

#### 间接参数扩展 ${!var}
```bash
parameter="var"
var="hello"
echo ${!parameter}

hello

```

### shell参数扩展- 空参数处理
`${var1:-var2}` 等价于 `(!isSet(va1) || var1 == NULL)`
即 判断var1为unset或者var1为NULL
参数扩展：取得var代表的变量的值

- `${var-word}` : var存在(可为空),就是var,不存在就是word
- `${var=word}` : var存在(可为空),就是var,不存在就是word, var也是word。
- `${var+word}` : var存在(可为空),就是var,不存在就是空
- `${var?word}` : var存在(可为空),就是var, 不存在将word写入到标准错误并退出


> :表示var非空

- `${var:-word}` :   var存在非空就是var，不存在就是word
  - 若var存在且非空,则${}=$var
  - 若var未定义或为空值, ${}=word，$var不变

- `${var:=word}` :   var存在非空就是var，不存在就是word，var也是word
  - 若var存在且非空, ${}=$var
  - 若var未定义或为空值,${}=word, 且$var=word

- `${var:+word}` :   var存在非空就是var，不存在就是空
  - 若var存在且非空, ${}=word
  - 若var未定义或为空值, ${}为空，$var不变

- `${var:?word}` :    var存在就非空是var，不存在打印word并终止
  - 若var存在且非空, ${}=$var
  - 若var未定义或为空值, 输出word,并终止脚本

> :表示var非空
> 未定义 表示未执行var=xxx 或set var=xxx
> 为NULL 表示 set var

1. 指定默认值
- `${VAR:=WORD}` 当VAR未定义(set) 给VAR赋默认值WORD, 结果也为WORD
- `${VAR=WORD}`  当VAR未定义(set)或为空，给VAR赋默认值WORD, 结果也为WORD

2. 使用默认值
- `${VAR:-WORD}` 当VAR未定义(set),或为NULL，结果为WORD
- `${VAR-WORD}` 当VAR未定义(set) ， 则结果为WORD 


3. 使用替代值
- `${VAR:+WORD}` 当VAR未定义,或为空, 则结果为空;
- `${VAR+WORD}`  当VAR未定义,或为空, 则结果为空;
当VAR被set且赋不为空值时，则会使用WORD


## 字符串操作
- `${#var}` : 返回${var}变量的长度
- `${var:m}` : 返回${var}中第m个字符开始到结尾部分(从0开始计算)
- `${var:m:len}` : 返回${var}中第m个字符开始，长度位len的部分

- `${var#pattern}` : 删除${var}中开头部分与pattern匹配的部分(非贪婪模式)
- `${var##pattern}` : 删除${var}中开头部分与pattern匹配的部分(贪婪模式)
- `${var%pattern}` : 删除${var}中结尾部分与pattern匹配的部分(非贪婪模式)
- `${var%%pattern}` : 删除${var}中结尾部分与pattern匹配的部分(贪婪模式)

- `${var/old/new}` : 用new替换${var}中第一次出现的old
- `${var//old/new}` : 用new替换${var}中所有old(全局替换)
- `${var/#old/new}` : 用new替换${var}中开头部分与old匹配的部分
- `${var/%old/new}` : 用new替换${var}中结尾部分与old匹配的部分

## 数组操作

- 整数索引的数字 `Arrary[index]` 可直接使用变量名创建
- 关联(字符串)数组 `Colors["red"]="#FF0000"` 必须使用`declare -A`声明创建

### 创建数组
```zsh
ARRAY[index]=value   # index为算数表达式,或(0,1,2,...)的整数

ARRAY=(Value1 Value2 Value3 [5]=Vlaue5 ...)   # 第三、四元素为空字符串""

declare -a Array_name # 声明Array_name是一个数组
read -a Array_name    # 将用户的命令行输入,当成Array_name的数组元素,以空格符分隔符
```
### 读取数组
```zsh
echo ${array[index]}            # 打印单个数组成员
echo ${array[@]} ${array[*]}    # 打印所有成员,推荐使用`"${array[@]}"`
array_copy=( "${array[@]}" )    # 拷贝数组
```

### 数组长度
```zsh
echo ${#array[@]} ${#array[*]}    # 打印数组长度
echo ${#array}                    #打印第一个成员的长度
echo ${#array[i]}                 # 打印指定成员长度 
```

### 打印数组序号
```zsh
echo ${!array[@]}  ${!array[*]}  
```

### 打印数组成员(数组切片)
```zsh
echo ${array[@]:position:lens}   ${array[*]:position:lens} 
# position从0开始
# lens为长度，不指定，返回从position开始的所有成员
```

### 追加数组成员
```zsh
array=(a b c)
array+=(d e f)    # 利用+=可以追加数组元素
echo ${array[@]}
```

### 删除数组及成员
```zsh
unset array     # 清空整个数组
array[i]=''     #隐藏第i+i个元素,设为空值=''
```
## shell内部变量
- `#` : 位置参数的个数
- `?` : 上条命令执行后的返回值
- `$` : 当前shell进程的PID
- `!` : 最后一个后台运行命令的PID
- `0` : 当前执行的shell程序的名称
- `@` : 位置参数的内容
- `*` : 位置参数的内容,受`IFS`影响
- `_` : shell启动时,为正在运行shell程序的绝对路径。shell结束后为上一条命令的最后一个参数 

## 内部特殊参数  
每个参数由空格符分隔，并在bash中使用一组特殊的标识符$[num]进行访问。

- $0 : 表示命令行输入的脚本名称
- $1 : 第一个参数
- $n : 第n个参数
- $# : 参数的个数,不包括$0
- $@ : 用空格分隔的所有参数$1 $2 $3 ... $n
- \$* : 根据$IFS分隔所有参数

 [注：`$*` 和 `$@` 的区别](http://c.biancheng.net/view/807.html)

`$*` 和 `$@` 都表示传递给函数或脚本的所有参数，不被双引号(" ")包含时，都以"$1" "![2" … "](https://juejin.cn/equation?tex=2%22%20%E2%80%A6%20%22)n" 的形式输出所有参数。

但是当它们被双引号(" ")包含时

-   `"$*"` 会将所有的参数作为一个整体，以"$1 ![2 …](https://juejin.cn/equation?tex=2%20%E2%80%A6)n"的形式输出所有参数；
-   `"$@"` 会将各个参数分开，以"$1" "![2" … "](https://juejin.cn/equation?tex=2%22%20%E2%80%A6%20%22)n" 的形式输出所有参数。



进程状态的相关参数

- `$$`: 输出当前进程的进程号<PID>
- `$!` : 输出后台运行的最后一个PID
- `$_`: 输出上一条命令的最后一个参数 

exit <n> # 设置返回状态码
- $? : 输出上条命令执行后的返回值
  - 0 : 成功
  - 1-255 : 不成功
    - 1 : 通用错误/执行失败
    - 126 : 命令或脚本没执行权限
    - 127 : 命令没找到

```zsh
# 练习脚本
echo "Hello,$USER, the output of this script are as follows:"
echo -e "The script name is              : \t $(basename $0)"
echo -e "The first param of the script is: \t $1"
echo -e "The second param of the script is: \t $2"
echo -e "The five param of the script is: \t $5"
echo -e "All the params you input are: \t $@"
echo -e "The PID of this script is: \t $$"
echo -e "The exit status of this script is: \t $?"

```

## 操作符
### 整数运算符
 `$[]`  和 `$((expression))` :+ - * / % **(幂)

### 整数比较操作符号
1. `[]`和`[[]]`的操作符号

- `[ int1 -eq int2 ]` : =   `((int1 == int2))` 
- `[ int1 -ne int2 ]` : !=  `((int1 != int2))` 
- `[ int1 -gt int2 ]` : >  `((int1 > int2))` 
- `[ int1 -ge int2]` : >=  `((int1 >= int2))` 
- `[ int1 -lt int2 ]` : <   `((int1 < int2))` 
- `[ int1 -le int2 ]` : <=  `((int1 <= int2))` 

⚠️：
`== != < >` 等操作符号在`[]` `[[]]`中使用需要转义,macOS中不可用此种方式
```zsh
a=2;b=6; [ $a \> $b ]; echo $?
```
### 字符串测试操作符号
```zsh
str=Tom ;[ -z "$str" ]; echo $?
```
- `[ str ]` : $str不为空,返回真
- `[ -z str ]` : $str长度为0,返回真
- `[ -n str ]` : $str长度不为0,返回真

- `[ str1 = str2 ]` : 测试str1与str2相等,返回真
- `[ str1 != str2 ]` : 测试str1与str2不想等,返回真
- `[[ str1 == str2 ]]` : 测试str1与str2相同,返回真
- `[[ str1 != str2 ]]` : 测试str1与str2不相同,返回真
- `[[ str1 =~ str2 ]]` : str2是str1的子串,返回真
- `[[ str1 > str2 ]]` : str1 大于str2,返回真
- `[[ str1 < str2 ]]` : str1小于str2,返回真

### 逻辑运算符
三元表达式: ` [  ] && echo T  || echo F`

1. 常用方式:
- `[[ pattern1 && pattern2 ]]` : 逻辑与
- `[[ pattern1 || pattern2 ]]` : 逻辑或
- `[[ !pattern ]]` : 逻辑非

2. 不常用方式:
- `[ expr1 -a expr2 ]` : and
- `[ expr1 -o expr2 ]` : or
- `[ !expr ]` : 非！
```zsh
x=1;name=Tom; [ $x -eq 1 -a -n $name ]; echo $?
```

### 文件测试操作符号

- `[ -b fname ]` : fname为块设备,返回真
- `[ -c fname ]` : fname为字符设备,返回真
- `[ -p fname ]` : fname为命名管道,返回真
- `[ -S fname ]` : fname为Socket,返回真

- `[ -s fname ]` : fname存在且size>0,返回真

- `[ -f fname ]` : fname存在且是普通文件,返回真
- `[ -L fname ]` : fname存在且是链接文件,返回真 

- `[ -e fname ]` : fname(文件或目录)存在,返回真
- `[ -d fname ]` : fname存在且是目录,返回真
- `[ -r fname ]` : fname(文件或目录)存在且可读,返回真
- `[ -w fname ]` : fname(文件或目录)存在且可写,返回真
- `[ -x fname ]` : fname(文件或目录)存在且可执行,返回真

# 流程控制语句

## 顺序语句
程序会顺序来执行代码，从上往下一行一行执行

## 分支语句

条件测试语句格式：
> [ <expression> ]   等效于test <expression>
> [[ <expression> ]]   

区别：
- `[]` 不支持逻辑运算符、正则表达式
- `[[]]` 支持逻辑运算符&&、||、!、和()，支持正则表达式匹配


### if型

- `if`语句可嵌套使用
- 必须以`if`开头，`fi`结尾
- `elif` 可以有`0个或多个`
- `else`最多只能有个一个
- `commands`为可执行语句块,shell提供空命令`:`
- `;` 相当于命令换行

```zsh
if [ expr1 ]; then
  <commands1>   #expr1为真时，执行commands1

elif [ expr2 ]; then
  <commands2>   # expr1为假，expr2为真时，执行commands2
elif [ expr3 ]; then
  <commands3>   # expr1到expr2为假，expr3为真时，执行commands3
    ...
elif [ exprn ]; then
  <commandsn>   # expr1到expr(n-1)为假，exprn为真时，执行commandsn
else
  <command>   # 当expr(1-n)都为假时，执行command 

fi
```

### case型
```zsh
case $expr in           
  pattern1)         # 若expr与pattern1匹配,
    commands1       # 执行语句块commands
    ;;              # 跳出case结构
  pattern2)
    commands2
    ;;
  *)                # 若expr与上面的pattern均不匹配
    commands        
    ;;
esac                # case语句必须以easc终止
```
### select型
> while+case语句可实现select

```zsh
select answer in <list>; do
  commands                     # 循环变量answer每取一次值，循环体commands就执行一遍
done                           # 循环结束标志
```



## 循环语句

- `break` 强行退出本层循环。  `break [n]` 退出第几层循环(最里面为第一层循环)
- `continue` 忽略本次循环的剩余部分，回到循环的顶部,继续下一次循环。 `continue [n]` 回到第n次循环的顶部

### for循环

- python格式

```zsh
for item in list ; do
  commands
done
```

- C语言格式(不常用)

```zsh
for ((i=0;i<100;i++))
do
  echo $i
done
```

### while循环

> 与until作用相反
```zsh
while expr ; do       # 执行expr表达式是否为真
  commands            # 循环体语句
done [< test.file]      # 循环结束标志,返回循环顶部
```

### until循环

> 与while作用相反
```zsh
until expr ; do      # 执行expr表达式是否为假,   expr为真退出循环
  commands           # expr退出状态为假, 执行循环体commands
done                 # 循环体结束标志, 返回循环顶部

```


# 函数
关键字`function`和`重定向`命令可选

```zsh
[function] myfun(){

  commands     # body of the funtion goes here
  [break]      # 退出当前函数
  [exit <n>]   # 退出整个脚本
  [return <n>] # 退出当前函数。未指定n，则返回函数最后一条milling执行后所返回的状态  

} [重定向]

myfun [arg1 arg2 ... argn]   # 运行函数,arg参数通过位置参数$n传入
echo $?       # 查看函数myfun返回值
```

⚠️
- 作用域: 函数内的变量的作用域默认是全局的，可使用`local`限定作用域于函数内
- 函数定义中不必声明参数个数，参数个数由此函数被调用时给定。

# 调试

## 打印变量
```zsh
#! /bin/bash
echo $var ; exit   # 单独调试某个变量 

trap "echo a=$a b=$b"  EXIT   # 程序退出打印程序内的变量  
a=20
b=30

exit
```

## sh命令参数
```zsh
bash [-n | -v | -x]  <file.sh>

-n : 仅模糊检查语法错误,不执行file.sh脚本。
-x : 打印每条命令的结果
```


# Tips
- `help` 查看shell内置命令

```zsh
ssh myserver ps > /tmp/ps.out  #重定向到本地/tmp目录
ssh myserver ps \> /tmp/ps.out #重定向到服务器/tmp目录 

```
- `. aa.sh` : 在当前shell环境下执行aa.sh内的命令


## 子shell运行
- `export <variables>[=value]` : 导出指定变量到子shell中

```zsh
var1=value1 var2=value2 ... varn=valuen  command
```
将var[1-n]及其值放入到command环境中,执行command。当前shell不存在变量var[1-n]

## 异步执行 `wait [PID]`
```
wait [PID]   # 不写PID, 则shell会等待所有的子进程执行完毕。在子进程执行完毕前,当前shell会被挂起。
```


## 命令组
- `(commands ...)` : 在子shell中运行
- `{ command;..;}` : 在当前shell中运行, 开头有`空格`,命令后带`;`

## 转义符`\`
转义就是转成字符字面含义  
`\x` : 使用x字符的字面意义。

## 引号'' ""
- `''` 单引号不能嵌套,引号内的所有字符转义,即保持字面含义。
- `""` 双引号内的字符,含有转义符`\` 后跟$、\`、\" 、\\保持特殊含义，不转义。


## 参数左移`shift [n] `
shell命令将shell程序的参数$1 ...$n分别向左移动n位。

## 单字符参数处理`getopts`
```zsh
getopts <optstring>  <opt>  [arg...]
```
- `optstring` : 代表参数字符串, 开头含有`:`,表示不打印错误信息
  - eg: `aa.sh -a -b -p value`     
  - `optstring`就是`abp:` , `-a -b`为开关型选项     
  -  当getopts匹配到-p参数时, `value`会被存放在shell内置变量`OPTARG`中 
- `opt` : getopts存放参数名的变量, 参数对应的值存在`OPTARG` 
  - 每次执行getopts,会从命令行中获取下一个参数，存放在`name`中  
  -  如果获取的参数不在`optstring`中, `name`的值为`?`
- `arg` : 默认是`$@`, 即shell脚本的全部参数
- `OPTIND` : 存放所有参数的下标, 开关型参数下标=1  含参型下标=2
  
## 多字符参数处理`getopt` 



## shell环境变量
- `HOME` : 存放用户主目录的完整路径名
- `PATH` : shell执行命令时顺序搜索可执行文件
- `TERM` : 终端类型
- `UID` : 当前用户标识
- `PS1` : 主提示符
- `PS2` : 辅助提示符
- `PS4` : `set -x` 模式下的提示符,默认+
- `IFS` : 输入域分隔符

## 重定向

|形式|含义|
|:---:|:---:|
|< file |标准输入重定向到file,将file内容输入到终端里|
|> file|标准输出重定向到file|
|command < file1 \>file2|将 file1 作为 command 的输入，并将 command 的处理结果输出到 file2。|

- 重定向标准输出及标准错误
```zsh
`commands`>file 2>&1   等效于   &>file    等效于  >&file
```
 按照重定向符号的顺序自左至右执行，
 首先标准输出重定向至file中，
 2>&1 表示将标准错误重定向至标准输出

- 追加重定向标准输出及标准错误
```zsh
`command` >> file 2>&1   等效于 &>> file
```

## 命令替换
将命令的输出作为命令替换位置的文本

var=`ps -ef |wc -l`
var=$(ps -ef | wc -l)

- ` ${} ` :用于定义变量
- ` $() ` : 用于命令替换,等效于\` \`
-  `$[ ] ` 和`$(())`: 用于整数运算 

## 脚本执行结束清理缓存文件
```zsh
#!/bin/bash 

# trap捕捉到EXIT信号时执行rm -f "$TMPFILE" 命令
# 最好放在shell脚本的开头
# trap  <command | function >  <SIGNAL> SINGAL2 ...SINGALN
trap 'rm -f "$TMPFILE"' EXIT

TMPFILE=$(mktemp) || exit 1
ls /etc > $TMPFILE
if grep -qi "kernel" $TMPFILE; then
  echo 'find'
fi
```

## 根据扩展名切分文件名
文件名格式:`name.extention`
- `%` : 提取文件名name部分
- `#` : 提取文件名extention部分
```zsh
file="sample.jpg"

echo "文件名: ${file%.*}"  # 从file中删除位于%右侧通配符(.*)匹配到的字符,匹配方向从右向左匹配,非贪婪模式,贪婪模式加%%
echo "扩展名: ${file#*.}"  # 从file中删除位于#右侧通配符(*.)匹配到的字符串,匹配方向从左到右匹配,非贪婪模式,贪婪模式加##
```

## shell命令行使用vi行编辑模式
修改命令时，不管处于哪种模式，按下`Enter`就会解释当前命令。
```zsh
set -o vi/emacs
```

## set命令切分字段
```zsh
aa="aa bb cc"
# set $aa会将aa的值根据IFS来切分赋给位置参数
set $aa
echo $1
echo $2
echo $3
```
## GUI界面Dialog

## 终端颜色

### tput显示颜色
- `tput setab <num>` : 设置背景色
- `tput setaf <num>` : 设置前景色

|||||||
|---|---|---|---|---|---|---|
|\<num>|0|1|2|3|4|5|6|7|
|color|黑|红|绿|黄|蓝|洋红|黄|白|

```zsh
RED=$(tput setaf 1)
GREEN=$(tput setaf 2)
RESET=$(tput sgr0)   # sgr0 表示颜色重置
echo "${RED}red text ${GREEN}green text${RESET}"
```

## bash注释

### 单行注释 \#
```zsh
# 单行注释符
: commnets here

```

### 多行注释 
1. 方式一
```zsh
<< COMMENT
comment here
....
comment here
COMMENT
```

2. 方式二
```zsh
:'
commnet here
...
commnet here
'
```


## 后台运行
```zsh
<command>    # 按下ctrl-z即可挂起该进程
```
- `fg` : 在前台恢复执行当前作业 
- `bg` : 在后台恢复执行当前作业

## 切换目录`cd -`
- `-` 表示上一个目录

## 开发规范
```zsh
# Date: 2020-8-8 8:8:8
# Author: askDing
# Blog: https://askding.github.io
#
# Version: 1.1
# Description: 
```

## 分割/合并文件
- 分割文件
```zsh
cut -f <col_list> -d ';' --complenment  --output-delimiter '-' file  
# 按列切分文件 -f 指定待提取的列号
# -d 指定分隔符
# --complenment 输出-f指定之外的所有列
# --output-delimiter 指定输出分隔符
split [-b size] [-d [-a <num>]]  file  
# -b指定分割文件的大小10k、10M、10G、
# -d 以数字为后缀
# -a num 指定后缀的长度
```
- 合并文件
```zsh
cat file1 file2 ... > file_total  # 按行合并文件
paste file1 file2 ... -d ',' # 按列合并文件,-d指定分隔符
```

## cat的特殊用法
```zsh
echo "aa bb cc" | cat - other_file   # - 作为stdin文本的文件名
```

## 终端截图
```zsh
xwd -root -out /tmp/xwd_test.xwd  # 截取整个屏幕,不需要鼠标选择区域
xwud -in /tmp/xwd_test.xwd      # 查看截屏文件
convert /tmp/xwd_test.xwd  /tmp/xwd_test.png   # 转换成png格式图片
```

## script录制终端会话
```zsh
script -t 2> rec.time  -a rec.his # 开始录制 -t 记录时序 -a 追加输出到文件, -a可忽略
exit                              # 结束录制
scriptreplay  -t  rec.time  -s  rec.his   # 回放 -t -s可省略
```

## termtosvg生成svg动画
[termtosvg](https://nbedos.github.io/termtosvg/)

```zsh
brew install termtosvg
termtosvg
exit
```

## find命令技巧
```zsh
find . -type f -name "*.a" -print0  -exec rm  {} \;     
find . \(-path "./code" -o -path "./code2" \) -prune -o -name "*.txt" -print     //在当前目录及除code和code2之外的子目录中查找txt文件
# 可用于删除-开头的文件,  
# -print0 使用0(NULL)字符分割查找到的元素
# {}代表find出的文件名 
# \; 对;进行转义,代表rm命令的结束
```

## xargs命令技巧
一般情况下可使用\``来执行命令，然后将其输出作为命令行参数，达到使用只能接收命令行参数的命令,   
但需要处理的文件过多,会出现"Argument list too long"的错误。可使用xargs来解决
```zsh
<command1> | xargs [-n] [-d 'X']  [-I {}]  [command2 -p {} -l] 

# -n 限制每行输出n个元素, 输出m行,command2命令执行m次 
# -d 'X'  以X作为分隔符分割command1输出的内容
# -I {} 以{}位置作为xargs传递给command 的参数位置
# 使用-I时,command2以循环的方式执行。如果m=3，command2连同{}一起执行3次,{}会在每次执行中被替换为相应的参数
```

`xargs` 紧跟在管道操作符`|`之后,  默认使用执行`/bin/echo`。  
将数据以空格或换行符分隔成单个元素，然后调用指定命令并将这些元素作为该命令的参数,  类似于`find -exec`  
```zsh
cat target.txt | xargs  # 多行输入转单行输出
cat args.txt | xargs -I {} ./aa.sh -p {} -l # -I {} 指定aa.sh命令执行时替换字符串的位置标识
find /smbMount  -iname "*.docx" -print0 | xargs -0 grep "askDing"  # -iname 忽略大小写, -print0 -0都是以0作为分隔符 
```

## 并发执行()&/{}&

主要方法是使用 & 符号，将命令fork到后台执行,然后配合wait等待进程结束

```zsh
for ip in 192.168.8.{1..255}; do
  (
    ping $ip -c2 &> /dev/null
    if [ $? -eq 0 ]; then
      echo $ip is alive
    fi

)&
done
wait
```

## 利用管道和文件描述符FD_id实现并发数控制

- FD关联命令管道,具有管道特性，并且可以 无限存不阻塞，无限取不阻塞，而不用关心管道内是否为空，也不用关心是否有内容写入
- 命名管道控制并发数
- 操作FD

1. 创建一个管道并用指定FD_ID打开 `mkfifo /tmp/$$.fifo; exec 3<>/tmp/$$.fifo` 以当前进程PID创建fifo文件，防止冲突
2. 循环向FD_ID(关联到管道)输入任意字符(建议echo输入空白字符\n)    >创建进程数
3. 在循环体中，通过`read -u <文件描述符>` 读取管道中的数据，执行命令，然后`echo >&FD_ID`,补充进程数  >执行程序
4. 全部任务完成后，`exec FD_ID<&- && exec FD_ID>&-` 关闭管道

### mkfifo介绍 ###
管道具有存一个读一个，读完一个就少一个，没有则阻塞，放回的可以重复取，这正是队列特性

``` shell
[ -e /tmp/$$.fifo ] || mkfifo /tmp/$$.fifo  # 创建命名管道
exec 3<> /tmp/$$.fifo                       # 将FD关联到管道
rm -rf /tmp/$$.fifo                         # 删除管道文件，FD具有管道的一切特性，可通过FD来操作
echo >&8                                    # 向FD内输入`\n`
```


### exec命令、操作文件描述符
1. exec操作命令或脚本时：
- `exec xx.sh` : xx.sh会替换当前进程, 执行xx.sh，就不会再返回调用exec的程序。

2. exec操作文件描述符时：
- `exec 3</tmp/1.txt` 以只读方式打开/tmp/1.txt文件，文件描述符为3
- `exec 3>/tmp/1.txt` 以只写方式打开/tmp/1.txt文件，文件描述符为3
- `exec 3<>/tmp/1.txt` 以读写方式打开/tmp/1.txt文件，文件描述符为3
- `exec 3<&-` 关闭文件描述符3的读
- `exec 3>&-` 关闭文件描述符的写

``` shell
#!/bin/bash

thread=10                                 # 定义进程数
start_time=`date +%s`                       

[ -e /tmp/$$.fifo ] || mkfifo /tmp/$$.fifo # 创建命名管道文件

exec 3<> /tmp/$$fifo                      # 创建FD 3，以可读（<）可写（>）的方式关联管道文件，FD 3具有命名管道的特性
rm -rf /tmp/$$fifo                        # 删除命名管道文件，通过FD 3

for i in $(seq $thread); do
    echo >&3                              # 循环$thread次向FD 3写入\n , 类比一个令牌 
done

for i in $(seq 1000); do
    read -u 3                             # 循环读取FD 3中取\n , 直到读取位置
    {
	# 需要并行执行的命令放在此处
	sleep 1 && echo "$i  Done"

	# 最后需要归还令牌
	echo >&3                          # 再次向FD 3写入\n , 类似归还令牌
    }&                                    # 并发执行标志{}&，fork放在后台执行
done
wait                                      # 等待并发进程执行完毕，执行后续命令 

stop_time=`date +%s`

echo "TIME: `expr $stop_time-$start_time`"        

exec 3<&-                                 # 关闭FD 3的读
exec 3>&-                                 # 关闭FD 3的写
```




## 监视命令输出
```zsh
watch [-d] [-n <num>] command  # 每隔num秒更新一次command命令的输出, -d 标记输出差异
```
## 特殊文件权限  S t/T
权限模式

|文件类型|所有者|所属组|其他用户|
|-|-|-|-|
|-/b/c/d/l/p/s|r w x|r w x|r w x|

特殊权限均出现在执行权限(x)的位置
- `setuid权限` 允许其他用户执行此(ELF格式的二进制)文件会以文件拥有者的权限来运行 `-rwS------`  
```zsh
chmod u+s executeable_file   
```
- `setgid权限` 其他用户运行此文件时具有所属组权限`----rwS---`
```zsh
chmod g+s  directory_name/executeable_file #一般设置目录 ,该目录下的文件集成该目录的属性
```
- `目录sticky bit`  针对其他用户设置的,只有目录/文件所有者和root才能删除的文件`-------rwt`或`---------T` 如`/tmp`目录  
  - `t` 表示目录内文件有可执行权限
  - `T` 表示目录内文件无可执行权限
```zsh
chmod o+t <directory_name>
```


## 正则表达式可视化工具
[Regexpr
](https://regexper.com/)


## 对别名进行转义
在不可信环境下执行特权命令时，在命令前加上`\`忽略可能存在的别名




