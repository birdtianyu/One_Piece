# Python 3 编程技巧总结

**备用名称:** <font color='purple'>ans,   dp,  cur,  pre, </font>

```python
a = 10 if a > b else 11

>>> divmod(11, 5)
(2, 1)
>>> any(l) # 有一个为True就返回True; 全部为False, 返回False
>>> all(l) # 全部为True, 返回True; 有一个为False就返回False

# 1. 计算数组 nums 的前缀和, 初始值为0
pre = list(itertools.accumulate(nums, initial=0)) # python 3.9
enumerate(sequence, start=1) # start: index的初始值，默认是0，依然从第一个元素开始

# 2. 要对数组中元素进行筛选, 最好使用列表表达式

# 3. 列表对象展开
>>> l = [[0], [1,2]] # l中必须都是列表对象
>>> sum(l, []) # 将多维列表l, 变成1维(扁平化), PS: 第二个参数表示初始值
[0, 1, 2]
>>> [] + [0] # 巧妙利用了列表加法
[0]
>>> [0] + [1, 2]
[0, 1, 2]

# 4. 快速幂和快速幂取余：Leetcode周赛惨痛的教训
>>> pow(2, 10, 100) # 快速幂取余: 2**10%100
24
# 或者  
        # 10**3 % 7 = 1000 % 7 = 6
        # (7+3)(7+3)(7+3) = (7*7 + 2*3*7 + 3*3)(7 + 3)%7 = (3*3*3)%7 
        def quickMul(x, n, mod=1):
            res = 1 # 记录多余出来的奇数部分
            while n:
                if n % 2 == 1:
                    res = res * x # % mod
                x = x * x # % mod
                n = n // 2
            return res

# 5. 类型检查
isinstance(obj, class)   # 判断obj是否是class及其子类的实例
type(obj)                # 返回当前obj的class类型，不考虑继承关系

# 6. 命名空间(一个字典)
# locals() 用来获取当前作用域的命名空间。 
# 如果在全局作用域中调用locals()则获取全局命名空间，如果在函数作用域中调用locals()则获取函数命名空间。返回的是一个字典。
locals()
# globals() 函数可以用来在任意位置获取全局命名空间
globals()
```

**换行编写多行语句：**`\`

**变量相当于给一个对象（id, type, value）起的别名**。



**禁止使用`eval("[1, 2, 3]")`转换输入为列表, 使用ast模块的`literal_eval()`函数代替**

```python
eval("[1, 2, 3]")              # 不安全，可能执行用户输入的意料之外的代码

import ast
# 安全，该函数要求输入字符串中只包含字面值数据，不能是可执行的代码，否则抛出异常
ast.literal_eval("[1, 2, 3]")  
```





## 面向过程编程

### <font color='red'>1. collections 默认库</font>

---

``````python
import collections

S = "cdasda"
init = collections.Counter() # 可以直接这样初始化成{key:nums}字典, 没有key也不会报错
counts = collections.Counter(S) # 统计可迭代对象的个数，并按从多到少返回字典
print(counts)
``````

输出结果：

```
Counter({'d': 2, 'a': 2, 'c': 1, 's': 1})
```

**1. 队列**

```python
import collections 

q = collections.deque() # 双向队列
q.appendleft('0')
q.append('1')
q.pop()
q.popleft()
```

**2. 带初始类型的字典**

**3. 有顺序的字典**

※ 见字典那一小节



### <font color='red'>2.  列表</font>

---

#### 2.1 数组删除要倒着删

#####  (<font color='red'>※数组删除需谨慎</font>)

Python中删除list中的某个元素，一般有三种方法:remove、pop、del：

**1.remove:** 删除单个元素，删除首个符合条件的元素，按值删除

<img src="D:\技法\笔记图片\image1.png" alt="image-20210407000141334" style="zoom: 80%;" />

**2.pop:** 删除单个或多个元素，按位删除(根据索引删除)

<img src="D:\技法\笔记图片\image2.png" alt="image-20210407000438457" style="zoom:80%;" />

**3.del：**它是根据索引(元素所在位置)来删除

<img src="D:\技法\笔记图片\image3.png" alt="image-20210407000722901" style="zoom:80%;" />

#### 2.2 颠倒列表

```python
list(reversed(a))
```

#### 2.3 对列表的值排序，并返回对应的index排序

```python
>>> l = [3, 9, 2, 1, 12]
>>> sorted(range(len(l)), key= lambda i: l[i]) 
[3, 2, 0, 1, 4]
>>> sorted(l)
[1, 2, 3, 9, 12]
```

#### 2.4  在list某位置插入数据

```python
list.insert(index, obj) # 这样就可以在列表头插入值了
```

#### 2.5 计算前缀和

```python
import itertools

# 计算数组 nums 的前缀和, 初始值为0
pre = list(itertools.accumulate(nums, initial=0)) # python 3.9
```



### <font color='red'>3. 集合</font>

---

#### 3.1 常见操作

```python
>>> s = set('abracadabra')  # {'r', 'd', 'b', 'c', 'a'}
>>> s.add(10)               # {10, 'r', 'd', 'b', 'c', 'a'}
# 删除集合元素不要用pop()和remove(), 不存在的话会报错
>>> s.discard(10)           # {'r', 'd', 'b', 'c', 'a'}
>>> s.discard(111)          # {'r', 'd', 'b', 'c', 'a'}
```



#### 3.2 交集，并集，差集，亦或集

```python
list(set(a).intersection(set(b))) # 交集
set(nums1)&set(nums2)             

list(set(a).union(set(b)))        # 并集
set(nums1)|set(nums2)

list(set(a).difference(set(b)))   # 差集: a-b
set(nums1)-set(nums2)

set(nums1)^set(nums2)             # 各自独有的元素
```



#### 3.3 判断子集

```python
b.issubset(a) # 判断b是否是a的子集
```



### <font color='red'>4. str</font>

---

#### 4.1 常见操作

```python
s.startswith("")
s.endswith("")
s.upper()
s.lower()
s.swapcase()          # 大小写互换

s.lstrip()            # 去除左端空格
s.rjust(width, char)  # 右对齐, 左端填充char
s.zfill(width)        # 右对齐, 左端填充0
s.strip()             # 去除左右两端空格

s.replace(old, new)
s.partition(part2)    # 根据part2, 把s变成(part1, part2, part3) 

list(s)  # 直接分割成列表
s.split("?") # "?"必须有
"".join(list_object)
```

#### 4.2 **字符串快捷取反**

```python
s[::-1]
```

#### 4.3 执行字符串表达式

```python
eval("10*12/10")
```

#### 4.4 ASCII码

```python
ord(s)  # 返回字符s的ASCII数值
chr(ASCII) # 返回ASCII数值对应的字符
```

#### 4.5 字符串与bytes（字节序列）转换

```python
str.encode("utf-8")   # 译码
bytes.decode("utf-8") # 解码
```







### <font color='red'>5. 字典</font>

---

可以直接利用`==`判断两个字典是否所有键值对都相等。

#### 5.1 常见操作

```python
d.keys()     # 返回set
d.values()   # 返回set
d.items()    # 返回set(key, value)

d.get(key, default)  # 返回value, 否则返回default值
d.pop(key)           # 删除(key, value)， 返回value
```

#### 5.2 创建字典

```python
d = dict(zip(l_keys, l_values))
```

PS:  `zip(l1, l2)` 只会截止到两者中长度较短的，返回`tuple`迭代器

#### 5.3 字典的最值

```python
max(d) or max(d, key=d.get) # 返回最大值的键
max(d.values())             # 返回最大值
```



#### 5.4 带初始类型的字典

```python
import collections

# 应用于图的path统计（染色问题）
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = collections.defaultdict(list) # 如果不存在此键，则创建此键，并默认其初始值为list类型
for k, v in s:
    d[k].append(v)

sorted(d.items()) # [('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```

#### 5.5 有顺序的字典

```python
>>> from collections import OrderedDict  # 有顺序的字典
>>> d = OrderedDict.fromkeys("abcde")
>>> d
OrderedDict([('a', None), ('b', None), ('c', None), ('d', None), ('e', None)])
>>> d.popitem()      # 默认pop最后一个
('e', None)
>>> d.popitem(False) # 这样写pop第一个
('a', None)
>>> d.move_to_end('b') # 把一个键值对挪到最后
>>> d
OrderedDict([('c', None), ('d', None), ('b', None)])
```



### <font color='red'>6. 浅拷贝</font>

---

#### 6.1 `==` 与 `is` 的区别

>+ `==` 用于比较对象的内容
>+ `is` 比较对象的id（简单理解为内存）

```python
# 1. 赋值&换标签
>>> a = [1, 2, 3]
>>> b = a
>>> b == a
True
>>> b is a
True
# id(): Python的一个内置函数，返回对象的唯一标识，用于获取对象的内存地址。
>>> id(a)    # 2333656558792
>>> id(b)    # 2333656558792

# 2. ==与is的区别
>>> c = [1, 2, 3]
>>> c == a
True
>>> c is a
False
>>> id(c)    # 2333688213320
```



#### 6.2 浅拷贝

**方式1. 使用list()能创建新对象, 但默认里面的可变对象是浅拷贝**

```python
>>> f = [1, [2, 3]]
>>> g = list(f)
>>> g is f
False
>>> g == f
True
>>> g[1][0] = 100
>>> f
[1, [100, 3]] # 但是, f和g内部的可变对象都指向同一地址
```

**方式2. list.copy()函数: 里面的可变对象仍是浅拷贝**

```python
>>> e = f.copy()
>>> e
[1, [100, 3]]
>>> e[1][0] = 1000
>>> f
[1, [1000, 3]]
```

**方式3. 完整切片: 里面的可变对象仍是浅拷贝**

```python
list[:]
```



#### 6.3 深拷贝

```python
>>> import copy
>>> x = copy.deepcopy(f)  # 深拷贝的方式
>>> x == f
True
>>> x[1][0] = -1
>>> x
[1, [-1, 3]]
>>> f
[1, [1000, 3]]   # 深拷贝成功
```





### <font color='red'>7. 位操作：二进制，八进制，十六进制数</font>

---

```python
bin(10) # 2进制 
oct(10) # 8进制
hex(10) # 16进制
```

#### 6.1 str -> 2进制

```python
int('10010',base=2)
```

*判断2进制数连续有多少个1

```python
def getWidth(num):
    w=0
    while num>0:
        num &= num<<1
        w += 1
    return w
```

#### <font color='red'>6.2 二进制数-格式问题：</font>

```python
map('{:032b}'.format, nums) # 数组中每个数，转换成左端用0填充的，32位二进制数

>>> list(zip('010', '201')) # 返回对应元素的tuple
[('0', '2'), ('1', '0'), ('0', '1')]

>>> nums = [12,34,56,78,99]
>>> list(map('{:010b}'.format, nums))
['0000001100', '0000100010', '0000111000', '0001001110', '0001100011']
## 可变参数
>>> list(zip(*map('{:010b}'.format, nums)))  # 统计对应元素时，最完美的写法
[('0', '0', '0', '0', '0'), ('0', '0', '0', '0', '0'), ('0', '0', '0', '0', '0'), ('0', '0', '0', '1', '1'), ('0', '1', '1', '0', '1'), ('0', '0', '1', '0', '0'), ('1', '0', '1', '1', '0'), ('1', '0', '0', '1', '0'), ('0', '1', '0', '1', '1'), ('0', '0', '0', '0', '1')]
```



#### 6.3 XOR异或操作

> 1、交换律
>
> 2、结合律（即(a\^b)\^c == a\^(b\^c)）
>
> 3、对于任何数x，都有x\^x=0，x\^0=x
>
> 4、自反性 A XOR B XOR B = A xor 0 = A   
>
> 例：google面试题：一个数组存放若干整数，一个数出现奇数次，其余数均出现偶数次，找出这个出现奇数次的数？
>
> 5、A \^ B = C  =>  C ^ A = B

**2021.04.17 力扣周赛惨痛教训**

```python
>>> 1^2 # XOR异或运算
3
>>> bin(5)
'0b101'
>>> bin(5^(2**3-1))  # 任何数与对应位数的1···11异或，相当于按位取反
'0b10'
```

#### 6.4 取反操作

```python
# 对于数字4进行3位取反
>>> '{:03b}'.format(4)
'100'
>>> '{:03b}'.format(4^7) # 异或3位二进制数最大值“111”
'011'

# 直接用~符号求得数会被解释成补码，最终存储的数值是其补码值，再用bin表示这个补码的原码是没用的
```

#### 6.5 同或操作

```python
异或取反
```



### <font color='red'>8. sorted & lambda</font>

---

```python
>>> buildings = [[2,9,10], [19,24,8], [3,7,15], [15,20,10], [5,12,12]]
>>> sorted(buildings, key=lambda x: x[0]) # 按照第一个元素排序
[[2, 9, 10], [3, 7, 15], [5, 12, 12], [15, 20, 10], [19, 24, 8]]
>>> buildings.sort(key=lambda x: x[0])
>>> buildings
[[2, 9, 10], [3, 7, 15], [5, 12, 12], [15, 20, 10], [19, 24, 8]]

# 按照第一个元素降序，并且按照第二个元素升序 (主key与副key)
>>> buildings = [[2,9], [19,24], [3,7], [2,20], [5,1]]
>>> sorted(buildings, key=lambda x:(x[0], -x[1]), reverse=True)
[[19, 24], [5, 1], [3, 7], [2, 9], [2, 20]]
```

等同于下面代码，对每一个可迭代对象中的元素，进行匿名函数f()处理

```python
>>> def f(x):
	return x[0]

>>> buildings = [[2,9,10], [19,24,8],[3,7,15],[15,20,10],[5,12,12]]
>>> sorted(buildings, key=f)
[[2, 9, 10], [3, 7, 15], [5, 12, 12], [15, 20, 10], [19, 24, 8]]
```



**※  根据比较函数进行sort**

例: 给定一组非负整数 `nums`，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。

```
输入：nums = [3,30,34,5,9]
输出："9534330"
```

```python
from functools import cmp_to_key
def compare(x, y):
    if int(x + y) < int(y + x):
        return 1
    else:
        return -1

sorted(mylist, key=cmp_to_key(compare))
```



```python
compare = lambda x, y: 1 if int(x + y) < int(y + x) else -1
```



### <font color='red'>9. 正无穷 & 负无穷</font>

---

```python
float('inf')
float('-inf')

>>> import math
>>> math.isinf(a) # 判断a是否是正无穷或负无穷
```



### <font color='red'>10. zip() & zip(*iterables)</font>

---

**1. zip(迭代器)： **将迭代器内每个元素变成元组，返回一个zip对象

```python
>>> a = [1, 2, 3]
>>> zip(a)
<zip object at 0x000002257316B508>
>>> list(zip(a))
[(1,), (2,), (3,)]  
# 元素是可变元素时同理
# [([1, 2],), ([3, 4],)]
```



**2. zip(迭代器A, 迭代器B, ...)： **分别将迭代器A，B, ...内每个元素取出来结合成一个元组，返回一个zip对象

```python
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> list(zip(a, b))
[(1, 4), (2, 5), (3, 6)]
```

注意：**迭代器的元素数量不等时，会取最小的元素数量匹配**

```python
>>> a = [1, 2, 3]
>>> b = [4, 5]
>>> list(zip(a, b))
[(1, 4), (2, 5)]
```



**语法糖：zip(*iterables)：** *iterables表示参数拆包，相当于把iterables每个元素取出来当做参数传入zip()函数。

等同于zip(元素1, 元素2, 元素3, ...)

**最终效果：<font color='red'>矩阵转置</font>**

```python
>>> grid = [[1, 2], [3, 4], [5, 6]]
>>> list(zip(*grid))
[(1, 3, 5), (2, 4, 6)]
```







### <font color='red'>11. 优先级操作模块</font>

---

#### 11.1 heapq 堆操作模块

```python
>>> import heapq
>>> h = [] # 如果有值, 对其不起作用, 所以一定要保证其最大
>>> heapq.heappush(h, (3, 4))
>>> heapq.heappush(h, (0, 1))
>>> heapq.heappush(h, (4, 2))
>>> h # 按第一个值从小到大排列, 但是h内部不是按顺序排列的！！！
[(0, 1), (4, 2), (3, 4)]
>>> heapq.heappop(h)
(0, 1)
>>> h
[(3, 4), (4, 2)]
```

**heapq只能将最小的放最前面，如果要实现最大堆，可以把主键值变为负数**

#### 11.2 bisect 数组二分查找维持有序列表

```python
>>> import bisect
>>> import random
>>> l = []
>>> for i in range(1, 11):
        r = random.randint(1, 100)
	    bisect.insort(l, r)
	    print(r, l)
    
2 [2]
49 [2, 49]
34 [2, 34, 49]
91 [2, 34, 49, 91]
65 [2, 34, 49, 65, 91]
61 [2, 34, 49, 61, 65, 91]
80 [2, 34, 49, 61, 65, 80, 91]
99 [2, 34, 49, 61, 65, 80, 91, 99]
68 [2, 34, 49, 61, 65, 68, 80, 91, 99]
78 [2, 34, 49, 61, 65, 68, 78, 80, 91, 99]
>>> bisect.bisect_right(l, 65) # 返回大于65的最近的元素的index
5
>>> bisect.bisect_right(l, 66) # 返回大于66的最近的元素的index
5
>>> bisect.bisect_left(l, 65)  # 返回小于等于65的最近的元素的index
4
# 注意如果不存在该元素bisect_left和bisect_right返回的index相等
```



### <font color='red'>12. filter函数 & map函数 & reduce函数</font>

---

```python
filter(function, iterable) # 返回过滤后的新序列对象
```

```python
>>> a = [2, 1, 3, 4, 1]
>>> filter(lambda x: x>1, a)
<filter object at 0x00000218628B6288>
>>> list(filter(lambda x: x>1, a))
[2, 3, 4]
```




```python
map(function, iterable, ...) # 针对每个迭代对象进行函数操作
```

```python
>>> def square(x) :         # 计算平方数
...     return x ** 2
...
>>> map(square, [1,2,3,4,5])    # 计算列表各个元素的平方
<map object at 0x100d3d550>     # 返回迭代器
>>> list(map(square, [1,2,3,4,5]))   # 使用 list() 转换为列表
[1, 4, 9, 16, 25]
>>> list(map(lambda x: x ** 2, [1, 2, 3, 4, 5]))   # 使用 lambda 匿名函数
[1, 4, 9, 16, 25]
```



```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

```python
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25
```

**reduce最重要的用法：阶乘**

```python
>>> n = 3
>>> reduce(lambda x, y: x*y, range(1, n+1))
6
```





### <font color='red'>13. 作用域：nonlocal & global</font>

---



```python
>>> a = 1
>>> def fun():
	    print(a)
	    a = 2
	    print(a)
    
>>> fun()
Traceback (most recent call last):
  File "<pyshell#7>", line 1, in <module>
    fun()
  File "<pyshell#6>", line 2, in fun
    print(a)
UnboundLocalError: local variable 'a' referenced before assignment
>>> def fun():
	nonlocal a # a为外层变量, 但不能是全局变量
	print(a)
	a = 2
	print(a)
	
SyntaxError: no binding for nonlocal 'a' found
>>> def fun():
	global a # a是外部的全局变量
	print(a)
	a = 2
	print(a)
	
>>> fun()
1
2
```

**※ 只要在一个作用域中有 global 或者 nonlocal 命令，则不管这个命令在哪个位置，在整个作用域的开始到结尾都是有效的。**



### <font color='red'>14. @functools.lru_cache( maxsize=None, typed=False )</font>

---

**用于记忆递归函数的计算结果**

如果 *typed* 设置为true，不同类型的函数参数将被分别缓存。例如， `f(3)` 和 `f(3.0)` 将被视为不同而分别缓存。

```python
@functools.lru_cache(maxsize=None, typed=False)
def fibonacci(n):
    if n < 2:
        return n
	return fibonacci(n-2) + fibonacci(n-1)

>>> fibonacci(100)
354224848179261915075
```



### <font color='red'>15. math & cmath</font>

---

```python
import math
# [x for x in dir(math) if not x.startswith('__')]

math.pi
math.e

math.ceil(float)   # 返回int
math.floor(float)  # 返回int

math.log(x, base)
math.log2(x)
math.log10(x)
math.sqrt(x)         # 返回平方根

math.factorial(x)    # 返回x!
math.comb(n, k)      # n里面取k个
```



### <font color='red'>16. 字典树（Trie树）</font>

---

根节点为空，每个节点自带字典，指向不同分支

<img src="D:\技法\字典树.jpg" alt="fig2" style="zoom: 33%;" />

```python
Trie = lambda: collections.defaultdict(Trie) # 返回值为defaultdict(Trie)的函数
trie = Trie()   # 根节点只有指向下一个节点的dict
```

x.\_\_getitem\_\_(y) <=> x[y] 相当于每次trie[S[i]].get(S[i+1]), 有的话返回那个trie, 没有的话新建返回值为defaultdict

```python
# reduce(两个参数的函数, S, trie) 等价于 trie[S[0]][S[1]][S[2]][...][S[S.length - 1]], 第三个参数为列表为空时默认返回值，即第一次调用值
nodes = [reduce(dict.__getitem__, word[::-1], trie) for word in words]

# [trie[S[0]][S[1]][S[2]], trie[S[0]][S[1]], trie[S[0]][S[1]][S[2]][S[3]], ...] 每个单词最后一个字母所指向的dict

#Add word to the answer if it's a Leaf.
return sum(len(word) + 1
           for i, word in enumerate(words)
           if len(nodes[i]) == 0) # 指向下一个节点的dict为空时，说明它是叶子节点, 最后一个字母不是叶子节点的说明重复了
```



### <font color='red'>17. 闭包</font>

---

```python
# 将函数作为返回值返回（一种高阶函数）
# 通过闭包可以创建一些只有当前函数能访问的变量，可以将一些私有的数据藏到的闭包中

def fn():
    a = 10
    
    # 函数内部再定义一个函数
    def inner():
        print('我是fn2' , a)

    # 将内部函数 inner作为返回值返回   
    return inner

# r是一个函数，是调用fn()后返回的函数。这个函数实在fn()内部定义，并不是全局函数
# 闭包好处：所以这个函数总是能访问到fn()函数内的变量
r = fn()    
# r()

# 形成闭包的要件
#   ① 函数嵌套
#   ② 将内部函数作为返回值返回
#   ③ 内部函数必须要使用到外部函数的变量
def make_averager():
    # 创建一个列表，用来保存数值
    nums = [] # 闭包可以保护这个列表免受外部修改

    # 创建一个函数，用来计算平均值
    def averager(n) :
        # 将n添加到列表中
        nums.append(n)
        # 求平均值
        return sum(nums)/len(nums)

    return averager

averager = make_averager()

print(averager(10))
print(averager(20))
print(averager(30))
print(averager(40))
```



### 18. 用<font color='red'>装饰器</font>(Decorator)维护批量函数

---

在不修改原函数的情况下，来对函数进行扩展：

```python
# 创建几个函数
def add(a , b):
    '''
        求任意两个数的和
    '''
    r = a + b
    return r


def mul(a , b):
    '''
        求任意两个数的积
    '''
    r = a * b
    return r   

#################################################################

# 扩展函数
def new_add(a,b):
    print('计算开始~~~')
    r = add(a,b)
    print('计算结束~~~')
    return r
# r = new_add(111,222)    
# print(r)

# 上边的方式，已经可以在不修改源代码的情况下对函数进行扩展了
#   但是，这种方式要求我们每扩展一个函数就要手动创建一个新的函数，实在是太麻烦了
#   为了解决这个问题，我们创建一个函数，让这个函数可以自动的帮助我们生产函数

##################################################################

# 1. 装饰器的定义:
def begin_end(fn):
    '''
        用来对其他函数进行扩展，使其他函数可以在执行前打印开始执行，执行后打印执行结束

        参数：
            fn:要扩展的函数对象
    '''
    # 创建一个新函数
    def new_function(*args , **kwargs):  # ※ 参数装包 -> (元组, 字典)
        print('开始执行~~~~')
        # 调用被扩展的函数
        result = old(*args , **kwargs)   # ※ 参数解包 <- (元祖解包, 字典解包)
        print('执行结束~~~~')
        # 返回函数的执行结果
        return result

    # 返回新函数        
    return new_function

f2 = begin_end(add)
f3 = begin_end(mul)

"""
#   通过装饰器，可以在不修改原来函数的情况下来对函数进行扩展，在开发中，我们都是通过装饰器来扩展函数的功能的。
#   在定义函数时，可以通过@装饰器，来使用指定的装饰器，来装饰当前的函数, 
#   可以同时为一个函数指定多个装饰器，这样函数将会安装从内向外的顺序被装饰 
"""

# 2. 装饰器的使用
@Other_Decorator
@begin_end
def say_hello():
    print('大家好~~~')

say_hello()
```

**※ 使用functools.warps定义自己的修饰器，否则被装饰函数的函数签名将被破坏**

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)  # 没有这行函数签名将变成wapper函数的
    def wapper(*args, **kwargs):
        print("call decorated function")
        return func(*args, **kwargs)
    return wapper


@my_decorator
def example():
    """this is example docstring"""
    print("call example function")
    

example()
print(example.__name__)
print(example.__doc__)
```



### <font color='red'>19. 生成器 & 迭代器</font>

---

<img src="D:\技法\笔记图片\生成器&迭代器.png" alt="img" style="zoom:80%;" />

#### 19.1 生成器

**方法1:（<font color='red'>生成器表达式</font>）把一个列表生成式的[]中括号改为（）小括号，就创建一个generator**。

```python
#列表生成式
lis = [x*x for x in range(10)]
print(lis)            # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

#生成器
generator_ex = (x*x for x in range(10))
print(generator_ex)   # <generator object <genexpr> at 0x000001A9E0085E48>

for i in generator_ex:
	print(i)   # for会自动调用next(generator_obj)实现生成器遍历
```



**方法2:（<font color='red'>生成器函数</font>）yield关键字**

```python
# 普通打印函数
def func(n):
    for i in range(n):
        print(i, end=" ")
        
>>> func(6)
0 1 2 3 4 5

# 使用关键字yield定义生成器函数, 每次执行到yield自动阻塞
def generator_func(n):
    for i in range(n):
        yield (i)

>>> generator_func(6)        # 注意这个函数返回的是对象！ yield其实相当于装饰器的功能
<generator object generator_func at 0x000001A9E0085EC8>
>>> gen = generator_func(6)  # 使用obj.__next__()或者next(obj)可以获得下一个元素, 最终将抛出StopIteration异常
>>> gen.__next__()
0
>>> gen.__next__()
1
>>> next(gen)
2
```



#### 19.2 迭代器

一个实现了\_\_iter\_\_方法的对象是**<font color='red'>可迭代的</font>**，一个实现\_\_next\_\_方法并且是可迭代的对象是**<font color='red'>迭代器</font>。**

> 迭代器是这样的对象：实现了无参数的\_\_next\_\_方法，返回序列的下一个元素；如果没有元素了，那么抛出StopIteration异常。Python中迭代器还实现了\_\_iter\_\_方法，因为迭代器也是可迭代的。

可以被next()函数调用并不断返回下一个值的对象称为**<font color='red'>迭代器：Iterator</font>。**

```python
from collections.abc import Iterable, Iterator

>>> isinstance([1, 2, 3], Iterable)  # True
>>> isinstance([1, 2, 3], Iterator)  # False

>>> isinstance((1, 2, 3), Iterable)  # True
>>> isinstance((1, 2, 3), Iterator)  # False

>>> isinstance({1:"1", 2:"2"}, Iterable)  # True
>>> isinstance({1:"1", 2:"2"}, Iterator)  # False
```

**可以这么理解，在Python中，每一个可迭代的对象中都隐藏了一个迭代器（在\_\_iter\_\_方法里），它很懒，一次只返回一个值；**

**用iter(obj)函数或者obj.\_\_iter\_\_()可以获取到这个迭代器。**

*斜体表示抽象方法*

![image-20220506211259130](D:\技法\笔记图片\Iterator.png)

**生成器是一种特殊的迭代器，内部支持了生成器协议，不需要明确定义 `__iter__` 方法和 `__next__` 方法**，

但`list`、`dict`、`str`虽然是`Iterable（可迭代对象）`，却不是`Iterator（迭代器）`。

```python
>>> iter([1, 2, 3])
<list_iterator object at 0x000002334ED76288>
>>> isinstance(iter([1, 2, 3]), Iterator)
True
>>> isinstance([1, 2, 3].__iter__(), Iterable)
True
```



#### 19.3 itertools模块

待定





### <font color='red'>21. random默认库（不建议使用）& secrets默认库</font>

---

```python
import random
# 由于生成的是伪随机数，所以不建议使用

# 等概率返回序列中任意值
random.choice(序列)

# 从range(start, end, step)中随机返回一个数
random.randrange(start, end, step)
```



```python
import secrets

>>> [ name for name in dir(secrets) if not name.startswith("__")]
['DEFAULT_ENTROPY', 'SystemRandom', '_sysrand', 'base64', 'binascii', 'choice', 'compare_digest', 'os', 'randbelow', 'randbits', 'token_bytes', 'token_hex', 'token_urlsafe']
```







### <font color='red'>22. 正则表达式</font>

---





















## 面向对象编程

### <font color='Violet'>1. 类的定义方法</font>

---

**一个标准的类定义：**

```python
# 类属性, 所有对象共享，可以通过类或类的实例访问。
# 但是类属性只能通过类对象来修改，无法通过实例对象修改（等于创建了一个新的实例属性）。
count = 0

def __new__():
    '''静态方法: 用于创建对象'''
    pass

def __init__():  # 禁止在__init__方法外定义新的实例属性
    '''对创建的对象初始化'''
    pass

def __post_init__():
    pass

# 其它魔法函数

# @property 修饰的对象属性

# @staticmethod 修饰的静态方法

# @classmethod 修饰的类方法

# 普通方法

# 保护方法 & 私有方法

```



### <font color='Violet'>2. 静态方法 & 类方法</font>

---

>+ 不需要访问实例成员<font color="red">但</font>需要访问类成员时，建议使用 @classmethod 将方法定义为**类方法**
>+ 不需要访问实例成员<font color="red">也不</font>需要访问类成员时，建议使用 @staticmethod 将方法定义为**静态方法**

```python
class MyClass:
    class_attr = "MyClass.class_attr"
    
    @classmethod
    def set_class_attr(cls, string_value: str): # 类方法第一个参数不管是什么名字都表示这个类本身
        cls.class_attr = string_value
        
    @staticmethod
    def add(a: int, b: int) -> int:
        return a + b
```



### <font color='Violet'>3. 类的\_\_slots\_\_属性</font>

---

> 在类实例较多时（一般百万级别以上）， 
>
> 通过使用\_\_slots\_\_可以减少类实例所占用的内存大小。
>
> 同时\_\_slots\_\_也限定了不能给对象添加除\_\_slots\_\_中包含的新的属性，但对继承的子类的实例无效。

原理：

默认情况下，Python类的实例都有一个**dict来存储实例的属性**。这在一般情况下还不错，而且非常灵活，乃至在程序中可以**随意设置新的属性**。但是，对一些在”编译”前就知道有几个固定属性的小class来说，这个dict就有点浪费内存了。当需要创建大量实例的时候，这个问题变得尤为突出。

在类中定义一个\_\_slots\_\_属性（**元组**），\_\_slots\_\_声明中包含若干实例变量，并为每个实例预留恰好足够的空间来保存每个变量；这样Python就不会再使用dict，从而节省空间。



### <font color='Violet'> 4. 封装</font>

---

确保对象中的数据安全

```python
class Person:
    def __init__(self,name,age):
        self._name = name
        self._age = age

    # property装饰器，用来将一个get方法，转换为对象的属性
    # 添加为property装饰器以后，我们就可以像调用属性一样使用get方法
    # 使用property装饰的方法，必须和属性名是一样的
    @property    
    def name(self):
        print('get方法执行了~~~')
        return self._name

    # setter方法的装饰器：@属性名.setter
    @name.setter    
    def name(self , name):
        print('setter方法调用了')
        self._name = name        

    @property
    def age(self):
        return self._age

    @age.setter    
    def age(self , age):
        self._age = age   

        
>>> p = Person('AAA',18)
>>> print(p.name, p.age)
get方法执行了~~~
AAA 18
>>> p.name = 'BBB'
setter方法调用了
```



### 6. 继承

---

保证了对象的可扩展性

```python
class Animal:
    def __init__(self,name):
        self._name = name

class Dog(Animal):

    def __init__(self,name,age):
        # 希望可以直接调用父类的__init__来初始化父类中定义的属性
        # super() 可以用来获取当前类的父类，
        # 并且通过super()返回对象调用父类方法时，不需要传递self
        super(Dog, self).__init__(name)
        self._age = age
```



### 7. 多态

---

保证了程序的灵活性

```python
# 鸭子类型
# 如果一个东西，走路像鸭子，叫声像鸭子，那么它就是鸭子

# len()
# 之所以一个对象能通过len()来获取长度，是因为对象中具有一个特殊方法__len__
# 换句话说，只要对象中具有__len__特殊方法，就可以通过len()来获取它的长度
l = [1,2,3]
s = 'hello'

len(l)
len(s)
```





### 8. Python的垃圾回收

---



```python
class A:
    def __init__(self):
        self.name = 'A类'

    # 析构方法，它会在对象被垃圾回收前调用
    def __del__(self):
        print('A()对象要被删除了~~~',self)
        
        
>>> a = A()
>>> # b = a # 又使用一个变量b，来引用a对应的对象，此时即使a = None，a也不是垃圾

>>> a = None # 将a设置为了None，此时没有任何的变量对A()对象进行引用，它就是变成了垃圾
A()对象要被删除了~~~ <__main__.A object at 0x000002356E28D4C8>

```





## Python Socket编程



![image.png-58.5kB](D:\技法\笔记图片\Socket.png)























# 备注：

1. **二叉搜索树**：左结点小， 右结点大

<img src="D:\技法\笔记图片\二叉搜索树.png" alt="二叉搜索树" style="zoom:50%;" />



2. **红黑树**是一种**伪平衡二叉搜索树**，可以保证在最坏情况下各种动态操作的时间复杂度为**$O(\log n)$**.
3. 根据一个**列表中的Node创建树**，需要新建一个Node，然后移动next指针一次遍历，不能直接遍历列表中的元素创建树。
4. 





#### a. 各种符号的函数对照表

| 运算         | 语法                | 函数                                |
| :----------- | :------------------ | :---------------------------------- |
| 加法         | `a + b`             | `add(a, b)`                         |
| 字符串拼接   | `seq1 + seq2`       | `concat(seq1, seq2)`                |
| 包含测试     | `obj in seq`        | `contains(seq, obj)`                |
| 除法         | `a / b`             | `truediv(a, b)`                     |
| 除法         | `a // b`            | `floordiv(a, b)`                    |
| 按位与       | `a & b`             | `and_(a, b)`                        |
| 按位异或     | `a ^ b`             | `xor(a, b)`                         |
| 按位取反     | `~ a`               | `invert(a)`                         |
| 按位或       | `a | b`             | `or_(a, b)`                         |
| 取幂         | `a ** b`            | `pow(a, b)`                         |
| 标识         | `a is b`            | `is_(a, b)`                         |
| 标识         | `a is not b`        | `is_not(a, b)`                      |
| 索引赋值     | `obj[k] = v`        | `setitem(obj, k, v)`                |
| 索引删除     | `del obj[k]`        | `delitem(obj, k)`                   |
| 索引取值     | `obj[k]`            | `getitem(obj, k)`                   |
| 左移         | `a << b`            | `lshift(a, b)`                      |
| 取模         | `a % b`             | `mod(a, b)`                         |
| 乘法         | `a * b`             | `mul(a, b)`                         |
| 矩阵乘法     | `a @ b`             | `matmul(a, b)`                      |
| 取反（算术） | `- a`               | `neg(a)`                            |
| 取反（逻辑） | `not a`             | `not_(a)`                           |
| 正数         | `+ a`               | `pos(a)`                            |
| 右移         | `a >> b`            | `rshift(a, b)`                      |
| 切片赋值     | `seq[i:j] = values` | `setitem(seq, slice(i, j), values)` |
| 切片删除     | `del seq[i:j]`      | `delitem(seq, slice(i, j))`         |
| 切片取值     | `seq[i:j]`          | `getitem(seq, slice(i, j))`         |
| 字符串格式化 | `s % obj`           | `mod(s, obj)`                       |
| 减法         | `a - b`             | `sub(a, b)`                         |
| 真值测试     | `obj`               | `truth(obj)`                        |
| 比较         | `a < b`             | `lt(a, b)`                          |
| 比较         | `a <= b`            | `le(a, b)`                          |
| 相等         | `a == b`            | `eq(a, b)`                          |
| 不等         | `a != b`            | `ne(a, b)`                          |
| 比较         | `a >= b`            | `ge(a, b)`                          |
| 比较         | `a > b`             | `gt(a, b)`                          |



#### b. 一些有趣的魔法函数

```python
# 序列.__sizeof__()函数：
>>> l = [1, 2, 3]
>>> l.__sizeof__()  # 单位: 字节
64

# __str__（）这个特殊方法会在尝试将对象转换为字符串的时候调用
# 它的作用可以用来指定对象转换为字符串的结果（print函数）  
def __str__(self):
    return 'Person [name=%s , age=%d]'%(self.name,self.age)        

# __repr__()这个特殊方法会在对当前对象使用repr()函数时调用
# 它的作用是指定对象在‘交互模式’中直接输出的效果    
def __repr__(self):
    return 'Hello'


# object.__bool__(self)
# 可以通过bool来指定对象转换为布尔值的情况
def __bool__(self):
    return self.age > 17

# __gt__会在对象做大于比较的时候调用，该方法的返回值将会作为比较的结果
# 他需要两个参数，一个self表示当前对象，other表示和当前对象比较的对象
# self > other
def __gt__(self , other):
    return self.age > other.age

# object.__lt__(self, other) 小于 <
# object.__le__(self, other) 小于等于 <=
# object.__eq__(self, other) 等于 ==
# object.__ne__(self, other) 不等于 !=
# object.__gt__(self, other) 大于 >
# object.__ge__(self, other) 大于等于 >= 



```



#### c. 编程规范

| 名称                                                     | 例子                   | 命名方法                                             |
| -------------------------------------------------------- | ---------------------- | ---------------------------------------------------- |
| <font color='green'>类名</font>                          | `MyClass`              | 驼峰式命名方法                                       |
| <font color='blue'>私有成员</font>                       | `_function_A()`        | 单下划线 `_` 开头                                    |
| <font color='blue'>不希望在派生类中进行访问的成员</font> | `__private_function()` | 双下划线 `__` 开头                                   |
|                                                          |                        |                                                      |
| <font color='orange'>普通函数</font>                     | `function_of_action()` | `小写单词 + _ ` 的组合                               |
| 辅助函数 （自己使用的函数，只在本模块内使用的函数）      | `_my_function()`       | `_函数名`                                            |
|                                                          |                        |                                                      |
| 常量，类常量，枚举                                       | `MEANS_OF_LENGTH`      | `大写单词 + _ ` 的组合                               |
| 变量名                                                   | `small_variable`       | `小写单词 + _ ` 的组合；重名时，在后面＋单下划线 `_` |



如果有`__future__`模块，必须放在第一行`import`

`__all__`，`__version__`放在其它`import`之前

**按内置模块，标准库，第三方库，自定义模块的顺序`import`，并且每组之间＋空格。**





#### d. 编程陷阱

1. **<font color='red'>浮点数的相等性比较:</font> **浮点数不要使用`==`判断相等

   ```python
   >>> print(0.7-0.4)     # 错误1
   0.29999999999999993
   >>> print(0.7-0.6)     # 错误2
   0.09999999999999998
   ```

   ```python
   >>> import math
   >>> math.isclose(0.9999999999998, 1)                # 正确比较方法
   True
   >>> math.isclose(0.9999999999998, 1, rel_tol=1e-5)  # 指定相对误差
   True
   ```

   

2. **<font color='red'>高精度表示实数:</font> **

   ```python
   >>> from decimal import Decimal
   >>> print(Decimal(3.14))     # 错误初始化, 浮点数本身就不精确
   3.140000000000000124344978758017532527446746826171875
   >>> print(Decimal('3.14'))   # 正确初始化, 用字符串初始化
   3.14
   
   # 设置精度范围
   >>> from decimal import getcontext
   >>> print(Decimal(1) / Decimal(7))
   0.1428571428571428571428571429
   >>> getcontext().prec = 6
   >>> print(Decimal(1) / Decimal(7))
   0.142857
   ```

   

3. **<font color='red'>256个神话</font> **

   ```python
   >>> a = 256
   >>> b = 256
   >>> a is b
   True
   >>> a = 257
   >>> b = 257
   >>> a is b
   False
   ```

   ```python
   >>> data = [300, 200, 300]
   >>> data[0] is data[1]
   False
   >>> data[0] is data[2]
   True
   ```

   

4. asd

5. as





#### e. 模块，库和包

> 一般来说，**<font color="red">模块</font>**指一个包含若干函数定义，类定义或常量的**.py文件**；
>
> **<font color="red">库或包</font>**指包含若干模块并且其中一个文件名为`__init__.py`的**文件夹**。
>
> 
>
> 对于包含完整功能代码的单个模块，叫做库也可以，例如标准库re和re模块这两种说法都可以。
>
> 但一般不把库叫做模块，例如thinter库包含若干模块文件，此时不能称之为thinter模块



**如果模块中定义了\_\_all\_\_列表，则使用`from moudle import *`导入语句导入时只导入该列表中声明的对象**



在每一个模块内部都有一个`__name__`属性，通过这个属性可以获取到模块的名字。`__name__`属性值为`__main__`的模块是主模块，一个程序中只会有一个主模块。



`__init__.py`文件做一些导入设置的便捷化设置：

```python
__all__ = ['test11', 'test12']               # 指定 * 所代表的的所有模块
# 默认要导入的模块等
from mypackage.subpackage_1 import test11
```





#### f. 常用os模块函数

```python
import os

# os.listdir() 获取指定目录的目录结构
# 需要一个路径作为参数，会获取到该路径下的目录结构，默认路径为 . 当前目录
# 该方法会返回一个列表，目录中的每一个文件（夹）的名字都是列表中的一个元素
r = os.listdir()

# os.getcwd() 获取当前所在的目录
r = os.getcwd()

# os.chdir() 切换当前所在的目录 作用相当于 cd
os.chdir('c:/')
r = os.getcwd()

# 创建目录
os.mkdir("aaa") # 在当前目录下创建一个名字为 aaa 的目录

# 删除目录
os.rmdir('abc')

# open('aa.txt','w')
# 删除文件
os.remove('aa.txt')

# os.rename('旧名字','新名字') 可以对一个文件进行重命名，也可以用来移动一个文件
os.rename('bb.txt','c:/users/lilichao/desktop/bb.txt') # 还可以用来移动文件
```

