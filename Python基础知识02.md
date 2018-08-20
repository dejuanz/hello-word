[TOC]

# 条件语句

## 分支语句 if

```
weather = input("请输入天气：")
if weather == "晴天":
    print("注意避暑")
elif weather == "阴天":
    print("预防下雨")
elif weather == "风":
    print("注意保暖 ")
else:
	print("适宜")
```

if 后面跟的条件为真时，才能输出相应的结果。

### 运算符

| 运算符 |                             描述                             |
| :----: | :----------------------------------------------------------: |
|   ==   |  检测两个操作数的值是否相等，如果相等则条件成立，返回值True  |
|  ！=   | 检测两个操作数的值是否不相等，如果不相等则条件成立，返回值True |
|   >=   | 检测左边操作数的值是否大于等于右边操作数的值，如果是则条件成立，返回值True |
|   <=   | 检测左边操作数的值是否小于等于右边操作数的值，如果是则条件成立，返回值True |
|   >    | 检测左边操作数的值是否大于右边操作数的值，如果是则条件成立，返回值True |
|   <    | 检测左边操作数的值是否小于右边操作数的值，如果是则条件成立，返回值True |

### 多个条件之间的关系

| 运算符 | 表达式  | 描述 |
| :----: | :-----: | :--: |
|  and   | x and y |      |
|   or   | x or y  |      |
|  not   | x not y |      |

### 猜拳游戏

```
import random  # 导入工具包

user_fist = int(input("剪刀（1）包袱（2）锤（3）:"))

computer_fist = random.randint(0,2)

if (user_fist == 1 and computer_fist == 2) or\
    (user_fist == 2 and computer_fist == 3) or\
    (user_fist == 3 and computer_fist == 1):
    print("你赢了")

elif user_fist == computer_fist:
    print("平局")
else:
    print("你输了")
```

### if 嵌套

需求：想要去听演唱会：

​	首先检查是否有票，如果有票，

​		检查是否有带打火机或者刀具

​		如果有带刀具或者打火机

​			不允许进入

​		如果没带刀具和打火机

​			允许进入

​	如果没票

​		不允许进入



```
has_tickets = True
has_fire = True
has_knife = True
if has_tickets:
    print("有票，检查是否有违禁物品")
    if has_fire or has_knife:
        print("有危险物品，不允许进入")
    if not (has_fire and has_knife):
        print("通过")
if not has_tickets:
    print("无票，不允许进入")
```

这种在if里再嵌套入if语句的方法就叫if嵌套。

## 循环语句 while

计算1-100的累积和：

```
i = 1
s = 0
while i <= 100:
    s = i * (i + 1)/2
    i += 1
print(s)
```

### 退出循环 和 跳过循环

退出循环：break

计算1-100的累积和：

```
i = 1
s = 0
while True:
    if i > 100:
        break
    s = i * (i + 1)/2
    i += 1
print(s)
```

跳过循环：continue

计算1-100的累积和：

```
i = 1
s = 0
while True:   
    if i > 10:
        break
    if i == 4:
        i += 1
        continue
    s = s + i
    i += 1
    print(s)
```

continue实际上是不论遇到什么代码，只要遇到条件，直接再跳到while，继续执行。例如：

```
i = 1
s = 0
while i <= 100:
    s = 100 - i
    i += 1
    continue
    print("123")
print(s)
```

上述程序在条件为真时，一直执行循环，且不会执行后续平级代码，当while后边的条件为False后退出循环，执行后面的程序。

**注**：在使用continue时，条件处理部分的代码需要特别注意，有可能会出现死循环。

### 循环嵌套

以九九乘法表为例：

首先确定行：row ,列：col，乘法表中（eg:3 * 4 = 12）第一个数字（3）代表列，第二个数字（4）代表，且第一个数字永远小于第二个数字，即col < row 。

```
row = 1
while row <= 9:
    col = 1
    while col <= row:
        print("%d * %d = %d" % (col, row, col * row), end="\t")   # end=""输出结束后，不换行
        col += 1
    print()  # 上面输出结束后换行。执行print()显示两行为空的行，第一行为现实的空字符串，第二行为换行达到的效果
    row += 1
```

