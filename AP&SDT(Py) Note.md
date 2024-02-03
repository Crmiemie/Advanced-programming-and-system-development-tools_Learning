 <img src="/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-08-31 09.48.04.png" alt="截屏2023-08-31 09.48.04" style="zoom:60%;" />

| **报告名称**：   | 复习笔记与系统开发工具课程知识总结 |
| ---------------- | ---------------------------------- |
| **学生姓名：**   | 杨超然                             |
| **学生学号：**   | 202106060220                       |
| **专业班级：**   | 信管2101班                         |
| **学**  **院：** | 工商管理学院                       |
| **指导老师：**   | 黄万艮                             |
| **日  期：**     | 2023.12.15                         |

------

[TOC]

------

# 一、Python函数式编程与递归算法

## （一）函数式编程

在之前的课程中，我们了解到Python是一种**多范式的编程语言**，其中包含了函数式编程范式。函数式编程是一种将计算过程看作是函数之间的转换和组合的编程范式，强调函数的不可变性和避免副作用。在Python中，函数式编程可以通过高阶函数、匿名函数、闭包、惰性计算等特性来实现。

### 1.函数式编程的优点与含义

函数式编程作为一种编程范式，有着以下一些优点：

1.代码重用，提高程序编写效率

2.使用系统或标准模块函数，提高程序运行效率

3.给一段代码一个合适的名字，提高程序的可读性。

4.使用函数装饰器，可以简化程序逻辑复杂性，可以使程序更容易调试，还可以极大提高程序运行效率

5.有利于python模块规范化

 Python函数式编程是一种编程风格，**将问题分解成一系列函数**，强调使用纯函数和不可变数据来编写程序，**允许把函数本身作为参数传入另一个函数，还允许返回一个函数**！使用函数式编程可编写更简洁、可复用、可测试和高效的代码。同时，函数式编程也提供了一些高级的功能和抽象概念，例如惰性求值和函数式数据结构。在函数式编程中，**对函数式数据结构的操作通常通过创建新的数据结构来完成，而不是修改原有数据结构**。需要根据具体的需求权衡其优缺点。

------

### 2.函数式编程的主要内容

(1)函数作为一等公民：在函数式编程中，函数被视为一等公民，可以像其他数据类型一样被传递、赋值以及作为返回值

(2)不可变数据：函数式编程鼓励使用不可变数据，即创建后不能被修改的数据。这使得程序更容易理解和推理，并能减少错误的发生。

(3)纯函数：纯函数是指没有副作用并且对于给定的输入总是产生相同的输出的函数。纯函数不依赖于和修改外部状态，因此更容易进行测试和并发处理。

(4)高阶函数：高阶函数是指能够接受一个或多个函数作为参数，并/或者返回一个函数的函数。高阶函数可以用于构建复杂的逻辑和实现代码的重用。

(5)函数组合与柯里化：函数组合是指将多个函数组合成一个新的函数的过程。柯里化是指将一个接受多个参数的函数转化为一系列只接受一个参数的函数。

(6)递归：递归是函数式编程的常用技术之一，通过函数调用自身来解决问题。

------

## （二）迭代器和可迭代对象

**·可迭代**：指能够被for循环遍历的对象(容器)。

**·可迭代对象**：指实现了__iter__方法的对象，是可以被迭代的对象，例如列表、元组、集合、字典等。

**·迭代器**：是实现了无参数的__next__方法，该方法返回序列中的下一个元素；如果没有元素了则抛出StopIteration异常的对象。Python的迭代器还实现了__iter__方法，因此迭代器也可以迭代。

```python
# 创造迭代器示例
class MyIterator:
    def __init__(self, start, end):
        self.current = start
        self.end = end
 
    def __iter__(self):
        return self
 
    def __next__(self):
        if self.current > self.end:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1

if __name__ == "__main__":

my_iterator = MyIterator(1, 5)
for i in my_iterator:
    print(i)
```

```python
# 可迭代对象判断——使用collections.abc模块中的Iterable类。
from collections.abc import Iterable

if isinstance(obj, Iterable):
    print('obj是可迭代的')
else:
    print('obj不是可迭代的')
```

补充——**惰性求值**：只有在需要结果的时候，才进行计算，如果暂时不需要结果，就不进行计算，从而最小化计算机的工作，提高程序的效率

------

## （三）装饰器

装饰器是一种函数，它可以**接受另一个函数作为输入，并返回一个新的函数作为输出**。它可以用于修改或增强原始函数的行为。

```python
# 装饰器的固定格式:
def decorator(func):
    
    def inner(*args, **kwargs):
        print(" before"+func.__name__ + " was called")
        res=func(*args, **kwargs)
        print(" after"+func.__name__ + " was called")
        return res
    return inner
```

装饰器的语法如下：

```python
@decorator
def function():         
	pass
```

其中，decorator是装饰器函数，function是被装饰的函数。可以看到，装饰器使用@符号将装饰器函数放在被装饰函数的上方，从而达到装饰的效果。

装饰器用例——**计时统计**：可以使用装饰器来计算函数的执行时间，以便优化程序性能。

```python
import pandas as pd 

def calcMethodsTimes(func): 
    def inner(*args, **kwargs): 
        start = pd.datetime.now() 
        res = func(*args, **kwargs) 
        end = pd.datetime.now() 
        print(f'methods:{func.__name__} ,运行共计耗时{ end - start}') 
        return res 
    return inner 
 
@calcMethodsTimes 
def fun(): 
    return
```

------

## （四）递归算法

递归（Recursion）是一种解决问题的思路，其精髓在于将问题分解为规模更小的相同问题，持续分解，直到问题规模小到可以用非常简单直接的方式来解决。递归的问题分解方式非常独特，其算法方面的明显特征就是：**在算法流程中调用自身**。

递归例题——**汉诺塔问题**

汉诺塔来源于印度传说的一个故事，上帝创造世界时作了三根金刚石柱子，在一根柱子上从上往下从小到大顺序摞着 64 片黄金圆盘。上帝命令婆罗门把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一回只能移动一个圆盘，只能移动在最顶端的圆盘。有预言说，这件事完成时宇宙会在一瞬间闪电式毁灭。也有人相信婆罗门至今仍在一刻不停地搬动着圆盘。当然这个传说并不可信，如今汉诺塔更多的是作为一个玩具存在。

**思路：**

- 将盘片塔从开始柱，经由中间柱，移动到目标柱：首先将上层 N-1个盘片的盘片塔，从开始柱，经由目标柱，移动到中间柱；然后将第N个（最大的）盘片，从开始柱，移动到目标柱。
- 最后将放置在中间柱的 N-1 个盘片的盘片塔，经由开始柱，移动到目标柱。基本结束条件，也就是最小规模问题变为：1个盘片的移动问题。

```python
def move_tower(height, start_pole, mid_pole, target_pole):
    if height >= 1:
        # 开始柱  目标柱  中间柱
        move_tower(height - 1, start_pole, target_pole, mid_pole)
        # 记录移动
        move_disk(height, start_pole, target_pole)
        # 中间柱  开始柱  目标柱
        move_tower(height - 1, mid_pole, start_pole, target_pole)
 
def move_disk(disk, start_pole, target_pole):
    print(f"将 {disk} 号盘子从 {start_pole}号柱 移到 {target_pole}号柱")
 
move_tower(3, "1", "2", "3")    # 2^n - 1次
print("Complete!")
```

![截屏2023-12-15 21.55.29](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-15 21.55.29.png)

# 二、Python类与对象

## （一）代码封装与面向对象OOP

### 1.代码封装

代码封装，其实就是隐藏实现功能的具体代码，仅留给用户使用的接口，就好像使用计算机，用户只需要使用键盘、鼠标就可以实现一些功能，而根本不需要知道其内部是如何工作的。

比如说，将乱七八糟的数据扔进列表中，这就是一种简单的封装，是数据层面的封装；把常用的代码块打包成一个函数，这也是一种封装，是语句层面的封装。

### 2.面向对象编程

面向对象编程，也是一种封装的思想，不过显然比以上两种封装更先进，它可以更好地模拟真实世界里的事物（将其视为对象），并把描述特征的数据和代码块（函数）封装到一起。

打个比方，若在某游戏中设计一个乌龟的角色，应该如何来实现呢？使用面向对象的思想会更简单，可以分为如下两个方面进行描述：

1. 从表面特征来描述，例如，绿色的、有 4 条腿、重 10 kg、有外壳等等。
2. 从所具有的的行为来描述，例如，它会爬、会吃东西、会睡觉、会将头和四肢缩到壳里，等等。

如果将乌龟用代码来表示，则其表面特征可以用变量来表示，其行为特征可以通过建立各种函数来表示。参考代码如下所示：

```python
class tortoise:
    bodyColor = "绿色"
    footNum = 4
    weight = 10
    hasShell = True
    #会爬
    def crawl(self):
        print("乌龟会爬")
    #会吃东西
    def eat(self):
        print("乌龟吃东西")
    #会睡觉
    def sleep(self):
        print("乌龟在睡觉")
    #会缩到壳里
    def protect(self):
        print("乌龟缩进了壳里")
```

### 3.面向对象相关术语

**·类：**可以理解是一个模板，通过它可以创建出无数个具体实例。比如，前面编写的 tortoise 表示的只是乌龟这个物种，通过它可以创建出无数个实例来代表各种不同特征的乌龟（这一过程又称为类的实例化）。
**·对象：**类并不能直接使用，通过类创建出的实例（又称对象）才能使用。这有点像汽车图纸和汽车的关系，图纸本身（类）并不能为人们使用，通过图纸创建出的一辆辆车（对象）才能使用。
**·属性：**类中的所有变量称为属性。例如，tortoise 这个类中，bodyColor、footNum、weight、hasShell 都是这个类拥有的属性。
**·方法：**类中的所有函数通常称为方法。不过，和函数所有不同的是，类方法至少要包含一个 self 参数（后续会做详细介绍）。例如，tortoise 类中，crawl()、eat()、sleep()、protect() 都是这个类所拥有的方法，类方法无法单独使用，只能和类的对象一起使用。

------

## （二）类的定义

Python 类是由类头（class 类名）和类体（统一缩进的变量和函数）构成。例如，下面程序定义一个 TheFirstDemo 类：

```python
class TheFirstDemo:
    '''这是一个学习Python定义的第一个类'''
    # 下面定义了一个类属性
    add = 'http://c.biancheng.net'
    # 下面定义了一个say方法
    def say(self, content):
        print(content)
```

我们创建了一个名为 TheFirstDemo 的类，其包含了一个名为 add 的类属性。注意，根据定义属性位置的不同，在各个类方法之外定义的变量称为类属性或类变量（如 add 属性），而在类方法中定义的属性称为实例属性（或实例变量）。

同时，TheFirstDemo 类中还包含一个 say() 类方法，细心的读者可能已经看到，该方法包含两个参数，分别是 self 和 content。可以肯定的是，content 参数就只是一个普通参数，没有特殊含义，但 self 比较特殊，并不是普通的参数，它的作用会在后续章节中详细介绍。

------

## （三）Python **init**()类构造方法

在创建类时，我们可以手动添加一个 **init**() 方法，该方法是一个特殊的类实例方法，称为构造方法（或构造函数）。

构造方法用于创建对象时使用，每当创建一个类的实例对象时，[Python](http://c.biancheng.net/python/) 解释器都会自动调用它。

另外，**init**() 方法可以包含多个参数，但必须包含一个名为 self 的参数，且必须作为第一个参数。也就是说，类的构造方法最少也要有一个 self 参数。例如，仍以 TheFirstDemo 类为例，添加构造方法的代码如下所示：

```python
class TheFirstDemo:
    '''这是一个学习Python定义的第一个类'''
    #构造方法
    def __init__(self):
        print("调用构造方法")
    # 下面定义了一个类属性
    add = 'http://c.biancheng.net'
    # 下面定义了一个say方法
    def say(self, content):
        print(content)
zhangshan = TheFirstDemo()
```

![截屏2023-10-22 14.54.31](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 14.54.31.png)

这行代码的含义是创建一个名为 zhangsan 的 TheFirstDemo 类对象，显然，在创建 zhangsan 这个对象时，隐式调用了我们手动创建的 **init**() 构造方法。

不仅如此，在 **init**() 构造方法中，除了 self 参数外，还可以自定义一些参数，参数之间使用逗号“,”进行分割。例如，下面的代码在创建 **init**() 方法时，额外指定了 2 个参数：

```python
class Website:
    '''这是学习python定义的一个类'''
    def __init__(self,name,add):
        print('The Website of',name,'is:',add)

PY_Class_Learning = Website('【Python学习教程】Python类和对象','https://blog.csdn.net/qq_41854911/article/details/122658138?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169795135716800213023704%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169795135716800213023704&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-122658138-null-null.142^v96^pc_search_result_base3&utm_term=python类与对象&spm=1018.2226.3001.4187')
```

![截屏2023-10-22 15.05.12](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 15.05.12.png)

注意，由于创建对象时会调用类的构造方法，如果构造函数有多个参数时，需要手动传递参数，传递方式如代码中所示

可以看到，虽然构造方法中有 self、name、add 3 个参数，但实际需要传参的仅有 name 和 add，也就是说，self 不需要手动传递参数。

关于 self 参数，后续章节会做详细介绍，这里只需要知道，在创建类对象时，无需给 self 传参即可。

------

## （四）类对象的创建和使用

### 1.类的实例化

我们已经学会了如何定义一个类，但要想使用他，还需要创建类的对象，创建类对象的过程，又称为类的实例化。

定义类时，如果没有手动添加 **init**() 构造方法，又或者添加的 **init**() 中仅有一个 self 参数，则创建类对象时的参数可以省略不写。

类变量和实例变量，简单地理解，定义在各个类方法之外（包含在类中）的变量为类变量（或者类属性），定义在类方法中的变量为实例变量（或者实例属性）

### 2.类对象的使用

总的来说，实例化后的类对象可以执行以下操作：

- 访问或修改类对象具有的实例变量，甚至可以添加新的实例变量或者删除已有的实例变量；
- 调用类对象的方法，包括调用现有的方法，以及给类对象动态添加方法。

```python
class CLanguage:
  # 下面定义两个类变量
  name = 'C语言中文网'
  add = 'http://c.biancheng.net'
  def __init__(self,name,add):
    # 下面定义两个实例变量
    self.name = name
    self.add = add
    print(name,'网址为:',add)
  # 下面定义一个say方法
  def say(self,content):
    print(content)
```

以上述创建的类为例，演示如何通过 clanguage 对象调用类中的实例变量和方法：

```python
# 将该CLanguage对象赋给clanguage变量
clanguage = CLanguage('C语言中文网','http://c.biancheng.net')
# 输出name和add实例变量的值
print(clanguage.name,clanguage.add)
# 修改实例变量的值
clanguage.name="Python教程"
clanguage.add="http://c.biancheng.net/python"
# 调用clanguage的say()方法
clanguage.say("人生苦短，我用Python")
# 再次输出name和add的值
print(clanguage.name,clanguage.add)
```

![截屏2023-10-22 15.53.40](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 15.53.40.png)

### 3.为对象添加动态方法

Python 也允许为对象动态增加方法。以上述的 Clanguage 类为例，由于其内部只包含一个 say() 方法，因此该类实例化出的 clanguage 对象也只包含一个 say() 方法。但其实，我们还可以为 clanguage 对象动态添加其它方法。

需要注意的一点是，为 clanguage 对象动态增加的方法，Python 不会自动将调用者自动绑定到第一个参数（即使将第一个参数命名为 self 也没用）。例如如下代码：

```python
# 先定义一个函数
def info(self):
    print("---info函数---", self)
# 使用info对clanguage的foo方法赋值（动态绑定方法）
clanguage.foo = info
# Python不会自动将调用者绑定到第一个参数，
# 因此程序需要手动将调用者绑定为第一个参数
clanguage.foo(clanguage)  # ①
# 使用lambda表达式为clanguage对象的bar方法赋值（动态绑定方法）
clanguage.bar = lambda self: print('--lambda表达式--', self)
clanguage.bar(clanguage) # ②
```

![截屏2023-10-22 16.05.45](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 16.05.45.png)

------

## （五）self用法详解

在定义类的过程中，无论是显式创建类的构造方法，还是向类中添加实例方法，都要求将 self 参数作为方法的第一个参数。例如，定义一个 Person 类：

```python
class Person:
    def __init__(self):
        print("正在执行构造方法")
    # 定义一个study()实例方法
    def study(self,name):
        print(name,"正在学Python")
```

事实上，Python 只是规定，无论是构造方法还是实例方法，最少要包含一个参数，并没有规定该参数的具体名称。之所以将其命名为 self，只是程序员之间约定俗成的一种习惯，遵守这个约定，可以使我们编写的代码具有更好的可读性（大家一看到 self，就知道它的作用）。

那么，self 参数的具体作用是什么呢？打个比方，如果把类比作造房子的图纸，那么类实例化后的对象是真正可以住的房子。根据一张图纸（类），我们可以设计出成千上万的房子（类对象），每个房子长相都是类似的（都有相同的类变量和类方法），但它们都有各自的主人，那么如何对它们进行区分呢？

当然是通过 self 参数，它就相当于每个房子的门钥匙，可以保证每个房子的主人仅能进入自己的房子（每个类对象只能调用自己的类变量和类方法）。

也就是说，同一个类可以产生多个对象，当某个对象调用类方法时，该对象会把自身的引用作为第一个参数自动传给该方法，换句话说，Python 会自动绑定类方法的第一个参数指向调用该方法的对象。如此，Python解释器就能知道到底要操作哪个对象的方法了。

因此，程序在调用实例方法和构造方法时，不需要手动为第一个参数传值。例如，更改前面的 Person 类，如下所示：

```python
class Person:
    def __init__(self):
        print("正在执行构造方法")
    # 定义一个study()实例方法
    def study(self):
        print(self,"正在学Python")
zhangsan = Person()
zhangsan.study()
lisi = Person()
lisi.study()
```

![截屏2023-10-22 16.29.13](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 16.29.13.png)

上面代码中，study() 中的 self 代表该方法的调用者，即谁调用该方法，那么 self 就代表谁。

另外，对于构造函数中的 self 参数，其代表的是当前正在初始化的类对象。举个例子：

```python
class Person:
    name = "xxx"
    def __init__(self,name):
        self.name=name
zhangsan = Person("zhangsan")
print(zhangsan.name)
lisi = Person("lisi")
print(lisi.name)
```

可以看到，zhangsan 在进行初始化时，调用的构造函数中 self 代表的是 zhangsan；而 lisi 在进行初始化时，调用的构造函数中 self 代表的是 lisi。

![截屏2023-10-22 16.34.21](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 16.34.21.png)

除了类对象可以直接调用类方法，还有一种函数调用的方式，例如：

```python
class Person:
    def who(self):
        print(self)
zhangsan = Person()
#第一种方式
zhangsan.who()
#第二种方式
who = zhangsan.who
who()#通过 who 变量调用zhangsan对象中的 who() 方法
```

![截屏2023-10-22 16.36.09](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-22 16.36.09.png)

显然，无论采用哪种方法，self 所表示的都是实际调用该方法的对象

总之，无论是类中的构造函数还是普通的类方法，实际调用它们的谁，则第一个参数 self 就代表谁。

------

## （六）类变量和实例变量（类属性和实例属性）

无论是类属性还是类方法，都无法像普通变量或者函数那样，在类的外部直接使用它们。我们可以将类看做一个独立的空间，则类属性其实就是在类体中定义的变量，类方法是在类体中定义的函数。

在类体中，根据变量定义的位置不同，以及定义的方式不同，类属性又可细分为以下 3 种类型：

·类体中、所有函数之外：此范围定义的变量，称为类属性或类变量；
·类体中，所有函数内部：以“self.变量名”的方式定义的变量，称为实例属性或实例变量；
·类体中，所有函数内部：以“变量名=变量值”的方式定义的变量，称为局部变量。

### 1.类变量（类属性）

类变量指的是在类中，但在各个类方法外定义的变量。举个例子：

```python
class CLanguage :
    # 下面定义了2个类变量
    name = "C语言中文网"
    add = "http://c.biancheng.net"
    # 下面定义了一个say实例方法
    def say(self, content):
        print(content)
```

上面程序中，name 和 add 就属于类变量。

类变量的特点是，**所有类的实例化对象都同时共享类变量**，也就是说，类变量在所有实例化对象中是作为公用资源存在的。类方法的调用方式有 2 种，既可以使用类名直接调用，也可以使用类的实例化对象调用。

**⚠️注：**因为类变量为所有实例化对象共有，通过类名修改类变量的值，会影响所有的实例化对象。但通过类对象是无法修改类变量的。通过类对象对类变量赋值，其本质将不再是修改类变量的值，而是在给该对象定义新的实例变量

### 2.实例变量（实例属性）

实例变量指的是在任意类方法内部，以“self.变量名”的方式定义的变量，其特点是只作用于调用方法的对象。另外，实例变量只能通过对象名访问，无法通过类名访问。

```python
class CLanguage :
    def __init__(self):
        self.name = "C语言中文网"
        self.add = "http://c.biancheng.net"
    # 下面定义了一个say实例方法
    def say(self):
        self.catalog = 13
```

此 CLanguage 类中，name、add 以及 catalog 都是实例变量。其中，由于 init() 函数在创建类对象时会自动调用，而 say() 方法需要类对象手动调用。因此，CLanguage 类的类对象都会包含 name 和 add 实例变量，而只有调用了 say() 方法的类对象，才包含 catalog 实例变量。

类中，实例变量和类变量可以同名，但这种情况下使用类对象将无法调用类变量，它会首选实例变量，这也是不推荐“类变量使用对象名调用”的原因。

另外，和类变量不同，通过某个对象修改实例变量的值，不会影响类的其它实例化对象，更不会影响同名的类变量,例如：

```python
class CLanguage :
    name = "xxx"  #类变量
    add = "http://"  #类变量
    def __init__(self):
        self.name = "C语言中文网"   #实例变量
        self.add = "http://c.biancheng.net"   #实例变量
    # 下面定义了一个say实例方法
    def say(self):
        self.catalog = 13  #实例变量
clang = CLanguage()
#修改 clang 对象的实例变量
clang.name = "python教程"
clang.add = "http://c.biancheng.net/python"
print(clang.name)
print(clang.add)
clang2 = CLanguage()
print(clang2.name)
print(clang2.add)
#输出类变量的值
print(CLanguage.name)
print(CLanguage.add)
```

![截屏2023-10-23 11.19.05](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-23 11.19.05.png)

不仅如此，Python 只支持为特定的对象添加实例变量。例如，在之前代码的基础上，为 clang 对象添加 money 实例变量，实现代码为：

```python
clang.money = 30
print(clang.money)
```

### 3.局部变量

除了实例变量，类方法中还可以定义局部变量。和前者不同，局部变量直接以“变量名=值”的方式进行定义，例如：

```python
class CLanguage :
    # 下面定义了一个say实例方法
    def count(self,money):
        sale = 0.8*money
        print("优惠后的价格为：",sale)
clang = CLanguage()
clang.count(100)
```

通常情况下，定义局部变量是为了所在类方法功能的实现。需要注意的一点是，局部变量只能用于所在函数中，函数执行完成后，局部变量也会被销毁。

------

## （七）实例方法、静态方法与类方法

区分这 3 种类方法是非常简单的，即采用 @classmethod 修饰的方法为类方法；采用 @staticmethod 修饰的方法为静态方法；不用任何修改的方法为实例方法。

其中 @classmethod 和 @staticmethod 都是函数装饰器，后续章节会对其做详细介绍。

### 1.实例方法

通常情况下，在类中定义的方法默认都是实例方法。前面章节中，我们已经定义了不只一个实例方法。不仅如此，类的构造方法理论上也属于实例方法，只不过它比较特殊。

比如，下面的类中就用到了实例方法：

```python
class CLanguage:
    #类构造方法，也属于实例方法
    def __init__(self):
        self.name = "C语言中文网"
        self.add = "http://c.biancheng.net"
    # 下面定义了一个say实例方法
    def say(self):
        print("正在调用 say() 实例方法")
```

实例方法最大的特点就是，它最少也要包含一个 self 参数，用于绑定调用此方法的实例对象（Python 会自动完成绑定）。实例方法通常会用类对象直接调用，例如：

```python
clang = CLanguage()
clang.say()
```

当然，Python 也支持使用类名调用实例方法，但此方式需要手动给 self 参数传值。例如：

```python
#类名调用实例方法，需手动给 self 参数传值
clang = CLanguage()
CLanguage.say(clang)
```

### 2.类方法

Python 类方法和实例方法相似，它最少也要包含一个参数，只不过类方法中通常将其命名为 cls，Python 会自动将类本身绑定给 cls 参数（注意，绑定的不是类对象）。也就是说，我们在调用类方法时，无需显式为 cls 参数传参。

和 self 一样，cls 参数的命名也不是规定的（可以随意命名），只是 Python 程序员约定俗称的习惯而已。

和实例方法最大的不同在于，类方法需要使用＠classmethod修饰符进行修饰，例如：

```python
class CLanguage:
    #类构造方法，也属于实例方法
    def __init__(self):
        self.name = "C语言中文网"
        self.add = "http://c.biancheng.net"
    #下面定义了一个类方法
    @classmethod
    def info(cls):
        print("正在调用类方法",cls)
```

**⚠️注：**如果没有 ＠classmethod，则 Python 解释器会将 fly() 方法认定为实例方法，而不是类方法。

类方法推荐使用类名直接调用，当然也可以使用实例对象来调用（不推荐）。例如，在上面 CLanguage 类的基础上，在该类外部添加如下代码：

```python
#使用类名直接调用类方法
CLanguage.info()
#使用类对象调用类方法
clang = CLanguage()
clang.info()
```

### 3.静态方法

静态方法，其实就是我们学过的函数，和函数唯一的区别是，静态方法定义在类这个空间（类命名空间）中，而函数则定义在程序所在的空间（全局命名空间）中。

静态方法没有类似 self、cls 这样的特殊参数，因此 Python 解释器不会对它包含的参数做任何类或对象的绑定。也正因为如此，类的静态方法中无法调用任何类属性和类方法。

静态方法需要使用`＠staticmethod`修饰，例如：

```python
class CLanguage:
    @staticmethod
    def info(name,add):
        print(name,add)
```

静态方法的调用，既可以使用类名，也可以使用类对象，例如：

```python
#使用类名直接调用静态方法
CLanguage.info("C语言中文网","http://c.biancheng.net")
#使用类对象调用静态方法
clang = CLanguage()
clang.info("Python教程","http://c.biancheng.net/python")
```

在实际编程中，几乎不会用到类方法和静态方法，因为我们完全可以使用函数代替它们实现想要的功能，但在一些特殊的场景中（例如工厂模式中），使用类方法和静态方法也是很不错的选择。

------

## （八）调用实例方法

通过前面的学习，类方法大体分为 3 类，分别是类方法、实例方法和静态方法，其中实例方法用的是最多的。我们知道，实例方法的调用方式其实有 2 种，既可以采用类对象调用，也可以直接通过类名调用。

通常情况下，我们习惯使用类对象调用类中的实例方法。但如果想用类调用实例方法，不能像如下这样：

```python
class CLanguage:
    def info(self):
        print("我正在学 Python")
#通过类名直接调用实例方法
CLanguage.info()
```

运行上面代码，程序会报出如下错误：

```terminal
Traceback (most recent call last):
File “D:\python3.6\demo.py”, line 5, in 
CLanguage.info()
TypeError: info() missing 1 required positional argument: ‘self’
```

最后一行报错信息提示我们，调用 info() 类方式时缺少给 self 参数传参。这意味着，和使用类对象调用实例方法不同，**通过类名直接调用实例方法时，Python 并不会自动给 self 参数传值。**

修改代码也很简单，即手动为self参数传值，修改后的代码如下：

```python
class CLanguage:
    def info(self):
        print("我正在学 Python")
clang = CLanguage()
#通过类名直接调用实例方法
CLanguage.info(clang)
```

![截屏2023-10-23 21.40.53](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-23 21.40.53.png)

可以看到，通过手动将 clang 这个类对象传给了 self 参数，使得程序得以正确执行。实际上，这里调用实例方法的形式完全是等价于 `clang.info()。`

值得一提的是，上面的报错信息只是让我们手动为 self 参数传值，但并没有规定必须传一个该类的对象，其实完全可以任意传入一个参数，例如：

```python
class CLanguage:
    def info(self):
        print(self,"正在学 Python")
#通过类名直接调用实例方法
CLanguage.info("zhangsan")
```

运行结果为：

```
zhangsan 正在学 Python
```

可以看到，“zhangsan” 这个字符串传给了 info() 方法的 self 参数。显然，无论是 info() 方法中使用 self 参数调用其它类方法，还是使用 self 参数定义新的实例变量，胡乱的给 self 参数传参都将会导致程序运行崩溃。

总的来说，Python 中允许使用类名直接调用实例方法，但必须手动为该方法的第一个 self 参数传递参数，这种调用方法的方式被称为“非绑定方法”。
**⚠️注：**用类的实例对象访问类成员的方式称为绑定方法，而用类名调用类成员的方式称为非绑定方法。

------

## （九）Python类命名空间

前面章节已经不只一次提到，Python 类体中的代码位于独立的命名空间（称为类命名空间）中。换句话说，所有用 class 关键字修饰的代码块，都可以看做是位于独立的命名空间中。

和类命名空间相对的是全局命名空间，即整个 Python 程序默认都位于全局命名空间中。而类体则独立位于类命名空间中。

举例：

```python
#全局空间定义变量
name = "C语言中文网"
add = "http://c.biancheng.net"
# 全局空间定义函数
def say ():
    print("我在学习Python--全局")
class CLanguage:
    # 定义CLanguage空间的say函数
    def say():
        print("我在学习Python--CLanguage独立空间")
    # 定义CLanguage空间的catalog变量
    name = "C语言中文网"
    add = "http://c.biancheng.net"
#调用全局的变量和函数
print(name,add)
say()
#调用类独立空间的变量和函数
print(CLanguage.name,CLanguage.add)
CLanguage.say()
```

可以看到，相比位于全局命名空间的变量和函数，位于类命名空间中的变量和函数在使用时，只需要标注 CLanguage 前缀即可。

甚至，Python 还允许直接在类命名空间中编写可执行程序（例如输出语句、分支语句等），例如：

```python
class CLanguage:
    #直接编写可执行代码
    print('正在执行 CLanguage 类空间中的代码')
    for i in range(5):
        print(i)
```

![截屏2023-10-23 21.56.44](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-23 21.56.44.png)

显然，上面这些位于类命名空间的可执行程序，和位于全局命令空间相比，并没有什么不同。

## （十）封装机制及实现方法

### 1.封装的定义与优势

简单的理解封装（Encapsulation），即在设计类时，刻意地将一些属性和方法隐藏在类的内部，这样在使用此类时，将无法直接以“类对象.属性名”（或者“类对象.方法名(参数)”）的形式调用这些属性（或方法），而只能用未隐藏的类方法间接操作这些隐藏的属性和方法。

就好比使用电脑，我们只需要学会如何使用键盘和鼠标就可以了，不用关心内部是怎么实现的，因为那是生产和设计人员该操心的。

封装的优势如下：

首先，封装机制保证了类内部数据结构的完整性，因为使用类的用户无法直接看到类中的数据结构，只能使用类允许公开的数据，很好地避免了外部对内部数据的影响，提高了程序的可维护性。

除此之外，对一个类实现良好的封装，用户只能借助暴露出来的类方法来访问数据，我们只需要在这些暴露的方法中加入适当的控制逻辑，即可轻松实现用户对类中属性或方法的不合理操作。

并且，对类进行良好的封装，还可以提高代码的复用性。

### 2.python类实现封装

和其它面向对象的编程语言（如 C++、Java）不同，Python 类中的变量和函数，不是公有的（类似 public 属性），就是私有的（类似 private），这 2 种属性的区别如下：

public：公有属性的类变量和类函数，在类的外部、类内部以及子类（后续讲继承特性时会做详细介绍）中，都可以正常访问；
private：私有属性的类变量和类函数，只能在本类内部使用，类的外部以及子类都无法使用。
但是，Python 并没有提供 public、private 这些修饰符。为了实现类的封装，Python 采取了下面的方法：

默认情况下，Python 类中的变量和方法都是公有（public）的，它们的名称前都没有下划线（_）；
如果类中的变量和函数，其名称以双下划线“__”开头，则该变量（函数）为私有变量（私有函数），其属性等同于 private。
除此之外，还可以定义以单下划线“_”开头的类属性或者类方法（例如 _name、_display(self)），这种类属性和类方法通常被视为私有属性和私有方法，虽然它们也能通过类对象正常访问，但这是一种约定俗称的用法，初学者一定要遵守。

```python
# 封装实现例
class CLanguage :
    def setname(self, name):
        if len(name) < 3:
            raise ValueError('名称长度必须大于3！')
        self.__name = name
    def getname(self):
        return self.__name
    #为 name 配置 setter 和 getter 方法
    name = property(getname, setname)
    def setadd(self, add):
        if add.startswith("http://"):
            self.__add = add
        else:
            raise ValueError('地址必须以 http:// 开头') 
    def getadd(self):
        return self.__add
   
    #为 add 配置 setter 和 getter 方法
    add = property(getadd, setadd)
    #定义个私有方法
    def __display(self):
        print(self.__name,self.__add)
clang = CLanguage()
clang.name = "C语言中文网"
clang.add = "http://c.biancheng.net"
print(clang.name)
print(clang.add)
```

------

## （十一）继承机制及其使用

### 1.继承机制与简单运用

python类的**封装、继承、多态 3 大特性**，前面章节已经详细介绍了 Python 类的封装，本节继续讲解 Python 类的继承机制。

继承机制经常用于创建和现有类功能类似的新类，又或是新类只需要在现有类基础上添加一些成员（属性和方法），但又不想直接将现有类代码复制给新类。也就是说，通过使用继承这种机制，可以轻松实现类的重复使用。

举个例子，假设现有一个 Shape 类，该类的 draw() 方法可以在屏幕上画出指定的形状，现在需要创建一个 Form 类，要求此类不但可以在屏幕上画出指定的形状，还可以计算出所画形状的面积。要创建这样的类，笨方法是将 draw() 方法直接复制到新类中，并添加计算面积的方法。实现代码如下所示：

```python
class Shape:
    def draw(self,content):
        print("画",content)
class Form:
    def draw(self,content):
        print("画",content)
    def area(self):
        #....
        print("此图形的面积为...")
```

当然还有更简单的方法，就是使用类的继承机制。实现方法为：让 From 类继承 Shape 类，这样当 From 类对象调用 draw() 方法时，Python 解释器会先去 From 中找以 draw 为名的方法，如果找不到，它还会自动去 Shape 类中找。如此，我们只需在 From 类中添加计算面积的方法即可，示例代码如下：

```python
class Shape:
    def draw(self,content):
        print("画",content)
class Form(Shape):
    def area(self):
        #....
        print("此图形的面积为...")
```

上面代码中，class From(Shape) 就表示 From 继承 Shape。

Python 中，实现继承的类称为子类，被继承的类称为父类（也可称为基类、超类）。因此在上面这个样例中，From 是子类，Shape 是父类。

子类继承父类时，只需在定义子类时，将父类（可以是多个）放在子类之后的圆括号里即可。语法格式如下：

class 类名(父类1, 父类2, …)：

### 2.多继承

事实上，大部分面向对象的编程语言，都只支持单继承，即子类有且只能有一个父类。而 Python 却支持多继承（C++也支持多继承）。
和单继承相比，多继承容易让代码逻辑复杂、思路混乱，一直备受争议，中小型项目中较少使用，后来的 Java、C#、PHP 等干脆取消了多继承。

使用多继承经常需要面临的问题是，多个父类中包含同名的类方法。对于这种情况，Python 的处置措施是：根据子类继承多个父类时这些父类的前后次序决定，即排在前面父类中的类方法会覆盖排在后面父类中的同名类方法。

举个例子：

```python
class People:
    def __init__(self):
        self.name = People
    def say(self):
        print("People类",self.name)
class Animal:
    def __init__(self):
        self.name = Animal
    def say(self):
        print("Animal类",self.name)
#People中的 name 属性和 say() 会遮蔽 Animal 类中的
class Person(People, Animal):
    pass
zhangsan = Person()
zhangsan.name = "张三"
zhangsan.say()
```

![截屏2023-12-15 22.05.10](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-15 22.05.10.png)

可以看到，当 Person 同时继承 People 类和 Animal 类时，People 类在前，因此如果 People 和 Animal 拥有同名的类方法，实际调用的是 People 类中的。

------

# 三、Python模块、结构与编程规范

## （一）python模块

模块在一个文件中合理组织Python代码，应该建立一种统一且容易的结构。下面就是一种非常合理地布局：
(1) 起始行(Unix)
(2) 模块文档
(3) 模块导入
(4) 变量定义（全局）
(5) 类定义
(6) 函数定义
(7) 主程序（测试代码）

```python
# Python模块的基本结构示例
# 起始行

# 模块文档
"""
这是一个示例Python模块，展示了Python模块的基本结构。
"""

# 模块导入
import os
from collections import deque

# 变量定义
my_variable = "Hello, World!"

# 类定义
class MyClass:
    def __init__(self):
        self.my_attribute = "This is a class attribute."
        self.my_list = [1, 2, 3]
    
    def my_method(self):
        print("This is a class method.")
    
# 函数定义
def my_function():
    print("This is a function.")
    
# 主程序
if __name__ == "__main__":
    my_variable += " --- Variables Rock!"
    print(my_variable)
    my_object = MyClass()
    print(my_object.my_attribute)
    my_object.my_method()
    my_function()
```

这个示例程序展示了以下内容：起始行：使用注释符号 # 开头的行，用于提供对代码的说明或注释。模块文档：使用三个引号 """ 括起来的文本，用于提供对模块的详细说明和文档。模块导入：使用 import 语句导入其他Python模块或库，以便在当前的模块中使用它们。示例中导入了 os 模块和 collections.deque 类型。变量定义：使用赋值语句来定义变量。示例中定义了一个字符串变量 my_variable。类定义：使用 class 关键字来定义一个类。示例中定义了一个名为 MyClass 的类，并定义了类的构造函数 __init__ 和一个方法 my_method。函数定义：使用 def 关键字来定义函数。示例中定义了一个名为 my_function 的函数，并使用 print 语句输出一条消息。主程序：使用 if __name__ == "__main__": 来标识主程序部分。当直接运行该模块时，该部分代码会被执行。在示例中，主程序部分使用了之前定义的变量、类和函数，并调用了一些内置函数，如 os.path.exists()。

## （二）Python风格规范

[参考链接](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)

一些具体的规范如下：

```
1.每行尽量不超过80个字符
Python会将 圆括号, 中括号和花括号中的行隐式的连接起来
如果一个文本字符串在一行放不下, 可以使用圆括号来实现隐式行连接:
x = ('This will build a very long long '
     'long long long long long long string')
可以使用反斜杠换行
例外：
长的导入模块语句
注释里的URL,路径以及其他的一些长标记
不便于换行，不包含空格的模块级字符串常量，比如url或者路径

2.缺毋滥的使用括号
除非是用于实现行连接, 否则不要在返回语句或条件语句中使用括号. 不过在元组两边使用括号是可以的.

3.用4个空格来缩进代码
绝对不要用tab, 也不要tab和空格混用
对于行连接的情况, 你应该要么垂直对齐换行的元素, 
或者使用4空格的悬挂式缩进(这时第一行不应该有参数)

4.序列元素尾部逗号
仅当 ], ), } 和末位元素不在同一行时，推荐使用序列元素尾部逗号
golomb3 = [0, 1, 3]
golomb4 = [
        0,
        1,
        4,
        6,
]

5.空行
顶级定义之间空两行, 方法定义之间空一行
顶级定义之间空两行, 比如函数或者类定义. 
方法定义, 类定义与第一个方法之间, 都应该空一行. 
函数或方法中, 某些地方要是你觉得合适, 就空一行

6.空格
按照标准的排版规范来使用标点两边的空格
括号内不要有空格.
不要在逗号, 分号, 冒号前面加空格, 但应该在它们后面加(除了在行尾)
参数列表, 索引或切片的左括号前不应加空格
在二元操作符两边都加上一个空格
当 = 用于指示关键字参数或默认参数值时, 不要在其两侧使用空格，
但若存在类型注释的时候,需要在 = 周围使用空格
不要用空格来垂直对齐多行间的标记, 因为这会成为维护的负担(适用于:, #, =等):

7.确保对模块, 函数, 方法和行内注释使用正确的风格
使用三重双引号 """
第一条语句
注释应有适当的大写和标点,句子应该尽量完整.
最需要写注释的是代码中那些技巧性的部分. 
对于复杂的操作, 应该在其操作开始前写上若干行注释. 
对于不是一目了然的代码, 应在其行尾添加注释.
为了提高可读性, 注释应该至少离开代码2个空格.
另一方面, 绝不要描述代码
# We use a weighted dictionary search to find out where i is in
# the array.  We extrapolate position based on the largest num
# in the array and the array size and then do binary search to
# get the exact number.
if i & (i-1) == 0:        # True if i is 0 or a power of 2.  x

8.类
如果一个类不继承自其它类, 就显式的从object继承
class SampleClass(object):
         pass

9.字符串
即使参数都是字符串, 使用%操作符或者格式化方法格式化字符串. 
不过也不能一概而论, 你需要在+和%之间好好判定。
x = '%s, %s!' % (imperative, expletive)
x = '{}, {}!'.format(imperative, expletive)
x=f'{imperative}, {expletive}!'
不使用
x = imperative + ', ' + expletive + '!'
避免在循环中用+和+=操作符来累加字符串
为多行字符串使用三重双引号"""而非三重单引号'''.

10.在文件和sockets结束时, 显式的关闭它
推荐使用 “with”语句 以管理文件:
with open("hello.txt") as hello_file:
    for line in hello_file:
        print(line)
对于不支持使用”with”语句的类似文件的对象,使用 contextlib.closing():
import contextlib
with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
    for line in front_page:
        print(line)

11.导入格式
每个导入应该独占一行, typing 的导入除外
导入总应该放在文件顶部, 位于模块注释和文档字符串之后. 
导入应该按照从最通用到最不通用的顺序分组
__future__ 导入   # 要“试用”某一新的特性from __futrue__ import division
标准库导入
import sys
第三方库导入
import tensorflow as tf
本地代码子包导入

12.通常每个语句应该独占一行
不过, 如果测试结果与测试语句在一行放得下, 你也可以将它们放在同一行. 如果是if语句, 只有在没有else时才能这样做. 特别地, 绝不要对 try/except 这样做, 因为try和except不能放在同一行

13.命名
模块名写法: module_name ;
包名写法: package_name ;
类名: ClassName ;
方法名: method_name ;
异常名: ExceptionName ;
函数名: function_name ;
全局常量名: GLOBAL_CONSTANT_NAME ;
全局变量名: global_var_name ;
实例名: instance_var_name ;
函数参数名: function_parameter_name ;
局部变量名: local_var_name . 
函数名,变量名和文件名应该是描述性的,尽量避免缩写,特别要避免使用非项目人员不清楚难以理解的缩写,不要通过删除单词中的字母来进行缩写. 始终使用 .py 作为文件后缀名,不要用破折号.

14.Main
主功能应该放在一个main()函数中
def main():
       pass
if __name__  ==  '__main__':
       main()
所有的顶级代码在模块导入时都会被执行
```

补充——**生成器函数**：指的是函数体中包含yield关键字的函数

## （三）Python项目结构

![截屏2023-12-16 09.40.46](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 09.40.46.png)

Python项目的结构通常会遵循一种一致的模式，以下是一种典型的Python项目结构：

```
ProjectName/
    main.py  # 这是项目的入口点，你可以在这里定义主函数或者应用的主类
    requirements.txt  # 列出项目依赖的所有Python包及其版本号
    readme.txt  #项目说明文件
    setup.py   #安装、部署、打包的脚本
    
    tests/  # 包含所有的测试文件，通常包括单元测试和集成测试
        test_main.py  # 测试主程序的文件
        test_module1.py  # 测试模块1的文件
        ...
    docs/    # 项目文档，包括API文档、用户手册等
        index.html  # API文档的主页
        module1.html  # 模块1的API文档
        ...
    src/    # 所有的源代码
        __init__.py  # 初始化文件，使目录可以作为Python包导入
        module1.py  # 模块1的源代码文件
        module2.py  # 模块2的源代码文件
        ...
```

其他文件说明：

**·readme.txt/readme.md**

README.txt文件可以为用户提供项目的详细描述，通常会包含以下内容：

项目名称、版本和简单的项目描述。

项目的主要功能和用途。

项目的安装和运行说明。

项目目录结构以及如何使用项目中的文件和模块。

项目的依赖项以及如何安装这些依赖项。

项目的贡献者和维护者信息。

其他有用的信息，例如示例代码、常见问题解答等。README.md文档的作用是让其他开发者更容易地理解项目的主要内容、用途、使用方法和维护情况等，从而更容易地使用和贡献该项目。

**·setup.py**
一般来说，用setup.py来管理代码的打包、安装、部署问题。业界标准的写法是用Python流行的打包工具setuptools来管理这些事情。一个项目一定要有一个安装部署工具，能快速便捷的在一台新机器上将环境装好、代码部署好和将程序运行起来。

对于Python项目，手动完成：安装环境、部署代码、运行程序，会遇到过以下问题:
1)安装环境时经常忘了最近又添加了一个新的Python包，结果一到线上运行，程序就出错了。
2)Python包的版本依赖问题。如果依赖的包很多的话，一个一个安装这些依赖是很费时的事情。

setup.py可以将这些事情自动化起来，提高效率、减少出错的概率。
"复杂的东西自动化，能自动化的东西一定要自动化。"是一个非常好的习惯。

setuptools的文档比较庞大，刚接触的话，可能不太好找到切入点。
可以参考一下Python的一个Web框架，flask的 setup.py是如何写的。

**·requirements.txt**
这个文件存在的目的是:方便开发者维护软件的包依赖。将开发过程中新增的包添加进这个列表中，避免在setup.py安装依赖时漏掉软件包。
方便读者明确项目使用了哪些Python包。

这个文件的格式是每一行包含一个包依赖的说明，
通常是flask>=0.10这种格式，要求是这个格式能被pip识别，
这样就可以简单的通过 pip install -r requirements.txt
来把所有Python包依赖都装好了

**·导出软件包环境：pip freeze > requirements.txt**
需要注意的是，这种方法导出的 requirements.txt 文件可能包含一些你的项目并不需要的包。如果你的项目依赖于一些非标准的 Python 包，或者你的项目使用了特定版本的包，你可能需要手动编辑这个文件，以确保它只包含你的项目需要的包和版本。

# 四、Python字符串与正则表达式

## （一）字符串

[参考链接](https://blog.csdn.net/jennifer_love_frank/article/details/116381040)

字符串是包含一系列字符的对象。而字符是长度为 1 的字符串。
在 Python 中，单个字符也是字符串。
但是比较有意思的是，Python 编程语言中是没有字符数据类型的，不过在 C、Kotlin 和 Java 等其他编程语言中是存在字符数据类型的

使用单引号、双引号、三引号或 str() 函数来声明 Python 字符串。

字符串属性
零索引： 字符串中的第一个元素的索引为零，而最后一个元素的索引为 len(string) - 1。
不变性： 这意味着我们不能更新字符串中的字符。

### 1.转义与格式化符号

在python中，我们用\表达转义，而使用%表达格式化，具体的含义如下图：

![截屏2023-12-16 10.33.16](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 10.33.16.png)

### 2.字符串格式化

输出中如何控制保留两位小数，整数补零填充，对齐，百分比格式打印，整数太长使用科学计数法打印等等。可以记住如下7种常见用法：

```
(1)保留小数点后两位
>>> print("{:.2f}".format(3.1415926)) 
3.14
(2). 带符号保留小数点后两位
>>> print("{:+.2f}".format(-1)) 
-1.00
(3)不带小数位
>>> print("{:.0f}".format(2.718)) # 不带小数位
3
(4) 整数补零，填充左边, 宽度为3
>>> print("{:0>3d}".format(5)) # 整数补零，填充左边, 宽度为3
005
(5)以逗号分隔的数字格式
>>> print("{:,}".format(10241024)) # 以逗号分隔的数字格式
10,241,024
(6)百分比格式
>>> print("{:.2%}".format(0.718)) # 百分比格式
71.80%
(7)指数记法
>>> print("{:.2e}".format(10241024)) # 指数记法
1.02e+07
```

### 3.字符串内建函数

字符串中字符大小写的变换：

```
1 str.lower() 
转换str中所有大写字符为小写

2 str.upper()
转换str中所有小写字符为大写

3 str.swapcase() 
翻转str中的大小写

4 str.title() 
返回标题化的字符串。所有单词首字母大写，其余小写

5 str.capitalize()
把字符串的第一个字母大写，其余小写
```

去空格和特殊符号：

```
1 str.strip()
去掉字符串左边和右边的空格和换行符

2 str.strip(str1) 
去掉字符串左边和右边的str1

3 str.lstrip() 
去掉字符串左边的空格和换行符

4 str.rstrip() 
去掉字符串右边的空格和换行符
```

字符串对齐，填充：

```
1 str.ljust(width) 
 返回width长度的字符串，左对齐，不足的用空格补充

2 str.rjust(width) 
 返回width长度的字符串，右对齐，不足的用空格补充

3 str.center(width) 
 返回width长度的字符串，居中对齐，不足的用空格补充

4 str.zfill(width)
返回长度为width长度的字符串，原字符串str右对齐，前面补充0
```

字符串搜索、统计功能：

```
str.find(string,beg=0,end=len(str)) 
 检测string是否包含在str中，beg和end指定检测范围，如果找到则返回开始的索引，否则返回-1

str.rfind(string,beg=0,end=len(str)) 
 检测string是否包含在str中，beg和end指定检测范围，如果找到则返回开始的索引，否则返回-1，类似find只不过是从右边往左边开始找。

str.count(string,beg=0,end=len(str)) 
 返回string在str中出现的次数，在beg-end检测范围内出现则返回出现的次数。

str.index(string,beg=0,end=len(str)) 
 和find方法一样，只不过如果string不在str中会报异常。

str.rindex(string,beg=0,end=len(str))
和index方法一样，不过是从右边开始。
```

字符串检测函数：

```
str.isalnum() ：如果str至少有一个字符，并且所有字符都是字母或者数字，则返回true，否则返回false

str.isalpha() ： 如果str至少有一个字符，并且所有字符都是字母，则返回true，否则返回false

str.isdigit() ：如果str至少有一个字符，并且所有字符都是数字，则返回true，否则返回false

str.isspace() ：如果str至少有一个字符，并且所有字符都是空格，则返回true，否则返回false

str.islower() ：如果str至少有一个字符，并且所有字符都是小写字母，则返回true，否则返回false

str.isupper() ：如果str至少有一个字符，并且所有字符都是大写字母，则返回true，否则返回false

str.istitle()： 如果str每个单词首字母大写其它小写，则返回true,否则返回false

str.startswith(prefix[,start[,end]]) :如果str是以prefix开头，则返回true,否则返回false

str.endswith(suffix[,start[,end]]) :如果str是以prefix结尾，则返回true,否则返回false
```

字符串替换、分割与连接：

```
str.replace(str1,str2,num=str.count(str1)) 
//替换指定次数的str1为str2,替换次数不超过num次。

str.split(str1=" ",num=str.count(str1)) 
以str1为分隔符，把str分成一个list。num表示分割的次数。默认的分割符为空白字符

str.rsplit(str1=" ",num=str.count(str1)) 
和split相似，只是从右边开始分割

str.splitlines(num=str.count(" \n"))
把str按行分割符分为一个list，num如果指定则是分割成多少行

字符串连接
str.join(seq) 
把seq代表的序列(字符串序列)，用str连接起来
```

⚠️注意：同样是处理字符串的操作，str和正则方法的区别是什么呢？其实可以简单理解为：str内置方法用来处理简单字符串；正则用来处理复杂的字符串。

------

## （二）正则表达式

正则表达式(regular expression)描述了一种字符串匹配的模式（pattern），是字符串处理的有力工具和技术，可以快速、准确地完成复杂的查找、替换等处理要求。

Python中，re模块提供了正则表达式操作所需要的功能。

![截屏2023-12-16 10.43.07](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 10.43.07.png)

### 1.锚字符（边界字符）

```
^     行首匹配，和在[]里的^不是一个意思
$     行尾匹配

\A    匹配字符串的开始，和^的区别是：
\A只匹配整个字符串的开头，即使在re.M的模式下也不会匹配其他行的行首
\Z    匹配字符串结束，它和$的区别是：
\Z只匹配整个字符串的结束，即使在 re.M的模式下也不会匹配其他行的行尾

\b    匹配一个单词的边界，也就是值单词和空格间的位置
\B    匹配非单词的边界 
```

### 2.普通字符

普通字符包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号

### 3.非打印字符

非打印字符也可以是正则表达式的组成部分，具体如下：

```
字符	描述
\cx	匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 
             或   回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为
             一个原义的 'c' 字符。
\f	匹配一个换页符。等价于 \x0c 和 \cL。
\n	匹配一个换行符。等价于 \x0a 和 \cJ。
\r	匹配一个回车符。等价于 \x0d 和 \cM。
\s	匹配任何空白字符，包括空格、制表符、换页符等等。
            等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。
\S	匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。
\t	匹配一个制表符。等价于 \x09 和 \cI。
\v	匹配一个垂直制表符。等价于 \x0b 和 \cK。
```

### 4.特殊字符

所谓特殊字符，就是一些有特殊含义的字符.若要匹配这些特殊字符，必须首先使字符"转义"，即，将反斜杠字符\ 放在它们前面

```
特别字符	描述
$	匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 
              属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用 \$。
( )	标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。
 *	匹配前面的子表达式零次或多次。要匹配 * 字符，请使用 \*。
+	匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 \+。
.	匹配任何单字符。要匹配 . ，请使用 \. 。
[	标记一个中括号表达式的开始。要匹配 [，请使用 \[。
?	匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。
\	将下一个字符标记为或特殊字符、或原义字符、或向后引用、 或八进
              制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"。
^	匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在
             方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。
{	标记限定符表达式的开始。要匹配 {，请使用 \{。
|	指明两项之间的一个选择。要匹配 |，请使用 \|。
```

### 5.限定符

限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 * 或 + 或 ? 或 {n} 或 {n,} 或 {n,m} 共6种。

```
字符	描述
*	匹配前面的子表达式零次或多次。
             例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。
+	匹配前面的子表达式一次或多次。例如，
            'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。
?	匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 、
            "does" 中的 "does" 、 "doxy" 中的 "do" 。? 等价于 {0,1}。
{n}	n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 
             中的 'o'，但是能匹配 "food" 中的两个 o。
{n,}	n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中
            的 'o'，但能匹配 "foooood" 中的所有 o。
           'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。
{n,m}	m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。
           例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 
           'o?'。请注意在逗号和两个数之间不能有空格。
```

### 6.定位符

定位符能够将正则表达式固定到行首或行尾。它们还使您能够创建这样的正则表达式，这些正则表达式出现在一个单词内、在一个单词的开头或者一个单词的结尾

```
字符	描述
^	匹配输入字符串开始的位置。
$	匹配输入字符串结尾的位置。
\b	匹配一个单词边界，即字与空格间的位置。
\B	非单词边界匹配。
```

------

# 五、数据存储

## （一）文件存储

文件存储形式多种多样，比如可以保存成TXT纯文本形式，也可以保存为JSON格式、CSV格式等。

### 1.TXT文本存储

TXT文本的操作非常简单，而且TXT文本几乎兼容任何平台，但是这有个缺点，那就是不利于检索。所以如果对检索和数据结构要求不高，追求方便第一的话，可以采用TXT文本存储。

打开方式：

![截屏2023-10-18 17.33.41](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-18 17.33.41.png)

```python
# e.g. 保存知乎上“发现”页面的“热门话题”部分，将其问题和答案统一保存成文本形式
import requests
from pyquery import PyQuery as pq

url = 'https://www.zhihu.com/explore'
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'
}
html = requests.get(url, headers=headers).text
doc = pq(html)
items = doc('.explore-tab .feed-item').items()

# 使用with as语法。在with控制块结束时，文件会自动关闭，所以就不需要再调用close()方法了
with open('explore.txt', 'a', encoding='utf-8') as file:
for item in items:
    question=item.find('h2').text()
    author=item.find('.autour-link-line').text()
    answer=pq(item.find('.content').html()).text()

    file.write('\n'.join([question, author, answer]))
        file.write('\n' + '=' * 50 + '\n')
```

## （二）JSON文件存储

### 1.读取JSON

调用json库的loads()方法将JSON文本字符串转为JSON对象，可以通过dumps()方法将JSON对象转为文本字符串。

**注：**JSON字符串的表示需要用双引号，否则loads()方法会解析失败。

```python
# 将JSON文本字符串转化为JSON对象
import json
 
str = '''
[{
    "name": "Bob",
    "gender": "male",
    "birthday": "1992-10-18"
}]
'''
data = json.loads(str)
print(data)

# 从JSON文本中读取内容
import json
 
with open('data.json', 'r') as file:
    str = file.read()
    data = json.loads(str)
    print(data)

```

### 2.输出JSON

调用dumps()方法将JSON对象转化为字符串。

```python
import json
 
data = [{
    'name': 'Bob',
    'gender': 'male',
    'birthday': '1992-10-18'
}]
with open('data.json', 'w') as file:
    file.write(json.dumps(data))

# 输出中文字符，需要指定参数ensure_ascii为False，另外还要规定文件输出的编码
import json 
data = [{
    'name': '王伟',
    'gender': '男',
    'birthday': '1992-10-18'
}]
with open('data.json', 'w', encoding='utf-8') as file:
    file.write(json.dumps(data, indent=2, ensure_ascii=False))

```

如果想保存JSON的格式，可以再加一个参数indent，代表缩进字符个数。with open('data.json', 'w') as file:    file.write(json.dumps(data, indent=2))

## （三）CSV文件存储

单行写入：

```python
import csv
 
with open('data.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow(['id', 'name', 'age'])
    writer.writerow(['10001', 'Mike', 20])
    writer.writerow(['10002', 'Bob', 22])
    writer.writerow(['10003', 'Jordan', 21])
```

可以调用writerows()方法同时写入多行，此时参数就需要为二维列表：

```python
import csv
 
with open('data.csv', 'w') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow(['id', 'name', 'age'])
    writer.writerows([['10001', 'Mike', 20], ['10002', 'Bob', 22], ['10003', 'Jordan', 21]])
```

字典写入方法与中文输入：

```python
import csv
 
with open('data.csv', 'a', encoding='utf-8') as csvfile:
    fieldnames = ['id', 'name', 'age']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    writer.writerow({'id': '10005', 'name': '王伟', 'age': 22})
```

## （四）关系型数据库存储

**注：**以下例子为Python 3下MySQL的存储，需要使用pymysql库

### 1.数据库连接与创建

```python
import pymysql
# 数据库连接
conn = pymysql.connect(host="localhost", user="root", password="ycr20030107",
                       cursorclass=pymysql.cursors.DictCursor) # password要输入自己root用户的密码

# 游标设置
cur = conn.cursor()
# 执行sql建表语句
cur.execute("create database performanceManagement")
# 关闭游标
cur.close()
# 断开数据库连接
conn.close()
```

通过如下代码查看已经创建的数据库：

```python
import pymysql  
  
# 创建连接  
connection = pymysql.connect(host='localhost', user='root', password='ycr20030107')  
  
try:  
    with connection.cursor() as cursor:  
        # 执行 SHOW DATABASES 查询  
        cursor.execute('SHOW DATABASES')  
  
        # 获取查询结果  
        databases = cursor.fetchall()  
  
        # 打印所有数据库  
        for db in databases:  
            print(db[0])  
  
finally:  
    # 关闭连接  
    connection.close()
```

### 2.创建表

![截屏2023-09-25 21.08.31](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-09-25 21.08.31.png)

```python
import pymysql
# 连接数据库
db = pymysql.connect(host='localhost', 
user="root", password='ycr20030107', 
database='performanceManagement',
                    cursorclass=pymysql.cursors.DictCursor)
 
# 设置游标
cur = db.cursor()
# 执行sql创建语句
#创建班级表
cur.execute('''CREATE TABLE `class` (
          `class_id` INT(2) NOT NULL AUTO_INCREMENT,
          `caption` VARCHAR (12) NOT NULL,
          PRIMARY KEY (`class_id`)    
        );
        ''')
#创建学生表
cur.execute('''CREATE TABLE student (
          student_id INT(2) NOT NULL AUTO_INCREMENT,
          sname VARCHAR (12) NOT NULL ,
          gender VARCHAR (3) NOT NULL ,
          class_id INT (2) NOT NULL ,
          PRIMARY KEY (student_id)    
        )
        ''')
#创建老师表
cur.execute('''CREATE TABLE teacher (
          teacher_id INT(2) NOT NULL AUTO_INCREMENT,
          tname VARCHAR (12) NOT NULL ,
          PRIMARY KEY (teacher_id)    
        )
        ''')
#创建课程表
cur.execute('''CREATE TABLE course (
          course_id INT(2) NOT NULL,
          cname VARCHAR (12) NOT NULL ,
          teacher_id INT(2) NOT NULL,
          PRIMARY KEY (course_id,teacher_id),
          CONSTRAINT teacher_id1
          foreign key(teacher_id) references teacher(teacher_id)
        )
        ''')
#创建成绩表
cur.execute('''CREATE TABLE score (
          score_id INT(2) NOT NULL AUTO_INCREMENT,
          student_id INT(2) NOT NULL,
          course_id INT(2) NOT NULL,
          number INT(3) NOT NULL,
          PRIMARY KEY (score_id,student_id,course_id),
          CONSTRAINT student_id1
          foreign key(student_id) references student(student_id),
          CONSTRAINT course_id1
          foreign key(course_id) references course(course_id)
        )
        ''')
# 关闭游标
cur.close()
# 关闭数据库连接
db.close()
```

查看已经创建的表

```python
import pymysql  
  
# 创建数据库连接  
connection = pymysql.connect(  
    host='localhost',  
    user='root',  
    password='ycr20030107',  
    database='performanceManagement'  
)  
  
try:  
    # 获取数据库游标  
    cursor = connection.cursor()  
  
    # 执行 SQL 查询语句  
    cursor.execute('SHOW TABLES')  
  
    # 获取查询结果  
    tables = cursor.fetchall()  
  
    # 打印已创建的表名  
    for table in tables:  
        print(table[0])  
  
finally:  
    # 关闭游标和数据库连接  
    cursor.close()  
    connection.close()
```

### 3.数据插入

```python
import pymysql
db = pymysql.connect(host='localhost', 
user="root", password='ycr20030107', 
database='performanceManagement',       
cursorclass=pymysql.cursors.DictCursor)
# 设置游标
cur = db.cursor()

cur.execute( "insert into class values(1,'三年二班')")
cur.execute( "insert into class values(2,'一年三班')")
cur.execute( "insert into class values(3,'三年一班')")
db.commit()#数据提交
cur.execute( "insert into student values(1,'钢蛋','女',1)")
cur.execute( "insert into student values(2,'铁锤','女',1)")
cur.execute( "insert into student values(3,'山炮','男',2)")
db.commit()#数据提交
cur.execute( "insert into teacher values(1,'波多')")
cur.execute( "insert into teacher values(2,'苍空')")
cur.execute( "insert into teacher values(3,'饭岛')")
db.commit()#数据提交
cur.execute( "insert into course values(1,'生物',1)")
cur.execute( "insert into course values(2,'体育',1)")
cur.execute( "insert into course values(3,'物理',2)")
db.commit()#数据提交
cur.execute( "insert into score values(1,1,1,60)")
cur.execute( "insert into score values(2,1,2,59)")
cur.execute( "insert into score values(3,2,2,100)")
db.commit()#数据提交
# 关闭游标
cur.close()
# 关闭数据库连接
db.close()
```

查看插入数据：

```python
import pymysql  
  
# 连接到数据库  
connection = pymysql.connect(  
    host='localhost',  # 例如：localhost  
    user='root',  # 例如：root  
    password='ycr20030107',  # 输入您的数据库密码  
    db='performanceManagement'  # 指定要连接的数据库名称  
)  
  
try:  
    with connection.cursor() as cursor:  
        # 查询class表数据  
        class_query = "SELECT * FROM class"  
        cursor.execute(class_query)  
        class_result = cursor.fetchall()  
        print("Class table: ")  
        for row in class_result:  
            print(row)  
  
        # 查询student表数据  
        student_query = "SELECT * FROM student"  
        cursor.execute(student_query)  
        student_result = cursor.fetchall()  
        print("Student table: ")  
        for row in student_result:  
            print(row)  
  
        # 查询teacher表数据  
        teacher_query = "SELECT * FROM teacher"  
        cursor.execute(teacher_query)  
        teacher_result = cursor.fetchall()  
        print("Teacher table: ")  
        for row in teacher_result:  
            print(row)  
  
        # 查询course表数据  
        course_query = "SELECT * FROM course"  
        cursor.execute(course_query)  
        course_result = cursor.fetchall()  
        print("Course table: ")  
        for row in course_result:  
            print(row)  
  
        # 查询score表数据  
        score_query = "SELECT * FROM score"  
        cursor.execute(score_query)  
        score_result = cursor.fetchall()  
        print("Score table: ")  
        for row in score_result:  
            print(row)  
finally:  
    connection.close()
```

### 4.数据更新

如下是一个去重更新方法：如果数据存在，则更新数据；如果数据不存在，则插入数据。另外，这种做法支持灵活的字典传值。

```python
data = {
    'id': '20120001',
    'name': 'Bob',
    'age': 21
} 
table = 'students'
keys = ', '.join(data.keys())
values = ', '.join(['%s'] * len(data))
update = ','.join([" {key} = %s".format(key=key) for key in data])
 
sql = f'INSERT INTO {table}({keys}) VALUES ({values}) \
       ON DUPLICATE KEY UPDATE {update}'
try:
    if cursor.execute(sql, tuple(data.values())*2):
        print('Successful')
        db.commit()
except:
    print('Failed')
    db.rollback()
db.close()
```

### 5.数据删除

```python
table = 'students'
condition = 'age > 20'
 
sql = f'DELETE FROM  {table}  WHERE {condition}'
        
try:
    cursor.execute(sql)
    db.commit()
except:
    db.rollback()
 
db.close()
```

### 6.数据查询

```python
sql = 'SELECT * FROM students WHERE age >= 20'
 
try:
    cursor.execute(sql)
    print('Count:', cursor.rowcount)
    one = cursor.fetchone()
    print('One:', one)
    results = cursor.fetchall()
    print('Results:', results)
    print('Results Type:', type(results))
    for row in results:   print(row)
except:
    print('Error')
```

## （五）非关系型数据库存储

NoSQL，全称Not Only SQL，意为不仅仅是SQL，泛指非关系型数据库。NoSQL是基于键值对的，而且不需要经过SQL层的解析，数据之间没有耦合性，性能非常高。

非关系型数据库又可细分如下。
· 键值存储数据库：代表有Redis、Voldemort和Oracle BDB等。
· 列存储数据库：代表有Cassandra、HBase和Riak等。
· 文档型数据库：代表有CouchDB和MongoDB等。
· 图形数据库：代表有Neo4J、InfoGrid和Infinite Graph等

### 1.MongoDB

**·准备工作**

开始之前，请确保已经安装好了MongoDB并启动了其服务，并且安装好了Python的PyMongo库。

**·连接MongoDB**

连接MongoDB时，我们需要使用PyMongo库里面的MongoClient。一般来说，传入MongoDB的IP及端口即可，其中第一个参数为地址host，第二个参数为端口port（如果不给它传递参数，默认是27017）：

```python
import pymongo
client = pymongo.MongoClient(host='localhost', port=27017)
```

**·指定数据库**

MongoDB中可以建立多个数据库，接下来我们需要指定操作哪个数据库。这里我们以test数据库为例来说明在程序中指定要使用的数据库：

```python
# 以下两周方式是等价的
db = client.test
db = client['test']   #创建数据库test
```

·**指定集合**

MongoDB的每个数据库又包含许多集合（collection），它们类似于关系型数据库中的表。与指定数据库类似，指定集合也有两种方式：

```python
collection = db.students
collection = db['students']
```

·**插入数据**

对于students这个集合，以字典形式新建一条数据，并直接调用collection的insert()方法插入数据：

```python
student = {
    'id': '20170101',
    'name': 'Jordan',
    'age': 20,
    'gender': 'male'
}

result = collection.insert(student)
print(result)
```

在MongoDB中，每条数据其实都有一个ObjectId类型的_id属性来唯一标识。insert()方法执行后返回_id值。运行结果如下：5932a68615c2606814c91f3d

·**查询**

插入数据后，我们可以利用find_one()或find()方法进行查询，其中find_one()查询得到的是单个结果，find()则返回一个生成器对象。

```python
result = collection.find_one({'name': 'Mike'})
print(type(result))
print(result)
```

对于多条数据的查询，我们可以使用find()方法。例如，这里查找年龄为20的数据，示例如下：

```python
results = collection.find({'age': 20})
print(results)
for result in results:
    print(result)
```

·**删除**

直接调用remove()方法指定删除的条件即可，符合条件的所有数据均会被删除

```python
result = collection.remove({'name': 'Kevin'})
print(result)
```



### 2.MongoDB中的比较符号

![截屏2023-12-16 15.09.00](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 15.09.00.png)

### 3.Redis存储中的相关操作

Redis是一个开源的基于内存也可持久化的Key-Value数据库。它拥有丰富的数据结构，拥有事务功能，保证命令的原子性。由于是内存数据库，读写非常高速，可达10w/s的评率，所以一般应用于数据变化快、实时通讯、缓存等。但内存数据库通常要考虑机器的内存大小。Redis有16个逻辑数据库（db0-db15），每个逻辑数据库项目是隔离的，默认使用db0数据库。若选择第2个数据库，通过命令 select 2 ，python中连接时可以指定数据库。

![截屏2023-12-16 15.11.20](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 15.11.20.png)

![截屏2023-12-16 15.11.34](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 15.11.34.png)

![截屏2023-12-16 15.11.48](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-16 15.11.48.png)

------

# 六、多线程与多进程编程

## （一）进程与线程

### 1.进程

只有将程序装载到内存中，系统为它分配资源才能运行，而这种**一个程序在一个数据集上的一次动态执行过程就称之为进程。**

- 进程就是程序执行的载体
- 我们打开的每个软件游戏，执行的每一个 python脚本都是启动个进程
- 软件（游戏，脚本）==进程

**每一个进程像人一样需要吃饭，他的粮食就是：cpu和内存**

进程的缺陷在：其只能在一个时间干一件事，如果想同时干两件事或多件事，进程就无能为力了.进程在执行的过程中如果阻塞，例如等待输入，整个进程就会挂起，即使进程中有些工作不依赖于输入的数据，也将无法执行。

### 2.线程

线程是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。**一条线程指的是进程中一个单一顺序的控制流**，一个进程中可以并发多个线程，每条线程并行执行不同的任务



## （二）threading模块

模块的相关方法如下：

![截屏2023-10-24 21.31.02](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-24 21.31.02.png)

## （三）Thread对象

可以通过为Thread类的构造函数传递一个可调用对象来创建线程。

**⚠️注：**创建了线程对象以后，可以调用其start()方法来启动，该方法自动调用该类对象的run方法，此时该线程处于alive状态，直至run方法结束。

如以下代码，通过继承Thread类创建线程类：

```python
import threading
import time

class mythread(threading.Thread):
    def __init__(self, num):
        threading.Thread.__init__(self)
        self.num = num
    def run(self):
        time.sleep(1)                     #线程运行的主要代码
        print('I am {0}'.format(self.num))

def main(): 
    t1 = mythread(1)                       #创建线程
    t2 = mythread(2)
    t3 = mythread(3)
    t1.start()                             #启动线程
    t2.start()
    t3.start()
	    
if __name__ == "__main__":
      main()
```

![截屏2023-10-24 22.07.14](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-24 22.07.14.png)

Thread对象成员:

![截屏2023-10-24 22.13.11](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-24 22.13.11.png)

### 1.Thread对象中的相关方法

**·join([timeout])**：等待被调线程结束后再继续执行后续代码，timeout为最长等待时间，单位为秒。

```python
import threading
import time
def func1(x, y):                    #线程函数
    for i in range(x, y):
        print(i)
        time.sleep(1)

def main(): 
    t1=threading.Thread(target = func1, args = (15, 20))
t1.start()
t1.join(3)
t2=threading.Thread(target = func1, args = (5, 10))
t2.start()

if __name__ == "__main__":
      main()
```

![截屏2023-10-24 22.41.55](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-24 22.41.55.png)

**·isAlive()：**测试线程是否处于运行状态

```python
import threading
import time
def func1(x, y):
    for i in range(x, y):
        print(i)
        time.sleep(1)

def main(): 
	t1=threading.Thread(target = func1, args = (15, 20))
	t1.start()
	t1.join(5)         #注释掉这里试试
	t2=threading.Thread(target = func1, args = (5, 10))
	t2.start()
	t2.join()          #注释掉这里试试
	print(t1.is_alive())
	print(t2.is_alive())

if __name__ == "__main__":
     main()
```

![截屏2023-10-25 07.52.10](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 07.52.10.png)

### 2.Thread对象中的daemon属性

在脚本运行过程中有一个主线程，若在主线程中创建了子线程，则：

·当子线程的daemon属性为**False**时，**主线程结束时会检测子线程是否结束，如果子线程尚未完成，则主线程会等待子线程完成后再退出。**

·当子线程的daemon属性为**True**时，**主线程运行结束时不对子线程进行检查而直接退出，同时子线程将随主线程一起结束，而不论是否运行完成。**

**⚠️注：**以上论述不适用于IDLE中的交互模式或脚本运行模式，因为在交互模式下的主线程只有在退出Python时才终止。

```python
import threading
import time

class mythread(threading.Thread):
    def __init__(self, num, threadname):
        threading.Thread.__init__(self, name = threadname)
        self.num = num
# run方法中，线程会先睡眠self.num秒（通过time.sleep(self.num)实现），然后打印出该线程的数字
    def run(self):        
        time.sleep(self.num)         #阻塞线程self.num秒
        print(self.num)

t1 = mythread(1, 't1')
t2 = mythread(5, 't2')
t2.daemon = True
print(t1.daemon)
print(t2.daemon)

t1.start()
t2.start()
```

![截屏2023-10-25 08.19.23](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 08.19.23.png)

以下代码演示了调用线程对象的普通方法：

```python
import threading  
import time  
  
class myThread(threading.Thread):  
    def __init__(self, threadName):  
        threading.Thread.__init__(self)  
        self.name = threadName          
  
    def run(self):   
        time.sleep(1)  
        print('In run:', self.name)  
  
    def output(self):   
        print('In output:', self.name)

t = myThread('test')
t.start()    #启动线程
t.output()   #调用普通方法
time.sleep(2)
print('OK')
```

![截屏2023-10-25 08.23.52](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 08.23.52.png)

## （四）线程同步技术

属于同一个任务的多个线程之间必然会有交互和同步以便互相协作地完成任务

**⚠️注：**多线程同步时如果需要获得多个锁才能进入临界区的话，可能会发生死锁，在多线程编程时一定要注意并认真检查和避免这种情况。

### 1.Lock/RLock对象

**·Lock:**是比较低级的同步原语，当被锁定以后不属于特定的线程

一个锁有两种状态：locked和unlocked。如果锁处于unclocked状态，acquire()方法将其修改为locked并立即返回；

如果锁已处于locked状态，则阻塞当前线程并等待其他线程释放锁，然后将其修改为locked并立即返回。release()方法将锁状态由locked修改为unlocked并立即返回，如果锁状态本来已经是unlocked，调用该方法将会抛出异常。

**·RLock:**可重入锁RLock对象也是一种常用的线程同步原语，可被同一个线程acquire多次。

当处于locked状态时，某线程拥有该锁；当处于unlocked状态时，该锁不属于任何线程。

如下代码使用Lock/RLock对象实现线程同步：

```python
import threading
import time
# 创建Thread的子类
class myThread(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    def run(self):
        global x # 声明全局变量
        lock.acquire() # 获取锁，进入临界区
        for i in range(3):
            x = x + i
        time.sleep(2)
        print(x)
        lock.release() # 释放锁，推出临界区

lock = threading.Lock() # 创建锁，这里也可使用RLock
x = 0

def main():
    tl= []
    for i in range(10): # 创建10个线程
        t = myThread()
        tl.append(t)
    for i in tl: # 启动10个线程
        i.start()
if __name__ == '__main__':
    main()
```

![截屏2023-10-25 08.42.20](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 08.42.20.png)

### 2.Condition对象

使用Condition对象可以在某些事件触发后才处理数据或执行特定的功能代码，可以用于不同线程之间的通信或通知，以实现更高级别的同步。

Condition对象除了具有acquire()和release()方法之外，还有wait()、notify()、notify_all()等方法。

```python
# 使用Condition对象实现线程同步

# 生产者类
import threading
class Producer(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self,name=threadname)

    def run(self):
        global x
        con.acquire()
        if x == 20:
            con.wait()
       
        print('\nProducer:', end=' ')
        for i in range(20):                
            print(x, end=' ')
            x = x + 1
        print(x)
        con.notify()
        con.release()

# 消费者类
class Consumer(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self, name =threadname)
    def run(self):
        global x
        con.acquire()
        if x == 0:
            con.wait()
        
        print('\nConsumer:', end=' ')
        for i in range(20):                
            print (x, end=' ')
            x = x - 1
        print(x)
        con.notify()
        con.release()
# 创建Condition对象以及生产者与消费者线程
con = threading.Condition()
x = 0

def main():
    p = Producer('Producer')
    c = Consumer('Consumer')
    p.start()  #p和c的启动交换次序，即可以先启动消费者
    c.start()
    p.join()
    c.join()
    print('\nAfter Producer and Consumer all done:',x)
if __name__== "__main__"   :
    main()

```

![截屏2023-10-25 08.49.54](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 08.49.54.png)

### 3.Queue对象

queue模块（在Python 2中为Queue模块）的Queue对象实现了多生产者/多消费者队列，尤其适合需要在多个线程之间进行信息交换的场合，实现了多线程编程所需要的所有锁语义。

在Python中，队列是最常用的线程间的通信方法，因为它是线程安全的，自带锁。而Condition等需要额外加锁的代码操作，在编程对死锁现象要很小心，Queue就不用担心这个问题。

相关方法如下：

**·Queue.put(item, block=True, timeout=None)**：向队列中放入元素

item，放入队列中的数据元素。block，当队列中元素个数达到上限继续往里放数据时：如果 block=False，直接引发 queue.Full 异常；如果 block=True，且 timeout=None，则一直等待直到有数据出队列后可以放入数据；如果 block=True，且 timeout=N，N 为某一正整数时，则等待 N 秒，如果队列中还没有位置放入数据就引发 queue.Full 异常。timeout，设置超时时间。

```python
import queue
try:
    q = queue.Queue(2)  # 设置队列上限为2
    q.put('python')  # 在队列中插入字符串 'python'
    q.put('-') # 在队列中插入字符串 '-'
    q.put('100', block = True, timeout = 5) # 队列已满，继续在队列中插入字符串 '100'，等待5秒后会引发 queue.Full 异常
except queue.Full:
    print('queue.Full')
```

![截屏2023-10-25 09.20.46](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 09.20.46.png)

**·Queue.get(block=True, timeout=None)**：从队列中取出数据并返回该数据内容

block，当队列中没有数据元素继续取数据时：

如果 block=False，直接引发 queue.Empty 异常；

如果 block=True，且 timeout=None，则一直等待直到有数据入队列后可以取出数据；

如果 block=True，且 timeout=N，N 为某一正整数时，则等待 N 秒，如果队列中还没有数据放入的话就引发 queue.Empty 异常。timeout，设置超时时间。

```python
import queue
try:
    q = queue.Queue()
    q.get(block = True, timeout = 5) #  队列为空，往队列中取数据时，等待5秒后会引发 queue.Empty 异常
except queue.Empty:
    print('queue.Empty')
```

**·task_down**

表示队列内的数据元素已经被取出，即每个 get 用于获取一个数据元素， 后续调用 task_done 告诉队列，该数据的处理已经完成。如果被调用的次数多于放入队列中的元素个数，将引发 ValueError 异常。

**·join**

一直阻塞直到队列中的所有数据元素都被取出和执行，只要有元素添加到 queue 中就会增加。当未完成任务的计数等于0，join 就不会阻塞。

```python
import queue
q = queue.Queue()
q.put('python')
q.put('-')
q.put('100')
for i in range(3):   #把3改成2 ，看看执行效果
    print(q.get())
    q.task_done()   #注释该语句，看看执行效果
q.join()
```

如果不执行 task_done，join 会一直处于阻塞状态，等待 task_done 告知它数据的处理已经完成

```python
# e.g. 一个生产者，两个消费者：模拟卖票
from queue import Queue
import threading
import time

queue = Queue(maxsize=0)  

def Producer(name):  
    count=1    
    while True:
        print(f'{name}生产第{count}张票')
        queue.put(count)
        count+=1
        time.sleep(5)
    
def  Consumer(name):
     while True:
         
         print(f'{name}卖出第{queue.get()}张票')
         time.sleep(1)
         queue.task_done()
t1=threading.Thread(target=Producer,args=('总部',))
t2=threading.Thread(target=Consumer,args=('售票1',))  
t3=threading.Thread(target=Consumer,args=('售票2',))    
t1.start()  
t2.start()  
t3.start() 
```

![截屏2023-10-25 09.29.45](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 09.29.45.png)

### 4.Event对象

Event对象包含一个可由线程设置的信号标志,它允许线程等待某些事件的发生。

在初始情况下,Event对象中的信号标志被设置为假。如果有线程等待一个Event对象, 而这个Event对象的标志为假,那么这个线程将会被一直阻塞直至该标志为真。

如果一个线程将一个Event对象的信号标志设置为真,它将唤醒所有等待这个Event对象的线程。如果一个线程等待一个已经被设置为真的Event对象,那么它将忽略这个事件, 继续执行。

![截屏2023-10-25 09.32.25](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 09.32.25.png)

```python
# e.g. 使用Event对象实现线程同步
import threading

class mythread(threading.Thread):
    def __init__(self, threadname):
        threading.Thread.__init__(self, name = threadname)

    def run(self):
        global myevent
        if myevent.isSet():
            myevent.clear()
            myevent.wait()
            print('true',self.getName())
        else:
            print('false',self.getName())
            myevent.set()
myevent = threading.Event()
myevent.set()

tl = []
for i in range(10):
    t = mythread(str(i))
    tl.append(t)

for i in tl:
    i.start()
```

## （五）多进程编程

进程是正在执行中的应用程序。一个进程是一个执行中的文件使用资源的总和，一个应用程序同时打开并执行多次，就会创建多个进程。

### 1.创建进程

```python
# 通过创建Process对象来创建进程，通过start()方法启动。
from multiprocessing import Process
import os

def f(name):
    print('module name:', __name__)
    print('parent process:', os.getppid())   #查看父进程ID
    print('process id:', os.getpid())        #查看当前进程ID
    print('hello', name)

if __name__ == '__main__':
    p = Process(target=f, args=('bob',))     #创建进程
    p.start()                                #启动进程
    p.join()                                 #等待进程结束
```

![截屏2023-10-25 17.09.31](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 17.09.31.png)

```python
# 使用Pool对象进行数据并行处理
from multiprocessing import Pool
from statistics import mean

def f(x):
    return mean(x)

if __name__ == '__main__':
    x = [list(range(10)), list(range(20,30)), list(range(50,60)), list(range(80,90))]
    with Pool(5) as p: # 创建一个包含5个进程的进程池，并将其命名为p。这意味着这段代码将使用5个不同的进程并行执行函数f
        print(p.map(f, x)) # 使用map方法并行应用函数f到列表x中的每个元素。这会返回一个结果列表，其中包含每个子列表的平均值
```

![截屏2023-10-25 17.15.16](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 17.15.16.png)

Pool类可以提供指定数量的进程供用户调用，当有新的请求提交到Pool中时，如果池还没有满，就会创建一个新的进程来执行请求。如果池满，请求就会告知先等待，直到池中有进程结束，才会创建新的进程来执行这些请求。

```python
# 顺序执行与并行执行
import time 
from multiprocessing import Pool
def run(fn):
    time.sleep(1)
    return fn*fn

if __name__ == '__main__':
    testFL = [1,2,3,4,5,6]
    print('串行执行') # 单进程执行方法
    start = time.time()
    for fn in testFL:
        print(run(fn))
    print(time.time() - start)

    print('并行执行') # 创建多个进程，并行执行
    start=time.time()
    pool = Pool(5)  #创建拥有5个进程数量的进程池
    rl =pool.map(run, testFL) # 应用函数run到testFL中的每一个元素
    pool.close()#关闭进程池，不再接受新的进程
    pool.join()#主进程阻塞等待子进程的退出
    print(rl)
    print(time.time()-start) 
```

![截屏2023-10-25 17.28.55](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 17.28.55.png)

# 七、python协程

协程是一种比线程更加轻量级的存在。正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程。协程相当于一个特殊的函数，可以在某个地方挂起，并且可以重新在挂起处继续运行。

协程不被操作系统内核所管理，而完全是由程序所控制(也就是在用户态执行),这样带来的好处就是性能得到了很大的提升，不会像线程切换那样消耗资源。

一个线程内的多个协程是串行执行的，不能利用多核，所以，显然，协程不适合计算密集型的场景。协程适合I/O 阻塞型。

Python的协程有这么三种

1. 由生成器变形来的 `yield/send`
2. Python 3.4版本引入的`@asyncio.coroutine和yield from`
3. Python 3.5版本引入的`async/await`

## （一）迭代器

在Python中，实现了**iter**方法的对象即为可迭代对象，可迭代对象能提供迭代器。实现了**next**方法的对象称为迭代器

因为迭代器也属于可迭代对象，所以说迭代器也要实现**iter**方法，即迭代器需要同时实现**iter**和**next**两个方法。

实例如下：

```python
class Miracle:
    def __init__(self,name,age):
        self.__name = name
        self.__age = age
    def __next__(self):
        self.__age += 1
        if self.__age > 778:
            raise StopIteration
        return self.__age
    def set(self,age):
        self.__age = age
    def __iter__(self):
        return self

m = Miracle("Miracle778",770)
```

上面定义了一个类，实现了`__next__、__iter__`方法，故这个类的对象是迭代器。迭代器可以调用内置方法next(另一种写法是`obj.__next__()`)，进行一次迭代。我们如果用上面的代码的上下文环境执行 `next(m)`语句，会输出`771`。

如果**next**方法被调用，但迭代器没有值可以返回的话，就会引发一个`StopIteration`异常，我们这里手动抛出该异常，主要是为了避免无限迭代。

我们可以知道，使用next()方法，迭代器会返回它的下一个值，那如果我们需要遍历所有迭代器的值，那岂不是要调用n次next()方法了吗？但还好，可以使用for循环进行遍历。

```python
class Miracle:
    def __init__(self,name,age):
        self.__name = name
        self.__age = age
    def __next__(self):
        self.__age += 1
        if self.__age > 778:
            raise StopIteration
        return self.__age
    def set(self,age):
        self.__age = age
        
    def __iter__(self):
        return self

m = Miracle("ye",770)
for i in m:
    print(i)
```

![截屏2023-10-25 09.43.47](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 09.43.47.png)

Python解释器会在第一次迭代时自动调用iter(obj)，之后的迭代会调用迭代器的next方法，for语句会自动处理最后抛出的StopIteration异常。也就是说写成`for i in iter(m)`的执行结果与`for i in m`一样

此外，因为迭代器也属于可迭代对象，于是也能使用内置方法`list()`,将对象转换成列表

```python
m = Miracle("ye",770)
m.set(770)
t = list(m)
print(t)
```

![截屏2023-10-25 12.51.55](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 12.51.55.png)

## （二）生成器

生成器其实是一种特殊的迭代器，不过这种迭代器更加优雅。它不需要再像上面的类一样写`__iter__()和__next__()`方法了，只需要一个yiled关键字。 生成器一定是迭代器（反之不成立）

```python
def fib():
    print("正在执行")
    prev, curr = 0, 1
    while True:
        yield curr # 使用 yield 关键字，这表明这是一个生成器函数。它产生当前斐波那契数列的数字（即 curr），并进入下一个循环迭代。
        prev, curr = curr, curr + prev
```

比如说上面的代码，函数`fib`里面出现了yield关键字，则这个函数即为生成器。函数的返回值是一个生成器对象。当执行f=fib()返回的是一个生成器对象，此时函数体中的代码并不会执行，只有显示或隐示地调用next的时候才会真正执行里面的代码。

```python
f = fib()
next(f)
```

![截屏2023-10-25 15.35.09](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 15.35.09.png)

如上图，执行了next后，函数里面代码才被执行，所以生成器更能节省内存和CPU。

开头说到，生成器也是一种特殊的迭代器，故生成器也可以被迭代，如下图。

![截屏2023-10-25 15.35.45](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 15.35.45.png)

在 for 循环执行时，每次循环都会执行 fib 函数内部的代码，执行到 yield curr 语句时，fib 函数就返回一个迭代值，下次迭代时，代码从 yield curr 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。看起来就好像一个函数在正常执行的过程中被 yield 中断了数次，每次中断都会通过 yield 返回当前的迭代值。

上面我们用到的都是使用`yield`语句抛出迭代值，如:`yield curr`，其实`yield`语句也能用来赋值。

```python
def consume():
    while True:
        i = yield 
        print("消费者",i)
```

这个例子中的`i = yield`语句，我们可以在函数外调用`send`函数对i进行赋值，不过在调用send函数之前，必须先要“激活”该生成器，使其执行到`yield`语句处。
下面我们就来调用一下send函数，运行一下。

```python
def consume():
    print("consume函数被激活")
    while True:
        i = yield 
        print("消费者",i)

consumer = consume() # 生成器
next(consumer)  # 激活生成器，使其运行到yield函数处
for i in range(7):
    consumer.send(i)
```

![截屏2023-10-25 15.46.42](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-25 15.46.42.png)

send函数跟next函数类似，把值传输进去，然后函数运行`yield`语句后面的代码，直至再次遇上`yield`，然后中断，等待下一个send或者next函数的运行。
因此，send函数也能用于生产器激活，不过激活的时候，要send(None)才行，此外send函数的返回值和next一样，都是yield抛出的当前迭代值。

# 八、Python GUI-tkinter

## （一）图形(tkinter)与对象

### 1.tkinter的变量类型

tkinter模块内有四种变量类型，通过对应的构造方法定义变量：
1)tkinter**整型变量** ：  x=IntVar()，        默认是0。
2)tkinter**浮点型变量**: x=DoubleVar()， 默认是0.0。
3)tkinter**字符串变量**：x=StringVar()，   默认是""。
4)tkinter**布尔型变量**：x=BooleanVar()，True是1，False是0。

### 2.tkinter中的常用控件

Button（按钮）, 
Checkbutton（复选按钮）,
Canvas（画布），
Entry（条目,输入框）, 
Frame（框架）,
 Label（标签）, 
LabelFrame（标签框架）, 
Listbox（列表框）， 
Message （消息），
menu（菜单），
Menubutton（菜单按钮）,
OptionMenu（选项菜单）， 
PanedWindow（中分栏窗口）, 
Radiobutton（单选按钮）, 

 Scale（刻度条）,
 Scrollbar（滚动条），
Spinbox（整数调节框），
Text（文本框），
 Combobox（下拉列表框）, 
Notebook（笔记本）, 
Progressbar（进度条）, 
Separator（分离器）, 
Sizegrip（尺寸调节器）,
 Treeview（树视图）。

### 3.编程案例——随机选号系统

(1)设计如下界面，其中有序号为15的袁野同学的图片。

(2)当按下开始按钮时，生成随机整数，作为序号，从excel表中找出对应同学姓名，并获取相关图片，显示在界面上，每间隔0.1秒更新界面。

(3)当按下停按钮时，停止界面更新，播报姓名。

**实验思路：**导入random、threading和thinter和xlrd模块，用于实现题目要求。from defineGUI import App 导入定义的GUI类。class RandomSelect(App): 定义RandomSelect类，该类继承自App类。def **init**(self, parent=None): 初始化函数，设置读取的Excel文件、表单、数据列表、停止标志等，并绑定事件处理函数。def stop1(self,eventobj): 停止函数，当按钮被点击时，设置停止标志为True，然后使用pyttsx3模块播放指定文本的语音，并将音量设置为2.0。def begin(self,eventobj): 开始函数，当按钮被点击时，设置停止标志为False，然后启动一个定时器，每0.1秒执行一次func函数。def func(self): 函数体，当停止标志为False时，生成一个随机数，然后根据该随机数从数据列表中获取一个姓名和编号，更新显示的姓名和编号，然后更新显示的图片，最后启动一个新的定时器，每0.1秒执行一次func函数。rs= RandomSelect() 创建RandomSelect类的实例。rs.run() 启动运行函数。

**实验代码：**

```python
#GUI.py
import tkinter  as tk
class App:
    def __init__(self):
        self.root=tk.Tk()
        self.root.minsize(600,400)
        
        self.beginButton=tk.Button(self.root, font=('Arial', 20),text='开始')
        self.beginButton.grid(row=0, column=1, columnspan=2,padx=10,pady=10) 
        
        self.order = tk.StringVar()
        tk.Entry(self.root,font=('Arial',20),justify=tk.CENTER,
                      textvariable=self.order).grid(row=0, column=3, columnspan=2,padx=10,pady=10)
 
        self.stopButton=tk.Button(self.root, font=('Arial', 20),text='停')
        self.stopButton.grid(row=0, column=5, columnspan=2,padx=10,pady=10) 
        self.image = tk.PhotoImage(file=r"figure1.png")
        self.pictureLabel = tk.Label(self.root,          
            height =250,              
            width = 200,         
            padx=50,                
            pady=70,         
             text = "欢迎使用随机选号系统",
            font=('Arial', 20),
            image=self.image,
            compound = "bottom")
        self.pictureLabel.grid(row=1, column=2, columnspan=4,rowspan=8,padx=10,pady=70)      
        self.name = tk.StringVar()
        self.nameLabel=tk.Label(self.root, font=('Arial', 20), text='name',
                                 textvariable=self.name)
        self.nameLabel.grid(row=9, column=3, columnspan=2,padx=10,pady=20)
    def run(self):
        self.root.mainloop()
```

```python
# Main.py
import random
import threading
import tkinter  as tk
import xlrd
import pyttsx3
from defineGUI import  App
class RandomSelect(App):
    def __init__(self, parent=None):
        super().__init__()
        self.readbook = xlrd.open_workbook(r"/Users/yangchaoran/Desktop/curriculum/2023-2024 Autumn/高级程序设计与系统开发工具/信管2101.xlsx")
        self.sheet = self.readbook.sheet_by_name('sheet1')
        self.order_list = [self.sheet.cell(i,0).value for i in range(1,30)]
        self.name_list = [self.sheet.cell(i,2).value for i in range(1,30)]
        self.stop=False        
        self.stopButton.bind('<Button-1>',self.stop1)
        self.beginButton.bind('<Button-1>',self.begin) 
 
    def stop1(self,eventobj):
         self.stop=True        
         engine = pyttsx3.init()              
         text=self.name.get()
         engine.say(text)
         volume = engine.getProperty('volume')
         engine.setProperty(volume, 2.0)
         engine.runAndWait()   

    def begin(self,eventobj): 
        self.stop=False
        timer = threading.Timer(0.1, self.func)
        timer.start()
    
    def func(self):        
        if not self.stop:            
            num = random.randint(1, len(self.name_list) )
            name = self.name_list[num%len(self.name_list)] 
            self.name.set(name)    
            self.order.set(num) 
            filename='figure'+str(num)+'.png'
            self.image = tk.PhotoImage(file=filename)
            self.pictureLabel.configure(image=self.image)            
            timer = threading.Timer(0.1, self.func)
            timer.start()      
        
rs= RandomSelect()
rs.run()
```

![截屏2023-10-27 08.02.58](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-27 08.02.58.png)

------

## （二）MVC设计模式

MVC，全称Model（模型）-View（视图）-Controller（控制器），这是一种开发模式，他的好处是可以将界面和业务逻辑分离。

​     Model（模型），是程序的主体部分，主要包含业务数据和业务逻辑。

​     View(视图），是程序呈现给用户的部分，是用户和程序交互的接口，用户会根据具体的业务需求，在View视图层输入自己特定的业务数据，并通过界面的事件交互，将对应的输入参数提交给后台控制器进行处理。

​     Controller（控制器）控制器中接收了用户与界面交互时传递过来的数据，并根据数据业务逻辑来执行服务的调用和更新业务模型的数据和状态。

------

# 九、Python网站设计

## （一）相关知识

Django是开放源代码的**Web应用框架**，由Python语言编写，

### 1.Web框架

web框架： 别人已经设定好的一个web网站模板，你学习它的规则，然后“填空”或“修改”成你自己需要的样子。一般web框架的架构是这样的

![截屏2023-11-13 17.28.08](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-13 17.28.08.png)

**HTTP**:**超文本传输协议**，它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。

**WSGI**：**服务器网关接口**，是一个 Python Web 应用程序与 Web 服务器之间的**接口规范**。它定义了应用程序和服务器之间的标准接口，使得应用程序可以在不同的 Web 服务器上运行。

**ORM**：**对象关系映射**，是一种程序技术，用于实现面向对象编程语言里**不同类型系统的数据之间的转换**。从效果上说，它其实是创建了一个可在编程语言里使用的“**虚拟对象数据库**”。

**Static**:指的是**静态文件**。这些文件通常是不需要服务器进行特殊处理或解释的文件，比如JavaScript文件、CSS文件、图片文件等。

**HTML**:HTML文件是用于**呈现网页内容**的一种**标记语言**。HTML文件通常包含一系列标签，用于定义网页中的不同元素和内容，例如标题、段落、图像、链接等。

**CSS**:CSS文件是用于**描述和定义网页样式的文件**，在Django中用于管理和引用静态文件。通过将CSS文件放置在静态文件夹中，并在HTML模板中引用它们，Django可以方便地管理和呈现动态网页样式。

JS:JS文件是**用于实现网页交互和动态效果**的JavaScript代码文件

### 2.MVC与MTC

![截屏2023-11-13 19.28.31](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-13 19.28.31.png)

## （二）Django建站基础

### 1.Django安装与项目创建

```bash
# 终端安装
pip install django -i https://pypi.tuna.tsinghua.edu.cn/simple
```

创建web-project文件夹，并在终端输入如下命令创建名为blog的Django项目

```bash
cd /Users/yangchaoran/Desktop/web-project
django-admin startproject blog
```

![截屏2023-11-13 19.53.26](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-13 19.53.26.png)

![截屏2023-11-13 19.53.46](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-13 19.53.46.png)

### 2.新建一个应用（app）

在blog项目下创建应用learn，命令如下：

```bash
python manage.py startapp learn
```

![截屏2023-11-15 08.06.22](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-15 08.06.22.png)

migrations：用于数据库属的迁移

__ init __.py：初始化文件

admin.py：当前app的后台管理系统

apps.py：当前app的配置信息，**一般情况下无须修改**

models.py：定义映射类关联数据库，实现数据持久化，即MTV里面的模型（Model）

tests.py：自动化测试的模块

views.py：逻辑处理模块，即MTV里面的视图（Views）

⚠️注：learn应用的层次，在项目目录blog下，与blog包在同一目录

![截屏2023-11-15 08.09.41](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-15 08.09.41.png)

把新定义的app加到settings.py中的INSTALL_APPS中

(修改blog/blog/settings.py,在INSTALLED_APPS中加入learn）

![截屏2023-11-15 08.12.28](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-15 08.12.28.png)

### 3.启动项目

启动服务器时，请注意 manage.py文件所在的位置

```bash
python   manage.py  runserver  8001
```

访问http://127.0.0.1:8001

![截屏2023-11-15 08.18.58](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-11-15 08.18.58.png)

⚠️注：在后面的操作中，**保持网站处于运行状态**，发现错误，及时修改！所有修改要存盘才有效

------

## （三）Django数据库操作

使用django进行数据库开发的步骤如下：

一、 配置数据库连接信息
二、在models.py中定义模型类
三、迁移
四、通过类和对象完成数据增删改查操作

·**配置**

在settings.py中保存了数据库的连接配置信息，Django默认初始配置使用sqlite数据库。

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

```python
# 配置数据库
DATABASES = {
# 默认的数据库，没有选择其他数据库时，就使用默认的，不使用时，可以配置一个空字典
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        "HOST": "xxxx",
        "PORT": "xxx",
        'USER': 'postgres_user',
        'PASSWORD': 's3krit',
        'NAME': 'app_data',
    },
    # users应用连接的数据库
    'users': {
        'ENGINE': 'django.db.backends.mysql',
        "HOST": "xxx",
        "PORT": 3306,
        'USER': 'mysql_user',
        'PASSWORD': 'priv4te',
        'NAME': 'users_data',
    }
}
```

·**定义模型类**

模型类被定义在   “应用/models.py” 文件中。模型类必须继承位于包django.db.models中的Model类

·**主键**

django会为表创建自动增长的主键列，每个模型只能有一个主键列，如果使用选项设置某属性为主键列后django不会再创建自动增长的主键列。默认创建的主键列属性为id，可以使用pk代替，pk全拼为primary key。自定义主键id = models.IntegerField(primary_key = True)

·**属性命名限制**

不能是python的保留关键字。
不允许使用连续的下划线，这是由django的查询方式决定的。
定义属性时需要指定字段类型，通过字段类型的参数指定选项

·**迁移**

即将模型类同步到数据库中，相关代码如下：

```python
# 生成迁移文件
python manage.py makemigrations

# 同步到数据库中
python manage.py migrate
```

·**增删改查操作**

[Reference link](https://blog.csdn.net/m0_65883616/article/details/125736469)

------

## （四）注册、登陆网页设计

在templates目录下创建regist.html文件：

```html
<html > 
 <head>    
      <title>regist</title> 
 </head>  
<body>
  <h1>regist</h1>  
<form method="post" enctype="multipart/form-data" >  {% csrf_token %} 
	{{uf.as_p}}  
	 <input type="submit" value="Regist"/>
 </form> 
 </body>  
</html>
```

修改learn/models.py文件,加入如下代码：

```python
class Account(models.Model):
    '''生成一个Account类'''
    username=models.CharField('用户名',max_length=30)
    password=models.CharField('密码',max_length=30)
    email=models.EmailField('电子邮箱',blank=True)
    desc=models.TextField('描述',max_length=500,blank=True)
```

同步一下数据库（我们使用默认的数据库 SQLite3，无需配置）

```bash
(mydjango) D:\web-project\blog>python manage.py makemigrations
(mydjango) D:\web-project\blog>python manage.py migrate
```

在learn下的views.py中加入：

```python
from django.http import HttpResponse
from django import forms

from learn.models  import Account

class UserForm(forms.Form):
    username = forms.CharField(label='用户名')
    password = forms.CharField(label='密   码',
                               widget=forms.PasswordInput())
def regist(request):
    if request.method == 'POST':
        uf = UserForm(request.POST)
        if uf.is_valid():
            username = uf.cleaned_data['username']
            password = uf.cleaned_data['password']
            ##判断用户原密码是否匹配
            user =Account.objects.filter(username = username)
            if user:
                info = '用户名已存在!'
            elif len(user) == 0:
                info = '注册成功!'	
                user =Account()
                user.username = username
                user.password = password
                user.save()
            return HttpResponse(info)
    else:
        uf = UserForm()
    return render(request,'regist.html', {'uf': uf})
```

修改blog目录下的urls.py：

```python
from django.contrib import admin
from django.urls import path
from django.conf.urls import include, url
from learn import views as learn_views  # new

urlpatterns = [
    path('', learn_views.index),  # new
    #url(r'^home$', learn_views.home, name='home'),
    path('home', learn_views.home, name='home'),
    path('add/', learn_views.add, name='add'),  # new
    path('add2/<int:a>/<int:b>/', learn_views.add1, name='add1'),
    path('list1/',  learn_views.list1),   
    path('testIMG/',  learn_views.testIMG),
    path('admin/', admin.site.urls),
    path('regist/', learn_views.regist),
  
    
]
```

![截屏2023-12-17 20.08.45](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-17 20.08.45.png)

![截屏2023-12-17 20.08.55](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-17 20.08.55.png)

# 十、Python爬虫基础

## （一）HTTP基本原理

网站是指在因特网上根据一定规则，使用HTML等工具制作并用于展示特定内容的相关网页的集合。网站是互联网上 ，展示特定内容的，  相互链接的网页集合。

### 1.URI和URL

URI的全称为Uniform Resource Identifier，即统一资源标志符，URL的全称为Universal Resource Locator，即统一资源定位符。       举例来说，https://github.com/favicon.ico是GitHub的网站图标链接，它是一个URL，也是一个URI。URL是URI的子集。URI还包括一个子类叫作URN，它的全称为Universal Resource Name，即统一资源名称。在目前的互联网中，URN用得非常少，所以几乎所有的URI都是URL，一般的网页链接我们既可以称为URL，也可以称为URI

### 2.超文本HTML

其英文名称叫作hypertext，我们在浏览器里看到的网页就是超文本解析而成的，其网页源代码是一系列HTML代码，里面包含了一系列标签，比如img显示图片，p指定显示段落等。浏览器解析这些标签后，便形成了我们平常看到的网页，而网页的HTML源代码就可以称作超文本。在Chrome浏览器里面打开任意一个页面，如淘宝首页，右击任一地方并选择“检查”项（或者直接按快捷键F12），即可打开浏览器的开发者工具，这时在Elements选项卡即可看到当前网页的源代码，这些源代码都是超文本其他浏览器如何查看当前网页的源代码？

### 3.http和https

在淘宝的首页https://www.taobao.com/中，URL的开头会有http或https，这就是访问资源需要的协议类型。有时，我们还会看到ftp、sftp、smb开头的URL，它们都是协议类型。在爬虫中，我们抓取的页面通常就是http或https协议的。HTTP的全称是Hyper Text Transfer Protocol，中文名叫作超文本传输协议。HTTP协议是用于从网络传输超文本数据到本地浏览器的传送协议，它能保证高效而准确地传送超文本文档。HTTPS的全称是Hyper Text Transfer Protocol over Secure Socket Layer，是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，简称为HTTPS。

我们在浏览器中输入一个URL，回车之后便会在浏览器中观察到页面内容。实际上，这个过程是浏览器向网站所在的服务器发送了一个请求，网站服务器接收到这个请求后进行处理和解析，然后返回对应的响应，接着传回给浏览器。响应里包含了页面的源代码等内容，浏览器再对其进行解析，便将网页呈现了出来，模型如图所示：

![截屏2023-12-18 08.13.45](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-18 08.13.45.png)

### 4.请求

请求，由客户端向服务端发出，可以分为4部分内容：请求方法（Request Method）、请求的网址（Request URL）、请求头（Request Headers）、请求体（Request Body）。

·**请求方法**

常见的请求方法有两种：GET和POST。在浏览器中直接输入URL并回车，这便发起了一个GET请求，请求的参数会直接包含到URL里。例如，在百度中搜索Python，这就是一个GET请求，链接为https://www.baidu.com/s?wd=Python，其中URL中包含了请求的参数信息，这里参数wd表示要搜寻的关键字。POST请求大多在表单提交时发起。比如，对于一个登录表单，输入用户名和密码后，点击“登录”按钮，这通常会发起一个POST请求，其数据通常以表单的形式传输，而不会体现在URL中。

·**请求的地址**

请求的网址，即统一资源定位符URL，它可以唯一确定我们想请求的资源。

·**请求头**

请求头，用来说明服务器要使用的附加信息，比较重要的信息有Cookie、Referrer、User-Agent等。下面简要说明一些常用的头信息。· Accept：请求报头域，用于指定客户端可接受哪些类型的信息 (传输文件类型)。· Accept-Language：指定客户端可接受的语言类型。· Accept-Encoding：指定客户端可接受的内容编码。· Host：用于指定请求资源的主机IP和端口号，其内容为请求URL的原始服务器或网关的位置。从HTTP 1.1版本开始，请求必须包含此内容。

· Cookie：也常用复数形式 Cookies，这是网站为了辨别用户进行会话跟踪而存储在用户本地的数据。它的主要功能是维持当前访问会话。例如，我们输入用户名和密码成功登录某个网站后，服务器会用会话保存登录状态信息，后面我们每次刷新或请求该站点的其他页面时，会发现都是登录状态，这就是Cookies的功劳。Cookies里有信息标识了我们所对应的服务器的会话，每次浏览器在请求该站点的页面时，都会在请求头中加上Cookies并将其发送给服务器，服务器通过Cookies识别出是我们，并且查出当前状态是登录状态，所以返回结果就是登录之后才能看到的网页内容。· Referrer：此内容用来标识这个请求是从哪个页面发过来的，服务器可以拿到这一信息并做相应的处理，如作来源统计、防盗链处理等。（从广告页面到商品购买页面）

· User-Agent：简称UA，它是一个特殊的字符串，可以使服务器识别客户使用的操作系统及版本、浏览器及版本等信息。在做爬虫时加上此信息，可以伪装为浏览器；如果不加，很可能会被识别出为爬虫。· Content-Type：也叫互联网媒体类型（Internet Media Type）或者MIME类型，在HTTP协议消息头中，它用来表示具体请求中的媒体类型信息。例如，text/html代表HTML格式，image/gif代表GIF图片，application/json代表JSON类型，更多对应关系可以查看此对照表：http://tool.oschina.net/commons。因此，请求头是请求的重要组成部分，在写爬虫时，大部分情况下都需要设定请求头

·**请求体**

请求体一般承载的内容是POST请求中的表单数据，而对于GET请求，请求体则为空。

### 5.响应

响应，由服务端返回给客户端，可以分为三部分：响应状态码（Response Status Code）、响应头（Response Headers）和响应体（Response Body）。

·**响应状态码**

响应状态码表示服务器的响应状态，如200代表服务器正常响应，404代表页面未找到，500代表服务器内部发生错误。在爬虫中，我们可以根据状态码来判断服务器响应状态，如状态码为200，则证明成功返回数据，再进行进一步的处理，否则直接忽略

·**响应头**

响应头包含了服务器对请求的应答信息，如Content-Type、Server、Set-Cookie等。下面简要说明一些常用的头信息。· Date：标识响应产生的时间。· Last-Modified：指定资源的最后修改时间。· Content-Encoding：指定响应内容的编码。· Server：包含服务器的信息，比如名称、版本号等。· Content-Type：文档类型，指定返回的数据类型是什么，如text/html代表返回HTML文档，application/x-javascript则代表返回JavaScript文件，image/jpeg则代表返回图片。· Set-Cookie：设置Cookies。响应头中的Set-Cookie告诉浏览器需要将此内容放在Cookies中，下次请求携带Cookies请求。· Expires：指定响应的过期时间，可以使代理服务器或浏览器将加载的内容更新到缓存中。如果再次访问时，就可以直接从缓存中加载，降低服务器负载，缩短加载时间。

·**响应体**

最重要的当属响应体的内容了。响应的正文数据都在响应体中，比如请求网页时，它的响应体就是网页的HTML代码；请求一张图片时，它的响应体就是图片的二进制数据。我们做爬虫请求网页后，要解析的内容就是响应体。在浏览器开发者工具中点击Preview，就可以看到网页的源代码，也就是响应体的内容，它是解析的目标。在做爬虫时，我们主要通过响应体得到网页的源代码、JSON数据等，然后从中做相应内容的提取。

------

## （二）网页基础

### 1.网页组成

网页可以分为三大部分——HTML、CSS和JavaScript。如果把网页比作一个人的话，HTML相当于骨架，JavaScript相当于肌肉，CSS相当于皮肤，三者结合起来才能形成一个完善的网页。

### 2.HTML

在Chrome浏览器中打开百度，右击并选择“检查”项，打开开发者模式，在Elements选项卡中即可看到网页的源代码：![截屏2023-12-18 08.38.18](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-18 08.38.18.png)

这就是HTML，整个网页就是由各种标签嵌套组合而成的。这些标签定义的节点元素相互嵌套和组合形成了复杂的层次关系，就形成了网页的架构。

### 3.css

HTML定义了网页的结构，但是只有HTML页面的布局并不美观，可能只是简单的节点元素的排列，为了让网页看起来更好看一些，这里借助了CSS。CSS，全称叫作Cascading Style Sheets，即层叠样式表。“层叠”是指当在HTML中引用了数个样式文件，并且样式发生冲突时，浏览器能依据层叠顺序处理。“样式”指网页中文字大小、颜色、元素间距、排列等格式。CSS是目前唯一的网页页面排版样式标准，有了它的帮助，页面才会变得更为美观。

### 3.Javascript

JavaScript，简称JS，是一种脚本语言。HTML和CSS配合使用，提供给用户的只是一种静态信息，缺乏交互性。我们在网页里可能会看到一些交互和动画效果，如下载进度条、提示框等，这通常就是JavaScript的功劳。它的出现使得用户与信息之间不只是一种浏览与显示的关系，而是实现了一种实时、动态、交互的页面功能。JavaScript通常也是以单独的文件形式加载的，后缀为js，在HTML中通过script标签即可引入，例如：<script src="jquery-2.1.0.js"></script>综上所述，HTML定义了网页的内容和结构，CSS描述了网页的布局，JavaScript定义了网页的行为

------

## （三）爬虫基本原理

我们可以把互联网比作一张大网，而爬虫（即网络爬虫）便是在网上爬行的蜘蛛。把网的节点比作一个个网页，爬虫爬到一个节点就相当于访问了该页面，获取了其信息。可以把节点间的连线比作网页与网页之间的链接关系，这样蜘蛛通过一个节点后，可以顺着节点连线继续爬行到达下一个节点，即通过一个网页继续获取后续的网页，这样整个网的节点便可以被蜘蛛全部爬行到，网站的数据就可以被抓取下来了

### 1.爬虫概述

·**获取网页**

​    爬虫首先要做的工作就是获取网页，这里就是获取网页的源代码。源代码里包含了网页的有用信息，所以只要把源代码获取下来，才可以从中提取想要的信息了。   前面讲了请求和响应的概念，向网站的服务器发送一个请求，返回的响应体便是网页源代码。所以，最关键的部分就是构造一个请求并发送给服务器，然后接收到响应并将其解析出来，那么这个流程怎样实现呢？总不能手工去截取网页源码吧？   不用担心，Python提供了许多库来帮助我们实现这个操作，如urllib、requests等。我们可以用这些库来帮助我们实现HTTP请求操作，请求和响应都可以用类库提供的数据结构来表示，得到响应之后只需要解析数据结构中的Body部分即可。

·**提取信息**

获取网页源代码后，接下来就是分析网页源代码，从中提取我们想要的数据。    首先，最通用的方法便是采用正则表达式提取，这是一个万能的方法，但是在构造正则表达式时比较复杂且容易出错。    另外，由于网页的结构有一定的规则，所以还有一些根据网页节点属性、CSS选择器或XPath来提取网页信息的库，如Beautiful Soup、pyquery、lxml等，可以高效快速地提取网页信息，如节点的属性、文本值等。    提取信息是爬虫非常重要的部分，它可以使杂乱的数据变得条理清晰，以便我们后续处理和分析数据。

·**保存数据**

 提取信息后，我们一般会将提取到的数据保存到某处以便后续使用。这里保存形式有多种多样，如可以简单保存为TXT文本或JSON文本，也可以保存到数据库，如MySQL和MongoDB等，也可保存至远程服务器，如借助SFTP进行操作等。

### 2.如何抓取数据

在网页中我们能看到各种各样的信息，最常见的便是常规网页，它们对应着HTML代码，而最常抓取的便是HTML源代码。另外，可能有些网页返回的不是HTML代码，而是一个JSON字符串（其中API接口大多采用这样的形式），这种格式的数据方便传输和解析，它们同样可以抓取，而且数据提取更加方便。此外，我们还可以看到各种二进制数据，如图片、视频和音频等。利用爬虫，我们可以将这些二进制数据抓取下来，然后保存成对应的文件名。另外，还可以看到各种扩展名的文件，如CSS、JavaScript和配置文件等，这些其实也是最普通的文件，只要在浏览器里面可以访问到，就可以将其抓取下来。上述内容其实都对应各自的URL，是基于HTTP或HTTPS协议的，只要是这种数据，爬虫都可以抓取。

### 3.JavaScript渲染页面

有时候，我们在用urllib或requests抓取网页时，得到的源代码实际和浏览器中看到的不一样。这是一个非常常见的问题。现在网页越来越多地采用Ajax、前端模块化工具来构建，整个网页可能都是由JavaScript渲染出来的，也就是说原始的HTML代码就是一个空壳，例如：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>This is a Demo</title>
    </head>
    <body>
        <div id="container">
        </div>
    </body>
    <script src="app.js"></script>
</html>
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>This is a Demo</title>
    </head>
    <body>
        <div id="container">
        </div>
    </body>
    <script src="app.js"></script>
</html>
```

body节点里面只有一个id为container的节点，但是需要注意在body节点后引入了app.js，它便负责整个网站的渲染。在浏览器中打开这个页面时，首先会加载这个HTML内容，接着浏览器会发现其中引入了一个app.js文件，然后便会接着去请求这个文件，获取到该文件后，便会执行其中的JavaScript代码，而JavaScript则会改变HTML中的节点，向其添加内容，最后得到完整的页面。

但是在用urllib或requests等库请求当前页面时，我们得到的只是这个HTML代码，它不会帮助我们去继续加载这个JavaScript文件，这样也就看不到浏览器中的内容了。这也解释了为什么有时我们得到的源代码和浏览器中看到的不一样。因此，使用基本HTTP请求库得到的源代码可能跟浏览器中的页面源代码不太一样。对于这样的情况，我们可以分析其后台Ajax接口，也可使用Selenium、Splash这样的库来实现模拟JavaScript渲染。

------

## （四）基本库的使用

学习爬虫，最初的操作便是模拟浏览器向服务器发出请求。Python提供了功能齐全的类库来帮助我们完成这些请求。最基础的HTTP库有**urllib、requests**等。

### 1.urllib

·**概述**

urllib是Python内置的HTTP请求库，包含如下4模块：
1)request 是最基本的 HTTP 请求模块，用来模拟发送请求，就像在浏览器里输入网址然后回车一样，只需要给库方法传入URL及额外的参数，就可以模拟实现这个过程了。

2)error 是异常处理模块。如果出现请求错误，error可以捕获这些异常，然后进行重试，以保证程序不会意外终止。

3)parse 是一个工具模块，提供了许多 URL 的处理方法，比如拆分、解析、合并等。

4)robotparser 主要用来识别网站的robots.txt文件，然后判断哪些网页可以爬，哪些网页不可以爬，它其实用得较少。

使用urllib .request模块，利用urlopen()方法，可以通过简单GET 请求来抓取网页，用type()方法输出响应的类型

**·异常处理**

urllib的error模块定义了由request模块产生的异常。如果出现了问题，request模块便会抛出error模块中定义的异常。

URLError类来自urllib库的error模块，它继承自OSError类，error是异常模块的基类，由request模块生的异常都可以通过捕获这个类来处理。

```python
from urllib import request, error
try: 
       response = request. urlopen(' https://cuiqingcai.com/index.htm')    
except error. URLError as e: 
       print(e.reason)
```

·**HTTPError**

它是URLError的子类，专门用来处理HTTP请求错误，比如认证请求失败等。它有如下3个属性：

code： 返回 HTTP 状态码，比如 404 表示网页不存在， 500 表示服务器内部错误等。

reason ：同父类一样，用于返回错误的原因

headers： 返回请求头

```python
# 因为URLError是HTTPError的父类，所以可以先选择捕获子类的错误，再去捕获父类的错误
from urllib import request, error 
try: 
    response = request.urlopen(' https://cuiqingcai.com/index.htm') 
except error.HTTPError as e: 
    print(e.reason, e.code, e.headers, sep='\ n') 
except error.URLError as e: 
    print(e .reason) 
else: 
    print('Request Successfully')
```

·**Robots协议**

Robots协议也称作爬虫协议、机器人协议，它的全名叫作网络爬虫排除标准（Robots Exclusion Protocol），用来告诉爬虫和搜索引擎哪些页面可以抓取，哪些不可以抓取。它通常是一个叫作**robots.txt**的文本文件，一般放在网站的根目录下。当搜索爬虫访问一个站点时，它首先会检查这个站点**根目录**下是否存在robots.txt文件，如果存在，搜索爬虫会根据其中定义的爬取范围来爬取。如果没有找到这个文件，搜索爬虫便会访问所有可直接访问的页面，示例如下：

```txt
# section 2
User-agent: *
Crawl-delay: 5
Disallow: /trap
```

无论哪种用户代理，都应该在两次下载请求之间有5秒的时延；/trap链接是禁止链接，如果访问了这个链接，服务器就会封禁你的IP一分钟或者永久封禁

### ✨2.request

Requests 是用Python语言编写，基于 urllib，比 urllib 更加方便。

urllib库中的 urlopen()方法实际上是以 GET方式请求网页，而 requests中相应的方法就是 get()方法 

·**get抓取网页**

```python
import requests
import re
 
headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
}

r = requests.get("https://www.zhihu.com/explore", headers=headers)

pattern = re.compile('class="css-1nd7dqm".*?>(.*?)</a>', re.S)
titles = re.findall(pattern, r.text)
print(titles)
```

这里我们加入了headers信息，其中包含了User-Agent字段信息，也就是浏览器标识信息。如果不加这个，知乎会禁止抓取。

·**get抓取二进制数据**

图片、音频、视频这些文件本质上都是由二进制码组成的，由于有特定的保存格式和对应的解析方式，我们才可以看到这些形形色色的多媒体。所以，想要抓取它们，就要拿到它们的二进制码。

```python
# 抓取GitHub的站点图标
import requests
 
r = requests.get("https://github.com/favicon.ico")
with open('favicon.ico', 'wb') as f:
    f.write(r.content)
```

·**POST请求**

前面我们了解了最基本的GET请求，另外一种比较常见的请求方式是POST。使用requests实现POST请求同样非常简单，示例如下：

```python
import requests
 
data = {'name': 'germey', 'age': '22'}
r = requests.post("http://httpbin.org/post", data=data)
print(r.text)
```

发送请求后，得到的自然就是响应。在上面的实例中，我们使用text和content获取了响应的内容。此外，还有很多属性和方法可以用来获取其他信息，比如状态码、响应头、Cookies等。示例如下：

```python
import requests
 
r = requests.get('http://www.jianshu.com')
print(type(r.status_code), r.status_code)
print(type(r.headers), r.headers)
print(type(r.cookies), r.cookies)
print(type(r.url), r.url)
print(type(r.history), r.history)
```

·**设置超时时间**

通过timeout属性设置超时时间，一旦超过这个时间还没获得响应内容，就会提示错误。

```python
import requests 
response =requests.get (‘ https://www.python.org',timeout=0.01)
```

·**通用代码框架**

有效处理爬虫过程中遇到的错误或者网络不稳定导致的问题

```python
 def getHTMLText(url):
 try:
  r = requests.get(url,timeout = 3)
  r.raise_for_status()     #如果状态不是200，引发HTTPError异常
  r.encoding = r.apparent_encoding  #保证返回的解码是正确的
  return r.text
 except:
       //可以根据异常代码进行进一步处理
        return("产生异常")
```

------

## （五）解析库的使用

提取页面信息时使用的是正则表达式，这还是比较烦琐，而且万一有地方写错了，可能导致匹配失败，因此需要使用一些解析库来解决这种问题

### 1.XPath

XPath，全称XML Path Language，是一门在XML文档中查找信息的语言。

几乎所有我们想要定位的节点，都可以用非常简洁明了的路径选择表达式来选择，所以XPath选择功能十分强大。另外，XPath还提供了超过100个内建函数，用于字符串、数值、时间的匹配以及节点、序列的处理等。

·**选取所有节点**

用双斜杠//开头的XPath规则来选取所有符合要求的节点。例如：

```python
from lxml import etree
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//*')
print(result)
```

·**查找元素的子节点**

```python
from lxml import etree
 
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//li/a')
print(result)
```

⚠️注意：斜杠/和双斜杠//有区别，其中斜杠/用于获取直接子节点，双斜杠//用于获取子孙节点

·**父节点**

通过连续的/或//可以查找子节点或子孙节点，那么假如我们知道了子节点，怎样来查找父节点呢？这可以用两个小原点..来实现。比如，现在首先选中href属性为link4.html的a节点，然后再获取其父节点，然后再获取其class属性

```python
# 获取href属性为link4.html的a节点的父节点的class属性
from lxml import etree
 
html = etree.parse('./test.html', etree.HTMLParser())
result = html.xpath('//a[@href="link4.html"]/../@class')
print(result)
```

### 2.Beautiful Soup

Beautiful Soup是Python的一个HTML或XML的解析库，用它可以方便地从网页中提取数据。

·**初始化**

由于lxml解析器有解析HTML和XML的功能，而且速度快，容错能力强，所以推荐使用它。使用lxml，在初始化Beautiful Soup时，要把第二个参数改为lxml即可：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup('<p>Hello</p>', 'lxml')
print(soup.p.string)
```

·**基本用法**

```html
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
```

这里声明了变量html，它是一个HTML字符串。但不是一个完整的HTML字符串，因为body和html节点都没有闭合。如何从HTML中获取字符串“The Dormouse's story”？

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.prettify())
print(soup.title.string)
```

将html当作第一个参数传给BeautifulSoup对象，该对象的第二个参数为解析器的类型（这里使用lxml），此时就完成了BeaufulSoup对象的初始化。在初始化BeautifulSoup时，对于不标准的HTML字符串，可以自动更正格式。然后，将这个对象赋值给soup变量。接下来，就可以调用soup的方法和属性来解析HTML了。首先，调用prettify()方法，把要解析的字符串以标准的缩进格式输出。然后调用soup.title.string，输出HTML中title节点的文本内容。soup.title选出HTML中的title节点，string属性得到里面的文本（The Dormouse's story），这样通过简单调用几个属性就完成了文本的提取。

具体详细用法见[beautifulsoup](https://blog.csdn.net/REfusing/article/details/102991087)

------

# 十一、Ajax动态渲染数据爬取

 有时候我们在用requests爬取页面的时候，得到的结果可能和在浏览器中看到的不一样。这是因为requests获取的都是原始的HTML文档，而浏览器中的页面则是经过JavaScript处理后生成的结果，这些数据的来源可能是通过Ajax加载的。       

Ajax加载是一种异步加载方式。原始页面加载完后，（浏览器会自动）再向服务器请求某个接口获取数据，然后（浏览器）将获取的数据处理后呈现到网页上，这其实就是发送了一个Ajax请求。从Web发展的趋势来看，这种形式的页面越来越多。如果遇到这样的页面，需要分析网页后台发送的Ajax请求。如果可以用requests来模拟Ajax请求，那么就可以成功抓取了。

## （一）什么是Ajax

Ajax，全称为Asynchronous JavaScript and XML，即异步的JavaScript和XML。它不是一门编程语言，而是利用JavaScript在保证页面不被刷新、页面链接不改变的情况下与服务器交换数据并更新部分网页的技术。对于传统的网页，如果想更新其内容，那么必须要刷新整个页面(更新整个页面就要换网页地址)，但有了Ajax，便可以在页面不被全部刷新(不要换网页地址)的情况下更新其内容。在这个过程中，页面实际上是在后台与服务器进行了数据交互，获取到数据之后，再利用JavaScript改变网页，这样网页内容就会更新了。

[示例链接](https://www.w3school.com.cn/tiy/t.asp?f=eg_js_ajax_onreadystatechange)

------

## （二）Ajax的基本原理

初步了解了Ajax之后，我们再来详细了解它的基本原理。发送Ajax请求到网页更新的这个过程可以简单分为以下3步：(1) 发送请求； (2) 解析内容； (3) 渲染网页

一个实例：

浏览网页的时候，我们会发现很多网页都可以下滑 查看更多的选项。比如，以https://m.weibo.cn/u/2830678474为例，切换到微博页面，一直下滑。

⚠️注意：页面没有整体刷新，也就意味着页面的链接没有变化，但是网页中却多了新内容。这就是通过Ajax获取新数据并呈现的过程。

------

## （三）Ajax分析方法

### 1.查看请求

首先，用Chrome浏览器打开微博的链接https://m.weibo.cn/u/2830678474，随后在页面中点击鼠标右键，从弹出的快捷菜单中选择“检查”选项，此时便会弹出开发者工具，或者使用下面的操作：

![截屏2023-12-19 11.16.43](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.16.43.png)

切换到Network选项卡，随后重新刷新页面，可以发现这里出现了非常多的条目：

![截屏2023-12-19 11.18.38](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.18.38.png)

Ajax其实有其特殊的请求类型，它叫作xhr。我们可以发现一个名称以getIndex开头的请求，其Type为xhr，这就是一个Ajax请求。用鼠标点击这个请求，可以查看这个请求的详细信息。

![截屏2023-12-19 11.19.02](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.19.02.png)

在右侧可以观察到其Request Headers、URL和Response Headers等信息。其中Request Headers中有一个信息为X-Requested-With:XMLHttpRequest，这就标记了此请求是Ajax请求

随后点击一下Preview，即可看到响应的内容，它是JSON格式的。这里Chrome为我们自动做了解析，点击箭头即可展开和收起相应内容。

![截屏2023-12-19 11.19.30](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.19.30.png)

观察可以发现，这里的返回昵称、简介、头像等，这也是用来渲染个人主页所使用的数据。JavaScript接收到这些数据之后，再执行相应的渲染方法，整个页面就渲染出来了。

### 2.过滤请求

接下来，再利用Chrome开发者工具的筛选功能筛选出所有的Ajax请求。在请求的上方有一层筛选栏，直接点击XHR，此时在下方显示的所有请求便都是Ajax请求了

![截屏2023-12-19 11.20.36](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.20.36.png)

### 3.Ajax结果提取

打开Ajax的XHR过滤器，然后一直滑动页面以加载新的微博内容。可以看到，会不断有Ajax请求发出。选定其中一个请求，分析它的参数信息。点击该请求，进入详情页面

![截屏2023-12-19 11.21.20](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.21.20.png)

随后，观察这个请求的响应内容。

![截屏2023-12-19 11.21.50](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-19 11.21.50.png)

可以发现，这个元素有一个比较重要的字段mblog。展开它，可以发现它包含的正是微博的一些信息，比如attitudes_count（赞数目）、comments_count（评论数目）、reposts_count（转发数目）、created_at（发布时间）、text（微博正文）等，而且它们都是一些格式化的内容。

这样我们请求一个接口，就可以得到10条微博，而且请求时只需要改变page参数即可。这样的话，我们只需要简单做一个循环，就可以获取所有微博了。

# 十二、动态渲染页面爬取

## （一）Selenium的使用

Selenium是一个自动化测试工具，利用它可以驱动浏览器执行特定的动作，如点击、下拉等操作，同时还可以获取浏览器当前呈现的页面的源代码，做到可见即可爬。对于一些JavaScript动态渲染的页面来说，此种抓取方式非常有效。

Selenium库的基本使用主要包括声明浏览器对象、访问界面、查找节点和节点交互等步骤。

### 1.准备工作

在使用Selenium之前，我们需要安装相应的库和下载浏览器驱动：

```bash
pip install selenium
```

Selenium通过浏览器驱动来控制浏览器，因此需要下载对应浏览器的驱动。例如，对于Chrome浏览器，需要下载[ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/)。

### 2.声明浏览器对象

在使用Selenium控制浏览器之前，首先需要声明一个浏览器对象。以Chrome浏览器为例，声明浏览器对象的代码如下：

```python
from selenium import webdriver  
  
# 声明浏览器对象，指定驱动路径  
browser = webdriver.Chrome(executable_path='path/to/chromedriver')
```

### 3.访问界面

声明浏览器对象后，我们可以使用`get`方法来访问指定的网址。例如，访问百度首页的代码如下：

```python
# 访问百度首页  
browser.get('https://www.baidu.com')
```

### 4.查找节点

在访问网页后，我们需要查找和操作网页中的节点。Selenium提供了多种查找节点的方法，如`find_element_by_id`、`find_element_by_name`、`find_element_by_xpath`等。例如，查找百度首页的搜索框并输入关键词的代码如下：

```python
# 查找搜索框节点并输入关键词  
input_box = browser.find_element_by_id('kw')  
input_box.send_keys('Selenium')
```

### 5.节点交互

查找到节点后，我们可以对节点进行各种交互操作，如点击按钮、提交表单等。例如，查找并点击百度首页的搜索按钮的代码如下：

```python
# 查找搜索按钮节点并点击  
button = browser.find_element_by_id('su')  
button.click()
```

## （二）Selenium运用——自动登录微博

```python
from selenium import webdriver  
from time import sleep  
    
browser = webdriver.Chrome(executable_path='path/to/chromedriver')  
  
browser.get('https://weibo.com/login')  
sleep(1)
    
username_box = browser.find_element_by_xpath('//*[@id="loginName"]')  
password_box = browser.find_element_by_xpath('//*[@id="loginPassword"]')  
username_box.send_keys('username')  
password_box.send_keys('password')  
sleep(1)   
    
login_button = browser.find_element_by_xpath('//*[@id="loginAction"]')  
login_button.click()
```

------

# 十三、代理的原理与使用

 在做爬虫的过程中经常会遇到这样的情况，最初爬虫正常运行，正常抓取数据，一切看起来都是那么美好，然而一杯茶的功夫可能就会出现错误，比如403 Forbidden，这时候打开网页一看，可能会看到“您的IP访问频率太高”这样的提示。出现这种现象的原因是网站采取了一些反爬虫措施。比如，服务器会检测某个IP在单位时间内的请求次数，如果超过了这个阈值，就会直接拒绝服务，返回一些错误信息，这种情况可以称为封IP。既然服务器检测的是某个IP单位时间的请求次数，那么借助某种方式来伪装我们的IP，让服务器识别不出是由我们本机发起的请求，不就可以成功防止封IP了吗？一种有效的方式就是使用代理，后面会详细说明代理的用法。在这之前，需要先了解下代理的基本原理，它是怎样实现IP伪装的呢？

## （一）代理的基本原理

代理实际上指的就是代理服务器，英文叫作**proxy server**，它的功能是代理网络用户去取得网络信息。形象地说，它是网络信息的中转站。在我们正常请求一个网站时，是发送了请求给Web服务器，Web服务器把响应传回给我们。

如果设置了代理服务器，实际上就是在本机和服务器之间搭建了一个桥，此时本机不是直接向Web服务器发起请求，而是向代理服务器发出请求，请求会发送给代理服务器，然后由代理服务器再发送给Web服务器，接着由代理服务器再把Web服务器返回的响应转发给本机。这样我们同样可以正常访问网页，但这个过程中Web服务器识别出的真实IP就不再是我们本机的IP了，就成功实现了IP伪装，这就是代理的基本原理。

![截屏2023-12-18 08.45.58](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-18 08.45.58.png)

## （二）代理的优缺点

那么，代理有什么作用呢？我们可以简单列举如下。
· 突破自身IP访问限制，访问一些平时不能访问的站点。
· 访问一些单位或团体内部资源：比如使用教育网内地址段免费代理服务器，就可以用于对教育网开放的各类FTP下载上传，以及各类资料查询共享等服务。
· 提高访问速度：通常代理服务器都设置一个较大的硬盘缓冲区，当有外界的信息通过时，同时也将其保存到缓冲区中，当其他用户再访问相同的信息时，则直接由缓冲区中取出信息，传给用户，以提高访问速度。
· 隐藏真实IP：上网者也可以通过这种方法隐藏自己的IP，免受攻击。对于爬虫来说，我们用代理就是为了隐藏自身IP，防止自身的IP被封锁。

·保护了真实对象的访问，可以对访问进行限制和控制；

·可以提高访问效率，通过代理对象可以缓存数据或者调用其他服务等；

·可以提高系统的灵活性，因为代理对象可以在不影响真实对象的情况下扩展其功能。

当然，代理还有一些缺点，比如：

·可能引入额外的复杂性，因为需要创建代理对象。

·如果代理对象没有正确实现与真实对象相同的接口，可能会导致客户端代码无法正常工作。

## （三）爬虫代理

​    对于爬虫来说，由于爬虫爬取速度过快，在爬取过程中可能遇到同一个IP访问过于频繁的问题，此时网站就会让我们输入验证码登录或者直接封锁IP，这样会给爬取带来极大的不便。    使用代理隐藏真实的IP，让服务器误以为是代理服务器在请求自己。这样在爬取过程中通过不断更换代理，就不会被封锁，可以达到很好的爬取效果。

## （四）代理的运用场景

一般来说，代理的运用场景有如下几种：

身份验证：在访问某些敏感数据或操作时，可以使用代理对象执行身份验证，以确保用户有权访问该数据或执行该操作。
缓存：使用代理对象来缓存数据，以便在下一次访问时可以更快地访问数据。
远程服务：代理对象可以用作远程服务的本地代表，在使用远程服务时提供更好的用户体验，同时也可以提高访问效率。
代理模式可以通过组合或继承实现。通常，代理对象会继承与真实对象相同的接口，并在继承的方法中调用真实对象的方法。

## （五）代理模式的实际使用

具体的设置和某部分的讲解可见黄万艮老师的PPT，此部分主要是我在课下学习中，对运用代理模式实现实际功能的总结

### 1.实现缓存功能

```python
 
from abc import ABC, abstractmethod
 
class Image():
    @abstractmethod
    def display(self):
        pass
 
class RealImage(Image):
    def __init__(self, filename): 
        self.filename = filename  
        self.load_from_disk()     

    def load_from_disk(self): #
        print("loading " + self.filename)
    def display(self):
        print("Displaying " + self.filename)
 
class ImageProxy(Image):
    def __init__(self, filename):
        self.filename = filename
        self.real_image = None
    def display(self):
        if self.real_image is None:
            self.real_image = RealImage(self.filename)
        self.real_image.display()
 
image = ImageProxy("test.jpg")
print("第一次调用display方法：图片没有加载真实图片，调用子类RealImage， 创建真实图片对象")
image.display()
print("第二次调用display方法: 直接从缓存中获取图片")
image.display()
```

代码定义了一个抽象基类Image，其中包含一个display()抽象方法。接着，定义了一个具体子类RealImage，它代表了真实的图片对象，负责从磁盘中加载图片数据，并提供display()方法用于显示图片。

此外，代码还定义了一个代理类ImageProxy，它也实现了Image接口。当客户端调用display()方法时，代理类会检查是否已经加载了真实的图片对象。如果没有，代理会先创建一个真实的图片对象，然后再调用它的display()方法。这样，代理对象就可以缓存真实对象，避免了重复加载图片资源，从而提高程序性能。

使用代理模式时，客户端代码只需要与代理对象交互，并不需要知道真实对象的存在。当代理对象需要访问真实对象时，它会自动创建并调用真实对象，从而实现对真实对象的间接访问。

### 2.实现身份验证

```python
class AbstractSubject():
    def request(self):
        pass
 
class RealSubject(AbstractSubject):
    def request(self):
        print("RealSubject: 处理请求...")
 
# 定义代理类, 继承抽象主题类
class Proxy(AbstractSubject):
    def __init__(self, realsubject: RealSubject, username: str, password: str):
        self._subject = realsubject
        self._username = username
        self._password = password
 
    def request(self):
        if self.authenticdate():
            self._subject.request()
        else:
            print("代理认证失败")
 
    def authenticdate(self):
        # 模拟身份认证过程
        if self._username == "admin" and self._password == "admin123456":
            print("代理认证成功")
            return True
        else:
            return False
 
# 创建真实主题对象
real_subject = RealSubject()
 
# 创建代理对象
proxy = Proxy(real_subject, "admin", "admin123456")
# 通过代理对象发送请求
proxy.request()
 
# 修改代理认证信息，再次发送请求
proxy._username = "a"
proxy._password = "b"
proxy.request()

```

在上述示例代码中，我们定义了一个抽象主题类AbstractSubject和其子类RealSubject，并在代理类Proxy中持有一个真实主题对象_subject。在客户端调用时，通过代理对象proxy发送请求。

在代理类中，我们重写了request()方法，在其中添加了身份验证的逻辑。如果身份验证成功，则代理对象将请求转发给真实主题对象；否则，代理对象将输出代理认证失败的消息。

代理类Proxy的构造函数包含三个参数：

**realsubject**参数是真实主题对象，使用类型提示指定为RealSubject类。
**username**参数是用于身份认证的用户名，使用类型提示指定为字符串类型。
**password**参数是用于身份认证的密码，使用类型提示指定为字符串类型。
在代理类的构造函数中，将输入的真实主题对象、用户名和密码保存到对应的成员变量中，以便后续使用。

在客户端调用过程中，我们创建了一个真实主题对象real_subject和代理对象proxy，并通过代理对象发送请求。我们可以通过修改代理认证信息，测试代理类的身份验证功能是否正常工作。

### 3.实现远程服务

```python
from abc import ABC, abstractmethod
 
# 定义抽象类：远程服务接口
class RemoteService():
    @abstractmethod
    def request(self, param: str) -> str: 
        pass
 
class RemoteServiceIml(RemoteService):
    def request(self, param: str) -> str:
        return f"Remote service received {param}"
 
class RemoteServiceProxy(RemoteService):
    def __init__(self, remote_service: RemoteService):
        self._remote_service = remote_service
 
    def request(self, param: str) -> str:
        print("Remote service aouthentication...")
        if not param:
            raise ValueError("Missing parameter")
        # 调用远程服务，返回结果
        return self._remote_service.request(param)
 
remote_service_impl = RemoteServiceIml()
proxy = RemoteServiceProxy(remote_service_impl)
 
res = proxy.request("test")
print(res)
```

在上述代码中，我们首先定义了一个远程服务接口RemoteService，其中有一个request方法，用于处理远程服务请求，返回一个字符串类型的结果。同时，我们还实现了RemoteServiceImpl类，该类用于实现具体的远程服务逻辑。在具体的request方法中，简单地将接收到的参数拼接成一条消息，并返回。

接下来，我们使用代理模式实现了RemoteServiceProxy类，该类也实现了RemoteService接口，并接收一个RemoteService实例作为构造函数参数。当我们调用request方法时，代理类先进行身份认证、参数校验等本地操作，然后再调用远程服务，最后将结果返回。

在使用示例中，我们先创建了RemoteServiceImpl的实例，然后将它传入代理类的构造函数中创建代理对象proxy。最后，我们调用代理对象的request方法，传入一个字符串参数，并将返回结果打印出来。这个例子展示了代理模式如何在应对远程服务时起到的作用。

def request(self, param: str) -> str:解释：
这段代码是一个抽象方法的定义，其中request是一个接受一个字符串类型的参数，返回一个字符串类型的结果的抽象方法，但是该方法没有具体实现，只是提供了一个方法头。这种方法被称为抽象方法，是一种在抽象类中定义，需要在子类中具体实现的方法。在Python中，我们可以通过定义抽象类和使用abstractmethod装饰器来实现抽象方法的定义。子类必须实现抽象类中的所有抽象方法才能被实例化，并且子类中的抽象方法也可以再次定义为抽象方法，以继续在更深层次上实现多态行为。

raise ValueError("Missing parameter")解释：
这段代码是一个异常抛出语句，当代码执行到此处时，会抛出一个ValueError类型的异常，并传递一个字符串参数"Missing parameter"作为异常信息。异常机制是Python中的一种错误处理机制，当程序出现错误时，可以使用异常机制提前中止程序的运行，并在异常发生的地方引发一个异常对象，这样就可以将错误信息传递给上层调用的代码，以便于更好地处理错误。在Python中，可以使用raise语句抛出一个异常对象，从而使得程序进入异常处理流程。

# 十四、大语言模型运用

随着人工智能技术的飞速发展，大语言模型在多个领域展现出了强大的应用潜力。我有幸在这个学期的高级程序设计与系统开发工具课程中，跟着黄万艮老师接触并学习使用了文心一言大模型。通过一系列的实践应用和案例分析，我深切感受到了大模型对于编程和系统开发领域的巨大帮助，因此虽然不是课上学习的知识，但也在此专门做一个总结，写一点自己的心得体会。

## （一）大语言模型在课程上对我的帮助

就以本学期的高级程序设计与系统开发工具课程为例，大语言模型对我的帮助主要体现在如下方面：

·**代码自动生成与补全**：文心一言具备强大的自然语言理解能力，能够快速将用户需求转化为准确的代码逻辑。在课程中的编程实践环节，当我遇到复杂的算法或数据结构实现时，通过文心一言，我能够输入自然语言描述的需求，模型会为我生成相应的代码片段或提供逻辑清晰的算法思路，大大提高了编程效率。

·**错误诊断与调试辅助**：在程序调试过程中，有时错误难以定位，自己找的话十分的浪费时间，有时还不一定能找到自己的代码错在哪。利用文心一言对代码上下文的分析能力，它能够为我指出可能的错误源头并给出修复建议。比如，在处理一次内存泄漏问题时，模型通过分析代码逻辑，成功定位了问题所在，节省了我大量的排查时间。

·**系统设计辅助与优化**：在系统开发工具课程中，我们需要设计复杂的软件架构和系统流程。文心一言能够根据我提供的初步设计思路，自动为我优化架构设计，提出性能改进的建议，以及预测潜在的风险点。这对于缺乏经验的大学生来说，无疑是一项强大的辅助工具。

------

## （二）使用体会&如何正确使用大语言模型

其实我也是这学期在黄万艮老师第一堂课上才初次了解到大语言模型这个东西，仿佛是打开了一道新世界的大门。

整个学期下来，我最直观的感受就是大语言模型让我的学习效率高了很多，比如给它一份代码它能帮我找出其中的错误，有什么问题也可以通过大模型得到答案，甚至有的时候我碰到不会写的代码，也可以问它获得完整的代码和详细的解释。总而言之就是为我提供了一种快速解决问题的手段。

但经过这一个学期的使用，我也在不断地思考如何正确使用大语言模型，因为实事求是的说，给你一个能快速完成作业的机器，对于一个大学生而言真是一种极大的诱惑。可能你可以用大语言模型完成大部分的作业，但这终究不是自己的思考，且长期滥用大模型会导致自己的学习惰性猛增，最终无论遇到什么问题都只想着找机器要答案，而自己本身却没学到东西。这想必与自己使用大模型的初衷背道而驰了。

所以，我觉得为了让语言大模型于自身学习发挥积极的作用，我们应该要做到如下几点：

**·先思考，而不是先要答案**：这也就是我先前所说的不要一碰到问题就去找大模型要答案，而是要先思考。可以想不出来，但不能完全不想，这是使用大语言模型的首要原则。

·**提问时要注意技巧**：承接上一点，在对大语言模型进行提问时也需要注意一些技巧，比如要在提问之前明确自己的需求，且可以多就自己的思考对大语言进行提问，比如“针对这个问题，xxx的思路是否正确”、“使用xxx方法或者如上代码是否体现了xxx特性”或者“上述代码在xxx方面是否有优化空间”等。即将其看作一个老师来提出自己的疑问。

·**在提问中学习其归纳信息和观点的思路**：大语言模型通常会根据你的问题分点分模块进行回答，只要提问得当，其生成的内容大多是框架调理清晰的，我们可以从大语言模型的回答中学习其信息归纳和分点的思路，为我们之后写论文提供参考。

综上，不可否认大语言模型是一个十分强大的工具，但同时对于大学生也是一把双刃剑，只有合理正确的利用它，我们才能从中有所收获。

------

# 十五、Scrapy框架的使用

## （一）Scrapy框架介绍

Scrapy 是用 Python 实现的一个为了爬取网站数据、
提取结构性数据而编写的应用框架。

Scrapy 常应用在包括数据挖掘，信息处理或存储历史数据等一系列的程序中。

可以很简单的通过 Scrapy 框架实现一个爬虫，抓取指定网站的内容或图片。

[reference link](http://www.runoob.com/w3cnote/scrapy-detail.html)

### 1.Scrapy架构图

如下是scrapy的框架图，其中绿色箭头是数据流向

![截屏2023-12-26 22.38.20](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-26 22.38.20.png)

框架各部分的解释如下：

- **Scrapy Engine(引擎)**: 负责Spider、ItemPipeline、Downloader、Scheduler中间的通讯，信号、数据传递等。
- **Scheduler(调度器)**: 它负责接受引擎发送过来的Request请求，并按照一定的方式进行整理排列，入队，当引擎需要时，交还给引擎。
- **Downloader（下载器）**：负责下载Scrapy Engine(引擎)发送的所有Requests请求，并将其获取到的Responses交还给Scrapy Engine(引擎)，由引擎交给Spider来处理，
- **Spider（爬虫）**：它负责处理所有Responses,从中分析提取数据，获取Item字段需要的数据，并将需要跟进的URL提交给引擎，再次进入Scheduler(调度器).
- **Item Pipeline(管道)**：它负责处理Spider中获取到的Item，并进行进行后期处理（详细分析、过滤、存储等）的地方。
- **Downloader Middlewares（下载中间件）**：你可以当作是一个可以自定义扩展下载功能的组件。
- **Spider Middlewares（Spider中间件）**：你可以理解为是一个可以自定扩展和操作引擎和Spider中间通信的功能组件（比如进入Spider的Responses;和从Spider出去的Requests

### 2.Scrapy的运作流程

1.引擎(Scrapy Engine)询问Spiders要处理哪一个网站

2.Spiders把需要处理的URL提交给引擎(Scrapy Engine)

3.引擎把request请求传递给调度器，调度器对请求排队

4.调度器把处理好的request请求提交给引擎

5.引擎要求下载器下载这个request请求

6.下载器把下载的东西传给引擎

7.引擎把下载的东西传给Spider，由 parse()进行解析

8.Spider把需要跟进的URL和获取到的Item数据提交给引擎

9.引擎把需要跟进的URL交给调度器

10.引擎把需要存储的Item数据交给管道存储。

### 3.制作Scrapy 爬虫的四步骤

1. 新建项目 (scrapy startproject xxx)：新建一个新的爬虫项目
2. 明确目标 （编写items.py）：明确你想要抓取的目标
3. 制作爬虫 （spiders/xxspider.py）：制作爬虫开始爬取网页
4. 存储内容 （pipelines.py）：设计管道存储爬取内容

通过上述框架和处理流程，我们不难发现，Scrapy通过多个组件的相互协作、不同组件完成工作的不同、组件对异步处理的支持， 使Scrapy最大限度地利用了网络带宽，大大提高了数据爬取和处理的效率。

## （二）Scrapy入门

### 1.基于Mac OS的安装

由于笔者的系统为MacOS，因此只在此总结该环境下的安装步骤，其他环境的安装可见[reference link](http://www.runoob.com/w3cnote/scrapy-detail.html)

对于Mac OS系统来说，由于系统本身会引用自带的python2.x的库，因此默认安装的包是不能被删除的，但是你用python2.x来安装Scrapy会报错，用python3.x来安装也是报错，我最终没有找到直接安装Scrapy的方法，所以我用另一种安装方式来说一下安装步骤，解决的方式是就是使用virtualenv来安装。

```bash
$ sudo pip install virtualenv
$ virtualenv scrapyenv
$ cd scrapyenv
$ source bin/activate
$ pip install Scrapy
```

安装后，只要在命令终端输入 scrapy，提示类似以下结果，代表已经安装成功。

![截屏2023-12-26 22.58.01](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-26 22.58.01.png)

### 2.入门案例

#### 学习目标

- 创建一个Scrapy项目
- 定义提取的结构化数据(Item)
- 编写爬取网站的 Spider 并提取出结构化数据(Item)
- 编写 Item Pipelines 来存储提取到的Item(即结构化数据)

#### 新建项目(scrapy startproject)

在开始爬取之前，必须创建一个新的Scrapy项目。进入自定义的项目目录中，运行下列命令：

```bash
scrapy startproject mySpider
```

其中， mySpider 为项目名称，可以看到将会创建一个 mySpider 文件夹，目录结构大致如下：

下面来简单介绍一下各个主要文件的作用：

```bash
mySpider/
    scrapy.cfg
    mySpider/
        __init__.py
        items.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
            ...
```

这些文件分别是:

- scrapy.cfg: 项目的配置文件。
- mySpider/: 项目的Python模块，将会从这里引用代码。
- mySpider/items.py: 项目的目标文件。
- mySpider/pipelines.py: 项目的管道文件。
- mySpider/settings.py: 项目的设置文件。
- mySpider/spiders/: 存储爬虫代码目录。

#### 实验目标(mySpider/items.py)

我们打算抓取 **http://www.itcast.cn/channel/teacher.shtml** 网站里的所有讲师的姓名、职称和个人信息。

1. 打开 mySpider 目录下的 items.py。

2. Item 定义结构化数据字段，用来保存爬取到的数据，有点像 Python 中的 dict，但是提供了一些额外的保护减少错误。

3. 可以通过创建一个 scrapy.Item 类， 并且定义类型为 scrapy.Field 的类属性来定义一个 Item（可以理解成类似于 ORM 的映射关系）。

   接下来，创建一个 ItcastItem 类，和构建 item 模型（model）。

```py
import scrapy

class ItcastItem(scrapy.Item):
   name = scrapy.Field()
   title = scrapy.Field()
   info = scrapy.Field()
```

#### 制作爬虫 （spiders/itcastSpider.py）

爬虫功能要分两步：

**1. 爬数据**

在当前目录下输入命令，将在mySpider/spider目录下创建一个名为itcast的爬虫，并指定爬取域的范围：

```py
scrapy genspider itcast "itcast.cn"
```

打开 mySpider/spider目录里的 itcast.py，默认增加了下列代码:

```py
import scrapy

class ItcastSpider(scrapy.Spider):
    name = "itcast"
    allowed_domains = ["itcast.cn"]
    start_urls = (
        'http://www.itcast.cn/',
    )

    def parse(self, response):
        pass
```

**2. 取数据**

爬取整个网页完毕，接下来的就是的取过程了，首先观察页面源码：

```html
<div class="li_txt">
    <h3>  xxx  </h3>
    <h4> xxxxx </h4>
    <p> xxxxxxxx </p>
```

是不是一目了然？直接上 XPath 开始提取数据吧。

xpath 方法，我们只需要输入的 xpath 规则就可以定位到相应 html 标签节点，详细内容可以查看 [xpath 教程](https://www.runoob.com/xpath/xpath-tutorial.html)。

不会 xpath 语法没关系，Chrome 给我们提供了一键获取 xpath 地址的方法（**右键->检查->copy->copy xpath**）,如下图:

![截屏2023-12-27 07.50.32](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-27 07.50.32.png)

举例我们读取网站 **http://www.itcast.cn/** 的网站标题，修改 itcast.py 文件代码如下：

```py
# -*- coding: utf-8 -*-
import scrapy

# 以下三行是在 Python2.x版本中解决乱码问题，Python3.x 版本的可以去掉
import sys
reload(sys)
sys.setdefaultencoding("utf-8")

class Opp2Spider(scrapy.Spider):
    name = 'itcast'
    allowed_domains = ['itcast.com']
    start_urls = ['http://www.itcast.cn/']

    def parse(self, response):
        # 获取网站标题
        context = response.xpath('/html/head/title/text()')   
       
        # 提取网站标题
        title = context.extract_first()  
        print(title) 
        pass
```

执行以下命令：

```bash
$ scrapy crawl itcast
...
...
传智播客官网-好口碑IT培训机构,一样的教育,不一样的品质
...
...
```

我们之前在 mySpider/items.py 里定义了一个 ItcastItem 类。 这里引入进来:

```py
from mySpider.items import ItcastItem
```

然后将我们得到的数据封装到一个 ItcastItem 对象中，可以保存每个老师的属性：

```py
from mySpider.items import ItcastItem

def parse(self, response):
    #open("teacher.html","wb").write(response.body).close()

    # 存放老师信息的集合
    items = []

    for each in response.xpath("//div[@class='li_txt']"):
        # 将我们得到的数据封装到一个 `ItcastItem` 对象
        item = ItcastItem()
        #extract()方法返回的都是unicode字符串
        name = each.xpath("h3/text()").extract()
        title = each.xpath("h4/text()").extract()
        info = each.xpath("p/text()").extract()

        #xpath返回的是包含一个元素的列表
        item['name'] = name[0]
        item['title'] = title[0]
        item['info'] = info[0]

        items.append(item)

    # 直接返回最后数据
    return items
```

我们暂时先不处理管道，后面会详细介绍。

#### 保存数据

scrapy保存信息的最简单的方法主要有四种，-o 输出指定格式的文件，命令如下：

```bash
scrapy crawl itcast -o teachers.json
```

json lines格式，默认为Unicode编码

```bash
scrapy crawl itcast -o teachers.jsonl
```

csv 逗号表达式，可用Excel打开

```bash
scrapy crawl itcast -o teachers.csv
```

xml格式

```bash
scrapy crawl itcast -o teachers.xml
```

## （三）Spider的用法

由于在 Scrapy 中，**要抓取网站的链接配置、抓取逻辑、解析逻辑，其实都是在 Spider 中配置的**。在前一节实例中，我们发现抓取逻辑也是在 Spider 中完成的。本节我们就来专门了解一下 Spider 的基本用法。

### 1.Spider 运行流程

在实现 Scrapy 爬虫项目时，最核心的类便是 Spider 类了，它定义了如何爬取某个网站的流程和解析方式。简单来讲，Spider 要做的事就是如下两件。

A.定义爬取网站的动作

B.分析爬取下来的网页

对于 Spider 类来说，整个爬取循环如下所述。

A.以初始的 URL 初始化 Request，并设置回调函数。 

B.当该 Request 成功请求并返回时，生成 Response，并作为参数传给该回调函数。

C.回调函数分析Response的网页内容，返回结果可以有两种形式，
一种是字典或 Item 对象，下一步可经过处理后（或直接）保存；
另一种是下一个链接，利用此链接构造 Request 并设置新的回调函数，返回 2)。


通过以上几步循环往复进行，便完成了站点的爬取。

### 2.Spider 类分析

在上一节的例子中我们定义的 Spider 是继承自 scrapy.spiders.Spider，这个类是最简单最基本的 Spider 类，每个其他的 Spider 必须继承这个类，还有后文要说明的一些特殊 Spider 类也都是继承自它。

这个类里提供了 start_requests() 方法的默认实现，读取并请求 start_urls 属性，并根据返回的结果调用 parse() 方法解析结果。另外它还有一些基础属性：

**name：**爬虫名称，是定义 Spider 名字的字符串。Spider 的名字定义了 Scrapy 如何定位并初始化 Spider，所以其必须是唯一的。 不过我们可以生成多个相同的 Spider 实例，这没有任何限制。 name 是 Spider 最重要的属性，而且是必须的。如果该 Spider 爬取单个网站，一个常见的做法是以该网站的域名名称来命名 Spider。 例如，如果 Spider 爬取 mywebsite.com ，该 Spider 通常会被命名为 mywebsite 。

**allowed_domains：**允许爬取的域名，是可选配置，不在此范围的链接不会被跟进爬取。

**start_urls：**起始 URL 列表，当我们没有实现 start_requests() 方法时，默认会从这个列表开始抓取。

**custom_settings**：这是一个字典，是专属于本 Spider 的配置，此设置会覆盖项目全局的设置，而且此设置必须在初始化前被更新，所以它必须定义成类变量。

**crawler**，此属性是由 from_crawler() 方法设置的，代表的是本 Spider 类对应的 Crawler 对象，Crawler 对象中包含了很多项目组件，利用它我们可以获取项目的一些配置信息，如最常见的就是获取项目的设置信息，即 Settings。

**settings**，是一个 Settings 对象，利用它我们可以直接获取项目的全局设置变量。

## （四）Downloader Middleware的用法

![截屏2023-12-27 08.05.31](/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-12-27 08.05.31.png)

Scrapy 内置的 Downloader Middleware 为 Scrapy 提供了基础的功能，但在项目实战中我们往往需要单独定义 Downloader Middleware。每个 Downloader Middleware 都定义了一个或多个方法的类，核心的方法有如下三个：

**process_request(request, spider)**
**process_response(request, response, spider)**
**process_exception(request, exception, spider)**

我们只需要实现至少一个方法，就可以定义一个 Downloader Middleware。下面我们来看看这三个方法的详细用法。

## （五）Spider Middleware的用法

Spider Middleware 是介入到 Scrapy 的 Spider 处理机制的钩子框架（框架中红色的那一部分）

Spider Middleware 有如下三个作用。

我们可以在 Downloader 生成的 Response 发送给 Spider 之前，也就是在 Response 发送给 Spider 之前对 Response 进行处理。

我们可以在 Spider 生成的 Request 发送给 Scheduler 之前，也就是在 Request 发送给 Scheduler 之前对 Request 进行处理。

我们可以在 Spider 生成的 Item 发送给 Item Pipeline 之前，也就是在 Item 发送给 Item Pipeline 之前对 Item 进行处理。

Scrapy 内置的 Spider Middleware 为 Scrapy 提供了基础的功能。如果我们想要扩展其功能，只需要实现某几个方法即可。

每个 Spider Middleware 都定义了以下一个或多个方法的类，核心方法有如下 4 个。

**process_spider_input(response, spider)**
**process_spider_output(response, result, spider)**
**process_spider_exception(response, exception, spider)**
**process_start_requests(start_requests, spider)**

## （六）Item Pipeline的用法

Item Pipeline 是项目管道，上述结构的最左侧即为 Item Pipeline，它的调用发生在 Spider 产生 Item 之后。当 Spider 解析完 Response 之后，Item 就会传递到 Item Pipeline，被定义的 Item Pipeline 组件会顺次调用，完成一连串的处理过程，比如数据清洗、存储等。
它的主要功能有：

清洗 HTML 数据
验证爬取数据，检查爬取字段
查重并丢弃重复内容
将爬取结果储存到数据库

我们可以自定义 Item Pipeline，只需要实现指定的方法就好，其中必须要实现的一个方法是：

process_item(item, spider)

另外还有几个比较实用的方法，它们分别是：

open_spider(spider)
close_spider(spider)
from_crawler(cls, crawler)

# 十六、结语、总结与致谢

前面的内容其实算是知识点的总结，我按照黄万艮老师上课的顺序，以章节的方式对课上所讲授的基本知识点、课件上的实例和课后自己学习碰到的实例进行了总结。毕竟是平常上课就在进行积累总结，到期末整理这个课程总结时才发现已经洋洋洒洒敲了四万多字了，期间也没少碰到困难和挫折，因此在写这个结语的时候还是感慨万千的。

在这周的课上（这部分和代理使用的学习总结是第十五周写的）老师给了我们一个课程框架，在很大程度上帮助我们梳理了这学期所学的内容。

大体来说，整个学期的高级程序设计与系统开发工具的学习分为三个部分，即**语言基础**、**工具学习**和**爬虫应用**。在语言学习的部分，我们在本次课程中使用的编程语言仍旧是接触了很多次的python，但在一开始我们仍旧花了很多时间去系统学习python知识，从最基本的面向对象编程思想、python项目结构和编程风格、编程规范，到较为进阶的正则表达式运用、数据存储与多进程、多线程编程，我们近乎用了六七周的时间来学习这些内容，但在我看来这种对编程语言的进一步学习是十分有必要的，因为这不仅有利于我们捡起之前学过但是忘记了的知识，还能通过进一步的学习运用一些更高阶的编程方法，为后续的系统开发工具和爬虫的学习做铺垫。然后，我们就进入了一些程序设计和系统开发工具的学习，包括GUI（图形化用户界面）编程和其中的tkinter模块的相关内容学习，并尝试着自己运用所学知识制作了随机选号系统和简易成绩管理系统，且对MVC编程模式有了更深的了解和实际运用。我对这次的实验印象特别深刻，因为与之前相比此次实验的难度陡增，让我们整个小组都十分头大，因此在制作随机选号系统的时候我使用了一张搞怪的图片来作为系统中我的头像，来展现我”半死不活“的精神状态。除此之外，我们还学习了web编程的知识，并按照老师的教导使用Django创建了模拟华为商城的网站，当建站成功的那一刻还是成就感满满的。

<img src="/Users/yangchaoran/Library/Application Support/typora-user-images/截屏2023-10-27 08.02.58.png" alt="截屏2023-10-27 08.02.58" style="zoom:33%;" />

最后，我们又系统学习了python爬虫的相关知识，从一些基础知识、基本库和解析库的使用到新学习的动态渲染界面爬取，使用代理来防止被封ip等，并通过实战爬取了微信公众号的文章。通过对爬虫的进一步学习，我了解到了爬虫不仅仅是以短期内爬取到自己想要的数据 ，而是以代码的长期可用为目的。因此我们需要需要为爬取设置间隔，遵循机器人协议和学会使用代理等。

针对这个学期的学习，有几点令我感受十分深刻（也是在老师给的框架中自己总结出来的哈哈哈）

首先，就是如今的python生态正在不断发展，不断完善。当下的python第三方库已经有数以万计，且生态仍在不断完善更新，一些比较好用的软件包和模块比如faker模块生成随机数，生成伪造数据；还有PRegEx可以用来编程正则表达式，都是十分好用的，在本学期的实验中我们也通过实际运用感受到了它们的方便。

其次，我们学习了一些特殊的数据结构（HTML DOM），一个模型是由各种元素、属性和文本构成（且其实依据图论中的相关内容，树其实是一种特殊的图形）。一个实际例子就是，对于超文本中目标的查找，使用正则表达式容易出错，且其是通过字符串进行比对，效率很低。因此我们会偏向于使用xpath，css选择器和bs，因为其更好的考虑了超文本的结构，这便是数据结构在其中所发挥的作用。我也期待下学期在数据结构课程中进行进一步的学习。

我们还学习了一些创新的原理，因为创新的方法多种多样，而创新的原理和内核却是大抵相通的，例如迂回原理，就是让我们面对困难问题时，先解决相关的问题，进而对该问题进行突破。在本学期的实验中，当我们在进行爬虫的过程中就使用了迂回的思想，例如爬取目标需要网址，当网址难以获得时，可以通过selenium模拟用户行为、自行解析网址。

在设计模式方面，我们主要学习了面向对象的设计模式和MVC设计模式。面向对象的模式提供了一种抽象的方式来解决常见的软件设计问题，通常将问题分解为一组可重用的、可组合的对象来解决。设计模式是为了可重用代码、让代码更容易被他人理解，保持代码可靠性；而mvc是模型-视图-控制器的缩写，它是一种用业务逻辑、数据与界面分离的方法来组织代码，将众多的业务逻辑聚集在一个部件里面，在编写个性化用户界面和用户交互的同时，不用更改核心的业务逻辑。

框架的学习方面，我们在复习了一些基本的语言知识后学习了Django，运用这个框架，我们要对既有的结构进行填空，能让我们比较方便的创建网站。在最后一节课我们还学了爬虫框架scrapy，能够对数据抓取、信息处理、存储和可视化等一系列过程进行集成，可惜在写这个个人总结时，还没来得及进行进一步的实践，希望之后能过痛过自己上手进一步了解这个框架吧。最后，其实每一个python项目也可以看作一个松散的框架

这个学期已经接近尾声了，我们现在也在运用所学的知识尽力制作一个相对美观合理的美丽家乡网站。但是我知道学习是一个贯穿人生的过程，学期的结束必然不是对这个课程学习的结束。就像老师说的那样，如果不对自己所学的知识进行总结、巩固与在实例中运用，那么这些知识迟早会被慢慢遗忘，终究不是自己的东西（何况我平常有整理知识点的习惯，现在回顾一下也发现自己遗忘了一些知识）。同时，我们在这学期花了很多时间接触和使用语言大模型，但是过度依赖和错误使用大模型只会让自己产生惰性，不愿意自己思考，最终让软件成为自己进步的阻碍。因此，保持独立思考与批判精神，合理的运用大语言模型也同样重要，因为这部分已经单独写成了总结，因此在这就不再赘述。

最后，感谢黄万艮老师一个学期以来的教导，我真的受益良多。在往后，我将会持续学习，保持巩固知识、学以致用、独立思考的习惯，争取在信管专业、乃至整个大学生活中学到尽可能多的知识，做一个对学校、社会、国家有用的人。
