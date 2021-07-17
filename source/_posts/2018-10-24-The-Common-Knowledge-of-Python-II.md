---
title: The Common Knowledge of Python - II
date: 2015-10-24 17:06:21
tags:
 - python
categories:
 - python
---
今天接着梳理Python之函数、类以及相关的知识点，通过简单的代码实例帮助消化这些具体的或抽象的概念对掌握一门编程语言尤为有用，一起来看看我是怎么整理的。
**<font color="red">1. 函数</font>**
 * ```
  def sayHelloToAnyone(): #<font color="red"> 1. 括号()不可少，即使没带形参，然后冒号 : 也是必须的，这是定义函数的基本特征</font>
      '''the purpose of this function aim to output greeting to ones you wan to say
      created Date - 2015.10
       ''' # 2. 这部分为函数的“文档字符串”用来生成程序中函数的文档。
      print("Hello everyone, I am Pyhon, I love you all!")

  sayHelloToAnyone() # 3. 要调用函数，可依次指定函数名以及用括号括起的必要信息。 此处由于该函数不需要任何信息参数，所有直接函数名+（）即可。
  ```
  <!--More-->
  1.1 实参和形参
  ```
  def sayHelloToSpecialone(userName): # 形参
      print("Hello", userName + " Nice to meet you again!")
  sayHelloToSpecialone('Alan') # 实参
  ```
- 实参的位置和形参对应很重要，一定要注意顺序

  1.2 关键字实参
  def myPets(name, color, character='Clever'):
      print "I have a", name, "\nit's ", color, "\nand it's very", character
  myPets('Cat', 'Yellow', 'compliant')
  输出为：
  ```
  I have a Cat
  it's  Yellow
  and it's very compliant
  ```
  myPets('Cat', 'Yellow')
  输出为：
  ```
  I have a Cat
  it's  Yellow
  and it's very Clever
  ```
  有时函数使用默认值时（关键字参数时），<font color='green'/>在形参列表中必须先列出没有默认值的形参，再列出有默认值的实参。这让Python依然能够正确地解读位置实参。

  1.3 函数返回值
  函数并非总是直接显示输出，相反，它可以处理一些数据，并返回一个或一组值。函数返回的值被称为返回值。在函数中，可使用return语句将值返回到调用函数的代码行。
  调用返回值的函数时，需要提供一个变量，用于存储返回的值。如下面代码所示 -
  ```
  def get_formatted_name(first_name, last_name):
    """返回整洁的姓名"""
    full_name = first_name + ' ' + last_name
    return full_name.title()
  musician = get_formatted_name('jimi', 'hendrix')
  print(musician)
  ```
  >另外，函数可以传递列表，以及在函数中修改列表，这些在现实编码中很常见，关键就是在函数中实现对列表的操作。有时要防止函数对传入列表的操作，可以传入该列表的副本，如 - function_name(list_name[:])

  1.4 传递任意数量的实参
  形参名*toppings中的星号让Python创建一个名为toppings的空元组，并将收到的所有值都封装到这个<font color='blue' font-weight:'bold' size=3>元组</font>中。
  ```
  def make_pizza(*toppings):
    """打印顾客点的所有配料"""
    print(toppings)
    make_pizza('pepperoni')
    make_pizza('mushrooms', 'green peppers', 'extra cheese')
  ```
  >如果要让函数接受不同类型的实参，必须在函数定义中将接纳任意数量实参的形参放在最后。 Python先匹配位置实参和关键字实参，再将余下的实参都收集到最后一个形参中。

  >有时候，需要接受任意数量的实参，但预先不知道传递给函数的会是什么样的信息。在这种情况下，可将函数编写成能够接受任意数量的键—值对,这就是使用任意数量的<font color='purple' size='3'>关键字实参</font>请看下面代码 -
  >>```
  def build_profile(first, last, **user_info):
    """创建一个字典，其中包含我们知道的有关用户的一切"""
    profile = {}
    profile['first_name'] = first
    profile['last_name'] = last
    for key, value in user_info.items(): # 关键字实参就是字典数据型参数；任意实参其实就是元组型数据参数
        profile[key] = value
    return profile
  user_profile = build_profile('albert', 'einstein', location='princeton', field='physics')
  print(user_profile)
 ```

 1.5 Python模块
 为什么把这个概念放在这里，是因为函数常常被放在python模块中，所谓模块就是扩展名为.py的python文件。那么如何使用模块中的函数呢？
 ```
  import pizza
  pizza.make_pizza(16, 'pepperoni')
  pizza.make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
 ```
  或者直接导入模块中的制定函数，这样就不需要通过点.操作去指明引用函数了。如下 -
  ```
  from pizza import make_pizza
  make_pizza(16, 'pepperoni')
  make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
  ```
  as 关键字给函数指定别名，特别是当函数定义较长时，如下-
  ```
  from pizza import make_pizza as mp
  mp(16, 'pepperoni')
  mp(12, 'mushrooms', 'green peppers', 'extra cheese')
  ```
  当然，也可以这么直接导入所有的函数，只是系统开销会增加而已
  > from pizza import *

   另外 -
    >编写函数时，需要牢记几个细节。应给函数指定描述性名称，且只在其中使用小写字母和下
    划线。描述性名称可帮助你和别人明白代码想要做什么。给模块命名时也应遵循上述约定。
    >所有的import语句都应放在文件开头，唯一例外的情形是，在文件开头使用了注释来描述整
    个程序。
