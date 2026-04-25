# linux常用命令第2弹

1. top 实时监控系统
---
类似于 Windows 任务管理器，实时查看 CPU、内存、进程。![alt text](image.png)

1. ps 查看进程瞬间状态
显示系统进程在瞬间的运行动态 
ps的选项非常之多，建议记住以下两个命令
---
```
ps -ef    # 显示所有进程完整信息
ps -aux   # 显示详细资源占用
```

ps -ef（常用）

![alt text](image-1.png) 
* uid表示进程所属用户，pid表示进程的编号，ppid表示他的父进程，cmd是进程的名称
ps -aux

![alt text](image-2.png)

2. df 查看磁盘空间
```
df -h     # 人性化显示磁盘容量
df -Th    # 显示文件系统类型
df -ih    # 查看 Inode 占用情况
```

执行实例：

![alt text](image-3.png)

|列名 | 含义 |
|----|----|
|Filesystem	| 分区对应的设备名，比如 /dev/sda1 是硬盘分区|
|Type|文件系统类型，服务器常用 xfs 或 ext4|
|Size|分区总容量|
|Used|已使用容量|
|Avail|剩余可用容量|
|Use%|已用百分比，超过 80% 就需要关注清理了|
|Mounted on	|分区挂载的目录（挂载点），/ 是系统根目录|

### 进程管理
1. kill [信号类型] 进程PID
---
kill 命令用于关闭一个进程  
其中信号类型有很多种,可以通过kill-I查看所有信号类型。常用的信号类型有SIGKILL,对应的数字 为9,还有SIGTERM和SIGINT,对应的数字分别为15和2。

kill -9 进程PID:表示强制结束进程

kill -2 进程PID:表示结束进程,但是并不是强制性的,常用的ctl+c组合键发出的就是一个 kil-2的信号。

kill -15 进程PID:表示正常结束进程,是kill的缺省选项,也就是ki不加任何信号类型时,默 认类型就是15。

PID是Linux系统中用于区分不同进程的唯一标识，最大为32768，所有的进程都是PID为1的systemd的进程后代

2. kill PID 关闭进程
```kill PID        # 温柔关闭
kill -9 PID     # 强制杀死（最常用）
pkill 进程名     # 不用PID，直接按名字杀
```

3. killall 按进程名批量关闭
killall也用于关闭进程，与kill不同的是，killall后面接进程的名字，而不是进程PID，因此可以关闭一组进程

```
killall 进程名称 #示例killall nginx
killall -9 进行名称 #killall -9 nginx
```
### 文件操作
1. cp 复制
将给出的文件或者目录拷贝到宁一个文件或者目录中

```
cp 源文件 目标文件       # 复制文件
cp -r 源目录 目标目录    # 递归复制目录
cp -a 源文件 目标文件    # 保留属性完整复制
```

* cp拷贝文件的时候，如果出现覆盖问题，默认是添加了“-i”选项(如root)
* cp拷贝文件只能把一个文件复制成另一个文件，cp后面跟多个文件可以把这些文件复制到一个目录中，那么最后必须跟一个目录
实例
![alt text](image-4.png)

2. mv 移动 / 重命名

mv是将文件/目录改名，或者将文件由一个目录移动到宁外一个目录

mv相比于cp多了一个删除文件的动作，mv将源文件复制目录下之后会删除源文件 