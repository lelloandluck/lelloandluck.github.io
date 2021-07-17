---
title: The Common Knowledge of Python - I
date: 2015-10-23 10:18:51
tags:
 - python
categories:
 - python
---
学习任何一门语言，首先要了解它的基本语法，熟练的掌握语法是写好程序的首要条件，任何一种编程语言，它的语法架构有着惊人的相似，对于有一定编程基础的童鞋来说，学习Python真的很得心应手。这里我粗略梳理一下Python语言的基本要点，希望对系统了解这门语言有所裨益-

<!--more-->
**1. 变量**
+ 和大多数语言一样，变量可由数字，字母和下划线组成，<font color="red"/>不能由数字打头。
+ 不要使用Python的保留字作为变量名，以防止解释器编译错误，保留字就是Python内部有特殊意义和用途的字符串定义，如函数名print，变量名类型名int，str, if, while 等等。
+ 变量名虽然没有强制规定大小写问题，但使用小写字母定义是个不错的主意，大多数语言都如此。

**2. 数据类型**
Python数据类型有-
- 字符串String（在Python中，用引号括起的都是字符串，其中的引号可以是单引号，也可以是双引号）
- 数字类型Number
- 列表List
- 元组Tuple
- 集合Sets
- 字典Dict
每种数据类型都有相应的计算规则，正式因为这些丰富的计算规则，才使得人们能通过它们实现多姿多彩的现实世界表述。

- *2.1 数值计算*
```
print("5+3=",5+3)
print("5-3=",5-3)
print("5*3=",5*3)
print("除法得到浮点数 2/4=",2/4)
print("除法得到整数 2//4=",2//4)
print("取余 10%3=",10%3)
print("乘方 4**2=",4**2)
print("开方 4**0.5=",4**0.5)
```
- *2.2 String(字符串)*
```
#元素是不可变的
string="abcdefg"
print(string)
print(string[0])
print(string[0:-1])#从头到尾
print(string[2:])#从下标2开始到尾
print(string[2:4])#从下标2到n-1  [m,n)
print(string*2)#输出2次
```
- *2.3 list(列表)*
```
#元素可变的
listDemo=["aa",1,"bb",2]
print(listDemo)
print(listDemo[0])#输出下标0
print(listDemo[2:])#从下标2开始到尾
print(listDemo[1:3])#从下标2到n-1  [m,n)
print(listDemo*2)#输出2次
listDemo[0]="替换的"
print(listDemo)#修改后的
```
- *2.4 tuple(元组)*
```
#元素不可变的
tupleDemo=("aa",1,"bb",2)
print(tupleDemo)
print(tupleDemo[0])#输出下标0
print(tupleDemo[2:])#从下标2开始到尾
print(tupleDemo[1:3])#从下标2到n-1  [m,n)
print(tupleDemo*2)#输出2次

tupleDemo=()#空元组
tupleDemo=(a,)#一个元素
print(tupleDemo)
```
- *2.5 Set(集合)*
```
#一个无序不可重复的序列
setDemo={"a","b","c"}
print("集合A ",setDemo)
#集合可以做 交集并集差集
setDemo2={"a","b"}
print("集合B ",setDemo2)
print("AB的差集 ",setDemo-setDemo2)
print("AB的并集 ",setDemo|setDemo2)
print("AB的交集 ",setDemo&setDemo2)
print("AB的不同时存在的 ",setDemo^setDemo2)
```
- *2.6 字典*
```
dictDemo={"tom":"90","jerry":"75"}
print(dictDemo)
print(dictDemo["tom"])
print("keys:",dictDemo.keys())
print("values",dictDemo.values())
#移除 key 返回value
print("移除tom ",dictDemo.pop("tom"))
print(dictDemo)
```
- *python常用数据转换*
'''
int(x)
str(x)
tuple(s) 将序列转换成元组
list(s) 将序列转换成列表
'''

- *python的注释*
print("单行注释 #")
print("多行注释 单引号(3个')")
print("多行注释 双引号(3个双引号)")

**3. 判断及循环语句**
+ 3.1 Python里的判断语句主要指if或if。。。else组合语句，常见形式有：
```
if condition:
  pass

或者
if conditon:
  pass
else:
  pass

或者
if condition：
  pass
elif condition1：
  pass
elif condition2：
  pass
。。。
else：
  pass
```
 这些用起来很简单, 条件简单时用if或if。。。else；多重条件判断时用if。。elif。。elif 。。。组合。

* 3.2 循环语句有-
for循环-
```
for each in string/list/tuple/dict:
  pass

for key, value in dict.items():
  pass
or
for key, value in dict：
  pass
```
* ‘in’后面是pytho集合数据类型，常见的有list， tuple， dict，string，而数据列表常会用range（）函数来实现。

 while循环-
  ```
  prompt = "\nTell me something, and I will repeat it back to you:"
  prompt += "\nEnter 'quit' to end the program. "
  active = True
  while active:
    message = input(prompt)
    if message == 'quit':
      active = False
    else:
      print(message)
  ```
+ 在任何Python循环中都可使用break语句。例如，可使用break语句来退出遍历列表或字典的for循环。
- while循环通常用于处理列表和字典的元素，在列表元素不断变化时，用while比用for更好控制和跟踪。
* 在任何Python循环语句中都可以使用continue语句，程序执行到continue语句时会忽略本次循环未执行的代码，进入下一轮循环。

**4. 复合数据的嵌套**
  这里主要介绍下字典，列表之间的嵌套关系，也就是说这类复合数据元素可以相互嵌套，即列表中的元素可以是字典，字典中的元素可以是列表，以及它们之间的相互嵌套关系。下面来看几个例子 -
* 4.1 列表字典（列表中的元素为字典的嵌套数据）
  ```
  perChar = {'Sexual': 'Female', 'Company':'Appl.', 'Work Location':'Shanghai', 'JobLevel':'SSE'}
  addList = []

  for i in range(20):
      addList.append(perChar)

  for j in addList:
      print j

  # 修改字典元素的值
  for j in addList[0:10]:
      if j['Work Location'] == 'Shanghai':
          j['Work Location'] = 'ChongQing'
          j['JobLevel'] = 'INT'

  print "#" * 58
  for j in addList:
      print j
  print "打印第二个列表元素", addList[1]
  ```

+ 4.2 字典列表（字典中的元素为列表的嵌套数据）
  ```
  pizza = {'crust': 'thick', 'topping':['hot', 'meaty', 'spicy']}
  print "What would you like the pizaa?\nI'd like the pizza :", pizza['crust'], "\nand the topping had better like these -"
  for top in pizza['topping']:
      print top
  ```

- 4.3 字典中嵌套字典的数据
  ```
  users = {'Alan':{'fullName':'Alan_yuan', 'Job':'SW QA Expert', 'Location':'Shanghai', 'Sex':'Male'}, 'Wendy':{'fullName':'Wendy_zhang', 'Job':'Chemicle Scientist', 'Location':'Shanghai', 'Sex':'Female'} }
  print "Please show me the user information\n"
  for name, info in users.items():
      print "User Name:", name
      information = "全名：" + info['fullName'] + " 工作：" + info['Job'] + " 工作地：" + info['Location'] + " 性别：" + info['Sex']
      print "And his/her information are -", information
  ```
    到此为止，我们已经梳理了Python的常用数据，以及循环和控制语句的基本知识点，牢记这些，在工作中加以练习，举一反三，就能做到熟能生巧了，下面的一篇blog将接着梳理Python的其他一些知识点。
