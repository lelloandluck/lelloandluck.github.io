---
title: The Common Knowledge of Python - III
date: 2015-10-25 18:53:20
tags: [python]
categories: [python]
---
上一篇文章中重点介绍了Python的函数及其使用场景，这篇文章将重点放在Python类及文件操作上，也是通过简单的实例加强对类的认知。类是面向对象编程的一个根本，使用面向对象编程可模拟现实情景，其逼真程度达到了令你惊讶的地步。这也是面向对象编程的魅力所在。在面向对象编程中，你编写表示现实世界中的事物和情景的类，并基于这些类来创建对象。编写类时，你定义一大类对象都有的通用行为。基于类创建对象时，每个对象都自动具备这种通用行为，然后可根据需要赋予每个对象独特的个性。
<!--More-->

### 类及其相关概念解析
**1.1 类及其相关属性说明**
还是先来看一个例子吧
```
class Person():
    """一次人的简单尝试"""
    def __init__(self, name, sex, age, job):
        """初始化属性- 姓名，性别，年龄，职业"""
        self.name = name
        self.sex = sex
        self.age = age
        self.job = job

    def commonInfo(self):
        judgeFlag =  "Do you wanna know a personal basic info. Yes or No?"
        if judgeFlag == "yes":
            print "the basic information is -\n", "Name - ", self.name, "\nSex - ", self.sex, "\n"
```
>注意Python类定义的结构，<font color='yellow' bgcolor='green'>类名用大写字母开头（通常变量，函数用小写打头），后面接形参、括号并冒号后缩行。也有一种说法是变量，函数小写打头，如果是多单词构成的名称则后面每个单词首字母大写或者全部小写，单词间加下划线"\_"分隔（即：驼峰命名法）

>初始化函数initial（），用来对类型的<font color="red">实例绑定属性</font>，有了这个初始化函数定义，在编写类方法和调用类的时候，更加方便便捷。如下代码，如果没有经过init（）函数定义的类，是没有属性的。

>在类的内部，使用 def 关键字可以为类定义一个方法，与一般函数定义不同，类方法必须包含参数 self,且为第一个参数

```
#没有__init__()方法的类
class Rectangle():
    def getPeri(self,a,b):
        return (a + b)*2
    def getArea(self,a,b):
        return a*b

rect = Rectangle()
print(rect.getPeri(3,4))
print(rect.getArea(3,4))
print(rect.__doc__) # 打印类的说明
print(rect.__dict__) # 打印类的属性

#有__init__()方法的类
class Rectangle():
    def __init__(self):
        self.side1 = a
        self.side2 = b

    def getPeri(self,a,b):
        return (a + b)*2
    def getArea(self,a,b):
        return a*b

rect = Rectangle()
print(rect.getPeri(4,4))
print(rect.getArea(4,4))
print(rect.__dict__) # 打印类的属性
```

**1.2 类的继承及覆盖/重写** -
```
#define parent class
class Car(): # class Car: 也是okay的
    '''模拟一个汽车实体的简单尝试'''
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_description(self):
        longName = str(self.year) + ' ' + self.make + ' ' + self.model
        print "long name is ", longName
        return longName.title()

    def read_odometer(self):
        print "This car has " + str(self.odometer_reading) + " miles on it."

    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print "You are prohibited to roll back an odometer!"

    def increment_odometer(self, miles):
        self.odometer_reading += miles

#myCar = Car('Honda', 'CRV', '2016-6')
#print "\n\n\nThis is my first Car - ", myCar.get_description()


#define child class
class ElectricCar(Car):
    """这里是电动汽车的类型描述"""
    def __init__(self, make, model, year):
        """初始化父类属性"""
        Car.__init__(self, make, model, year)

    def read_odometer(self):
        print "this is the electronic car, and its odometer will generate automatically! won't be changed by manual"

myFirstCar = ElectricCar('Honda', 'CRV', '2016')
print "My first car describle - ", myFirstCar.get_description()
print "My first car odometer description >>> ", myFirstCar.read_odometer()

#define a new parent class
class Car(object):
    '''模拟一个汽车实体的简单尝试'''
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_description(self):
        longName = str(self.year) + ' ' + self.make + ' ' + self.model
        print "long name is ", longName
        return longName.title()

    def read_odometer(self):
        print "This car has " + str(self.odometer_reading) + " miles on it."
        #odometerDesc =  "This car has " + str(self.odometer_reading) + " miles on it."
        #return odometerDesc

    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print "You are prohibited to roll back an odometer!"

    def increment_odometer(self, miles):
        self.odometer_reading += miles


class ElectronicCar(Car):
    def __init__(self, make, model, year):
        super(ElectronicCar, self).__init__(make, model, year)

mySecCar = ElectronicCar('Tesla', 'Model S', '2018-8')

print "Return my second car information -", mySecCar.get_description()
print "My second car odometer description >>> ", mySecCar.read_odometer()
```
>子类如果重写了__init__ 时，要继承父类的构造方法，可以使用 super 关键字：

```
super(子类，self).__init__(参数1，参数2，....)
```
还有一种经典写法如下 -
```
父类名称.__init__(self,参数1，参数2，...)
```
> 虽然没有init（）的类可以使用，但代码写起来会很低效，这不是一个好的代码习惯，遵守约定是一个优秀程序员的需要具备的姿态。

> 字类不能继承父类praviate的属性和方法，在python中指的是加下划线或双下划线的属性及方法。

**1.3 类的导入**
随着你不断地给类添加功能，文件可能变得很长，即便你妥善地使用了继承亦如此。为遵循Python的总体理念，应让文件尽可能整洁。为在这方面提供帮助， Python允许你将类存储在模块中，然后在主程序中导入所需的模块。
导入模块中的类和导入模块中的函数写法类似，参看如下表述 -
```
导入单个类
from car import Car
导入多个类
from car import Car, ElectricCar
导入全部模块中的类
from car import *
或者
import car
```
><b>小结</b>-<font color='green'>导入模块时，要么from，要么import；而函数或类只能import。
有了类可以更好的封装现实世界，并且高效和易于理解，这点是函数无法比拟的，尽管函数也常常用于封装。

> <font color='orange'>实例化一个类就是类名+（）. ex. student = StudentClass()

> <font color='blue'>和普通数相比，在类中定义函数只有一点不同，*<b>就是第一参数永远是类的本身<font color='red'/>实例变量self，在定义时不可以省略</b>*, 并且调用时，不用传递该参数。除此之外，类的方法(函数）和普通函数没啥区别，你既可以用默认参数、可变参数或者关键字参数（*args是可变参数，args接收的是一个tuple，**kw是关键字参数，kw接收的是一个dict）

> 类是抽象的模板，而实例是根据类创建出来的一个个具体的对象，每个对象都拥有相同的方法，但各自的数据可能不同，这是学习OOP编程必须要建立的基本概念，由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的\__init__方法。

> <font color='pink'>如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线\__，在Python中，实例的变量名如果以__开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问

> 在Python中，变量为为双划线开头的，如\__xxx,是私有变量private，只能类的内部调用，外部对象是不能调用的；
变量名类似\__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量， 编程时变量名不要使用这样的命名；
单下划线开头的变量名类似这样\_xxx的，这样的实例变量外部是可以访问的，但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，“虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”

> <font color='purple'>继承可以把父类的所有功能都直接拿过来，这样就不必重零做起，子类只需要新增自己特有的方法，也可以把父类不适合的方法覆盖重写；
有了继承，才能有多态。在调用类实例方法的时候，尽量把变量视作父类类型，这样，所有子类类型都可以正常被接收；

**1.4 类的多态、属性限制、装饰**
所谓多态 - 就是同一类事务有多种不同的形态，一个抽象类有多个子类，因而多态是依赖于继承的，也就是说每个子类可以对同一个消息有不同的响应方式。请看下面的例子 -
```
'''
继承和多态
'''
class Animal():
    def run(self):
        print "Animal can run .... "

class Dog(Animal):
    def run(self):
        print "Dog also run and run fast."

class Chick(Animal):
    def run(self):
        print "Chick not only run and also fly."

animal = Animal()
animal.run()

animal1 = Dog()
animal1.run()

animal2 = Chick()
animal2.run()

class Pig(Animal):
    pass
animal3 = Pig()
animal3.run()

# define a new method, and make an instant of Animal as the parameter, so that we can see if the outputs are changed by the parameter.
def runTwice(animal):
    animal.run()
    animal.run()


runTwice(Animal())
runTwice(Dog())
runTwice(Chick())

# another subclass of animal
class Horse(Animal):
    def run(self):
        print "Horse runs very fast, and it used to be widely used as a transportation"

runTwice(Horse())
```
**使用\__slots__** 来限制class的属性，例如只允许对某一个类添加name，sex属性，可以这样来做
```
class Enrollment(object):
  __slots__ = ('name', 'sex') # 用tuple定义允许绑定的属性名称
这样一来如果想给该类的对象绑定新的属性时就会报错，如 -
s = Enrollment()
s.name = 'alan'
s.sex = 'Male'
s.age = 33 # 此处age属性不允许被绑定，程序执行会报错
```

**类的装饰decrate** 请参阅之前一篇文章介绍 。。。
