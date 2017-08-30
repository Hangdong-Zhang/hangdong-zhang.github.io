## Map函数

__map__(fx, [1, 2, 3, 4]) = [fx(1), fx(2), fx(3), fx(4)]



## Reduce函数

__reduce__(fx, [1, 2, 3, 4]) = fx(fx(fx(1, 2), 3), 4)



## Filter函数

__filter__(fx, [1, 2, 3, 4]) = [1 _if fx(1) is true_, 2 _if fx(2) is true_, 3 _if fx(3) is true_, 4 _if fx(4) is true_]



## Sorted函数

sorted([36, 5, 12, 9, 21]) = [5, 9, 12, 21, 36]

sorted([36, 5, 12, 9, 21], reversed_cmp) = [36, 21, 12, 9, 5]

    def reversed_cmp(x, y):
        if x > y:
            return -1
        if x < y:
            return 1
        return 0
###   

## \_\_str\_\_函数

类似于Java里的toString()方法，让实例可以被print出有价值的信息。

```
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):
...         return 'Student object (name: %s)' % self.name
...
>>> print Student('Michael')
Student object (name: Michael)
```



## _\_iter\_\_函数

让类可以被 for...in 循环，for循环调用实例的next()方法获取下一个值。

```
class add(object):
    def __init__(self):
        self.a = 0

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def next(self):
        self.a = self.a + 1
        return self.a
```

```
>>> for n in add():
...     print n
...
1
2
3
4
...
```



## _\_getitem\_\_函数

让实例可以像list一样，使用下标的方式获取对应位置的元素。

```
class add(object):
    def __getitem__(self, n):
        a = 1
        for x in range(n):
            a = a + 1
        return a
```

```
>>> f = add()
>>> f[0]
1
>>> f[1]
2
>>> f[2]
3
```



## _\_getattr\_\_函数

当外部调用不存在的类属性时，会报错。如果定义该函数，则调用就会进入该函数。

```
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        return attr
```

```
>>> s = Student()
>>> s.name
'Michael'
>>> s.score
score
```



## _\_call\_\_函数

通常我们调用实例的方法时，都是使用`instance.method()`的方式，使用\_\_call\_\_函数，相当于定义了一个`instance()`的方法

```
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
```

```
>>> s = Student('Michael')
>>> s()
My name is Michael.
```



## _\_all\_\_

用于控制模块（.py文件)）export（对外暴露）的符号：变量，函数，类等。

```
__all__ = ['b', 'c']

a = 5
b = 10
def c(): return 'c'
```

```
from foo import *

print b
print c

print a   # 出错
```



## with, _\_enter\_\_(), _\_exit\_\_()

事先设定好“准备”和“清理”操作，这样在使用时，就只需关注中间处理，类似单元测试的前置和后置函数。

```
>>> class A:  
    def enter(self):  
        print 'in enter'  
    def exit(self, e_t, e_v, t_b):  
        print 'in exit'  
  
 >>> with A() as a:  
   print 'in with'  
  
in enter  
in with  
in exit  
```

