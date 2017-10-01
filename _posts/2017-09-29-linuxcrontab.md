---
layout: post
title:  linux中的定时任务crontab命令
date:   2017-09-29
tag: linux 学习
---

### crontab命令

crontab是用于设置周期性执行的命令，通过crontab，我们可以在固定的时间执行指定的系统指令或者是shell脚本。

#### 1.命令格式

crontab \[-u user\]file

crontab \[-u user\] \[-e|-l|-r|-i\]

#### 2.参数介绍

>* -u 设定某个用户的crontab文件内容
>* file 是命令文件的名字，表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
>* -e 编辑当前用户的crontab文件内容
>* -l 列出当前用户的crontab内的文件内容
>* -r 删除/var/spool/cron目录下当前用户的crontab文件
>* -i 在删除用户的cron文件之前进行确认

#### 3.crontab文件格式

分 时 日 月 周 命令或者脚本名字

>* 第1列分钟0～59
>* 第2列小时0～23（0表示子夜）
>* 第3列日1～31
>* 第4列月1～12
>* 第5列星期0～7（0和7表示星期天）

##### 操作符号

在一个区域里填写多个数值的方法：

>* 逗号（','）分开的值，例如：“1,3,4,7,8”
>* 连词符（'-'）指定值的范围，例如：“1-6”，意思等同于“1,2,3,4,5,6”
>* 星号（'*'）代表任何可能的值。例如，在“小时域”里的星号等于是“每一个小时”，等等

某些cron程序的扩展版本也支持斜线（'/'）操作符，用于表示跳过某些给定的数。例如，“*/3”在小时域中等于“0,3,6,9,12,15,18,21”等被3整除的数；

例1:

    5,25 * * * * echo "tets" #每小时的第5和第25分钟执行echo命令
    
例2：

    5,25 7-12 * * * echo "test"  #在上午7点到12点的第5和第25分钟执行
    
例3：

    5,25 8-11 */3  *  * echo "test" #每隔三天的上午8点到11点的第5和第25分钟执行

#### 4.cron服务的启动与停止

    $sudo /etc/init.d/cron start    #启动cron服务
$sudo /etc/init.d/cron stop     #停止服务
    $sudo /etc/init.d/cron restart  #重启服务
$sudo /etc/init.d/cron reload   #重新加载服务

#### 5.常见错误

一个常见的错误是，命令行双引号中使用%时，未加反斜线\，例如：

##### 错误的例子：

    1 2 3 4 5 touch ~/error_`date "+%Y%m%d"`.txt

在守护进程发出的电子邮件中会见到错误信息：

/bin/sh: unexpected EOF while looking for `'''''''

##### 正确的例子：
    1 2 3 4 5 touch ~/right_$（date +\%Y\%m\%d）.txt

##### 使用单引号也可以解决问题：

    1 2 3 4 5 touch ~/error_$（date '+%Y%m%d'）.txt

##### 使用单引号就不用加反斜线了。这个例子会产生这样一个文件~/error_\2006\04\03.txt

    1 2 3 4 5 touch ~/error_$（date '+\%Y\%m\%d'）.txt

下例是另一个常见错误：

##### 准备夏令时转换

    59 1 1-7 4 0 /root/shift_my_times.sh

##### 初看似要在四月的第一个星期日早晨1时59分运行shift_my_times.sh，但是这样设置不对。与其他域不同，第三和第五个域之间执行的是“或”操作。所以这个程序会在4月1日至7日以及4月余下的每一个星期日执行。这个例子可以重写如下：

##### 准备夏令时转换

    59 1 1-7 4 * test `date +\%w` = 0 && /root/shift_my_times.sh

另一个常见错误是对分钟设置的误用。下例欲一个程两个小时运行一次：

##### 将时间保存为日志文件

    * 0,2,4,6,8,10,12,14,16,18,20,22 * * * date >> /var/log/date.log

而上述设置会使该程序在偶数小时内的每一分钟执行一次。正确的设置是：

##### 每隔两个小时运行一次date命令

    0 0,2,4,6,8,10,12,14,16,18,20,22 * * * date >> /var/log/date.log

##### 简化写法

    0 */2 * * * date >> /var/log/date.log
    
#### 6.注意事项

##### 注意环境变量问题

有时我们创建了一个crontab，但是这个任务却无法自动执行，而手动执行这个任务却没有问题，这种情况一般是由于在crontab文件中没有配置环境变量引起的。

在crontab文件中定义多个调度任务时，需要特别注环境变量的设置，因为我们手动执行某个任务时，是在当前shell环境下进行的，程序当然能找到环境变量，而系统自动执行任务调度时，是不会加载任何环境变量的，因此，就需要在crontab文件中指定任务运行所需的所有环境变量，这样，系统执行任务调度时就没有问题了。

不要假定cron知道所需要的特殊环境，它其实并不知道。所以你要保证在shelll脚本中提供所有必要的路径和环境变量，除了一些自动设置的全局变量。所以注意如下3点：

①脚本中涉及文件路径时写全局路径；
②脚本执行要用到java或其他环境变量时，通过source命令引入环境变量，如:

    cat start_cbp.sh
    !/bin/sh
    source /etc/profile
    export RUN_CONF=/home/d139/conf/platform/cbp/cbp_jboss.conf
    /usr/local/jboss-4.0.5/bin/run.sh -c mev &

③当手动执行脚本OK，但是crontab死活不执行时,很可能是环境变量惹的祸，可尝试在crontab中直接引入环境变量解决问题。如:

    0 * * * * . /etc/profile;/bin/sh 
    /var/www/java/audit_no_count/bin/restart_audit.sh

##### 注意清理系统用户的邮件日志

每条任务调度执行完毕，系统都会将任务输出信息通过电子邮件的形式发送给当前系统用户，这样日积月累，日志信息会非常大，可能会影响系统的正常运行，因此，将每条任务进行重定向处理非常重要。 例如，可以在crontab文件中设置如下形式，忽略日志输出:
    
    0 */3 * * * /usr/local/apache2/apachectl restart >/dev/null 2>&1

“/dev/null 2>&1”表示先将标准输出重定向到/dev/null，然后将标准错误重定向到标准输出，由于标准输出已经重定向到了/dev/null，因此标准错误也会重定向到/dev/null，这样日志输出问题就解决了。

##### 系统级任务调度与用户级任务调度

系统级任务调度主要完成系统的一些维护操作，用户级任务调度主要完成用户自定义的一些任务，可以将用户级任务调度放到系统级任务调度来完成（不建议这么做），但是反过来却不行，root用户的任务调度操作可以通过”crontab –uroot –e”来设置，也可以将调度任务直接写入/etc/crontab文件，需要注意的是，如果要定义一个定时重启系统的任务，就必须将任务放到/etc/crontab文件，即使在root用户下创建一个定时重启系统的任务也是无效的。

##### 其他注意事项

新创建的cron job，不会马上执行，至少要过2分钟才执行。如果重启cron则马上执行。

当crontab失效时，可以尝试/etc/init.d/crond restart解决问题。或者查看日志看某个job有没有执行/报错tail -f /var/log/cron。

千万别乱运行crontab -r。它从Crontab目录（/var/spool/cron）中删除用户的Crontab文件。删除了该用户的所有crontab都没了。

在crontab中%是有特殊含义的，表示换行的意思。如果要用的话必须进行转义%，如经常用的date ‘+%Y%m%d’在crontab里是不会执行的，应该换成date ‘+%Y%m%d’。

更新系统时间时区后需要重启cron,在ubuntu中服务名为cron:


    $service cron restart
    
#### 参考文档
1.[linux Tool Quick Tutorial](http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/crontab.html)
2.[全面解析Linux计划任务cron](http://os.51cto.com/art/201003/187722.htm)
3.[维基百科](https://zh.wikipedia.org/wiki/Cron)




