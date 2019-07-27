# Python编码规范

## 一、命名

|   Type   | Public | Internal |
| ---- | ---- | ---- |
| Modules | lower_with_under | _lower_with_under |
| Packages | lower_with_under |      |
| Classes | CapWords | _CapWords |
| Exceptions | CapWords |      |
| Functions | lower_with_under() | _lower_with_under() |
| Global/Class Constants | CAPS_WITH_UNDER | _CAPS_WITH_UNDER |
| Global/Class Variables | lower_with_under | _lower_with_under |
| Instance Variables | lower_with_under | _lower_with_under (protected) or __lower_with_under (private) |
| Method Names | lower_with_under() | _lower_with_under() (protected) or __lower_with_under() (private) |
| Function/Method Parameters| lower_with_under |      |
| Local Variables | lower_with_under |      |

#### 1. 应该避免的名称

* 除了计数器和迭代器，不使用单字符命名
* 包/模块名中的连字符
* 双下划线开头并结尾的名称（Python保留，如`__init__`)

#### 2.命名约定

* “internal”表示仅模块内可用，或者在类内是保护或私有的
* 用单下划线（_）开头表示模块变量或者函数是protected的（即使用`import * from`时不会包含）
* 用双下划线（__）开头的实例变量或方法表示类内私有
* 将相关的类和顶级函数放在同一个模块中，没有必要像java一样限制一个类一个模块
* 对类名使用大写字母开头的单词（例`CapWords`），但是模块名应该用小写加下划线的方式（例`lower_with_under.py`），尽量不要让模块名和类名一致

## Main

即使是一个打算被用作脚本的文件，也应该是可导入的，并且简单的导入不应该导致这个脚本的主功能（main functionality）被执行，主功能应该放在一个`main()`函数中。

在Python中，`pydoc`以及单元测试要求模块必须是可导入的，代码在执行主程序前应该总是检查if `__name__ == '__main__'`，这样当模块被导入时主程序就不会执行。

```python
def main():
    //...
    
    
if __name__ == '__main__':
    main()
```

## 风格规范

#### 1.分号

不要在行尾加分号，也不要用分号把两条命令放在一行

#### 2.行长度

除非以下两种情况，否则不要超过每行80个字符：

* 长的导入模块语句
* 注释中的URL

不要使用反斜杠连接行

当表达式较长时，可以使用圆括号、中括号和花括号进行隐式连接，如：

```python
if (width == 0 and height == 0 and 
   color == 'red' and emphasis == 'strong')

//字符串拼接
x = ('当字符串太长时，可以用这个方法'
    '拼接字符串')
```

#### 3.括号

```python
//除非进行行连接，否则不要在return和if中使用括号
//但是在元组两边使用括号是可以的

//Good
if foo:
    bar()
while x:
    x = bar()
if x and y:
    bar()
return foo
//元组可以使用括号
for (x,y) in dict.items():
    
//Bad 
if (x):
    bar()
if not(x):
    bar()
return (foo)
```

#### 4.缩进

* 4个空格缩进
* 绝对不能用tab，也不能tab和空格混用
* 对于行连接的情况，要么垂直对齐换行的元素，要么使用4空格的悬挂式缩进（此时第一行不应该有参数）

```python
//God
# 与原始变量对齐
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# 字典中与起始值对齐
foo = {
    long_dictionary_key: value1 +
                         value2,
    ...
}

# 四个空格缩进时第一行不要有参数
foo = longdunction_name(
    var_one,var_two,var_three,
    var_four)

# 字典中也可以进行四个空格缩进
foo = {
    long_dictionary_key:
        long_dictionary_value,
    // ...
}

```

#### 5.空行

* 顶级定义之间空两行，方法定义之间空一行
* 函数或方法内部，如果有需要的话可以空一行

#### 6.空格

* 括号内侧不要有空格
* 逗号，分号，冒号前面不加空格，但应该在它们后面加（除了在行尾）
* 二元操作符两边都应该加上一个空格*，但是算数操作符两边的空格需要自己判断
* 当`=`用于指示关键字参数或默认参数值时，不要在其两侧使用空格

```python
//Good
def complex(real, image=0.0): return magic(r=real, i= imag)

//Bad
def complex(real, image = 0.0): return magic(r = real, i = imag)
```

* 不要用空格来垂直对齐多行间的标记，因为这会成为维护的负担（适用于：，#，=等）：

```python
//Good
foo = 1000  # 注释
long_name = 2 # 注释不需要对齐

//Bad
dictionary = {
    "foo":       1, #没有必要对齐逗号
    "long_name": 2,
}
```

#### 7.Shebang

大部分`*.py`文件不必以`#!`作为文件的开头，根据`PEP-394`，程序的`main`文件应该以`#!/usr/bin/python3`开头。

> 计算机科学中，`Shebang/Hashbang`是一个由井号和叹号构成的字符串行`#!`，出现在文本文件的第一行的前两个字符。文件存在`Shebang`的情况下，类`Unix`操作系统程序载入器会分析`Shebang`后的内容并将这些内容作为解释器指令，将载有`Shebang`的文件路径作为该解释器的参数调用该指令。  
>
> 简单而言，`#!`先用于帮助内核找到`Python`解释器，但是在导入模块时会被忽略，因此只有被直接执行的文件中才有必要加入`Shebang`。

#### 8.注释

##### 8.1文档字符串

`python`有一种独一无二的注释方式：使用文档字符串（包、模块、类或函数里的第一个语句，这些字符串可以通过对象的`__doc__`成员被自动提取，并且被`pydoc`引用。文档字符串惯例是使用三重双引号`"""`。

>  一个文档字符串首先应该是一行以句号，问号或惊叹号结尾的概述。紧接着是一个空行，接下来描述文档字符串剩下的部分。

##### 8.2模块

每个文件根据项目使用的许可选择合适的许可样板。

##### 8.3函数和方法

一个函数必须要用文档字符串，除非：

* 外部不可见
* 非常短小
* 简单明了

```python
def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
    """Fetches rows from a Bigtable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by big_table.  Silly things may happen if
    other_silly_variable is not None.

    Args:
        big_table: An open Bigtable Table instance.
        keys: A sequence of strings representing the key of each table row
            to fetch.
        other_silly_variable: Another optional variable, that has a much
            longer name than the other args, and which does nothing.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {'Serak': ('Rigel VII', 'Preparer'),
         'Zim': ('Irk', 'Invader'),
         'Lrrr': ('Omicron Persei 8', 'Emperor')}

        If a key from the keys argument is missing from the dictionary,
        then that row was not found in the table.

    Raises:
        IOError: An error occurred accessing the bigtable.Table object.
    """
    pass
```

##### 8.4类

类应该在其定义下有一个用于描述该类的文档字符串. 如果你的类有公共属性(Attributes)， 那么文档中应该有一个属性(Attributes)段，并且应该遵守和函数参数相同的格式。

```python
class SampleClass(object):
    """Summary of class here.

    Longer class information....
    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam=False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def public_method(self):
        """Performs operation blah."""
```

##### 8.5块注释和行注释

* 对于复杂的操作，应该在操作开始前写上若干行注释，对于不是一目了然的代码应该在行尾添加注释。

```python
# We use a weighted dictionary search to find out where i is in
# the array.  We extrapolate position based on the largest num
# in the array and the array size and then do binary search to
# get the exact number.

if i & (i-1) == 0:        # true iff i is a power of 2
```

## 语言规范

#### 1.类

如果一个类不继承自其它类，就显式的从object继承，嵌套类也一样。

```python
class SampleClass(object):
    pass

class OuterClass(object):

    class InnerClass(object):
         pass

class ChildClass(ParentClass):
    """Explicitly inherits from another class already."""
```

> 继承自 `object` 是为了使属性`properties`正常工作，并且这样可以保护你的代码， 使其不受`Python 3000`的一个特殊的潜在不兼容性影响。

#### 2.字符串

```python
# Good 
x = a + b
x = '%s, %s!' % (imperative, expletive)
x = '{}, {}!'.format(imperative, expletive)
x = 'name: %s; score: %d' % (name, n)
x = 'name: {}; score: {}'.format(name, n)

# Bad
x = '%s%s' % (a, b)  # use + in this case
x = '{}{}'.format(a, b)  # use + in this case
x = imperative + ', ' + expletive + '!'
x = 'name: ' + name + '; score: ' + str(n)
```

> 避免在循环中使用`+`和`+=`来累加字符串，因为字符串是不可变的，这样会创建不必要的临时对象，导致二次方而不是线性的运行时间。可以把每个子串加入列表，然后在循环结束后用`.join`连接列表（也可以将每一个子串写入一个`cStringIO.StringIo`缓存中）。

```python
items = ['<table>']
for last_name, first_name in employee_list:
    items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
items.append('</table>')
employee_table = ''.join(items)
```

使用行连接而非多行字符串：

```python
# Good
print ("This is much nicer.\n"
       "Do it this way.\n")

# Bad
  print """This is pretty ugly.
Don't do this.
"""
```

#### 3.文件和sockets

* 在文件和sockets结束时，显示的关闭它
* 除文件外，sockets或其他类似文件的对象在没有必要的情况下打开，会有副作用，如：
  * 它们可能消耗有限的系统资源，如文件描述符。如果这些资源在使用后没有归还系统，那么用于处理这些对象的代码会将资源消耗殆尽。
  * 持有文件会阻止对文件的移动、删除之类的操作。
  * 仅仅从逻辑上关闭文件和sockets，那么它们仍然可能被其共享的程序在无意中进行读写，只有真正关闭后，对于它们尝试进行读写会抛出异常从而快速显现问题。

推荐使用`with`语句管理文件：

```python
with open("hello.txt") as hello_file:
    for line in hello_file:
        print line
```

对于不支持使用`with`的对象，使用`contexlib.closing()`：

```python
import contextlib

with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
    for line in front_page:
        print line
```

#### 4.TODO注释

临时代码使用`TODO`注释，它是一种短期解决方案，统一的风格如下：

```python
# TODO(kl@gmail.com): Use a "*" here for string repetition.
# TODO(Zeke) Change this to use relations.
```

#### 5.导入格式

* 每个导入单独一行
* 导入放在文件顶部，位于模块注释和文档字符串之后
* 按照标准库、第三方库、应用程序指定的顺序导入
* 每个分组中尽量按照字典序排序

## reference

https://www.runoob.com/w3cnote/google-python-styleguide.html