# linux基本命令
严格区分大小写
***
pwd 显示你当前终端所在的绝对路径
```
[root@localhost ~]# pwd
/root  # 输出：当前在root管理员的家目录

|参数|作用|
|----|----|
|-l|显示逻辑路径（软链接本身的路径|
|-p|显示实际物理路径（穿透软链接，显示真实目录|

# 创建一个软链接，把/home/dev 链接到 /tmp/dev_link
ln -s /home/dev /tmp/dev_link
# 进入软链接目录
cd /tmp/dev_link
# 用-L显示逻辑路径（软链接路径）
pwd -L  # 输出 /tmp/dev_link
# 用-P显示实际路径（真实目录）
pwd -P  # 输出 /home/dev
```
cd 切换当前工作目录 

|命令|作用|
|----|----|
|cd /home|切换到 /home 目录（绝对路径，不受当前位置影响）|
|cd ~或cd|直接回到当前用户的家目录（root 用户就是 /root）|
|cd ..|回到上一级目录（比如现在在 /home，cd .. 就回到 / 目录）|
|cd -|回到上一次所在的目录（比如刚才从 /root 到 /home，cd - 就切回 /root）|

clear  清屏命令
```
# 任意提示符下直接执行
[root@localhost home]# clear
```
* 终端里所有之前的命令、报错、输出都会被清空；
* 只剩下当前提示符（比如 [root@localhost home]#），视觉上回到 “全新终端” 的状态。

CentOS 7 中，Ctrl + L 快捷键和 clear 效果完全一致

* clear 只是清空屏幕显示，不会删除你的命令历史：按「上箭头」还是能翻到之前执行过的 cd/ls/pwd 命令；
* 如果想彻底清除命令历史记录（连「上箭头」都翻不到），可以用 history -c，但这和清屏是两个不同的功能，别混淆使用。

whoami  用户身份查看命令
```
# 任意提示符下执行
[root@localhost home]# whoami
```
1. 你当前用 root 管理员登录时：
```
[root@localhost home]# whoami
root  # 输出结果，说明当前是管理员身份
```
2. 切换到普通用户（比如 user1）后：
```
[user1@localhost ~]$ whoami
user1  # 输出普通用户名，提示符也会从 # 变成 $
```
|命令|作用|
|----|----|
|whoami|只显示当前终端登录的用户名|
|who|显示所有登录到系统的用户列表|
|id|显示当前用户的 UID、GID、所属组等信息|

su 切换用户命令
1. 普通用户执行 su root
```
# 假设当前是普通用户 user，提示符是 $
[user@localhost ~]$ su root
Password:  # 输入 root 用户的密码（输入时屏幕不显示，正常现象）
[root@localhost ~]#  # 提示符变成 #，说明切换成功
```
* 化简写法（等价于 su root）
```
su  # 不加用户名时，默认切换到 root 用户，效果和 su root 完全一致
```
2. su root 和 su - root 区别
|命令|环境变量/工作目录|
|su root|保留原普通用户的环境变量，工作目录不变|
|su - root（推荐）|完全加载 root 用户的环境变量，工作目录切到 root 家目录（/root）|
实例：
```
# 普通用户在 /home/user 目录执行 su root
[user@localhost user]$ su root
[root@localhost user]# pwd  # 工作目录还是 /home/user

# 执行 su - root
[user@localhost user]$ su - root
[root@localhost ~]# pwd  # 工作目录切到 /root
```
* 退出 root 回到普通用户
```
[root@localhost ~]# exit
[user@localhost user]$  # 回到普通用户
```
* 日常操作优先用 su - root（带 -），避免环境变量混乱导致命令执行异常；

### 安装虚拟机完成后，如何创建普通用户
1. 创建用户
```
# 格式：useradd 用户名
# 示例：创建一个名为 dev 的普通用户
useradd dev
```
2. 设置用户密码
```
# 格式：passwd 用户名
passwd dev
```
* 系统会提示你输入两次密码（输入时屏幕不显示，正常现象）。

#### 进阶配置：配置 Sudo 权限（关键！）

默认情况下，新创建的普通用户不能执行 root 权限的命令（如 yum install、cp /etc/...）。需要把普通用户加入 wheel 组，才能获得 sudo 权限。
1. 将用户添加到 wheel 组
```
# 格式：usermod -aG wheel 用户名
usermod -aG wheel dev
```
2. （可选）验证配置
```
id dev
```
#### 切换到普通用户测试
1. 切换用户
```
# 方式1：直接切换（不加载root环境，推荐）
su - dev

# 方式2：仅切换身份（保留当前环境，不推荐）
# su dev
```
2. 测试 Sudo 权限
```
ls /root
# 输出结果：ls: 无法打开目录/root: 权限不够

# 加上 sudo 执行
sudo ls /root
```
* 第一次执行 sudo 会要求输入当前普通用户的密码（不是 root 密码）。
* 输入密码后，成功显示 /root 目录内容，说明配置无误。

##### 执行 passwd 123456 时，系统默认给 ** 当前用户（root）** 改密码
