
>也可以参考的语雀空间。
<https://www.yuque.com/chachadi/em6l3g/aixpgw>



# 建议使用format()方法
字符串操作 对于 `%`， 官方以及给出这种格式化操作已经过时，在 `Python` 的未来版本中可能会消失。 在新代码中使用新的字符串格式。因此推荐大家使用`format()`来替换 %.


format 方法系统复杂变量替换和格式化的能力，因此接下来看看都有哪些用法。

# format()
这个方法是来自 `string` 模块的`Formatter`类里面的一个方法，属于一个内置方法。因此可以在属于 string 对象的范畴都可以调用这个方法。

# 语法结构

这个方法太强大了，官方的用户是。

```
replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"
field_name        ::=  arg_name ("." attribute_name | "[" element_index "]")*
arg_name          ::=  [identifier | integer]
attribute_name    ::=  identifier
element_index     ::=  integer | index_string
index_string      ::=  <any source character except "]"> +
conversion        ::=  "r" | "s" | "a"
format_spec       ::=  <described in the next section>
```
针对 format_spec 的用法如下
```
format_spec ::=  [[fill]align][sign][#][0][width][,][.precision][type]
fill        ::=  <a character other than '}'>
align       ::=  "<" | ">" | "=" | "^"
sign        ::=  "+" | "-" | " "
width       ::=  integer
precision   ::=  integer
type        ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
```
说明：
 - `<` 强制字段在可用空间内左对齐
- `>` 强制字段在可用空间内右对齐
- `=` 填充位于符号(如果有的话)之后，但位于数字之前
- `^` 强制场位于可用空间的中心

常用的方法有下面几个，format()方法中<模板字符串>的槽除了包括参数序号，还可以包括格式控制信息。此时，槽的内部样式如下：
>{<参数序号>: <格式控制标记>}

"{" [[identifier | integer]("." identifier | "[" integer | index_string "]")*]["!" "r" | "s" | "a"] [":" format_spec] "}"

其中，<格式控制标记>用来控制参数显示时的格式，包括：<填充><对齐><宽度>,<.精度><类型>6 个字段，这些字段都是可选的，可以组合使用，逐一介绍如下。


# 常用表达
- 指定位置
```python
>>> '{0}, {1}, {2}'.format('a', 'b', 'c')
'a, b, c'
>>> '{}, {}, {}'.format('a', 'b', 'c')  # 3.1+ only
'a, b, c'
>>> '{2}, {1}, {0}'.format('a', 'b', 'c')
'c, b, a'
>>> '{2}, {1}, {0}'.format(*'abc')      # unpacking argument sequence
'c, b, a'
>>> '{0}{1}{0}'.format('abra', 'cad')   # arguments' indices can be repeated
'abracadabra'
```
如果要显示 `{` `}`

```python
>>> '{{}}, {}, {}'.format('b', 'c')
'{}, b, c'
```

- 指定名称
```py
>>> 'Coordinates: {latitude}, {longitude}'.format(latitude='37.24N', longitude='-115.81W')
'Coordinates: 37.24N, -115.81W'
>>> coord = {'latitude': '37.24N', 'longitude': '-115.81W'}
>>> 'Coordinates: {latitude}, {longitude}'.format(**coord)
'Coordinates: 37.24N, -115.81W'
```

- 指定属性
对于复数来说，有2个属性。如果不知道属性具体名字是什么，可以用`dir`来查看。


```py
>>> c = 3-5j
>>> dir(c)
[....... 'imag', 'real']
>>> ('The complex number {0} is formed from the real part {0.real} '
...  'and the imaginary part {0.imag}.').format(c)
'The complex number (3-5j) is formed from the real part 3.0 and the imaginary part -5.0.'
>>> class Point:
...     def __init__(self, x, y):
...         self.x, self.y = x, y
...     def __str__(self):
...         return 'Point({self.x}, {self.y})'.format(self=self)
...
>>> str(Point(4, 2))
'Point(4, 2)'
```
- 访问数组之类
```py
>>> coord = (3, 5)
>>> 'X: {0[0]};  Y: {0[1]}'.format(coord)
'X: 3;  Y: 5'
```
- !s 区别 !r
```py
>>> "repr() shows quotes: {!r}; str() doesn't: {!s}".format('test1', 'test2')
"repr() shows quotes: 'test1'; str() doesn't: test2"
```

- 文本对齐

下面文本居中和左有对齐
```py
>>> '{:<30}'.format('left aligned')
'left aligned                  '
>>> '{:>30}'.format('right aligned')
'                 right aligned'
>>> '{:^30}'.format('centered')
'           centered           '
>>> '{:*^30}'.format('centered')  # use '*' as a fill char
'***********centered***********'
```
- 指定类型

 `b`: 输出整数的二进制方式；
   `c`: 输出整数对应的 Unicode 字符；
   `d`: 输出整数的十进制方式；
   `o`: 输出整数的八进制方式；
    `x`: 输出整数的小写十六进制方式；
  ` X`: 输出整数的大写十六进制方式；

```py
>>> '{:+f}; {:+f}'.format(3.14, -3.14)  # show it always
'+3.140000; -3.140000'
>>> '{: f}; {: f}'.format(3.14, -3.14)  # show a space for positive numbers
' 3.140000; -3.140000'
>>> '{:-f}; {:-f}'.format(3.14, -3.14)  # show only the minus -- same as '{:f}; {:f}'
'3.140000; -3.140000'
```

- 逗号作为数千个分隔符:
```py
>>> '{:,}'.format(1234567890)
'1,234,567,890'
```

- 表示百分比
```py
>>> points = 19
>>> total = 22
>>> 'Correct answers: {:.2%}.'.format(points/total)
'Correct answers: 86.36%'
```

- 特定格式，比如日期
```py
>>> import datetime
>>> d = datetime.datetime(2010, 7, 4, 12, 15, 58)
>>> '{:%Y-%m-%d %H:%M:%S}'.format(d)
'2010-07-04 12:15:58'
```


# 骚操作
可以用来输出一个表格，有点类似三方库prettytable的效果

```py
>>> width = 5
>>> for num in range(5,12):
...     for base in 'dXob': # 分别为10/16/8/2进制
...         print('{0:{width}{base}}'.format(num, base=base, width=width), end=' ')
...     print()
...
    5     5     5   101
    6     6     6   110
    7     7     7   111
    8     8    10  1000
    9     9    11  1001
   10     A    12  1010
   11     B    13  1011
```
# 高级用法 - 模板字符串
如果你是一个看Python语言工具的源码的话，会发现这么一个用法 - 模板字符串，比如`robot`里面`__init__.py`里面就有这么一个用法。先看例子
```py
from string import Template

errorMessageTemplate = Template("""$reason
RIDE depends on wx (wxPython). .....""")

....

print(errorMessageTemplate.substitute(reason="wxPython not found."))
```
如果是有问题就会打印
```py
wxPython not found.
RIDE depends on wx (wxPython). .....
```
首先要导入模板`Template`，看看里面有哪些属性
```py
>>> from string import Template as t
>>> dir(t)
[.....'braceidpattern', 'delimiter', 'flags', 'idpattern', 'pattern', 'safe_substitute', 'substitute']

>>> s = Template('$who likes $what')
>>> s.substitute(who='tim', what='kung pao')
'tim likes kung pao'


>>> d = dict(who='tim')
>>> Template('Give $who $100').substitute(d)
Traceback (most recent call last):
[...]
ValueError: Invalid placeholder in string: line 1, col 10


>>> Template('$who likes $what').substitute(d)
Traceback (most recent call last):
[...]
KeyError: 'what'
>>> Template('$who likes $what').safe_substitute(d)
'tim likes $what'
```

## 相关阅读
https://stackoverflow.com/questions/5082452/string-formatting-vs-format

https://www.python.org/dev/peps/pep-3101

https://blog.csdn.net/i_chaoren/article/details/77922939

https://docs.python.org/release/3.1.5/library/stdtypes.html#old-string-formatting-operations

https://docs.python.org/release/3.1.5/library/string.html#string-formatting

看完了是不是对 format 已经有很深的认识了吧。赶紧动起来，实践一下。

Hi,Sup，如果觉得我写的不错，不妨帮个忙

>1、可以关注我的公众号「程序员汇聚地」，每天分享互联网前沿技术，让你的琐碎时间不在无聊，听说关注了的人越来越优秀。


> 2、顺手点个赞呗，可以让更多的人看到这篇文章，顺便激励下我，嘻嘻。

