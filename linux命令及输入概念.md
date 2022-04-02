## 数据流定义

`linux`中，一个程序启动（命令语句）会自动开启三个数据流通道：标准输入流（`stdin:0`）、标准输出流（`stdout:1`）、标准错误输出流（`stderr:2`）。

>例子

输入命令时，`linux`打印输出的正常信息为标准输出流，报错的信息为标准错误输出流，键盘载入的信息为标准输出流。

### 重定向

`linux`中，标准输入流默认来自键盘输入，标准输出流和标准错误流默认发送到屏幕。必要的时候可以修改输入流的来源、输出流的目的地，改变IO流的输入和输出位置就是重定向。

```shell
# 常用的重定向符号
> 将标准输出流重定向到文件（清空文件后加入）
>> 将标准输出流重定向到文件（追加写入）
< 将文件作为命令的标准输入流
```
> 标准输出流重定向

```shell
# 输出流重定向
ls > a.txt

# 将ls命令结果追加写入到文件
ls >> a.txt
```

![image-20220310114602246](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310114602246.png)

<img src="C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310114551922.png" alt="image-20220310114551922" style="zoom:200%;" />

追加

![image-20220310114639950](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310114639950.png)

![image-20220310114623816](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310114623816.png)

>错误流重定向

```shell
rm / > b.txt
```

![image-20220310115051762](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310115051762.png)

![image-20220310115012311](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310115012311.png)

`rm / `删除根目录会报错，且错误信息依然被输出到屏幕上。

> `">"`默认只会重定向标准输出流，而不会重定向标准错误流。

将标准错误流重定向，重定向符号前加上`文件描述符2`

```shell
rm / 2>b.txt
```

![image-20220310152159958](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310152159958.png)

![image-20220310152152016](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310152152016.png)



>输出信息有标准输出和错误输出两种情况

```shell
cat a.txt c.txt >output.txt 2>error.txt
```

`cat`查看`a.txt`和`b.txt`中的内容，将其中存在的内容通过`>`输出到`output.txt`中，不存在的错误信息通过`2>`输出到`error.txt`中。

![image-20220310175100696](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310175100696.png)

![image-20220310175051302](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220310175051302.png)

>管道符

`"|"`可以把一个程序的标准输出流作为另一个程序的标准输入流，即前一个程序的输出作为后一个程序的输入。

```shell
wc -l $binFile | awk '{print $1}'
```

`wc -l`统计`binFile`的行数，并将输出结果传递给`awk`，`awk` 匹配去掉多余的部分，标准化输出行数信息。

### 全量备份脚本

```shell
#!/bin/bash
#在使用之前，请提前创建以下各个目录
#获取当前时间
date_now=$(date "+%Y%m%d-%H%M%S")
# 数据库备份路径
backUpFolder="/data/backup"
# mysql账号密码
username="root"
password="5KVnjy0M6x)"
# 需要备份的数据库
db_name="aos-beijing-8090"
#定义备份文件名
fileName="${db_name}_${date_now}.sql"
#定义备份文件目录
backUpFileName="${backUpFolder}/${fileName}"
echo "starting backup mysql ${db_name} at ${date_now}."
# mysqldump 安装路径 一般为mysql/bin 路径下
/usr/local/mysql8/mysql-8.0.23-linux-glibc2.12-x86_64/bin/mysqldump -u${username} -p${password}  --lock-all-tables --flush-logs ${db_name} > ${backUpFileName}
#进入到备份文件目录
cd ${backUpFolder}
#压缩备份文件
tar zcvf ${fileName}.tar.gz ${fileName}

date_end=$(date "+%Y%m%d-%H%M%S")
echo "finish backup mysql database ${db_name} at ${date_end}."
```

将sh脚本部署在`linux`上，赋予脚本权限并执行

![image-20220311172315161](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220311172315161.png)

成功备份

![image-20220311172410245](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220311172410245.png)

>`--flush-logs`：清除日志信息
>
>`--lock-all-tables`：锁定所有数据库
>
>`tar zcvf`：压缩格式为 `tar.gz`

