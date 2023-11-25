---
title: "Linux 实用运维脚本分享"
date: "2023-03-10"
categories: 
  - "linux"
---

转载自：[点击蓝色名字订阅 Linux运维进阶之路 _2022-09-05 08:49_ _发表于上海_](https://mp.weixin.qq.com/s/oM2AQYxAFv648OLpr4LGaA)

```bash
#查看僵尸进程
ps -al | gawk '{print $2,$4}' | grep Z
# 匹配电子邮件的地址
cat index.html | egrep -o "[A-Za-z0-9._]+@[A-Za-z0-9.]+\.[a-zA-Z]{2,4}" > ans.txt
#匹配http URL
cat index.html | egrep -o "http://[A-Za-z0-9.]+\.[a-zA-Z]{2,3}" > ans.txt 
#纯文本形式下载网页
lynx -dump www.baidu.com > plain.txt
#只打印HTTP头部信息，无须远程下载文件
curl --head www.baidu.com
#使用POST提交数据
curl -d "param2=nickwolfe¶m2=12345" http://www.linuxidc.com/login.cgi
#显示分组途经的网关
traceroute www.baidu.com
#列出系统中的开放端口以及运行在端口上的服务
lsof -i 
#nc命令建立socket连接
#设置监听　nc -l 5555
#连接到套接字 nc 192.0.0.1 5555
#快速文件传输
#接收端　nc -l 5555 > destination_filename
#发送端　nc 192.0.0.1 5555 < source_filename
#找出指定目录最大的n个文件
du -ak target_dir | sort -nrk 1 | head -n 4
# du中a为递归,k为kb；sort中n为数字,r为降序,k指定列
#向终端中的所有登陆用户发送广播信息
cat message.txt | wall
#创建新的screen窗口
screen
#打印所有的.txt和.pdf文件
find . \( -name "*.txt" -o -name "*.pdf" \) -print
# -exec command {} \;是连用的，所有符合的都会放置在{}中，去执行command 
#将文件分割成多个大小为10kb的文件
split -b 10k data.file 
#打印两个文件的交集
comm A.txt B.txt -3 | sed 's/^\t//'
#sed移除空白行
sed '/^$/d' file
```

#### mysql备份

```sh
#!/bin/bash
set -e
USER="backup"
PASSWORD="backup"
# 数据库数据目录 #
DATA_DIR="/data/mysql"
BIN_INDEX=$DATA_DIR"/mysql-bin.index"
# 备份目录 #
BACKUP_DIR="/data/backup/mysql"
BACKUP_LOG="/var/log/mysql/backup.log"
DATE=`date +"%Y%m%d"`
TIME=`date +"%Y%m%d%H"`
LOG_TIME=`date +"%Y-%m-%d %H:%M:%S"`
DELETE_BINLOG_TIME="7 day"
INCREMENT_INTERVAL="3 hour"
note() {
    printf "[$LOG_TIME] note: $*\n" >> $BACKUP_LOG;
}
warning() {
    printf "[$LOG_TIME] warning: $*\n" >> $BACKUP_LOG;
}
error() {
    printf "[$LOG_TIME] error: $*\n" >> $BACKUP_LOG;
    exit 1;
}
full_backup() {
    local dbs=`ls -l $DATA_DIR | grep "^d" | awk -F " " '{print $9}'`
    for db in $dbs
    do
        local backup_dir=$BACKUP_DIR"/full/"$db
        local filename=$db"."$DATE
        local backup_file=$backup_dir"/"$filename".sql"
        if [ ! -d $backup_dir ]
        then
            mkdir -p $backup_dir || { error "创建数据库 $db 全量备份目录 $backup_dir 失败"; continue; }
            note "数据库 $db 全量备份目录 $backup_dir  不存在，创建完成";
        fi
        note "full backup $db start ..."
        mysqldump --user=${USER} --password=${PASSWORD} --flush-logs --skip-lock-tables --quick $db > $backup_file || { warning "数据库 $db 备份失败"; continue; }
        cd $backup_dir
        tar -cPzf $filename".tar.gz" $filename".sql"
        rm -f $backup_file
        chown -fR mysql:mysql $backup_dir
        note "数据库 $db 备份成功";
        note "full backup $db end."
    done
}
increment_backup() {
    local StartTime=`date "-d $INCREMENT_INTERVAL ago" +"%Y-%m-%d %H:%M:%S"`
    local DELETE_BINLOG_END_TIME=`date "-d $DELETE_BINLOG_TIME ago" +"%Y-%m-%d %H:%M:%S"`
    local dbs=`ls -l $DATA_DIR | grep "^d" | awk -F " " '{print $9}'`
    mysql -u$USER -p$PASSWORD -e "purge master logs before '$DELETE_BINLOG_END_TIME'" && note "delete $DELETE_BINLOG_TIME days before log";
    filename=`cat $BIN_INDEX | awk -F "/" '{print $2}'`
    for i in $filename
    do
        for db in $dbs
        do
            local backup_dir=$BACKUP_DIR"/increment/"$db
            local filename=$db"."$TIME
            local backup_file=$backup_dir"/"$filename".sql"
            if [ ! -d $backup_dir ]
            then
                mkdir -p $backup_dir || { error "创建数据库 $db 增量备份目录 $backup_dir 失败"; continue; }
                note "数据库 $db 增量备份目录 $backup_dir  不存在，创建完成";
            fi
            note "increment backup $db form time $StartTime start ..."
            mysqlbinlog -d $db --start-datetime="$StartTime" $DATA_DIR/$i >> $backup_file || { warning "数据库 $db 备份失败"; continue; }
            note "increment backup $db end."
        done
    done
    for db in $dbs
    do
        local backup_dir=$BACKUP_DIR"/increment/"$db
        local filename=$db"."$TIME
        local backup_file=$backup_dir"/"$filename".sql"
        cd $backup_dir
        tar -cPzf $filename".tar.gz" $filename".sql"
        rm -f $backup_file
        note "数据库 $db 备份成功";
    done
}
case "$1" in
    full)
        full_backup
    ;;
    increment)
        increment_backup
    ;;
    *)
        exit 2
    ;;
esac
exit 1
```

#### 目录备份

```sh
#!/bin/bash
#
#
# 时间
DATE=$(date '+%Y-%m-%d_%H_%M_%S')
# 备份目录 
BACKUPDIR="/home/backups"
# 需要备份的目录
SORFILE=/opt
# 目标文件名
DESFILE=/home/backups/$SORFILE.$(date '+%Y-%m-%d_%H_%M_%S').zip
[ ! -d $BACKUPDIR ] && mkdir -p $BACKUPDIR
cd $BACKUPDIR
echo "start backup $SORFILE ..."
sleep 3
#echo "$DESFILE"
#tar cvf $DESFILE $SORFILE
#gzip -f .zip $DESFILE
zip -r $DESFILE $SORFILE &>/dev/null
if [ "$?" == "0" ]
then
   echo $(date +%Y-%m-%d)" zip sucess">>backup.log
else
   echo $(date +%Y-%m-%d)" zip failed">>backup.log
   exit 0
fi
# 删除3天前的备份
find $BACKUPDIR -type f -ctime +3 | xargs rm -rf
```

#### PING查询

```sh
#!/bin/bash
#用途：根据网络配置对网络地址192.168.0进行修改，检查是否是活动状态
#{start..end}shell扩展生成一组地址
for ip in 192.168.0.{1..255}
do 
    (
    ping $ip -c 2 &> /dev/null 
    # > 标准输出重定向，和1>一致
    # 2>&1 将标准错误输出　重定向　到标准输出
    # &>file 将标准输出和标准错误输出都重定向到文件filename中
    if [ $? -eq 0 ];then
        echo $ip is alive
    fi
    )&
done
wait
#并行ping,加速
```

#### 磁盘IO检查

```sh
##iostat是查看磁盘活动统计情况
##显示所有设备负载情况 r/s:  每秒完成的读 I/O 设备次数。即 rio/s；w/s:  每秒完成的写 I/O 设备次数。即 wio/s等
iostat 
##每隔2秒刷新磁盘IO信息，并且每次显示3次
iostat 2 3
#显示某个磁盘的IO信息
iostat -d sda1
##显示tty和cpu信息
iostat -t
##以M为单位显示磁盘IO信息
iostat -m
##查看TPS和吞吐量信息  kB_read/s：每秒从设备（drive expressed）读取的数据量；kB_wrtn/s：每秒向设备（drive expressed）写入的数据量；kB_read：读取的总数据量；kB_wrtn：写入的总数量数据量；
iostat -d -k 1 1
#查看设备使用率（%util）、响应时间（await）
iostat -d -x -k 1 1
#查看CPU状态
iostat -c 1 3
#统计进程(pid)的stat,进程的stat自然包括进程的IO状况
pidstat
#只显示IO
pidstat -d  1 
#-d IO 信息,-r 缺页及内存信息-u CPU使用率-t 以线程为统计单位1  1秒统计一次
pidstat -u -r -d -t 1
#文件级IO分析,查看当前文件由哪些进程打开
lsof   
ls /proc/pid/fd
#利用 sar 报告磁盘 I/O 信息DEV 正在监视的块设备 tps 每秒钟物理设备的 I/O 传输总量 rd_sec/s 每秒从设备读取的扇区数量 wr_sec/s 每秒向设备写入的扇区数量 avgrq-sz I/O 请求的平均扇区数
#avgqu-sz I/O 请求的平均队列长度 await I/O 请求的平均等待时间，单位为毫秒 svctm I/O 请求的平均服务时间，单位为毫秒 %util I/O 请求所占用的时间的百分比，即设备利用率
sar -pd 10 3 
#iotop  top的io版
iotop
#查看页面缓存信息 其中的Cached 指用于pagecache的内存大小（diskcache-SwapCache）。随着写入缓存页，Dirty 的值会增加 一旦开始把缓存页写入硬盘,Writeback的值会增加直到写入结束。
cat /proc/meminfo 
#查看有多少个pdflush进程 Linux 用pdflush进程把数据从缓存页写入硬盘
#pdflush的行为受/proc/sys/vm中的参数的控制/proc/sys/vm/dirty_writeback_centisecs (default 500): 1/100秒, 多长时间唤醒pdflush将缓存页数据写入硬盘。默认5秒唤醒2个（更多个）线程。如果wrteback的时间长于dirty_writeback_centisecs的时间，可能会出问题
cat /proc/sys/vm/nr_pdflush_threads
#查看I/O 调度器
#调度算法
#noop anticipatory deadline [cfq] 
#deadline :    deadline 算法保证对既定的IO请求以最小的延迟时间。
#anticipatory：有个IO发生后，如果又有进程请求IO，则产生一个默认6ms猜测时间，猜测下一个进程请求IO是干什么。这对于随机读取会造成较大的延时。对数据库应用很糟糕，而对于Web Server等则会表现不错。
#cfq:        对每个进程维护一个IO队列，各个进程发来的IO请求会被cfq以轮循方式处理，对每一个IO请求都是公平。适合离散读的应用。
#noop:        对所有IO请求都用FIFO队列形式处理。默认IO不会存在性能问题。
cat /sys/block/[disk]/queue/scheduler
#改变IO调度器
$ echo deadline > /sys/block/sdX/queue/scheduler
#提高调度器请求队列的
$ echo 4096 > /sys/block/sdX/queue/nr_requests
```

#### 性能相关

```sh
#查看当前系统load
uptime
#查看系统状态和每个进程的系统资源使用状况
top
#可视化显示CPU的使用状况
htop
#查看每个CPU的负载信息
mpstat -P ALL 1
#每隔1秒查看磁盘IO的统计信息
iostat -xkdz 1
#每隔一秒查看虚拟内存的使用信息
vmstat 1
#查看内存使用统计信息
free
#查看网络使用信息
nicstat -z 1
#类似vmstat的显示优化的工具
dstat 1
#查看系统活动状态，比如系统分页统计，块设备IO统计等
sar
#网络连接状态查看
netstat -s
#进程资源使用信息查看
pidstat 1
pidstat -d 1
#查看某个进程的系统调用信息 -p后面是进程id，-tttT 进程系统后的系统调用时间
strace -tttT -p 12670
#统计IO设备输入输出的系统调用信息
strace -c dd if=/dev/zero of=/dev/null bs=512 count=1024k
#tcpdump 查看网络数据包
tcpdump -nr /tmp/out.tcpdump
#块设备的读写事件信息统计
btrace /dev/sdb 
#iotop查看某个进程的IO操作统计信息
iotop -bod5
#slabtop 查看内核 slab内存分配器的使用信息
slabtop -sc
#系统参数设置
sysctl -a
#系统性能指标统计信息
perf stat gzip file1
#系统cpu活动状态查看
perf record -a -g -F 997 sleep 10
```

#### 进程相关

```sh
## processes  进程管理
##ps查看当前系统执行的线程列表，进行瞬间状态，不是连续状态，连续状态需要使用top名称查看  更多常用参数请使用 man ps查看
ps
##显示所有进程详细信息
ps aux
##-u 显示某个用户的进程列表
ps -f -u www-data 
## -C 通过名字或者命令搜索进程
ps -C apache2
## --sort  根据进程cpu使用率降序排列，查看前5个进程  -pcpu表示降序  pcpu升序
ps aux --sort=-pcpu | head -5 
##-f 用树结构显示进程的层次关系，父子进程情况下
ps -f --forest -C apache2 
##显示一个父进程的所有子进程
ps -o pid,uname,comm -C apache2
ps --ppid 2359 
##显示一个进程的所有线程  -L 参数
ps -p 3150 -L 
##显示进程的执行时间 -o参数
ps -e -o pid,comm,etime 
##watch命令可以用来实时捕捉ps显示进程
watch -n 1 'ps -e -o pid,uname,cmd,pmem,pcpu --sort=-pmem,-pcpu | head -15' 
##jobs 查看后台运行的进程  jobs命令执行的结果，＋表示是一个当前的作业，减号表是是一个当前作业之后的一个作业，jobs -l选项可显示所有任务的PID,jobs的状态可以是running, stopped, Terminated,但是如果任务被终止了（kill），shell 从当前的shell环境已知的列表中删除任务的进程标识；也就是说，jobs命令显示的是当前shell环境中所起的后台正在运行或者被挂起的任务信息
jobs
##查看后台运营的进程号
jobs -p
##查看现在被终止或者退出的进程号
jobs -n
##kill命令 终止一个前台进程可以使用Ctrl+C键   kill  通过top或者ps获取进程id号  kill [-s 信号 | -p ] [ -a ] 进程号 ...
##发送指定的信号到相应进程。不指定型号将发送SIGTERM（15）终止指定进程。关闭进程号12的进程
kill 12
##等同于在前台运行PID为123的进程时按下Ctrl+C键
kill -2 123
##如果任无法终止该程序可用“-KILL” 参数，其发送的信号为SIGKILL(9) ，将强制结束进程  
kill -9 123
##列出所有信号名称
##HUP    1    终端断线
##INT     2    中断（同 Ctrl + C）
##QUIT    3    退出（同 Ctrl + \）
##TERM   15    终止
##KILL    9    强制终止
##CONT   18    继续（与STOP相反， fg/bg命令）
##STOP    19    暂停（同 Ctrl + Z）
kill -l
##得到指定信号的数值
kill -l KILL
##杀死指定用户所有进程
kill -u peidalinux
kill -9 $(ps -ef | grep peidalinux) 
##将后台中的命令调至前台继续运行  将进程123调至前台执行
fg 123
##将一个在后台暂停的命令，变成继续执行
bg  123
##该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。nohup就是不挂起的意思  下面输出被重定向到myout.file文件中
nohup command > myout.file 2>&1 &
##at：计划任务，在特定的时间执行某项工作，在特定的时间执行一次。
## 格式：at HH:MM YYYY-MM-DD //HH（小时）:MM（分钟） YYYY（年）-MM（月份）-DD（日）
##HH[am pm]+D(天) days //HH（小时）[am（上午）pm（下午）]+days（天）
at 12:00（时间） //at命令设定12:00执行一项操作
#at>useradd aaa //在at命令里设定添加用户aaa
#ctrl+d //退出at命令
#tail -f /etc/passwd //查看/etc/passwd文件后十行是否增加了一个用户aaa
##计划任务设定后，在没有执行之前我们可以用atq命令来查看系统没有执行工作任务。
atq
##启动计划任务后，如果不想启动设定好的计划任务可以使用atrm命令删除。
atrm 1 //删除计划任务1
##pstree命令：列出当前的进程，以及它们的树状结构  格式：pstree [选项] [pid|user]
pstree
##nice命令：改变程序执行的优先权等级 应用程序优先权值的范围从-20～19，数字越小，优先权就越高。一般情况下，普通应用程序的优先权值（CPU使用权值）都是0，如果让常用程序拥有较高的优先权等级，自然启动和运行速度都会快些。需要注意的是普通用户只能在0～19之间调整应用程序的优先权值，只有超级用户有权调整更高的优先权值（从-20～19）。
nice [-n <优先等级>][--help][--version][命令]
nice -n 5 ls
##sleep命令：使进程暂停执行一段时间
date;sleep 1m;date
##renice命令 renice命令允许用户修改一个正在运行进程的优先权。利用renice命令可以在命令执行时调整其优先权。
##其中，参数number与nice命令的number意义相同。（1） 用户只能对自己所有的进程使用renice命令。（2） root用户可以在任何进程上使用renice命令。（3） 只有root用户才能提高进程的优先权
renice -5 -p 5200  #PID为5200的进程nice设为-5 
##pmap命令用于显示一个或多个进程的内存状态。其报告进程的地址空间和内存状态信息 #pmap PID
pmap 20367
```

#### javadump.sh

```sh
#!/bin/sh
DUMP_PIDS=`ps  --no-heading -C java -f --width 1000 |awk '{print $2}'`
if [ -z "$DUMP_PIDS" ]; then
    echo "The server $HOST_NAME is not started!"
    exit 1;
fi
DUMP_ROOT=~/dump
if [ ! -d $DUMP_ROOT ]; then
  mkdir $DUMP_ROOT
fi
DUMP_DATE=`date +%Y%m%d%H%M%S`
DUMP_DIR=$DUMP_ROOT/dump-$DUMP_DATE
if [ ! -d $DUMP_DIR ]; then
  mkdir $DUMP_DIR
fi
for PID in $DUMP_PIDS ; do
#Full thread dump 用来查线程占用，死锁等问题
  $JAVA_HOME/bin/jstack $PID > $DUMP_DIR/jstack-$PID.dump 2>&1
  echo -e ".\c"
#打印出一个给定的Java进程、Java core文件或远程Debug服务器的Java配置信息，具体包括Java系统属性和JVM命令行参数。
  $JAVA_HOME/bin/jinfo $PID > $DUMP_DIR/jinfo-$PID.dump 2>&1
  echo -e ".\c"
#jstat能够动态打印jvm(Java Virtual Machine Statistics Monitoring Tool)的相关统计信息。如young gc执行的次数、full gc执行的次数，各个内存分区的空间大小和可使用量等信息。
  $JAVA_HOME/bin/jstat -gcutil $PID > $DUMP_DIR/jstat-gcutil-$PID.dump 2>&1
  echo -e ".\c"
  $JAVA_HOME/bin/jstat -gccapacity $PID > $DUMP_DIR/jstat-gccapacity-$PID.dump 2>&1
  echo -e ".\c"
#未指定选项时，jmap打印共享对象的映射。对每个目标VM加载的共享对象，其起始地址、映射大小及共享对象文件的完整路径将被打印出来，  
  $JAVA_HOME/bin/jmap $PID > $DUMP_DIR/jmap-$PID.dump 2>&1
  echo -e ".\c"
#-heap打印堆情况的概要信息，包括堆配置，各堆空间的容量、已使用和空闲情况  
  $JAVA_HOME/bin/jmap -heap $PID > $DUMP_DIR/jmap-heap-$PID.dump 2>&1
  echo -e ".\c"
#-dump将jvm的堆中内存信息输出到一个文件中,然后可以通过eclipse memory analyzer进行分析
#注意：这个jmap使用的时候jvm是处在假死状态的，只能在服务瘫痪的时候为了解决问题来使用，否则会造成服务中断。
  $JAVA_HOME/bin/jmap -dump:format=b,file=$DUMP_DIR/jmap-dump-$PID.dump $PID 2>&1
  echo -e ".\c"
#显示被进程打开的文件信息
if [ -r /usr/sbin/lsof ]; then
  /usr/sbin/lsof -p $PID > $DUMP_DIR/lsof-$PID.dump
  echo -e ".\c"
  fi
done
#主要负责收集、汇报与存储系统运行信息的。
if [ -r /usr/bin/sar ]; then
/usr/bin/sar > $DUMP_DIR/sar.dump
echo -e ".\c"
fi
#主要负责收集、汇报与存储系统运行信息的。
if [ -r /usr/bin/uptime ]; then
/usr/bin/uptime > $DUMP_DIR/uptime.dump
echo -e ".\c"
fi
#内存查看
if [ -r /usr/bin/free ]; then
/usr/bin/free -t > $DUMP_DIR/free.dump
echo -e ".\c"
fi
#可以得到关于进程、内存、内存分页、堵塞IO、traps及CPU活动的信息。
if [ -r /usr/bin/vmstat ]; then
/usr/bin/vmstat > $DUMP_DIR/vmstat.dump
echo -e ".\c"
fi
#报告与CPU相关的一些统计信息
if [ -r /usr/bin/mpstat ]; then
/usr/bin/mpstat > $DUMP_DIR/mpstat.dump
echo -e ".\c"
fi
#报告与IO相关的一些统计信息
if [ -r /usr/bin/iostat ]; then
/usr/bin/iostat > $DUMP_DIR/iostat.dump
echo -e ".\c"
fi
#报告与网络相关的一些统计信息
if [ -r /bin/netstat ]; then
/bin/netstat > $DUMP_DIR/netstat.dump
echo -e ".\c"
fi
echo "OK!"
```

#### 常用工具安装

```sh
#!/usr/bin/env bash
# ---------------------------------------------------------------------------------
# 控制台颜色
BLACK="\033[1;30m"
RED="\033[1;31m"
GREEN="\033[1;32m"
YELLOW="\033[1;33m"
BLUE="\033[1;34m"
PURPLE="\033[1;35m"
CYAN="\033[1;36m"
RESET="$(tput sgr0)"
# ---------------------------------------------------------------------------------
printf "${BLUE}\n"
cat << EOF
###################################################################################
# 安装常用命令工具
# 命令工具清单如下：
# 核心工具：df、du、chkconfig
# 网络工具：ifconfig、netstat、route、iptables
# IP工具：ip、ss、ping、tracepath、traceroute
# DNS工具：dig、host、nslookup、whois
# 端口工具：lsof、nc、telnet
# 下载工具：curl、wget
# 编辑工具：emacs、vim
# 流量工具：iftop、nethogs
# 抓包工具：tcpdump
# 压缩工具：unzip、zip
# 版本控制工具：git、subversion
#
###################################################################################
EOF
printf "${RESET}\n"
printf "\n${GREEN}>>>>>>>>> 安装常用命令工具开始${RESET}\n"
# 核心工具
printf "\n${CYAN}>>>> install coreutils(df、du)${RESET}\n"
yum install -y coreutils
printf "\n${CYAN}>>>> install chkconfig${RESET}\n"
yum install -y chkconfig
# 网络工具
printf "\n${CYAN}>>>> install net-tools(ifconfig、netstat、route)${RESET}\n"
yum install -y net-tools
printf "\n${CYAN}>>>> install iptables${RESET}\n"
yum install -y iptables
# IP工具
printf "\n${CYAN}>>>> install iputils(ping、tracepath)${RESET}\n"
yum install -y iputils
printf "\n${CYAN}>>>> install traceroute${RESET}\n"
yum install -y traceroute
printf "\n${CYAN}>>>> install iproute(ip、ss)${RESET}\n"
yum install -y iproute
# 端口工具
printf "\n${CYAN}>>>> install lsof${RESET}\n"
yum install -y lsof
printf "\n${CYAN}>>>> install nc${RESET}\n"
yum install -y nc
printf "\n${CYAN}>>>> install netstat${RESET}\n"
yum install -y netstat
# DNS工具
printf "\n${CYAN}>>>> install bind-utils(dig、host、nslookup)${RESET}\n"
yum install -y bind-utils
printf "\n${CYAN}>>>> install whois${RESET}\n"
yum install -y whois
# 下载工具
printf "\n${CYAN}>>>> install curl${RESET}\n"
yum install -y curl
printf "\n${CYAN}>>>> install wget${RESET}\n"
yum install -y wget
# 编辑工具
printf "\n${CYAN}>>>> install emacs${RESET}\n"
yum install -y emacs
printf "\n${CYAN}>>>> install vim${RESET}\n"
yum install -y vim
# 流量工具
printf "\n${CYAN}>>>> install iftop${RESET}\n"
yum install -y iftop
printf "\n${CYAN}>>>> install nethogs${RESET}\n"
yum install -y nethogs
# 抓包工具
printf "\n${CYAN}>>>> install tcpdump${RESET}\n"
yum install -y tcpdump
# 压缩工具
printf "\n${CYAN}>>>> install unzip${RESET}\n"
yum install -y unzip
# 版本控制工具
printf "\n${CYAN}>>>> install git${RESET}\n"
yum install -y git
printf "\n${CYAN}>>>> install subversion${RESET}\n"
yum install -y subversion
printf "\n${GREEN}<<<<<<<< 安装常用命令工具结束${RESET}\n"
```

#### 常用lib库安装

```bash
#!/usr/bin/env bash
# ---------------------------------------------------------------------------------
# 控制台颜色
BLACK="\033[1;30m"
RED="\033[1;31m"
GREEN="\033[1;32m"
YELLOW="\033[1;33m"
BLUE="\033[1;34m"
PURPLE="\033[1;35m"
CYAN="\033[1;36m"
RESET="$(tput sgr0)"
# ---------------------------------------------------------------------------------
printf "${BLUE}\n"
cat << EOF
###################################################################################
# 安装常见 lib
# 如果不知道命令在哪个 lib，可以使用 yum search xxx 来查找
# lib 清单如下：
# gcc gcc-c++ kernel-devel libtool
# openssl openssl-devel
# zlib zlib-devel
# pcre
###################################################################################
EOF
printf "${RESET}\n"
printf "\n${GREEN}>>>>>>>>> 安装常见 lib 开始${RESET}\n"
printf "\n${CYAN}>>>> install gcc gcc-c++ kernel-devel libtool${RESET}\n"
yum -y install make gcc gcc-c++ kernel-devel libtool
printf "\n${CYAN}>>>> install openssl openssl-devel${RESET}\n"
yum -y install make openssl openssl-devel
printf "\n${CYAN}>>>> install zlib zlib-devel${RESET}\n"
yum -y install make zlib zlib-devel
printf "\n${CYAN}>>>> install pcre${RESET}\n"
yum -y install pcre
printf "\n${GREEN}<<<<<<<< 安装常见 lib 结束${RESET}\n"
```

#### 系统检查脚本

```bash
#!/usr/bin/env bash
##############################################################################
# console color
C_RESET="$(tput sgr0)"
C_BLACK="\033[1;30m"
C_RED="\033[1;31m"
C_GREEN="\033[1;32m"
C_YELLOW="\033[1;33m"
C_BLUE="\033[1;34m"
C_PURPLE="\033[1;35m"
C_CYAN="\033[1;36m"
C_WHITE="\033[1;37m"
##############################################################################
printf "${C_PURPLE}"
cat << EOF
###################################################################################
# 系统信息检查脚本
###################################################################################
EOF
printf "${C_RESET}"
[[ $(id -u) -gt 0 ]] && echo "请用root用户执行此脚本！" && exit 1
sysversion=$(rpm -q centos-release | cut -d- -f3)
double_line="==============================================================="
line="----------------------------------------------"
# 打印头部信息
printHeadInfo() {
  cat << EOF
+---------------------------------------------------------------------------------+
|                           欢迎使用 【系统信息检查脚本】                          |
+---------------------------------------------------------------------------------+
EOF
}
# 打印尾部信息
printFootInfo() {
  cat << EOF
+---------------------------------------------------------------------------------+
|                            脚本执行结束，感谢使用！|
+---------------------------------------------------------------------------------+
EOF
}
options=( "获取系统信息" "获取服务信息" "获取CPU信息" "获取系统网络信息" "获取系统内存信息" "获取系统磁盘信息" "获取CPU/内存占用TOP10" "获取系统用户信息" "输出所有信息" "退出" )
printMenu() {
  printf "${C_BLUE}"
  printf "主菜单：\n"
  for i in "${!options[@]}"; do
    index=`expr ${i} + 1`
    val=`expr ${index} % 2`
    printf "\t(%02d) %-30s" "${index}" "${options[$i]}"
    if [[ ${val} -eq 0 ]]; then
      printf "\n"
    fi
  done
  printf "${C_BLUE}请输入需要执行的指令：\n"
  printf "${C_RESET}"
}
# 获取系统信息
get_systatus_info() {
  sys_os=$(uname -o)
  sys_release=$(cat /etc/redhat-release)
  sys_kernel=$(uname -r)
  sys_hostname=$(hostname)
  sys_selinux=$(getenforce)
  sys_lang=$(echo $LANG)
  sys_lastreboot=$(who -b | awk '{print $3,$4}')
  sys_runtime=$(uptime | awk '{print  $3,$4}' | cut -d, -f1)
  sys_time=$(date)
  sys_load=$(uptime | cut -d: -f5)
  cat << EOF
【系统信息】
系统: ${sys_os}
发行版本:   ${sys_release}
系统内核:   ${sys_kernel}
主机名:    ${sys_hostname}
selinux状态:  ${sys_selinux}
系统语言:   ${sys_lang}
系统当前时间: ${sys_time}
系统最后重启时间:   ${sys_lastreboot}
系统运行时间: ${sys_runtime}
系统负载:   ${sys_load}
EOF
}
# 获取CPU信息
get_cpu_info() {
  Physical_CPUs=$(grep "physical id" /proc/cpuinfo | sort | uniq | wc -l)
  Virt_CPUs=$(grep "processor" /proc/cpuinfo | wc -l)
  CPU_Kernels=$(grep "cores" /proc/cpuinfo | uniq | awk -F ': ' '{print $2}')
  CPU_Type=$(grep "model name" /proc/cpuinfo | awk -F ': ' '{print $2}' | sort | uniq)
  CPU_Arch=$(uname -m)
  cat << EOF
【CPU信息】
物理CPU个数:$Physical_CPUs
逻辑CPU个数:$Virt_CPUs
每CPU核心数:$CPU_Kernels
CPU型号:$CPU_Type
CPU架构:$CPU_Arch
EOF
}
# 获取服务信息
get_service_info() {
  port_listen=$(netstat -lntup | grep -v "Active Internet")
  kernel_config=$(sysctl -p 2> /dev/null)
  if [[ ${sysversion} -gt 6 ]]; then
    service_config=$(systemctl list-unit-files --type=service --state=enabled | grep "enabled")
    run_service=$(systemctl list-units --type=service --state=running | grep ".service")
  else
    service_config=$(/sbin/chkconfig | grep -E ":on|:启用" | column -t)
    run_service=$(/sbin/service --status-all | grep -E "running")
  fi
  cat << EOF
【服务信息】
${service_config}
  ${line}
运行的服务:
${run_service}
  ${line}
监听端口:
${port_listen}
  ${line}
内核参考配置:
${kernel_config}
EOF
}
# 获取系统内存信息
get_mem_info() {
  check_mem=$(free -m)
  MemTotal=$(grep MemTotal /proc/meminfo | awk '{print $2}') #KB
  MemFree=$(grep MemFree /proc/meminfo | awk '{print $2}') #KB
  let MemUsed=MemTotal-MemFree
  MemPercent=$(awk "BEGIN {if($MemTotal==0){printf 100}else{printf \"%.2f\",$MemUsed*100/$MemTotal}}")
  report_MemTotal="$((MemTotal/1024))" "MB" #内存总容量(MB)
  report_MemFree="$((MemFree/1024))" "MB" #内存剩余(MB)
  report_MemUsedPercent=$(free | sed -n '2p' | gawk 'x = int(( $3 / $2 ) * 100) {print x}' | sed 's/$/%/')
  cat << EOF
【内存信息】
内存总容量(MB): ${report_MemTotal}
内存剩余量(MB):${report_MemFree}
内存使用率: ${report_MemUsedPercent}
EOF
}
# 获取系统网络信息
get_net_info() {
  pri_ipadd=$(ip addr | awk '/^[0-9]+: / {}; /inet.*global/ {print gensub(/(.*)\/(.*)/, "\\1", "g", $2)}')
  pub_ipadd=$(curl ifconfig.me -s)
  gateway=$(ip route | grep default | awk '{print $3}')
  mac_info=$(ip link | egrep -v "lo" | grep link | awk '{print $2}')
  dns_config=$(egrep -v "^$|^#" /etc/resolv.conf)
  route_info=$(route -n)
  cat << EOF
【网络信息】
系统公网地址:${pub_ipadd}
系统私网地址:${pri_ipadd}
网关地址:${gateway}
MAC地址:${mac_info}
路由信息:
${route_info}
DNS 信息:
${dns_config}
EOF
}
# 获取系统磁盘信息
get_disk_info() {
  disk_info=$(fdisk -l | grep "Disk /dev" | cut -d, -f1)
  disk_use=$(df -hTP | awk '$2!="tmpfs"{print}')
  disk_percent=$(free | sed -n '2p' | gawk 'x = int(( $3 / $2 ) * 100) {print x}' | sed 's/$/%/')
  disk_inode=$(df -hiP | awk '$1!="tmpfs"{print}')
  cat << EOF
【磁盘信息】
${disk_info}
磁盘使用: ${disk_use}
磁盘使用百分比: ${disk_percent}
inode信息: ${disk_inode}
EOF
}
# 获取系统用户信息
get_sys_user() {
  login_user=$(awk -F: '{if ($NF=="/bin/bash") print $0}' /etc/passwd)
  ssh_config=$(egrep -v "^#|^$" /etc/ssh/sshd_config)
  sudo_config=$(egrep -v "^#|^$" /etc/sudoers | grep -v "^Defaults")
  host_config=$(egrep -v "^#|^$" /etc/hosts)
  crond_config=$(for cronuser in /var/spool/cron/*; do
    ls ${cronuser} 2> /dev/null | cut -d/ -f5; egrep -v "^$|^#" ${cronuser} 2> /dev/null;
    echo "";
  done)
  cat << EOF
【用户信息】
系统登录用户:
${login_user}
  ${line}
ssh 配置信息:
${ssh_config}
  ${line}
sudo 配置用户:
${sudo_config}
  ${line}
定时任务配置:
${crond_config}
  ${line}
hosts 信息:
${host_config}
EOF
}
# 获取CPU/内存占用TOP10
get_process_top_info() {
  top_title=$(top -b n1 | head -7 | tail -1)
  cpu_top10=$(top -b n1 | head -17 | tail -11)
  mem_top10=$(top -b n1 | head -17 | tail -10 | sort -k10 -r)
  cat << EOF
【TOP10】
CPU占用TOP10:
${cpu_top10}
内存占用TOP10:
${top_title}
  ${mem_top10}
EOF
}
show_dead_process() {
  printf "僵尸进程：\n"
  ps -al | gawk '{print $2,$4}' | grep Z
}
get_all_info() {
  get_systatus_info
  echo ${double_line}
  get_service_info
  echo ${double_line}
  get_cpu_info
  echo ${double_line}
  get_net_info
  echo ${double_line}
  get_mem_info
  echo ${double_line}
  get_disk_info
  echo ${double_line}
  get_process_top_info
  echo ${double_line}
  get_sys_user
}
main() {
  while [[ 1 ]]
  do
    printMenu
    read option
    local index=$[ ${option} - 1 ]
    case ${options[${index}]} in
      "获取系统信息")
        get_systatus_info ;;
      "获取服务信息")
        get_service_info ;;
      "获取CPU信息")
        get_cpu_info ;;
      "获取系统网络信息")
        get_net_info ;;
      "获取系统内存信息")
        get_mem_info ;;
      "获取系统磁盘信息")
        get_disk_info ;;
      "获取CPU/内存占用TOP10")
        get_process_top_info ;;
      "获取系统用户信息")
        get_sys_user ;;
      "输出所有信息")
        get_all_info > sys.log
        printf "${C_GREEN}信息已经输出到 sys.log 中。${C_RESET}\n\n"
      ;;
      "退出")
        exit ;;
      *)
        clear
        echo "抱歉，不支持此选项" ;;
    esac
  done
}
######################################## MAIN ########################################
printHeadInfo
main
printFootInfo
printf "${C_RESET}"
```

#### sed进阶

```bash
#!/bin/bash
#多个空格只保留一个
#sed '/./,/^$/!d' test
#删除开头的空白行
#sed '/./,$!d' test
#删除结尾的空白行
sed '{
:start
/^\n*$/{$d; N; b start}
}' test
#删除html标签
#有问题
#s/<.*>//g
#sed 's/<[^>]*>//g' test1
#sed 's/<[^>]*>//g;/^$/d' test1
#and符号，代表替换命令中的匹配模式，不管预定义模式是什么文本，都可以用and符号替换，and符号会提取匹配替换命令中指定替换模式中的所有字符串
echo "The cat sleeps in his hat" | sed 's/.at/"&"/g'
#替换单独的单词
echo "The System Administrator manual" | sed 's/\(System\) Administrator/\1 user/'
#在长数字中插入逗号
echo "1234567" | sed '{:start; s/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/; t start}'
#给文件中的行编号
sed '=' test | sed 'N; s/\n/ /'
```

\- EOF -
