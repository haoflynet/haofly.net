---
title: "Python教程"
date: 2016-12-20 12:05:30
updated: 2017-01-05 14:44:00
categories: python
---
# Python

[Python Developer’s Guide](http://cpython-devguide.readthedocs.io/en/latest/#python-developer-s-guide)

##  安装方法

```shell
# for CentOS
yum groupinstall -y 'development tools'
yum install -y zlib-dev openssl-devel sqlite-devel bzip2-devel xz-libs
# for Ubuntu
apt-get install -y libssl-dev

wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tar.xz
xz -d Python-3.5.2.tar.xz
tar -xvf Python-3.5.2.tar
cd Python-3.5.2
./configure && make && make altinstall
cd
wget https://bootstrap.pypa.io/get-pip.py
python3.5 get-pip.py

# Python3.5版本默认有安装pip的，如果没有，那么就酱紫
wget https://bootstrap.pypa.io/get-pip.py
python3.3 get-pip.py
```

## 基本语法

#### 列表

```python
li[::-1]	# 逆序列表
li[::2]		# 列表里面的奇数位，最后那个2表示不长，前面::表示整个数组
li[1::2]	# 列表里面的偶数位
del(list['下标'])	# 删除指定位置的元素，要注意每次都会更新，比如del(list[0], list[0])就是删除前面两个元素
li.index(min(li))	# 列表最小值，返回位置，最大用max
li.index(obj)		# 获取指定值在数组里面的下标
li.remove(obj)	# 删除指定元素

# while/for循环都能用else，我擦嘞
while false:
	sdaghoahg
else:
	aosdhgo

for x in reversed(list) # 列表的反向迭代
list(set(list))  # 列表去重，不过会乱序
li.append([1,2])	# 添加一个元素
li.extend([1,2])	# 添加多个元素
```
#### 字典

	# 字典遍历
	for key in dict:
		print(key, dict[key])
	for key, vlaue in dict.items():
		print(key, value)
		
	# 特殊的key
	li = {
		'a': 'b',
		None: 'c',
		'': ''
	}
	
	if 'a' in dict		# 判断key是否存在
	dict.get('a', 'b')	# 如果不存在那么给一个默认值
	dict.keys()			# 获取所有的key
	
	# 表达式解析
	a = {'x': 1, 'y': 2}
	globals().update(a)
	print(x, y)
	
	# 漂亮地打印json数据
	print(json.dump(sdata, indent=2))
	
	# 有序字典(占用的内存是普通字典的两倍)
	from collections import OrderedDict
	d = OrderedDict()
	
	# 字典组成的列表排序
	通过某个关键字来排序
	rows = [{}, {}]  # 假设这是一个由字典组成的列表
	from operation import itemgetter
	result = sorted(rows, key=itemgetter('onekey'))
	通过某个值排序，使用zip()函数，它会先将键和值翻转过来，需要注意的是zip()函数是一个只能访问一次的迭代器
	prices = {'A': 1, 'B': 2, 'C': 3}
	min_price = min(zip(prices.values(), prices.keys()))  # 获取value最小的
	prices_sorted = sorted(zip(prices.values(), prices.keys()))
	
	# 字典合并（ChainMap只是将两个字典在逻辑上变为一个，在它上面的修改只会影响第一个字典a)
	from collections import ChainMap
	c = ChainMap(a, b)
#### 类/函数

```python
# 几个特殊的方法
x.__class__.__name__  # 获取实例的类名

# 继承相关
ChildClass.mro()	# 按顺序打印类当前类的继承顺序，多继承也会有顺序
super().func()	# 调用父类的方法
super(ChildClass, self).func()	# 2里面调用父类的方法

# 通过字符串调用方法
getattr(foo, 'bar')()

# lambda表达式，相当于一个简单的函数，例如:
g = lambda x: x*2
g(3) # 输出6
```
#### 类型检查(Type Hint)

从3.5开始，Python提供了类型检查功能，当然类型检查仅仅用于检查，并不会对程序的执行有任何的影响，但是配合IDE有代码提示过后，一切都变得方便了起来

```python
def func(a: int) -> int	# 这表示该函数的参数a要求是整型，返回值是整型号

# 返回组合类型
from typing import List, Tuple
Result = Tuple[Tuple[int, int], str]
def foo(strings: str, lines: List[str], line_number: int) -> Result:	# 这样子定义组合的返回类型
```

#### 数字

```python
# 随机数
import random
random.choice(list)  # 随机选择一个
random.sample(list, n) # 随机选择n个
random.shuffle(list)  # 打乱选择择后的顺序
random.randint(0, 10)  # 0到10的随机整数
random.random()   # 0到1得浮点数
random.getrandbits(100) # 获取100位随机数
random.uniform(begin, end)  # 生成0到10的随机浮点数

# 四舍五入
round(1.23, 1)   # 第二个参数表示保留几位小数
format(x, '0.2f')  # 保留两位小数

# 除法
10%3 = 3  # 整除

# 生成一组数
range(2)	// 生成[0, 1]
range(1, 2) // 生成[1]
range(0, 6, 2)	// 生成(0, 2, 4)
xrange用法与range一样，只是返回的不是一个生成好的列表，而是一个生成器，所以性能更好
```
#### 文件目录

```python
os.mkdir # 新建目录、文件
os.makedires('a/b')  # 创建多级目录
os.path.join(path, filename)	# 合并成全路径，用这个函数不用管路径用/还是\，也不用管最后有没有/
os.remove(filename)  # 删除单个文件
os.rmdir(dirname)   # 删除空目录

# 用其他库
import shutil
shutil.rmtree('mydir')   # 删除非空目录
shutil.copy(originame, tmpname)  # 复制单个文件
shutil.copytree(root_of_tree, desetination_dir, True) # 复制目录树
os.listdir('dirname')  # 显示一个目录下的所有文件和文件夹的名称
os.path.isdir(filename)  # 是否是目录
os.path.isfile(filename) # 是否是文件
os.path.islink  # 是否是链接
os.path.getsize(filename) # 获取文件大小
os.path.basename(fname)  # 从完整路径获取其路径
os.path.dirname(fname) # 从完整路径获取其名称
os.getcwd()   # 获取当前目录
os.chdir(newdir)  # 切换目录
os.path.exists(name)  # 判断目录是否存在
os.rename(original_name, new_name) # 修改文件名称

# 文件打开与关闭
fp = open('a.txt', 'w')
fp.close()
或者
with open('a.txt', 'w') as fp:   # 这种方式不需要close
	print('it's...', file=fp)
# 或者
import codecs
fp = codecs.open(filename, 'w', 'utf-8') # 这种方式可以解决很多编码问题
fp.write(string)
fp.close()

# 文件操作的标识
w: 只读
r: 只写
r+: 可用于读写，但是如果打开不读，直接就写，可能会覆盖，因为一打开的时候文件指针是在文件开头的

# 读取文件
fp.readline()  # 从文件读取一行数据
for each_line in fp:   # 可迭代获取每一行数据
	print(each_line)
fp.read()  # 读取所有数据
fp.readlines()  # 读取所有的行，返回一个列表，需要注意的是这个只会读取一次，读取第二次的时候就会返回空了
codecs.open(path, 'r', 'utf-8').read().splitlines() # 获取所有的数据，并使用splitlines()分隔行，这样在每行的后面就不会出现换行符了
print('string', file=fp)  # 直接写入文件

# 清空文件内容
fp.seek(0, 0) # 这一句可以保证之前是否读取，都能清空
fp.truncate() # 清空语句

# 文件压缩zip库，这个库是纯Python写的，不是用C，解压速度比较慢，而且不支持压缩的时候加密，如果要想在压缩的时候加密，可以使用网上现成的一个库https://github.com/smihica/pyminizip，依赖于zlib库，但是如果要依赖zlib库为什么不直接用python调用zlib库的代码呢
          
# 计算文件的md5值
import hashlib
hashlib.md5(open('filename.exe', 'rb').read()).hexdigest()
```
#### 异常处理

	try:
		raise RuntimeError('错误原因')
	except Exception as e:
		print(e)或者print(str(e))或者print(unicode(e))
		# 上面是打印基本的错误信息，如果要打印错误信息／文件名／错误行数，那么可以这样子:
		exc_type, exc_obj, exc_tb = sys.exc_info()
		fname = os.path.split(exc_tb.tb_frame.f_code.co_filename)[1]
		print(exc_type, fname, exc_tb.tb_lineno)
#### 系统相关

```python
# 执行系统命令
import subprocess
subprocess.check_output(command, shell=True)	# 这个命令就是为了代替os.system,os.spawn,os.popen,popen2,commands的

# 接收输入
a = input('Input: ')
```
#### 包

```python
在目录下面加上__init__.py就变成了一个包，import包内部的模块使用'.'来分割，__init__.py里面可以定义一些全局变量，__all__指定了此包被import *的时候，哪些模块会被import进来
# 带点号，表示当前目录下的模块，如
from . import client
# 可以在一个模块的根目录下得__init__.py定义一些基本的东西，比如加载一些模块呀，设置一些全局变量(__author__这样的东西)啥的

package.__version__  ＃获取package的版本

# 从github直接安装包的方法http://stackoverflow.com/questions/8247605/configuring-so-that-pip-install-can-work-from-github?answertab=active#tab-top
# 需要注意的是github上的库要有个固定的目录格式，还要有个setup.py文件，才能直接使用如下的命令，
pip install git+git@github.com:lynzt/python_people_names.git
    
# 将python包打包成debian包，可以用https://github.com/spotify/dh-virtualenv

# 使用豆瓣的PIP源，例如
sudo pip3 install scrapy -i https://pypi.douban.com/simple
```
#### 名字空间

程序在使用一个名字时，会从当前名字空间开始搜索，顺序则是LEGB:

```python
# locals: 函数内部的名字空间，一般包括函数的局部变量以及形参
# enclosing function: 潜逃函数中外部函数的名字空间
# globals: 当前的模块空间，类似于全局变量
# __builtins__: 内置模块空间
```

## 魔术/自省方法

```python
# __dir__
# 实现动态属性
class AttrDict(dict):
    def __getattr__(self, item):	# 是为了直接用点号可以访问动态属性
        return self[item]

    def __dir__(self):				# 是为了能自动完成，和用__dir__能够查找到该动态属性
        return super().__dir__() + [str(k) for k in self.keys()]
    
# __new__和__init__
## __new__用来创建类并返回这个类的实例，而__init__只是将传入的参数来初始化实例。__new__在创建一个实例的过程中必定会被调用，但__init__不一定，比如通过pickle.load方式反序列化一个实例就不会调用__init__

# __getattr__(self, name)
## 定义了试图访问一个不存在的属性时的行为，重载该方法可以实现捕获错误拼写然后进行重定向, 或者对一些废弃的属性进行警告

# __setattr__(self, name, value)
## 定义了对属性进行赋值和修改操作时的行为

# __delattr__(self, name)
## 定义删除属性时的行为

# __getattribute__(self, name)
## 定时访问属性时的行为，无论属性存不存在

# __get__(self, instance, owner)
## 描述起对象，instance是拥有者类的实例，参数owner是拥有者类本身。__get__在其拥有者对其读值的时候调用。

# __set__(self, instance, value)
## 在其拥有者对其进行修改值的时候调用。

# __delete(self, instance)
## 在其拥有者对其进行删除的时候调用。

# __getitem__(self, key)
## 当执行self[key]的时候，调用的就是该方法

# __setitem__(self, key, value)
## 当执行self[key] = value时调用

# __delitem__(self, key)
## 执行del self[key]时调用

# __iter__(self)
## 返回一个迭代器，当执行for x in container或者iter(container)时调用

# __contains__(self, item)
## 执行item in container或者item not in container时调用

# __missing__(slef, key)
## 定义key不在容器中的触发行为

# with关键字的几个魔术方法，用with可以实现在函数前后执行某些语句
## __enter__(self): 会返回一个值，并赋值给as关键词之后的变量
## __exit__(self, exception_type, exception_value, traceback): 定义代码段结束后的一些操作，如果返回True，那么下面的异常会被屏蔽，如果返回None，那么会抛出
class Count():
    def __enter__(self):
        print(time())
    def __exit__(self):
        print(time())
with Count():
    func()		# 这样在函数的开始与结束都能打印出时间了，这只是一个简单例子而已

# 用于序列化的魔术方法
## __getinitargs__(self): 
## __getnewargs__(self)
## __getstate__(self)
## __setstate__(self, state)
## __reduce__(self)
## __reduce_ex__(self)

# 运算符相关的魔术方法，相当于重载运算符
## __cmp__(self, other)
## __eq__(self, other)
## __ne__(self, other)
## __lt__(self, other)
## __ge__(self, other)
## __pos__(self)
## __neg__(self)
## __invert__(self)
## __abs__(self)
## __round__(self, n)
## __floor__(self)
## __ceil__(self)
## __trunc__(self)
## __add__(self, other)
## __sub__(self, other)
## __mul__(self, other)
## __floordiv__(self, other)
## __div__(self, other)
## __truediv__(self, other)
## __mod__(self, other)
## __divmod__(self, other)
## __pow__(self, other)
## __lshift__(self, other)
## __rshift__(self, other)
## __and__(self, other)
## __or__(self, other)
## __xor__(self, other)

# 类型转换相关魔术方法
## __int__(self): 实现了类型转化为int的行为
## __long__(self)
## __float__(self)
## __complex__(self)
## __oct__(self)
## __hex__(self)
## __index__(self)
## __str__(self)
## __sizeof__(self)
```

## 装饰器

#### property(描述符)

可将类的方法变为类的属性，比如之前用`person.name()`，现在可以直接`person.name`了

## 标准库

#### argparse(命令行程序)

```python
import argparse

parser = argparse.ArgumentParser(description='命令介绍')
parser.add_argument('-d', help='添加一个参数')
parser.add_argument('-f', help='再添加一个参数')
args = parser.parse_args()

args.d	# 获取名为d的参数
```

#### cProfile/Profile: 函数运行时间度量

```python
import cProfile
from time_profile import *
 
cProfile.run("timeit_profile()")
```

#### functools

```python
# 偏函数partial: 用于固定函数中的某几个参数形成新的函数
import functools

def add(a, b):
    return a + b

plus3 = functools.partial(add, 3)
plus3(4)	# 输出7

# update_wrapper：主要用在装饰器函数中，使得装饰器返回函数反射得到的是包装函数定义而不是原始函数定义

# wraps：调用函数装饰器partial(update_wrapper, wrapped=wrapped, assigned=assigned, updated=updated)的简写

# reduce：等同于内置函数reduce()

# cmp_to_key：将一个函数转换为比较函数
# total_ordering
```

#### logging日志模块

日志格式

```tex
%(name)s Logger的名字
%(levelno)s 数字形式的日志级别
%(levelname)s 文本形式的日志级别
%(pathname)s 调用日志输出函数的模块的完整路径名，可能没有
%(filename)s 调用日志输出函数的模块的文件名
%(module)s 调用日志输出函数的模块名|
%(funcName)s 调用日志输出函数的函数名|
%(lineno)d 调用日志输出函数的语句所在的代码行
%(created)f 当前时间，用UNIX标准的表示时间的浮点数表示|
%(relativeCreated)d 输出日志信息时的，自Logger创建以来的毫秒数|
%(asctime)s 字符串形式的当前时间。默认格式是“2003-07-08 16:49:45,896”。逗号后面的是毫秒
%(thread)d 线程ID。可能没有
%(threadName)s 线程名。可能没有
%(process)d 进程ID。可能没有
%(message)s 用户输出的消息
```

实例:

```python
# 最复杂的使用
import logging

logger = logging.getLogger('AppName')	# 初始化日志处理器实例，可以用AppName来区分不同的模块
formatter = logging.Formatter('%(asctime)s %(levelname)-8s: %(message)s')	# 定义日志格式

file_handler = logging.FileHandler("test.log")		# 文件日志
console_handler = logging.StreamHandler(sys.stdout)	# 控制台日志
file_handler.setFormatter(formatter)				# 设置日志格式

logger.addHandler(file_handler)			# 为logger添加的日志处理器
logger.removeHandler(file_handler)		# 移除日志处理器

logger.setLevel(logging.INFO)		# 设置日志输出最低级别

logger.debug('this is debug info')
logger.info('this is information')
logger.warn('this is warning message')
logger.error('this is error message')	# 与logger.exception()相同
logger.fatal('this is fatal message, it is same as logger.critical')
logger.critical('this is critical message')

logger.error('%s service is down', 'own')	# 格式化输出日志

# 比较简单的使用
logging.basicConfig(filename='test.log', level=logging.DEBUG)	# 这样接口
```

#### smtplib: 用于发送邮件

```python
import smtplib
from email import encoders
from email.mime.text import MIMEText
from email.header import Header

# 腾讯SMTP
mail_host = '发件服务器'
mail_user = '发件人'
mail_pass = '收件人'

sender = '发件人'
receivers = ['收件人']

message = MIMEText('这是邮件内容', 'plain', 'utf-8')
message['From'] = "\"%s\" <%s>" % (Header('发件人', 'utf-8'), Header('发件人', 'utf-8'))
message['To'] = Header('to', 'utf-8')
message['Subject'] = Header('邮件主题', 'utf-8')

smtpObj = smtplib.SMTP()
smtpObj.connect(mail_host)
smtpObj.login(mail_user, mail_pass)
smtpObj.sendmail(sender, receivers, message.as_string())
print('邮件发送成功')
```

#### timeit: 时间度量

```python
import timeit
timeit.Timer('x=range(1000)').timeit()	# 可以直接得到代码的执行时间
timeit.Timer('sum(x)', 'x = (i for i in range(1000)').timeit() # 参数
```



- **collections**：[参考](http://my.oschina.net/leejun2005/blog/222236)，提供额外的数据类型
  - **namedtuple()**：生成可以使用名字来访问元素内容的tuple子类
  - **deque**：双端队列
  - **Counter**：计数器，可用于统计字符串中字符数量
  - **OrderedDict**：有序字典
  - **defaultdict**：带有默认值的字典，这样访问不存在的dict就不会出错了

- **heapq**：Python里面的最小堆

- **itertools**：[参考](http://www.wklken.me/posts/2013/08/20/python-extra-itertools.html#itertoolscountstart0-step1)
  - **count(start=0, step=1)**：创建连续整数
  - **cycle(iterable)**：创建一个迭代器，可以反复循环的，此时用在for里面如果不加终止条件会无限循环
  - **repeat(object, times)**：创建一个迭代器，根据指定数量，生成重复的对象
  - **chain(\*iterables)**：将多个迭代器作为参数, 但只返回单个迭代器, 它产生所有参数迭代器的内容, 就好像他们是来自于一个单一的序列.
  - **compress(data, selectors)**：提供一个选择列表，对原始数据进行筛选
  - **dropwhile(predicate, iterable)**：创建一个迭代器，只要函数predicate(item)为True，就丢弃iterable中的项，如果predicate返回False，就会生成iterable中的项和所有后续项
  - **groupby(iterable, key)**：返回一个产生按照key进行分组后的值集合的迭代器.
  - **ifilter(predicate, iterable)**：与dropwhile相反
  - **ifilterfalce(predicate, iterable)**：与上面这个相反
  - **islice(iterable, stop)**：返回的迭代器是返回了输入迭代器根据索引来选取的项
  - **imap(function, \*iterables)**：返回一个迭代器, 它是调用了一个其值在输入迭代器上的函数, 返回结果. 它类似于内置函数 map() , 只是前者在任意输入迭代器结束后就停止(而不是插入None值来补全所有的输入).
  - **starmap(function, iterable)**：创建一个迭代器，生成值func(*item),其中item来自iterable，只有当iterable生成的项适用于这种调用函数的方式时，此函数才有效。
  - **tee(iterable[, n=2])**：从iterable创建n个独立的迭代器，创建的迭代器以n元组的形式返回，n的默认值为2
  - **takewhile(predicate, iterable)**：与takewhile相反
  - **izip(\*iterables)**：返回一个合并了多个迭代器为一个元组的迭代器. 它类似于内置函数zip(), 只是它返回的是一个迭代器而不是一个列表
  - **izip_longest(\*iterables[, fillvalue])**：与izip()相同，但是迭代过程会持续到所有输入迭代变量iter1,iter2等都耗尽为止，如果没有使用fillvalue关键字参数指定不同的值，则使用None来填充已经使用的迭代变量的值。
  - **product(\*iterables[, repeat])**：笛卡尔积
  - **permutations(iterable[, r])**：排列
  - **combinations(ierable, r)**：创建一个迭代器，返回iterable中所有长度为r的子序列，返回的子序列中的项按输入iterable中的顺序排序 (不带重复)
  - **combinations_with_replacement(iterable, r)**：创建一个迭代器，返回iterable中所有长度为r的子序列，返回的子序列中的项按输入iterable中的顺序排序 (带重复)

- **SocketServer**：[参考](http://blog.marchtea.com/archives/60)两种服务模型：ThreadingMinxln(有新请求时，创建一个新的进程)、ForkingMinln(有新请求时，创建一个新的线程)
  - **TCPServer**
  - **UDPServer**
  - **UnixStreamServer**
  - **UnixDatagramServer**

- **weakref**：弱引用，与常规的引用相对，这种引用在对象只剩下一个弱引用的时候，就可能会被回收，多见于类的嵌套定义防止错误回收，weakref的失效依赖于对象实际销毁。gc销毁的时机未知，引用计数的销毁则是可控的，比如(del)，可以减少异常的发生。使用方式例如:

  ```python
  class Parent(object):
      def __init__(self):
          self.children = [Child(self)]

  class Child(object):
      def __init__(self, parent):
          self.parent = weakref.proxy(parent)
  ```


## 语言本身

#### 性能分析与优化

- 使用timeit
- 使用cProfile，精确到函数
- 使用vprof，可视化
- line_profiler，精确到行

#### Python设计模式



- **Python设计模式**：https://github.com/faif/python-patterns
- **SimpleHTTPServer**：最简单的web服务，十分方便，可以方便局域网其它设备访问本机文件目录`python -m SimpleHTTPServer 8000`
- **静态方法(@staticmethod)**：跟类有管，和实例无关.静态方法没有默认的第一个参数.
- **类方法(@classmethod)**：跟类有管，和实例无关.默认的第一个参数是类本身，而不像其它函数那样是对象本身
- **元类**：[参考](http://blog.jobbole.com/21351/) [参考2](http://www.jianshu.com/p/d643d6f0ec82) [参考3](http://www.cnblogs.com/russellluo/p/3409602.html)元类的主要用途是创建API，比如Django的Model里面，record.field，这会返回该字段的值，而不是model定义里面的Field对象.最简单的例子：

  	# 给类添加作者信息
  	class AuthorTag(type):
  	    def __new__(cls, name, bases, dict):
  	        dict['__author__'] = 'RussellLuo'
  	        return super(AuthorTag, cls).__new__(cls, name, bases, dict)
  	
  	class MyBlog:
  	    __metaclass__ = AuthorTag
  	
  	class MyGitHub:
  	    __metaclass__ = AuthorTag
- **自省**：[参考](https://www.ibm.com/developerworks/cn/linux/l-pymeta/)自省对象能够描述自己：实例属于哪个类？类有哪些祖先？对象可以用哪些方法和属性？自省让处理对象的函数或方法根据传递给函数或方法的对象类型来做决定
- **管理属性**：
  - **\_\_new\_\_**：在构造函数之前，可以决定是否用\_\_init\_\_方法来实例化类，是一个静态方法.
  - **\_\_init\_\_**：负责将类实例化
  - **\_\_del\_\_**：类的析构函数
  - **\_\_call\_\_**：在定义类的时候，实现该函数，这样该类的实例就变成可调用的了，相当于重载了括号运算符.例如，md5那几个库，使用的时候就是md5(...)，但其实它肯定是个类的实例而不是个简单的函数撒.[使用场景](http://qa.helplib.com/523801)例如：

    	class A():
    		def __call__(self, key):
    			print(key)
    	a = A()
    	a('key')	# 打印'key'
- **多态**：Python中的多态，就相当于重载的方法
- **Mixin**：在运行期间动态改变类的基类或类的方法，哦，就是在运行时给改变基类，这样随之所有的子类都改变了
- **迭代器**：[参考](https://github.com/lzjun567/note/blob/master/note/python/iterator_generator.md)对象的类有next和iter方法返回自身
- **生成器**：[参考](https://github.com/lzjun567/note/blob/master/note/python/iterator_generator.md)生成器都是迭代器，使用yield来生成的结果
- **下划线**: 单下划线开头的变量可以理解为不重要的需要抛弃的变量，比如循环中的计数，而如果是一根下划线作为函数，通常用于翻译功能
- **包管理器pip**:

  	# 升级所有的包
  	pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs pip install -U









				    # 跳过可迭代对象的前面几个值，例如for循环里面跳过前面几个
				    from itertools import islice
				    for x in islice(items, 3, None)   # 这里跳过了前面3个
	
				    # 迭代时获取序号
				    for index, val in enumerate(my_list)
	
				    # 多个列表同时迭代，让长度取决于最短的那一个
				    for a, b in zip(x, y) # 这样就不会超出长度
	
				  * 输入与输出  


				        # 在调试某些代码的时候发现print没有输出，这是有可能将print重定向了，这是用sys.stdout.write('')可以实现打印输出到控制台

				    # 将输出重定向到文件
				    fp = open('log', 'w')
				    sys.stdout = fp
	
				  * 系统环境相关  


				        import sys, platform
				    platform.system()  # 当前操作系统
				    platform.release()  # 当前系统版本
				    sys.version     # python版本
				    os.environ['name']  # 获取系统环境变量
				    os.environ['name'] = value  # 设置系统环境变量  


				  * 执行系统命令  


				        import os, sys
				    os.system('命令')
				    os.popen('命令')  # 这个的返回值直接就是输出内容了
	
				    exec：总是返回None
				    eval：会获取到返回值
	
				  * 动态导入模块  


				        __import__(module_name)   # 相当于import
				    __import__(name = module_name, formlist=[a,b]) # 相当于from module_name import a, b


				  * 类、函数、对象  


				        x.__class__  # 获取对象的类名
				    id(x)   # 返回对象标识，即内存地址
				    setattr(my_model, 'name', 'value')  # 给对象动态添加属性
	
				    # 函数参数注释:
				    def fun(参数: '这是注释', 参数: '这是注释') -> 返回释义
	
				  * 序列化：有时候还是挺方便的，比如后面的操作依赖前面的数据，但是前面的数据不能多次快速抓取，就需要将变量直接写到本地  


				        import pickle
				    # 如果序列化出现maximum recursion depth错误，那么可以设置sys.setrecursionlimit(1000)来解决
	
				  * 排列组合  


				        import itertoolrs
				    for i in itertools.product('ABCD', repeat
	
				  * 其他模块：  


				        collections.Counter：找出序列中出现次数最多的元素
				    heqpq：查找最大或最小的几个元素
				    itertoolrs：排列组合
	
				  * 装饰器  


​	

				  * 变量  


				        sys.getsizeof(name)  # 获取变量占用内存的大小

				    # 如果要改变全局变量的值，那么需要用global来声明，如果仅仅是使用该值那么可以不用global声明

				    del name  # 删除一个变量

				    # 弱引用：在不增加引用计数的情况下使用对象的引用，以防止对象错误回收

				  *   

## TroubleShooting
- **CentOS安装pip**  

  	yum install epel-release
  	rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
  	yum install -y python-pip

- **AttributeError: 'EntryPoint' object has no attribute 'resolve'**
  原因是`cryptography`版本过高或过低，需要制定版本，一般是`pip install cryptography==1.2.1`

- **PEP8三目运算符的换行**  
  可以加括号，例如:

  	re = (
  		'a'
  		if 'a' == 'b'
  		else 'c'
  	)


- **ValueError: Attempted relative import in non-package**  

  相对路径问题，所谓的相对路径其实是相对于当前module的路径，而不是当前目录的路径，如果当前目录不是module，那么当前module的name就是`__main__`，所以会出错

## 推荐阅读

[Hidden features of Python](http://stackoverflow.com/questions/101268/hidden-features-of-python)