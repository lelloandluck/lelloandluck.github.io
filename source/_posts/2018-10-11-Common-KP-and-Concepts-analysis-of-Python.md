---
title: Common KP and Concepts analysis of Python
date: 2018-10-11 17:47:38
tags:
 - python
categories:
 - python
 - Python常用知识点解析
---
函数在Python中应用十分广泛，它是Python高级应用的基础，网上介绍Python函数的资料枚不胜举，这里我重点梳理下Python函数的几个常用概念和相关知识点以及使用注意事项 -

##### 函数

```
def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

+ 上面是一个常见的函数定义，关键字def，函数名，以及后面的小括弧一个都不能少，还有别忘了后面的冒号：
- 函数体及代码块是在冒号缩进后开始写的
* 语句return [expression] 用于退出一个函数，可选地将一个表达式传回给调用者。如果没有使用参数的return语句，则它与return None相同。
<!--MORE-->

**函数的参数**
 1. 默认参数
 调用上面的函数，只需给出x和n的初始值就行了，如果把上面函数的参数写成 def power(x, n=3)，那么就是参数的默认化，此时调用函数只需给出x值就可以了。当我们调用power(5)时，相当于调用power(5, 3)

 <font color='red'/>如果有默认参数，则必选参数在前，默认参数在后，否则Python的解释器会报错
 当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数（即给出该参数的初始值）。

 2. 可变参数
 ```
 def calc(*numbers):
      sum = 0
      for n in numbers:
          sum = sum + n * n
      return sum
 ```
 > 可变字参数，就是在参数前加了一个 * 号，这样一来，在调用该函数时，可以传入任意个参数也可以给出0个。
可变参数允许你传入0个或任意个参数，***这些可变参数在函数调用时自动组装为一个tuple***。

 3. 关键字参数
 ```
 def person(name, age, **kw):
    print 'name:', name, 'age:', age, 'other:', kw
 ```
 > 而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict

 > 关键字参数的用处主要表现在，它可以扩展函数的功能。比如，在person函数里，我们保证能接收到name和age这两个参数，但是，如果调用者愿意提供更多的参数，我们也能收到。试想你正在做一个用户注册的功能，除了用户名和年龄是必填项外，其他都是可选项，利用关键字参数来定义这个函数就能满足注册的需求。

 4. 组合函数
在Python中定义函数，可以用必选参数、默认参数、可变参数和关键字参数，这4种参数都可以一起使用，或者只用其中某些，但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数和关键字参数。


5. 匿名函数
关键字lambda表示匿名函数，lambda [arg1 [,arg2,.....argn]]:expression 冒号前面的x表示函数参数。看下面一个例子
```
a = map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9])
等同于这个表达式 -
def f(x):
    return x * x
```
 lambda只是一个表达式，不是一个代码块，函数体比def简单很多，只能封装有限的逻辑，可以起到速写函数的作用


6. 闭包函数
就是函数内部定义的函数，并且包含了对外部作用域的引用。下面的例子能很好的帮助阐述什么是闭包函数。
```
# 这个不是闭包函数
def functionA():
    def inner():
        print "this is the inner outputs"
    inner()
    print "this is the functionA outputs"

# 这个也不是闭包函数
x = 1
def functionA():
    def inner():
        print "this is the inner outputs"
    inner()
    print "this is the functionA outputs"


# 只有这个才是闭包函数  
def functionA():
    x = 1
    def inner():
        print "this is the inner outputs"
    inner()
    print "this is the functionA outputs"
```

7. 装饰函数
装饰函数就是在不更改原函数任何代码的情况下通过引用原函数来丰富原函数的功能的函数。也可以这么表述
 + 装饰器：外部函数传入被装饰函数名，内部函数返回装饰函数名。
 - 特点：1.不修改被装饰函数的调用方式 2.不修改被装饰函数的源代码

还是有点晦涩，来看个例子吧 -
```
def decoration(func):
    def inner(a, b, c):
        print "\n\n\n", "*" * 40
        func(a, b, c)
        print "*" * 40
    return inner

@decoration
def functionA(a, b, c):
    print "count an addition result - \na + b + c = ", a + b + c
functionA(3, 5, 7)
```
这个例子中，functionA被作为参数传入decoration(func)，执行inner函数后再传回给调用的函数。要装饰的功能在inner函数中实现。
