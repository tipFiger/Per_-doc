## 文件读写操作
1. 有一个jsonline格式的文件file.txt大小约为10K(读取文件)
```
def get_lines():
    with open('file.txt', 'rb') as f:
	return f.readlines()

if __name__ == '__main__':
    for e in get_lines():
	process(e) # 处理每一行数据
```

2. 现在要处理一个大小为10G的文件，但是内存只有4G，如果在只修改get_lines 函数而其他代码保持不变的情况下，应该如何实现？需要考虑的问题都有那些？
```
def get_lines():
    with open('file.txt','rb') as f:
        for i in f:
            yield i


from mmap import mmap


def get_lines(fp):
    with open(fp,"r+") as f:
        m = mmap(f.fileno(), 0)
        tmp = 0
        for i, char in enumerate(m):
            if char==b"\n":
                yield m[tmp:i+1].decode()
                tmp = i+1

if __name__=="__main__":
    for i in get_lines("fp_some_huge_file"):
        print(i)

```
> 要考虑的问题有：内存只有4G无法一次性读入10G文件，需要分批读入分批读入数据要记录每次读入数据的位置。分批每次读取数据的大小，太小会在读取操作花费过多时间。

3、补充缺失的代码
```
def print_directory_contents(sPath):
"""
这个函数接收文件夹的名称作为输入参数
返回该文件夹中文件的路径
以及其包含文件夹中文件的路径
"""
import os
for s_child in os.listdir(s_path):
    s_child_path = os.path.join(s_path, s_child)
    if os.path.isdir(s_child_path):
        print_directory_contents(s_child_path)
    else:
        print(s_child_path)

```

## 模块与包
4、控制台输入日期，判断这一天是这一年的第几天？
```
import datetime
def dayofyear():
    year = int(input("请输入年份："))
    month = int(input("请输入月份："))
    day = int(input("请输入天："))
    date1 = datetime.date(year=year,month=month,day=day)
    date2 = datetime.date(year=year,month=1,day=1)   #定义起始日期
    return (date1-date2).days + 1
```

5、打乱有序list？
```
import random
alist = [1,2,3,4,5]
random.shuffle(alist)#调用shuffle()方法
print(alist)
```

## 数据类型
6、给现有字典按value值排序？
```
d= {'a':24,'g':52,'i':12,'k':33}

sorted(d.items(),key=lambda x:x[1])
```
7、字典推导式
```
d = {key:value for (key:value} in iterable}
```
8、反转字符串
```
print("aStr"[::-1])  #反向切片
```
9、将字符串 "k:1 |k1:2|k2:3|k3:4"，处理成字典 {k:1,k1:2,...}，字符串转字典
```
str1 = "k:1 |k1:2|k2:3|k3:4"
def strtodict(str1):
    dict1 = {}
    for items in str1.split('|'):
    key,value = iterms.split(':')
        dict1[key] = value
    return dict1
```
10、按字典元素的值排序：请按alist中元素的age由大到小排序
```
alist = [{'name':'a','age':20},{'name':'b','age':30},{'name':'c','age':25}]
def sort_by_age(list1):
    return sorted(alist,key=lambda x:x['age'],reverse=True)
```
11、越界与越界切片：下面代码的输出结果将是什么？
```
list = ['a','b','c','d','e']
print(list[10:])

代码将输出[],不会产生IndexError错误，就像所期望的那样，尝试用超出成员的个数的index来获取某个列表的成员。例如，尝试获取list[10]和之后的成员，会导致IndexError。然而，尝试获取列表的切片，开始的index超过了成员个数不会产生IndexError，而是仅仅返回一个空列表。这成为特别让人恶心的疑难杂症，因为运行的时候没有错误产生，导致Bug很难被追踪到。
```
12、列表生成式，产生一个公差为11的等差数列
```
print([x*11 for x in range(10)])
```
13、集合去重的使用：给定两个列表，怎么找出他们相同的元素和不同的元素？
```
list1 = [1,2,3]
list2 = [3,4,5]
set1 = set(list1)
set2 = set(list2)
print(set1 & set2)
print(set1 ^ set2)
```
14、Python去重的方法：请写出一段python代码实现删除list里面的重复元素？
```
集合去重：
l1 = ['b','c','d','c','a','a']
l2 = list(set(l1))
print(l2)

用list类的sort方法:
l1 = ['b','c','d','c','a','a']
l2 = list(set(l1))
l2.sort(key=l1.index)
print(l2)
也可以这样写:
l1 = ['b','c','d','c','a','a']
l2 = sorted(set(l1),key=l1.index)
print(l2)

也可以用遍历：
l1 = ['b','c','d','c','a','a']
l2 = []
for i in l1:
    if not i in l2:
        l2.append(i)
print(l2)
```
## 企业面试题
15、python新式类和经典类的区别？
```
a. 在python里凡是继承了object的类，都是新式类
b. Python3里只有新式类 
c. Python2里面继承object的是新式类，没有写父类的是经典类 
d. 经典类目前在Python里基本没有应用
```
16、python内置的数据类型？
```
a. 整型 int、 长整型 long、浮点型 float、 复数 complex 
b. 字符串 str、 列表list、 元祖tuple 
c. 字典 dict 、 集合 set
d. 布尔型 Boolean
```
17、python单例设计模式
```
第一种方法:使用装饰器
def singleton(cls):
    instances = {}
    def wrapper(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return wrapper
@singleton
class Foo(object):
    pass
foo1 = Foo()
foo2 = Foo()
print foo1 is foo2 #True

第二种方法：使用基类 New 是真正创建实例对象的方法，所以重写基类的new 方法，以此保证创建对象的时候只生成一个实例
class Singleton(object):
    def __new__(cls,*args,**kwargs):
        if not hasattr(cls,'_instance'):
            cls._instance = super(Singleton,cls).__new__(cls,*args,**kwargs)
        return cls._instance
    
class Foo(Singleton):
    pass

foo1 = Foo()
foo2 = Foo()

print foo1 is foo2 #True

第三种方法：元类，元类是用于创建类对象的类，类对象创建实例对象时一定要调用call方法，因此在调用call时候保证始终只创建一个实例即可，type是python的元类
class Singleton(type):
    def __call__(cls,*args,**kwargs):
        if not hasattr(cls,'_instance'):
            cls._instance = super(Singleton,cls).__call__(*args,**kwargs)
        return cls._instance
class Foo(object):
    __metaclass__ = Singleton

foo1 = Foo()
foo2 = Foo()
print foo1 is foo2 #True
```
18、反转整数：反转一个整数，例如-123 --> -321
```
class Solution(object):
    def reverse(self,x):
        if -10<x<10:
            return x
        str_x = str(x)
        if str_x[0] !="-":
            str_x = str_x[::-1]
            x = int(str_x)
        else:
            str_x = str_x[1:][::-1]
            x = int(str_x)
            x = -x
        return x if -2147483648<x<2147483647 else 0
if __name__ == '__main__':
    s = Solution()
    reverse_int = s.reverse(-120)
    print(reverse_int)
```
19、设计实现遍历目录与子目录，抓取.pyc文件
```
第一种方法：
import os

def get_files(dir,suffix):
    res = []
    for root,dirs,files in os.walk(dir):
        for filename in files:
            name,suf = os.path.splitext(filename)
            if suf == suffix:
                res.append(os.path.join(root,filename))

    print(res)

get_files("./",'.pyc')

第二种方法：
import os

def pick(obj):
    try:
        if obj.[-4:] == ".pyc":
            print(obj)
        except:
            return None
    
def scan_path(ph):
    file_list = os.listdir(ph)
    for obj in file_list:
        if os.path.isfile(obj):
    pick(obj)
        elif os.path.isdir(obj):
            scan_path(obj)
    
if __name__=='__main__':
    path = input('输入目录')
    scan_path(path)

第三种方法
from glob import iglob


def func(fp, postfix):
    for i in iglob(f"{fp}/**/*{postfix}", recursive=True):
        print(i)

if __name__ == "__main__":
    postfix = ".pyc"
    func("K:\Python_script", postfix)

> 深度遍历、广度遍历、递归遍历等
```
20、一行代码实现1-100求和
```
print(sum(range(1,101)))
```
21、Python-遍历列表时删除元素的正确做法
```
- 反向遍历
num_list = [1, 2, 3, 4, 5]
print(num_list)

for i in range(len(num_list)-1, -1, -1):
    if num_list[i] == 2:
        num_list.pop(i)
    else:
        print(num_list[i])

print(num_list)
- 遍历拷贝的list，操作原始的list
num_list = [1, 2, 3, 4, 5]
print(num_list)

for item in num_list[:]:
    if item == 2:
        num_list.remove(item)
    else:
        print(item)

print(num_list)
- while循环
num_list = [1, 2, 3, 4, 5]
print(num_list)

i = 0
while i < len(num_list):
    if num_list[i] == 2:
        num_list.pop(i)
        i -= 1
    else:
        print(num_list[i])

    i += 1
print(num_list)

```
22、Python的可变类型和不可变类型
```
python的每个对象都分为可变和不可变，主要的核心类型中，数字、字符串、元组是不可变的，列表、字典是可变的。
对不可变类型的变量重新赋值，实际上是重新创建一个不可变类型的对象，并将原来的变量重新指向新创建的对象（如果没有其他变量引用原有对象的话（即引用计数为0），原有对象就会被回收）。
```
23、python中is和==的区别
```
Python中对象包含的三个基本要素，分别是：id(身份标识)、type(数据类型)和value(值)。
is和==都是对对象进行比较判断作用的，但对对象比较判断的内容并不相同：
- is是同一性运算符，这个运算符比较判断的是对象间的唯一身份标识，也就是id是否相同
- ==是比较操作符，用来比较判断两个对象的value(值)是否相等
> 只有数值型和字符串型，并且在通用对象池中的情况下，a is b才为True，否则当a和b是int，str，tuple，list，dict或set型时，a is b均为False。
```
24、求出列表所有奇数并构造新列表
```
a = [1,2,3,4,5,6,7,8,9]
- filter方法
def fn(a):
    return a%2==1
nowlist = filter(fn,a)
nowlist = [i for i in newlist]
print(newlist)
- 列表推导式
res = [i for i in a if i%2==1]
- 简单判断与append新列表
konglist=[]
for i in a:
    if i%2==1:
        konglist.append(i)
print(konglist)
```




















