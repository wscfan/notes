# python笔记

## 1、tuple 元组
tuple 一旦创建完毕，就不能修改。
```python
t = ('apple', 'orange', 'banana')
```

单元素 tuple 要多加一个逗号 ","
```python
t = (1,)
```

## 2、dict
```python
d = {
	'Jack': 27,
	'Ben': 24,
	'Alia': 22
}
```
dict 本身提供一个 get 方法，在 Key 不存在时，返回 None
```python
print d.get('Jack')  # 27
print d.get('Paul')  # None
```
dict 储存的 key-value 序对是没有顺序的。

dict 作为 key 的元素必须不可变。 list 不能作为 key

dict 有一个 values() 方法，可以把 dict 转换成一个包含所有 value 的 list
```python
d = {'Jack': 27, 'Ben': 24, 'Alia': 22}
print d.values()  # [27, 24, 22]
for v in d.values()
	print v

"""
27
24
22
"""
```
在迭代的过程中，itervalues() 方法和 values() 方法的迭代效果一样。
不同的是 itervalues() 方法不会把 dict 转换成一个包含所有 value的 list
```python
d = {'Jack': 27, 'Ben': 24, 'Alia': 22}
for v in d.itervalues():
	print v

"""
27
24
22
"""
```

dict 对象的 items() 方法可以把 dict 对象转换成包含 tuple 的 list ，然后通过对这个 list 的迭代，可以同时获得 key 和 value
```python
d = {'Jack': 27, 'Ben': 24, 'Alia': 22}
print d.items()  # [('Alia', 22), ('Ben', 24), ('Jack', 27)]
for key, value in d.items():
	print key, ':', value
"""
Alia : 22
Ben : 24
Jack : 27
"""
```
在迭代的过程中，iteritems() 方法和 items() 方法的迭代效果一样。


## 3、set
set 持有一系列元素，但是 set 的元素没有重复，并且是无序的。
```python
s = set(['a', 'b', 'c'])
print s  # set(['a', 'b', 'c'])
len(s)  # 3
```
判断一个元素是否在 set 中
```python
s = set(['a', 'b', 'c'])
'a' in s  # True
```
set 中添加元素时，用 add() 方法，如果添加的元素已经存在，add() 不会报错，但是不会加进去。
set 中删除元素时，用 remove() 方法，如果删除的元素不存在，remove() 会报错。

## 4、函数
```python
def my_fun(x):
	return x

print myfun(66)  # 66
```
定义可变参数：可变参数的名字前面有一个 * 号。
```python
def fn(*args):
	print args

fn()  # ()
fn('a')  # ('a',)
fn('a', 'b')  # ('a', 'b')
```

## 5、slice 切片
```python
L = ['a', 'b', 'c', 'd']

L[1:3]  # ['b', 'c']
L[:3]  # ['a', 'b', 'c']
L[:]  # ['a', 'b', 'c', 'd']
L[::2]  #['a', 'c']

# 第三个参数表示每N个取一个
```
倒序切片
```python
L = ['a', 'b', 'c', 'd']

L[-2:]  # ['c', 'd']
L[:-2]  # ['a', 'b']
L[-3:-1]  # ['b', 'C']
L[-4:-1:2] # ['a', 'c']
```

## 6、索引迭代
迭代永远是取出元素本身，而非元素的索引。
如果想得到索引，可以使用 `enumerate()` 函数
```python
L = ['a', 'b', 'c', 'd']
for index, name in enumerate(L):
	print index, '-', name

"""
0 - a
1 - b
2 - c
3 - d
"""
```

## 7、列表生成式
写列表生成式时，把要生成的元素放在前面，后面跟for循环。
```python
[x * x for x in range(1, 11)]

# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
for循环可以嵌套，因此在列表生成式中，也可以用多层for循环来生成列表。
```python
[m + n for m in 'ABC' for n in '123']

# ['A1', 'A2', 'A3', 'B1', 'B2', 'B3', 'C1', 'C2', 'C3']
```
翻译成循环代码如下：
```python
L = []
for m in 'ABC':
	for n in '123':
		L.append(m + n)
```

## 8、map()
map() 接收一个函数 f 和一个 list ，并通过把函数 f 依次作用在 list 的每个元素上，得到一个新的 list 并返回。
```python
def f(x)
	return x * x
print map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
# [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

## 9、reduce()
reduce() 函数接收的参数和 map() 类似，一个函数 f ，一个 list 。 reduce() 传入的函数 f 必须接收两个参数，reduce() 对 list 的每一个元素反复调用函数 f ，并返回最终结果。
reduce() 还可以接收第三个参数，作为计算的初始值。
```python
def f(x, y):
	return x + y
print reduce(f, [1, 3, 5, 7, 9])  # 25
print reduce(f, [1, 3, 5, 7, 9], 100)  # 125
```

## 10、filter()
filter() 函数接收一个函数 f 和一个 list。
```python
def is_odd(x):
	return x % 2 == 1
filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
# [1, 3, 5, 7, 9]
```

## 11、自定义排序函数 sorted()
接收一个比较函数来实现自定义排序，比较函数传入两个元素 x, y ，如果 x 应该排在 y 前面，返回 -1 ，如果 x 应该排在 y 的后面，返回 1 。如果 x 和 y 相等，返回 0 。
```python
def reversed_cmp(x, y):
	if x > y:
		return -1
	if x < y:
		return 1
	return 0

sorted([23, 45, 13, 38, 49], reversed_cmp)
# [49, 45, 38, 23, 13]
```

## 12、导入模块
python 使用 import 语句导入一个模块。
```python
import math
```
如果只希望导入用到的几个函数，可以用下面的语句：
```python
from math import pow, sin, log
```
给函数起别名
```python
from logging import log as logger
```
