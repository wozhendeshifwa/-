



shell是我们通过命令行与操作系统沟通的语言



shell脚本语言有两种执行方式；

1. 作为可执行文件

   ```bash
   chmod +x test.sh
   ./test.sh
   ```

2. 指定解释器

   ```bash
   bash test.sh
   ```

# 变量：

1. 变量定义

   ```bash
   name1="myf" #可以加上双引号
   name2='myf' #可以加上单引号
   name3=myc   #shell变量默认类型为 字符串
   ```

2. 变量使用

   ```bash
   # 使用 ${} 即可, 不加也可以。但加上之后，便于识别变量边界
   echo ${name1}
   echo $name2
   echo ${name3}acwing 
   echo $name3acwing
   ```

只读变量：

1. ```bash
   readonly name1=yxc
   echo ${name1}
   ```

   ```bash
   declare -r name1=yxc #两种写法
   echo ${name1}
   ```

环境变量 - 可以在当前进程和子进程之下使用

```bash
# 第一种写法
export name1=yxc
# 第二种写法
declare -x name1=yxc
```

```bash
# 将一个环境变量变成普通变量
declare -x name1
```

# 字符串

1. 单引号

   ```bash
   # 单引号中的内容会原样输出，不会执行，也不会取变量
   echo 'acwing ${name1}'
   ```

2. 双引号

   ```bash
   # 双引号中的内容可以执行，也可以取变量
   echo "acwing ${name1}"
   ```

3. 字符串长度

   ```shell
   # 字符串长度
   name1=yxcacwingacwingacwingacwing
   echo ${#name1}
   ```

4. 字符串字串

   ```shell
   # 字符串字串
   name1=yxcacwingacwingacwing
   echo ${name1:2:5}
   ```

   

   # 默认变量
   
   文件参数变量
   
   ``` 
   $0 -- 代表文件路径
   $1 -- 代表传入的第一个文件参数
   $2 -- 代表传入的第二个文件参数
   $3 -- 代表传入的第三个文件惨呼
   ${12} -- 代表传入的第十二个文件参数
   $# -- 文件参数个数
   $* -- 所有文件参数的展示
   $@ 所有参数列表，每个视为独立字符串（推荐遍历用）
   `ls`  ls命令的stdio
   $(ls) ls命令的stdio
   $?
   ```
   
   # 数组
   
   ```bash
   # 第一种定义方法
   a=(fesaifjh dfoapwjdop dfjawoijd 12 j)
   # 第二种定义方法
   a[0]=daw
   a[1]=21
   a[2]=3
   a[4]=4daw
   ```
   
   ```bash
   ${a[*]} -- 代表所有数组 
   ```
   
   # expr 命令
   
   ```shell
   a=21
   b=3
   echo `expr ${a} + ${b}` # a + b
   echo `expr ${a} - ${b}` # a - b
   echo `expr ${a} \* ${b}` # a * b
   echo `expr ${a} / ${b}` # a / b
   echo `expr \( a + b \) \* \( a - b \)` # 单行命令实现 (a + b) * (a - b)`
   ```
   
   # read命令
   
   ```bash
   # 从标准输入读取一行，赋值给一个或多个变量
   read -p "What's your name? " -t 5 name1
   echo hello ${name1}!
   echo hello ${name1}!
   ```
   
   # echo命令
   
   ```bash
   # 开启换行
   echo -e “ddwa\n3412123”
   # 输出重定向
   echo “hello world” > hello.txt # 将内容以覆盖的形式输出到hello.txt
   ```
   
   # printf格式化输出
   
   ```bash
   printf "%-10d\n" 1223  # 左对齐
   printf "%-10.2lf\n" 21.321 # 保留2位小数
   printf "hello world%s\n" "acwing yxc" # 格式化输出字符串
   ```
   
   # 逻辑运算符 && 和 ||
   
   ```bash
   # $$ 表示与 
   # || 表示或
   # 二者具有短路原则
   expr1 $$ expr2 # 当 expr1 为假时，直接忽略 expr2
   expr1 || expr2 # 当 expr1 为真时，直接忽略 expr2
   ```
   
   # test 命令 完全可以用 [] 和 [[]] 指令替代
   
   ```bash
   # test 命令用于判断
   [ 2 -lt 3 ]
   echo $? # test [] [[]] 用 exit code 返回结果，而不是使用 stdout  
    # 使用进程执行状态运行结果 表示
   ```
   
   ```bash
   # 文件类型判断
   [ -e hello.txt ] # hello.txt 文件是否存在
   [ -f hello.txt ] # hello.txt 是否为文件
   [ -d hello.txt ] # hello.txt 是否为文件夹
   ```
   
   ```bash
   # 文件权限判断
   [ -r hello.txt ] # hello.txt 是否可读
   [ -w hello.txt ] # hello.txt 是否可写
   [ -x hello.txt ] # hello.txt 是否可执行 
   ```
   
   # if 判断语句
   
   ```shell
   # 单循环
   if [ condition ] && [ condition ]
   then 
   	expr
   else 
   	expr
   fi
   ```
   
   ```shell
   # 多重循环
   if [ condition_1 ] $$ [ condition_2 ] # condition_1 && condition_2 ?
   then
   	expr
   elif [ condition_3 ] || [ condition_4 ] # condition_3 || condition_4 ?
   then 
   	expr
   elif [[ ( condition_5 && condition_6 ) || ( condition_7 && condition_8 ) ]]# 判断( condition_5 && condition_6 ) || ( condition_7 && condition_8 ) 
   then
   	expr
   fi  
   如有问题，请纠正，并根据注释，补全代码
   ```
   
   # 循环语句
   
   ```bash
   for var in var1 var2 var3
   do
   	xxx
   done
   ```
   
   ```bash
   
   ```
   
   

# 