# 流程控制语句
## if条件判断
只有满足指定条件，才会执行对应的代码逻辑

```
if 要判断的条件:
    条件成立时,要执行对应的操作
```

* 判断语句的结果,必须是布尔类型True或False(True会执行if内的代码;False则不会执行)
* Python中是通过缩进来描述代码的归属的,归属if代码块的语句,需要在前方缩进4个空格(按Tab键Pycharm会自动转为空格)

```python
score = 700
if score >= 688:
    print("欢迎来到清华")
    print("恭喜")
```


## if进阶语句
if ... else

```
if 要判断的条件:
    条件成立时,要执行对应的操作1
else:
    条件不成立时,执行的操作2
```

if...elif...else

```
if 要判断的条件1:
    条件成立时,要执行对应的操作1
elif 要判断的条件2:
    条件成立时,要执行对应的操作2
else:
    条件不成立时,执行的操作3
```

```python
num = input("输入数字：")

if num > 0 :
    print("正数")
elif num < 0 :
    print("负数")
else:
    print("0")
```

案例

```python
a = input("请输入边长")
b = input("请输入边长")
c = input("请输入边长")

# pass是一个空语句，起到一个语法占位的作用
if a + b > c and a + c > b and b + c > a:
    if a == b and b == c:
        print("等边三角形")
    elif a == b or b == c or c == a:
        print("等腰三角形")
    else:
        print("普通三角形")
else:
    print("不是三角形")
```

## 模式匹配 match...case
结构模式匹配就是用一个清晰的模板去匹配数据的结构和内容,匹配成功则执行响应的操作

某个变量的多个固定值进行分支判断

```python
day = input("请输入星期几：")

match day:
    case "1":
        print("周一：")
    case "2":
        print("周二：")
    case "3":
        print("周三：")
    case "4":
        print("周四：")
    case "5":
        print("周五")
    case "6" | "7" :
        print("周末放松")
    case _:
        print("错误")


a = float(input("请输入数字1："))
b =float(input("请输入数字2："))
oper = input("请输入运算符（+ - * /）：")

match oper:
    case "+":
        print(a+b)
    case "-":
        print(a-b)
    case "*":
        print(a*b)
    case "/" if b != 0:
        print(a/b)
    case _:
        print("运算符输入有误")
```


* 其中的 | 表示或的关系,匹配多个模式中的任意一个
* _ 表示匹配其他所有情况
* case后可以加上判断

## while循环

```
while 条件表达式:
    循环体语句1
    循环体语句2
    ...
else:
    条件为False,循环正常结束时执行
```

* while循环是通过条件表达式,来控制是否要进行下一次循环
* 表达式的结果为布尔类型
* else可不写

```python
# 案例 1~100之间偶数累加的和
total = 0
i  = 1
while i <= 100:
    if i % 2 == 0:
        total += i
    i += 1
else:
    print(total)
```

## for循环
while循环是通过条件表达式来控制是否要进行下一次循环。而for循环,本质是一种轮询遍历机制，对一批内容进行逐个处理

```
for 元素 in 待处理数据集:
    循环体代码（对元素进行处理）
else:
    循环结束时，执行的代码
```

* else可以不写

### for循环与while循环的场景

* while循环：用于在某个条件满足时一直循环，循环的次数通常是未知的，只知道循环开始/结束的条件。（关注的是循环的条件）
* for循环：用于对一个知己的数据集进行遍历或已知次数的循环（关注的是遍历每一个元素）

### range语句
作用：生成指定规则的数字序列

1. range(end) -> 获取一个从0开始，到end结束的数字序列（不含end本身）
2. range(start,end) -> 获取一个从start开始，到end结束的数字序列（不含end本身）
3. range(start,end,step) -> 获取一个从start开始，到end结束的数字序列，step步长（不含end本身）

```python
#计算1 - 100 所有奇数之和

sum = 0
for i in range(1,101,2):
    sum += i
else:
    print(sum)
```

## 嵌套循环
外层循环执行一次，内层循环执行n次

```python
# 根据输入的长方形的长度 m , 宽度 n, 打印一个长方形
m = int(input("请输入长方形的长度："))

n = int(input("请输入长方形的宽度："))
for j in range(n):
    for i in range(m):
        print("*",end=" ")
    print() #换行

# print("*"): 自带换行效果，每一次执行都会在新的一行输出
# print("*",end=" "): end表示的是每一次输出以什么结束；默认\n，表示换行
```

### 关键字

* brak:只能够出现在循环中，表示结束，跳出循环(break跳出循环时，while后面else中的代码将不会执行)

* continue：只能够出现在循环中，表示中断本次循环，直接进入下一次循环

```python
while True:
    user = input("请输入用户名：")
    password = input("请输入密码：")

    # 校验
    if user == "" or password == "":
        print("用户名或密码不能为空")
        continue #结束当次循环

    if user == "admin" and password == "666888":
        print("登录成功")
        break
    elif user == "zhangsan" and password != "123456":
        print("登录成功")
        break
    elif user == "taoge" and password == "888666":
        print("登录成功")
        break
    else:
        print("用户名或密码错误，请重新输入")
```

### random生成随机数

```python
# 案例 猜数字
import random
random_num = random.randint(1, 100) #生成随机数
while True:
    sum = int(input("请输入一个数字："))

    if sum > random_num:
        print("猜大了")
    elif sum < random_num:
        print("猜小了")
    else:
        print("猜对了")
        break
print("随机数是：",random_num)
```
