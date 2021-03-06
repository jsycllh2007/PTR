
# Python 3.6新特性简介

原文：[What’s New In Python 3.6][1]

翻译团队：[Py字幕组](https://github.com/PyCN/PTR) 

首发刊物：[PyCN技术评论](https://pycntech.github.io)

Github：[https://github.com/PyCN/PTR](https://github.com/PyCN/PTR/blob/master/What%E2%80%99s%20New%20In%20Python%203.6.md)

由阿橙([@sinoandywong](https://github.com/sinoandywong))召集，Ele([@ictar](https://github.com/ictar))、苍冥([@eastrd](https://github.com/eastrd))组织翻译。

**译者：**

[eastrd](https://github.com/eastrd)：苍冥

[ictar](https://github.com/ictar) ：Ele

[linchart](https://github.com/linchart)

[szthanatos ](https://github.com/szthanatos)

[alex-marmot](https://github.com/alex-marmot)

[heyuanree](https://github.com/heyuanree)

[sinoandywong](https://github.com/sinoandywong)：阿橙

[sxqs-yang](https://github.com/sxqs-yang):若木羊

[zilongcc](https://github.com/zilongcc)：水手

[bubuyo](http://www.bububupt.com):铲屎官

[yifan1024](https://github.com/yifan1024)

[YoungZiyi](https://github.com/YoungZiyi)

[fuckexception](https://github.com/fuckexception)

[Lving](https://github.com/Lving)

[LoveSn0w](https://github.com/LoveSn0w)

[phdhorse41](https://github.com/phdhorse41)


**特别感谢：**

Ele([@ictar](https://github.com/ictar))：她为本文档的翻译做了大量工作，一个热爱美食的菇凉。

[Py字幕组](https://github.com/PyCN/PTR)：一个各路大(dou)神(bi)聚集的团队。

**感谢他们所作出的工作，圣诞快乐！**

**2016.12.25**



版本:| 3.6.0
---|---
日期:| December 15, 2016
编辑:| Elvis Pranskevichus
&lt;[elvis@magic.io][2]&gt;, Yury Selivanov
&lt;[yury@magic.io][3]&gt;

这篇文章介绍了与3.5相比， Python 3.6中多出的新特性。

另请参阅

[**PEP 494**][4] \- Python 3.6 发布时间表

## 摘要 - 发布亮点

新的语法特性：

  * PEP 498, 格式化字符串字面量
  * PEP 515, 数字字面量中的下划线
  * PEP 526, 变量注解中的语法
  * PEP 525, 异步生成器
  * PEP 520: 异步解析式

新的库模块

  * [`secrets`][5]: PEP 506 - 在标准库中添加了Secrets模块

CPython实现的改进：

  * 重新实现了[字典(dict)][6]类型，以便能像[PyPy的字典类型][7]一样使用更紧凑的表达方式。与Python 3.5相比，这使字典的内存用量减少了20%到25%。
  * 用新协定优化了类的自定义建立。
  * 类属性定义顺序(class attribute definition order)现在被保留了
  * \*\*kwargs内的元素顺序现在对应于将关键字(保留字）参数传递给函数的顺序
  * 新增了对DTrace和SystemTap probing的支持。
  * 新PYTHONMALLOC环境变量现在可用于调试解释器内存分配与访问错误。

标准库的重大改进：

  * 为[asyncio][8]模块开发了新功能、显著的可用性、性能优化，以及大量的错误修复。 从Python 3.6开始，asyncio模块不再是临时的了，其API也进入了稳定状态。
  * 实现了用于支持[类路径对象(path-like objects)][9]的新文件系统路径协议。 所有在路径(path)上使用的标准库函数都已更新，以便适应于新协议。
  * [datetime][10]模块已获得对本地时间消歧(Local Time Disambiguation)的支持。
  * 针对[typing][11]模块的一些改进，使其不再是临时模块。
  * [tracemalloc][12]模块已重大改进，现用于为ResourceWarning提供更好的输出，并为内存分配错误提供更好的诊断。 欲知详情，请参阅PYTHONMALLOC部分。

安全相关的改进:

  * 新[secrets模块][13]被用于简化那些适用于管理密文的密码学安全伪随机数生成器(cryptographically strong pseudo-random numbers)的生成过程，如认证、token等。
  * 在Linux上，现将[os.urandom()][14]改成了阻塞模式，直到系统的urandom的熵池(entropy pool)的初始化具有更高的安全性。 解释请参见[**PEP 524**][15]。
  * [`hashlib`][16]和[`ssl`][17]模块现已支持OpenSSL 1.1.0。
  * 改进了[`ssl模块`][18]的默认设置和特性集。
  * 新增了[`hashlib模块`][19]对BLAKE2、SHA-3、SHAKE哈希算法以及[`scrypt()`][20]密钥导出函数的支持。

Windows上的改进:

  * PEP 528与PEP 529，Windows文件系统和控制台的编码已更改为UTF-8。
  * 当用户没有指定版本（通过命令行参数或配置文件）时，py.exe启动器以交互方式使用时，不再以Python 2优先于Python 3。 处理shebang行的方式保持不变 - 此处的“python”依旧指Python 2。
  * python.exe和pythonw.exe已标记为长路径敏感(long-path aware)，这意味着260字符路径限制可能不再适用。 有关详细信息，请参阅[删除MAX\_PATH限制][21]。
  * 可以添加`._pth`文件以强制隔离模式(isolated mode)并完全指定所有搜索路径，以避免注册表查找和环境查找。 有关详细信息，请参阅[文档][22]。
  * 一个python36.zip文件现可用作一个地标(landmark)以臆指[`PYTHONHOME`][23]。 有关详细信息，请参阅[文档][24]。

## 新特性

### PEP 498: 格式化字符串

[**PEP 498**][25]引入了一种新的字符串：\_f-strings\_, 或者[格式化字符串][26]。

格式化字符串带`'f'`前缀，类似于[`str.format()`][27]接受的格式字符串。它们包含了由花括号括起来的替换字段。替换字段是表达式，它们会在运行时计算，然后使用[`format()`][28]协议进行格式化：

```

    >>> name = "Fred"
    >>> f"He said his name is {name}."
    'He said his name is Fred.'
    >>> width = 10
    >>> precision = 4
    >>> value = decimal.Decimal("12.34567")
    >>> f"result: {value:{width}.{precision}}"  # nested fields
    'result:      12.35'

```

又见

[**PEP 498**][29] - 字符串插值。

	PEP由Eric V. Smith编写和实现。

[特性文档][30]。

### PEP 526: 变量注释语法

[**PEP 484**][31]引入了函数参数的类型注释的标准，又名类型提示。这个PEP添加了用来注释变量（包括类变量和实例变量）类型的语法：

```

    primes: List[int] = []

    captain: str  # Note: no initial value!

    class Starship:
        stats: Dict[str, int] = {}

```

正如函数注释，Python解释器不附加任何特殊意义到变量注释上，只是将它们存储在一个类或者模块的`__annotations__`属性中。

与静态类型语言中的变量声明相比，注释语法的目的在于提供一种简单的方式，通过抽象语法树和`__annotations__`属性，来为第三方工具和库指定结构化类型元数据。

又见

[**PEP 526**][32] - 变量注释语法。

	PEP由Ryan Gonzalez, Philip House, Ivan Levkivskyi, Lisa Roach, 和Guido van Rossum编写。由Ivan Levkivskyi实现。

使用或将要使用这个新语法的工具：[mypy][33], [pytype][34], PyCharm等等。

### PEP 515: 数值文字中的下划线

[**PEP 515**][35]添加了在数值文字中使用下划线的能力，以提高可读性。例如：

```

    >>> 1_000_000_000_000_000
    1000000000000000
    >>> 0x_FF_FF_FF_FF
    4294967295

```

数字之间和任何基本符号之后允许单个下划线。不允许前置、后置或者多个连续的下划线。

[字符串格式化][36]语言现在还支持`'_'`选项，该选项用来通知对浮点表示类型和整型表示类型`'d'`，会把下划线当成千位分隔符使用。对于整型表示类型`'b'`, `'o'`, `'x'`, 和`'X'`, 下划线将会被插入到每4个数字之间：

```

    >>> '{:_}'.format(1000000)
    '1_000_000'
    >>> '{:_x}'.format(0xFFFFFFFF)
    'ffff_ffff'

```

又见

[**PEP 515**][37] - 数值文字中的下划线

	PEP由Georg Brandl和Serhiy Storchaka编写。

### PEP 525: 异步生成器

[**PEP 492**][38] 引入支持原生协程和`async` /`await`的语法到Python 3.5。 在Python 3.5实现里的一个值得注意的
局限性就在于它不可能使用`await`和`yield'在同一个函数体中。 而在Python 3.6中，这个限制
已解除，这使得定义_异步生成器_成为可能：

```

    async def ticker(delay, to):
        """Yield numbers from 0 to *to* every *delay* seconds."""
        for i in range(to):
            yield i
            await asyncio.sleep(delay)

```
新的语法允许更快更简洁的代码。

参见
[**PEP 525**][39] - 异步生成器

	由Yury Selivanov撰写并实现的PEP。

### PEP 530: 异步解析式

[**PEP 530**][40] 添加了对`async for`在list、set、dict解析式以及generator表达式中的使用支持：

```

    result = [i async for i in aiter() if i % 2]

```

此外，所有解析式都支持“await”表达式：

```

    result = [await fun() for fun in funcs if await condition()]

```

参见
[**PEP 530**][41] - 异步解析式

	由Yury Selivanov撰写并实现的PEP。

### PEP 487: 用于建立类的更简单的自定义

现在可以在不使用元类的情况下自定义子类。每当创建一个新的子类时，新的`__init_subclass__`类方法将在基类上被调用，：

```

    class PluginBase:
        subclasses = []

        def __init_subclass__(cls, **kwargs):
            super().__init_subclass__(**kwargs)
            cls.subclasses.append(cls)

    class Plugin1(PluginBase):
        pass

    class Plugin2(PluginBase):
        pass

```
为了允许零参数
[`super（）`] [42]
从[`_init_subclass __（）`](https://docs.python.org/3
.6/reference/datamodel.html#object.\_\_init\_subclass\_\_
"object.\_\_init\_subclass\_\_" )实现中被正确的调用并工作，自定义元类必须确保
新的`__classcell__`命名空间输入传递到`type .__ new__`
（如[创建类
对象](https://docs.python.org/3.6/reference/datamodel.html#class-object-
creation))

参见

[**PEP 487**][43] - 用于建立类的更简单的自定义

	由Martin Teichmann撰写并实现的PEP。

[功能文档](https://docs.python.org/3.6/reference/datamodel.html#class-customization)

### PEP 487: 描述符协议增强

[**PEP 487**][44] 扩展描述符协议必须包括新的可选的[`__set_name __（）`](https://docs.python.org/3.6/reference/datamodel.html#object.\_\_set\_name\_\_ "object.\_\_set\_name\_\_" )方法。 每当定义一个新类时，新方法将会调用定义中所有的描述符，并给它们提供定义类的引用，以及类命名空间中给予描述符的名字。 换句话说，描述符的实例现在可以获知所有者类的属性名：
```

    class IntField:
        def __get__(self, instance, owner):
            return instance.__dict__[self.name]

        def __set__(self, instance, value):
            if not isinstance(value, int):
                raise ValueError(f'expecting integer in {self.name}')
            instance.__dict__[self.name] = value

        # this is the new initializer:
        def __set_name__(self, owner, name):
            self.name = name

    class Model:
        int_field = IntField()

```

参见

[**PEP 487**][45] - 用于建立类的更简单的自定义

	由Yury Selivanov撰写并实现的PEP。

[功能文档](https://docs.python.org/3.6/reference/datamodel.html#d
escriptors)

### PEP 519: 添加文件系统路径协议

文件系统路径过去被表示为[`str`][46]或[`bytes`][47]对象。这会导致那些编写操作文件系统路径代码的人，假定这些对象只能是这两种类型之一(一个代表着文件描述符的[`int`][48]对象将不被计入即它不是一个文件路径)。
不幸的是，这种假设局限了文件系统路径表示代方法，如已经存在的[`pathlib`](https://docs.python.org/3.6/library/pathlib.html#module-pathlib
"pathlib: Object-oriented filesystem paths" )，同时也包括python的一些标准库。
为了解决这种情况，定义了一个由[`os.PathLike`](https://docs.python.org/3.6/library/os.html#os.PathLike
"os.PathLike" )表示的新接口。通过实现[`__fspath__()`](https://docs.python.org/3.6/library/os.html#os.PathLike.\_\_fspath\_\_
"os.PathLike.\_\_fspath\_\_" )方法，一个对象表示一个路径，然后，可以将文件系统路径表示为一个较低等级的[`str`](https://docs.python.org/3.6/library/stdtypes.html#str "str")或者[`bytes`](https://docs.python.org/3.6/library/functions.html#bytes
"bytes" )对象。这意味着，如果一个对象实现[`os.PathLike`](https://docs.python.org/3.6/library/os.html#os.PathLike
"os.PathLike" )或者是[`str`][49]或[`bytes`][50]，该对象被认为是[path-
like](https://docs.python.org/3.6/glossary.html#term-path-like-object),它代表一个文件系统路径。你可以使用[`os.fspath()`](https://docs.python.org/3.6/library/os.html#os.fspath
"os.fspath" ),[`os.fsdecode()`](https://docs.python.org/3.6/library/os.html#os.fsdecode
"os.fsdecode" )或[`os.fsencode()`](https://docs.python.org/3.6/library/os.html#os.fsencode
"os.fsencode" )显式获取[`str`][51]以及/或[`bytes`][52]来表示一个path-like对象。
内建函数[`open()`][53]已经更新，可以接受[`os.PathLike`](https://docs.python.org/3.6/library/os.html#os.PathLike
"os.PathLike" )对象，以及在[`os`][54]和[`os.path`](https://docs.python.org/3.6/library/os.path.html#module-os.path
"os.path: Operations on pathnames." )模块中的所有函数，以及标准库中的大多数其他函数和类。类[`os.DirEntry`](https://docs.python.org/3.6/library/os.html#os.DirEntry
"os.DirEntry" )以及[`pathlib`](https://docs.python.org/3.6/library/pathlib.html#module-pathlib
"pathlib: Object-oriented filesystem paths" )中相关的类也已经可以实现[`os.PathLike`](https://docs.python.org/3.6/library/os.html#os.PathLike
"os.PathLike" )。
希望对操作文件系统路径基本功能的更新能够让第三方代码在不改变任何代码，或者至少是非常少的代码（例如，在操作path-like对象之前，在代码的开头调用[`os.fspath()`](https://docs.python.org/3.6/library/os.html#os.fspath "os.fspath" )）的情况下，能够隐含地支持所有[path-like objects](https://docs.python.org/3.6/glossary.html#term-path-like-object)对象。
下面举一些例子说明新接口是如何让预先存在的代码简单透明地使用[`pathlib.Path`](https://docs.python.org/3.6/library/pathlib.html#pathlib.Path
"pathlib.Path" ):
```

    >>> import pathlib
    >>> with open(pathlib.Path("README")) as f:
    ...     contents = f.read()
    ...
    >>> import os.path
    >>> os.path.splitext(pathlib.Path("some_file.txt"))
    ('some_file', '.txt')
    >>> os.path.join("/a/b", pathlib.Path("c"))
    '/a/b/c'
    >>> import os
    >>> os.fspath(pathlib.Path("some_file.txt"))
    'some_file.txt'

```

（由Brett Cannon, Ethan Furman, Dusty Phillips, and Jelle Zijlstra实现）

参见

[**PEP 519**][55] - 添加文件系统路径协议

	PEP 由Brett Cannon 和 Koos Zevenhoven撰写.

### PEP 495: 本地时间消歧

世界上大多数地方，都曾经出现也将出现多次的时间回调。在这种时候，引入时间间隔用以表示本地时钟在同一天中出现两次相同的时间的情况，在这些情况下，本地时钟显示的时间（或存在在python datetime中的实例）不足及时表示特定的时刻。
为了区分本地时间相同的两个时刻，[**PEP 495**][56] 在类 [`datetime.datetime`](https://docs.python.org/3.6/li
brary/datetime.html#datetime.datetime "datetime.datetime" ) 和 [\`datetime.tim
e\`](https://docs.python.org/3.6/library/datetime.html#datetime.time
"datetime.time" ) 的实例中增加了新的fold属性:

```

    >>> u0 = datetime(2016, 11, 6, 4, tzinfo=timezone.utc)
    >>> for i in range(4):
    ...     u = u0 + i*HOUR
    ...     t = u.astimezone(Eastern)
    ...     print(u.time(), 'UTC =', t.time(), t.tzname(), t.fold)
    ...
    04:00:00 UTC = 00:00:00 EDT 0
    05:00:00 UTC = 01:00:00 EDT 0
    06:00:00 UTC = 01:00:00 EST 1
    07:00:00 UTC = 02:00:00 EST 0

```

属性 [`fold`](https://docs.python.org/3.6/library/datetime.html#d
atetime.datetime.fold "datetime.datetime.fold" ) 的值除了表示不明确情况下的第二（时间顺序）时刻之外，其他所有实例的值都为 `0`。
See also

[**PEP 495**][57] - 本地时间消歧

	PEP 由 Alexander Belopolsky 和 Tim Peters撰写, 由Alexander Belopolsky实现.

### PEP 529: 更改windows下文件系统编码格式为UTF-8

使用str (Unicode) 表示文件系统路径比bytes能获得更好的效果。尽管如此，在某些情况下bytes也足以胜任并且也是正确的。

在3.6之前,使用bytes路径可能导致数据丢失。改进后, windows下现在支持使用bytes表示路径,这些bytes将以[`sys.getfilesy
stemencoding()`](https://docs.python.org/3.6/library/sys.html#sys.getfilesystemencoding "sys.getfilesystemencoding" )的方式编码，默认编码格式为`'utf-8'`。

不使用str方式表示路径的应用程序应当使用[`os.fsencode()`](https://docs.python.org/3.6/library/os.html#os.fsencode "os.fsencode" )和[`os.fsdecode()`](https://docs.python.org/3.6/library/os.html#os.fsdecode "os.fsdecode" ) 以确保他们的bytes被正确编码。要回复到之前的状态, 设置 [`PYTHONLEGACYWINDOWSFSENCODING`](https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONLEGACYWINDOWSFSENCODING) 或者调用 [`sys._enablelegacywindowsfsencoding()`](https://docs.python.org/3.6/library/sys.html#sys._enablelegacywindowsfsencoding "sys._enablelegacywindowsfsencoding" )。

查看 [**PEP 529**](https://www.python.org/dev/peps/pep-0529)以获取更多信息并讨论可能需要变更的代码。

### PEP 528: 更改windows控制台编码为UTF-8

windows下的默认控制台现在支持所有的Unicode字符并可以正确读取Python代码中的str对象。 `sys.stdin`, `sys.stdout`
以及 `sys.stderr` 现在的默认使用utf-8编码。

这一变化仅适用于使用交互控制台之时，而非重定向文件或者管道。如果要使用之前的交互控制台, 需设置 [`PYTHONLEGACYWINDOWSIOENCODING`](https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONLEGACYWINDOWSIOENCODING)。

另请参阅

[**PEP 528**](https://www.python.org/dev/peps/pep-0528) - 修改windows控制台编码为UTF-8

    PEP 由Steve Dower编写和实现。

### PEP 520: 保存类属性定义顺序

类的定义体中的属性有一个自然顺序：即源码中属性名出现的顺序。 这个顺序现在保存在新的类[`__dict__`](https://docs.python.org/3.6/library/stdtypes.html#object.__dict__ "object.__dict__" ) 的属性中.

同样， 有效的缺省类和执行空间 (从[type.__prepare__()](https://docs.python.org/3.6/reference/datamodel.html#prepare)返回)是一个保存插入顺序的映射。

另请参阅

[**PEP 520**](https://www.python.org/dev/peps/pep-0520) - 保存类属性定义顺序

    该PEP由Eric Snow编写和实现。

### PEP 468: 保存关键字参数顺序

函数声明中的`**kwargs` 的顺序现在被保证是插入顺序的映射。

另请参阅

[**PEP 468**](https://www.python.org/dev/peps/pep-0468) - 保存关键字参数顺序

    该PEP由Eric Snow编写和实现。

### 新的 [字典dict](https://docs.python.org/3.6/library/stdtypes.html#typesmapping)类型的实现

[字典dict](https://docs.python.org/3.6/library/stdtypes.html#typesmapping)类型现在使用 [PyPy首创](https://morepypy.blogspot.com/2015/01/faster-more-memory-efficient-and-more.html)的 "紧凑" 表达方式。 新[`字典dict()`](https://docs.python.org/3.6/library/stdtypes.html#dict "dict" ) 的内存占用比Python3.5中减少20%到25%。

新的实现中保存顺序的功能被认为是不可过于依赖的(未来也许会改变，不过在将所有当前和未来的Python实现的语言规范转换为保证顺序的语法之前的几个版本中，新的dict有望被实现的; 这也能帮助保证对那些仍旧是随机迭代顺序的旧版本的向后兼容，比如Python 3.5)。

(由INADA Naoki在[issue 27350](https://bugs.python.org/issue27350)提供。 想法 [最初由Raymond Hettinger提出](https://mail.python.org/pipermail/python-dev/2012-December/123028.html).)

### PEP 523：添加一个frame解析API到CPython

虽然Python提供了自定义代码执行方式的广泛支持，但是还有一个它没这样做的地方是frame对象的解析。如果你想要通过某些方式在Python中拦截frame解析，那么除了直接对定义的函数操作函数指针之外，真的没有什么其他方式。

[**PEP 523**][65]改变了这个处境，它提供了一个API，让frame解析在C层次上可插拔。这将允许诸如调试器或者JIT这样的工具在Python代码开始执行之前拦截frame解析。这使得python代码的可替换解析实现、跟踪frame解析等地使用成为可能。

这个API并不是受限的C API的一部分，并且被标为私有，表示期望限制这个API的使用，并且只能应用在非常选定的低层次用例上。这个API的语义将在必要的时候随着Python改动。

又见

[**PEP 523**][66] - 添加一个frame解析API到CPython

	PEP由Brett Cannon和Dino Viehland编写

### PYTHONMALLOC环境变量

新的[`PYTHONMALLOC`](https://docs.python.org/3.6/using/cmdline.html# envvar-PYTHONMALLOC)环境变量允许设置Python内存分配器和安装调试钩子。

现在有可能在发布模式下使用`PYTHONMALLOC=debug`编译的Python中，在Python内存分配器上安装调试钩子。调试钩子的效果：

  * 新分配内存由字节`0xCB`填充
  * 被释放内存由字节`0xDB`填充
  * 检测Python内存分配器API的违规。例如，在由[`PyMem_Malloc()`][67]分配的内存块上调用`PyObject_Free()`。
  * 检测缓存开始位置之前的写入 (缓存下溢)
  * 检测缓存结束位置之后的写入 (缓存上溢)
  * 当调用了[`PYMEM_DOMAIN_OBJ`][69] (例如，`PyObject_Malloc()`)和[`PYMEM_DOMAIN_MEM`][70] (例如，[`PyMem_Malloc()`][71])域的分配器函数时，检测持有[GIL][68]。

检测是否持有GIL也是Python 3.6的一个新特性。

见[`PyMem_SetupDebugHooks()`](https://docs.python.org/3.6/c-api/memory.html#c.PyMem\_SetupDebugHooks "PyMem\_SetupDebugHooks" )函数，了解更多关于Python内存分配器上的调试钩子信息。

现在也有可能使用`PYTHONMALLOC=malloc`，从而强制所有Python内存分配使用C库的`malloc()`分配器。这在以发布模式编译的Python上，使用诸如Valgrind这样的外部内存调试器是有帮助的。

对于错误，Python内存分配器上的调试钩子现在使用[`tracemalloc`](https://docs.python.org/3.6/library/tracemalloc.html#module-tracemalloc "tracemalloc: Trace memory allocations." )模块来获取分配内存块的地址的回溯信息。

使用`python3.6 -X tracemalloc=5` (存储5个帧到回溯中)时，在缓存上溢时的致命错误的例子：

```

    Debug memory block at address p=0x7fbcd41666f8: API 'o'
        4 bytes originally requested
        The 7 pad bytes at p-7 are FORBIDDENBYTE, as expected.
        The 8 pad bytes at tail=0x7fbcd41666fc are not all FORBIDDENBYTE (0xfb):
            at tail+0: 0x02 *** OUCH
            at tail+1: 0xfb
            at tail+2: 0xfb
            at tail+3: 0xfb
            at tail+4: 0xfb
            at tail+5: 0xfb
            at tail+6: 0xfb
            at tail+7: 0xfb
        The block was made by call #1233329 to debug malloc/realloc.
        Data at p: 1a 2b 30 00

    Memory block allocated at (most recent call first):
      File "test/test_bytes.py", line 323
      File "unittest/case.py", line 600
      File "unittest/case.py", line 648
      File "unittest/suite.py", line 122
      File "unittest/suite.py", line 84

    Fatal Python error: bad trailing pad byte

    Current thread 0x00007fbcdbd32700 (most recent call first):
      File "test/test_bytes.py", line 323 in test_hex
      File "unittest/case.py", line 600 in run
      File "unittest/case.py", line 648 in __call__
      File "unittest/suite.py", line 122 in run
      File "unittest/suite.py", line 84 in __call__
      File "unittest/suite.py", line 122 in run
      File "unittest/suite.py", line 84 in __call__
      ...

```

(由Victor Stinner于[issue 26516](https://bugs.python.org/issue26516)和[issue 26564](https://bugs.python.org/issue26564)贡献。)

### DTrace和SystemTap探测支持

Python现在可以使用`--with-dtrace`来构建，它为解释器中的以下事件启用了静态标志器：

  * 函数调用／返回
  * 垃圾回收开始/结束
  * 执行代码行。

这可以被用来检测生产中运行的解释器，而无需重新编译指定调试构建，或者提供应用特有的配置/调试代码。

详情请见[用DTrace和SystemTap检测CPython](https://docs.python.org/3.6/howto/instrumentation.html#instrumentation)。

在Linux和macOS上测试了当前实现。未来可能会添加额外的标志器。

(由Łukasz Langa在[issue 21590](https://bugs.python.org/issue21590)中贡献，基于Cea Avión, David Malcolm, 和Nikhil Benesch提供的补丁)

## 其他语言方面的变化

我们还对 Python 语言的核心做了一些小的改变：

  * 现在在同一作用域内的 `global` 或 `nonlocal` 语句必须在受其影响的命名使用前显式出现。在先前版本中会发出 `语法警告`。
  * 现在能以将 [魔术方法][72] 设置为 `None` 的方式来表示相应的操作不可用。 例如，如果将一个类的 [`__iter__()`][73] 设置为None，则该类不可迭代。(由 Andrew Barnert 和 Ivan Levkivskyi 在 [issue 25958][74] 中贡献。)
  * 在 traceback 中大量重复出现的 traceback lines 将会被略写成 `"[前一行已经重复出现 {count} 次了]"` (以 traceback 为例。). (由 Emanuel Barry 在 [issue 26823][75] 中贡献。)
  * 现在模块导入在找不到模块时引发新的异常 [`ModuleNotFoundError`][76]（[`ImportError`][77] 的子类）。 当前检测 ImportError 的代码（try-except中）仍然可以工作。(由 Eric Snow 在 [issue 15767][78] 中贡献。)
  * 在类创建时，被元类调用的依赖无参数的 `super()` 的类方法时会正确工作。(由 Martin Teichmann 在 [issue 23722][79]中贡献。)

## 新增模块

### secrets

新增模块 [`secrets`](https://docs.python.org/3.6/library/secrets.html#module-secrets
"secrets: Generate secure random numbers for managing secrets." ) 的主要目的是：提供一个
显式可靠的方式来产生适合于管理诸如账户认证，令牌之类的保密信息的加密的强伪随机值。

警告

注意在 [`random`](https://docs.python.org/3.6/library/random.html#module-random
"random: Generate pseudo-random numbers with various common distributions." )
模块中的随机数生成器 **不该** 用于安全目的。Python 3.6 及以上版本请使用 [`secrets`](https://docs.python.org/3.6/library/secrets.html#module-secrets
"secrets: Generate secure random numbers for managing secrets." ) 而 Python 3.5 及更早的版本请使用
[`os.urandom()`](https://docs.python.org/3.6/library/os.html#os.urandom
"os.urandom" )。

又见

[**PEP 506**][80] - 向标准库添加 Secrets 模块

	PEP 由 Steven D'Aprano 编写及实现。

## 改进模块

### array

耗尽的迭代器[`array.array`](https://docs.python.org/3.6/library/array.html#array.array)现在将会保持被耗尽，即使被迭代的数组被拓展了。这与其他可变序列的行为保持了一致性。

由Serhiy Storchaka在[issue
26492](https://bugs.python.org/issue26492)中贡献


### ast

添加了新的`ast.Constant`的AST节点。它可以被外部的AST优化器出于永久聚合(constant foldind)。

由Victor Stinner在[issue
26146](https://bugs.python.org/issue26146)中贡献

### asyncio

始于Python 3.6，`asyncio`模块将不再是临时的了，并且它的API被认为是稳定的。

从Python 3.5开始，在`asyncio`模块中值得注意的变化（由于临时的状态，所有的修改都应用到了3.5.x）:

  * [`get_event_loop()`](https://docs.python.org/3.6/library/asyncio-eventloops.html#asyncio.get_event_loop "asyncio.get_event_loop" )函数已经被改变为，当从协同程序中调用和被回调调用时，总是返回当前正在运行的循环。（由 Yury Selivanovgong贡献于[issue 28613](https://bugs.python.org/issue28613)。）
  * [`ensure_future()`](https://docs.python.org/3.6/library/asyncio-task.html#asyncio.ensure_future "asyncio.ensure_future" )函数和所有使用其的函数，比如`loop.run_until_complete()`，现在接收所有种类的[awaitable objects](https://docs.python.org/3.6/glossary.html#term-awaitable)。（由 Yury Selivanov贡献）。
  * 新的[`run_coroutine_threadsafe()`](https://docs.python.org/3.6/library/asyncio-task.html#asyncio.run_coroutine_threadsafe "asyncio.run_coroutine_threadsafe" )函数去从其他线程中提交协同程序到事件循环中。（由Vincent Michel贡献）
  * 新的[`Transport.is_closing()`](https://docs.python.org/3.6/library/asyncio-protocol.html#asyncio.BaseTransport.is_closing "asyncio.BaseTransport.is_closing" )方法去检查是否传输是关闭的或被关闭了（由Yury Selivanov贡献）。
  * `loop.creat_server()`现在可以以列表接收主机了。（由Yann Sionneaugong贡献）
  * 新的`loop.creat_future()`方法去创建一个Future对象。这允许了替代事件循环的实现，比如[uvloop](https://github.com/MagicStack/uvloop)，去提供了一种更快的[`asyncio.Future`](https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Future "asyncio.Future" )的实现。（由Yury Selivanov在[issue 27041](https://bugs.python.org/issue27041)中贡献）
  * 新的`loop.get_exception_handler()`方法去得到一个当时的异常的句柄。（由Yury Selivanov在[issue 27040](https://bugs.python.org/issue27040)中贡献）
  * 新的[`StreamReader.readuntil()`](https://docs.python.org/3.6/library/asyncio-stream.html#asyncio.StreamReader.readuntil "asyncio.StreamReader.readuntil" )方法从流中读取数据读取数据直到出现分隔符字符序列。（由Mark Korenberg贡献）
  * [`StreamReader.readexactly()`](https://docs.python.org/3.6/library/asyncio-stream.html#asyncio.StreamReader.readexactly "asyncio.StreamReader.readexactly" )的表现被提升了。（由Mark Korenberg在[issue 28370](https://bugs.python.org/issue28370)中贡献）
  * `loop.getaddrinfo()`方法被优化了，避免了如果地址问题已经解决，去调用系统函数`getaddrinfo`。（由A. Jesse Jiryu Davis贡献）
  * `loop.stop()`方法已经被更改，以便在当前迭代后立即停止循环。任何新的，作为最后一次迭代的结果的预定的回调将会被丢弃。（由Guido van Rossum在[issue 25593](https://bugs.python.org/issue25593)中贡献）
  * `Future.set_exception`在通过了一个[`StopIteration`](https://docs.python.org/3.6/library/exceptions.html#StopIteration "StopIteration" )实例的异常后，将会引发[`TypeError`](https://docs.python.org/3.6/library/exceptions.html#TypeError "TypeError" )。（由Chris Angelico在[issue 26221](https://bugs.python.org/issue26221)贡献）
  * 新的[`loop.connect_accepted_socket()`](https://docs.python.org/3.6/library/asyncio-eventloop.html#asyncio.BaseEventLoop.connect_accepted_socket "asyncio.BaseEventLoop.connect_accepted_socket" )方法被服务器用来接收asyncio外部的连接，但是使用asyncio去处理。（由Jim Fulton在[issue 27392](https://bugs.python.org/issue27392)中贡献）
  * `TCP_NODELAY`标志位现在被默认设置为TCP传输。（由Yury Selivanov在[issue 27456](https://bugs.python.org/issue27456)中贡献）
  * 新的[`loop.shutdown_asyncgens()`](https://docs.python.org/3.6/library/asyncio-eventloop.html#asyncio.AbstractEventLoop.shutdown_asyncgens "asyncio.AbstractEventLoop.shutdown_asyncgens" )在关闭循环之前恰当的关闭挂起的异步生成器。（由 Yury Selivanov在[issue 28003](https://bugs.python.org/issue28003)中贡献）
  * [`Future`](https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Future "asyncio.Future" )和[`Task`](https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Task "asyncio.Task" )类现在有一个已优化的C语言的实现，使得asyncio编码速度提高了30%。（由Yury Selivanov和INADA Naoki在[issue 26081](https://bugs.python.org/issue26081)和[issue 28544](https://bugs.python.org/issue28544)中贡献）

### binascii

[`b2a_base64()`](https://docs.python.org/3.6/library/binascii.html#binascii.b2a_base64 "binascii.b2a_base64" )功能现在接收现在接收一个可选_newline_的关键词参数去控制是否新的一行的字符被加到返回值中。（由Victor Stinnergong贡献于[issue25357](https://bugs.python.org/issue25357)。）

### cmath

新增了方法 [`cmath.tau`](https://docs.python.org/3.6/library/cmath.html#cmath.tau
"cmath.tau" ) 来表示(τ) 常量. (由Lisa Roach在[issue 12345][107]中贡献, 详情请查看 [\*\*PEP
628\*\*](https://www.python.org/dev/peps/pep-0628) .)

新增的常量还有:
[`cmath.inf`](https://docs.python.org/3.6/library/cmath.html#cmath.inf
"cmath.inf" ) 和
[`cmath.nan`](https://docs.python.org/3.6/library/cmath.html#cmath.nan
"cmath.nan" ) 来对应
[`math.inf`](https://docs.python.org/3.6/library/math.html#math.inf "math.inf"
) 和 [`math.nan`](https://docs.python.org/3.6/library/math.html#math.nan
"math.nan" ), 另外还有
[`cmath.infj`](https://docs.python.org/3.6/library/cmath.html#cmath.infj
"cmath.infj" ) 和
[`cmath.nanj`](https://docs.python.org/3.6/library/cmath.html#cmath.nanj
"cmath.nanj" ) 来匹配复数表达式的格式化. (由Mark
Dickinson在[issue 23229][108]中贡献.)

### collections

新的[`Collection`](https://docs.python.org/3.6/library/collections.abc.htm
l#collections.abc.Collection "collections.abc.Collection" ) 抽象基类已经增加到可表示大小的迭代器类里面
(由Ivan Levkivskyi贡献, 文档由Neil Girdhar在[issue
27598](https://bugs.python.org/issue27598)中贡献.)

新的[`Reversible`](https://docs.python.org/3.6/library/collections.abc.htm
l#collections.abc.Reversible "collections.abc.Reversible" ) 有迭代器类的抽象基类 [`__reversed__()`](htt
ps://docs.python.org/3.6/reference/datamodel.html#object.\_\_reversed\_\_
"object.\_\_reversed\_\_" ) 方法. (由Ivan Levkivskyi在[issue
25987](https://bugs.python.org/issue25987)中贡献.)

新的 [`AsyncGenerator`](https://docs.python.org/3.6/library/collections.abc
.html#collections.abc.AsyncGenerator "collections.abc.AsyncGenerator" )
，是一种异步生成器类的抽象基类. (Contributed by Yury
Selivanov in [issue 28720][109].)

这个[`namedtuple()`](https://docs.python.org/3.6/library/collections.html#coll
ections.namedtuple "collections.namedtuple" ) 函数现在接受一个可选的关键字参数 _module_, 它指定用于`__module__`返回的命名元组类的属性. (由Raymond Hettinger
在[issue 17941][110]中贡献.

[`namedtuple()`](https://docs.python.org/3.6/library/collections.html#collections.namedtuple"collections.namedtuple" )的_verbose_ 和 _rename_ 参数是强制关键字参数. (由Raymond
Hettinger 在[issue 25628][111]中贡献.)

递归方法[`collections.deque`](https://docs.python.org/3.6/library/collection
s.html#collections.deque "collections.deque" ) 实例现在可使用pickle持久化存储.
(由Serhiy Storchaka在[issue
26482](https://bugs.python.org/issue26482)中贡献.)

### concurrent.futures¶

[`ThreadPoolExecutor`](https://docs.python.org/3.6/library/concurrent.futu
res.html#concurrent.futures.ThreadPoolExecutor
"concurrent.futures.ThreadPoolExecutor" ) 类的构造函数现在接受一个可选的 _thread\_name\_prefix_ 参数，使自定义线程池中的线程名字成为可能 (由Gregory P. Smith 贡献在[issue 27664][112].)

### contextlib¶

[`contextlib.AbstractContextManager`](https://docs.python.org/3.6/library/
contextlib.html#contextlib.AbstractContextManager
"contextlib.AbstractContextManager" ) 类已经成为上下文管理器中的一个抽象基类
。它提供了一个合理的默认返回`self`的 __enter__()方法，和一个 __exit__()抽象方法. 一个匹配的类[\`typing.Context
Manager\`](https://docs.python.org/3.6/library/typing.html#typing.ContextManage
r "typing.ContextManager" ).被添加到
[`typing`](https://docs.python.org/3.6/library/typing.html#module-typing
"typing: Support for type hints \(see PEP 484\)." )。 (由 Brett Cannon 贡献在[issue
25609](https://bugs.python.org/issue25609).)

### datetime¶

[`datetime`](https://docs.python.org/3.6/library/datetime.html#datetime.datetime "datetime.datetime" )和[`time`](https://docs.python.org/3.6/library/datetime.html#datetime.time)类有了新的`fold`属性，用于在必要时消除本地时间歧义。[`datetime`](https://docs.python.org/3.6/library/datetime.html#module-datetime"datetime: Basic date and time types." )中的许多功能已经更新来支持本地时间消歧。详细信息请参阅[Local Time Disambiguation](https://docs.python.org/3.6/whatsnew/3.6.html#whatsnew36-pep495)。（由Alexander Belopolsky在[issue 24773](https://bugs.python.org/issue24773)中提供。）

[`datetime.strftime()`](https://docs.python.org/3.6/library/datetime.html\#datetime.datetime.strftime "datetime.datetime.strftime" )和[`date.strftime()`](https://docs.python.org/3.6/library/datetime.html#datetime.date.strftime"datetime.date.strftime" )方法现在支持ISO 8601标准中的日期指令`%G`,`%u` and `%V`。（由Ashley Anderson在[issue 12006](https://bugs.python.org/issue12006)中提供。）

[`datetime.isoformat()`](https://docs.python.org/3.6/library/datetime.html# datetime.datetime.isoformat "datetime.datetime.isoformat" )现在接受一个可选的_timespec_参数，该参数指定函数中要包含的时间值的附加组件数。（由Alessandro Cucci和 Alexander Belopolsky在[issue 19475](https://bugs.python.org/issue19475)中提供。）

[`datetime.combine()`](https://docs.python.org/3.6/library/datetime.html#datetime.datetime.combine "datetime.datetime.combine" )现在接受一个可选的_tzinfo_参数。（由Alexander Belopolsky在[issue 27661](https://bugs.python.org/issue27661)中提供。

### decimal¶

新的[`Decimal.as_integer_ratio()`](https://docs.python.org/3.6/library/decimal.html#decimal.Decimal.as\_integer\_ratio "decimal.Decimal.as\_integer\_ratio" )方法返回一对整数`(n, d)`，将给定的[`Decimal`](https://docs.python.org/3.6/library/decimal.html#decimal.Decimal"decimal.Decimal" )转换成最简分数，且分母为正：

```

    >>> Decimal('-3.14').as_integer_ratio()
    (-157, 50)

```

（由Stefan Krah和Mark Dickinson在[issue 25928](https://bugs.python.org/issue25928)中提供。）

### distutils¶

`default_format`属性已经从`distutils.command.sdist.sdist`中移除。同时，`formats`属性被设置为`['gztar']`。尽管这无法预见，但是所有使用`default_format`的代码都需要修改。详细信息参见[issue 27819](https://bugs.python.org/issue27819)。

### email¶

允许通过关键字_policy_为各种函数构造的新emailAPI不再是临时的。[`email`](https://docs.python.org/3.6/library/email.html#module-email "email:Package supporting the parsing, manipulating, and generating email messages.")文档重组重写之后着重于新的API，但仍然保留了之前的API的文档。（由R.David Murray在[issue 24277][113]中提供。）

[`email.mime`](https://docs.python.org/3.6/library/email.mime.html#module-email.mime "email.mime: Build MIME messages." )类现在都接受可选择的关键字_policy_。（由Berker Peksag在[issue 27331](https://bugs.python.org/issue27331)中提供。）

[`DecodedGenerator`](https://docs.python.org/3.6/library/email.generator.html#email.generator.DecodedGenerator "email.generator.DecodedGenerator" )现在支持关键字_policy_。

有一个新的[`policy`](https://docs.python.org/3.6/library/email.policy.html#module-email.policy "email.policy: Controlling the parsing and generating ofmessages" )属性[`message_factory`](https://docs.python.org/3.6/library/email.policy.html#email.policy.Policy.message\_factory"email.policy.Policy.message\_factory" )，它控制当解释器创造新的消息对象时使用哪一个默认的类。 [`email.policy.compat32`](https://docs.python.org/3.6/library/email.policy.html#email.policy.compat32 "email.policy.compat32" ) policy使用[`Message`](https://docs.python.org/3.6/library/email.compat32-message.html#email.message.Message"email.message.Message" )类，new policies使用[`EmailMessage`](https://docs.python.org/3.6/library/email.message.html#email.message.EmailMessage"email.message.EmailMessage" )类。（由R. David Murray在[issue 20476](https://bugs.python.org/issue20476)中提供。）

### encodings¶

在Windows中，增加了`'oem'`编码来使用`CP_OEMCP`，给现有的`'mbcs'`编码增加了别名`'ansi'`，它使用`CP_ACP`代码页。（由Steve Dower在[issue 27959](https://bugs.python.org/issue27959)中提供。）

### enum¶

两个新的枚举基类已经添加到[`enum`][114]模块中：[`Flag`][115]和`IntFlags`。两种类都是用来定义能用位操作符组合的常数。（由Ethan Furman在[issue 23591](https://bugs.python.org/issue23591)中提供。）

许多标准库模块已经更新来为它们的常数使用`IntFlags`类。

新的[`enum.auto`](https://docs.python.org/3.6/library/enum.html#enum.auto"enum.auto" )值可以用来为enum成分自动分配值：

```

    >>> from enum import Enum, auto
    >>> class Color(Enum):
    ...     red = auto()
    ...     blue = auto()
    ...     green = auto()
    ...
    >>> list(Color)
    [<Color.red: 1>, <Color.blue: 2>, <Color.green: 3>]

```

### faulthandler

在Windows平台, 
[`faulthandler`](https://docs.python.org/3.6/library/faulthandler.html#module-
faulthandler "faulthandler: Dump the Python traceback." )模块安装了一个指示Windows异常的句柄 : 详情可见于 [`faulthandler.enable()`](https://docs.p
ython.org/3.6/library/faulthandler.html#faulthandler.enable
"faulthandler.enable" ). (由 Victor Stinner 在 [issue
23848](https://bugs.python.org/issue23848)中贡献.)

### fileinput

[`hook_encoded()`](https://docs.python.org/3.6/library/fileinput.html#fileinpu
t.hook\_encoded "fileinput.hook\_encoded" ) 模块现在可支持errors参数.
(由 Joseph Hackman 在 [issue
25788](https://bugs.python.org/issue25788)贡献.)

### hashlib

[`hashlib`](https://docs.python.org/3.6/library/hashlib-blake2.html#module-
hashlib "hashlib: BLAKE2 hash function for Python" ) 支持 OpenSSL 1.1.0。
推荐使用的最低版本为1.0.2. (由Christian Heimes 在
[issue 26470][116]贡献.)

BLAKE2 hash 函数也被收录进这一模块.
[`blake2b()`](https://docs.python.org/3.6/library/hashlib-
blake2.html#hashlib.blake2b "hashlib.blake2b" ) 与
[`blake2s()`](https://docs.python.org/3.6/library/hashlib-
blake2.html#hashlib.blake2s "hashlib.blake2s" ) 将长期支持BLAKE2的所有特性. (由Christian Heimes in
依据Dmitry
Chestnykh 和 Samuel Neves的代码在[issue 26798][117] 贡献. 文档由Dmitry Chestnykh撰写.)

新增SHA-3哈希函数   `sha3_224()`, `sha3_256()`, `sha3_384()`,
`sha3_512()`,  与SHAKE 哈希函数 `shake_128()` 、`shake_256()` . (由Christian Heimes 在 [issue
16113](https://bugs.python.org/issue16113)贡献. Keccak 代码包由Guido
Bertoni, Joan Daemen, Michaël Peeters, Gilles Van Assche, and Ronny Van Keer撰写.)

基于密码的密钥导出函数
[`scrypt()`](https://docs.python.org/3.6/library/hashlib.html#hashlib.scrypt
"hashlib.scrypt" ) 可使用 OpenSSL 1.1.0 或更新版本. (由Christian Heimes 在 [issue 27928][118]中贡献.)

### http.client

[`HTTPConnection.request()`](https://docs.python.org/3.6/library/http.client.h
tml#http.client.HTTPConnection.request "http.client.HTTPConnection.request" )
and [`endheaders()`](https://docs.python.org/3.6/library/http.client.html#http
.client.HTTPConnection.endheaders "http.client.HTTPConnection.endheaders" )
将全部支持分块编码请求体. (由Demian
Brecht and Rolf Krahl 在 [issue 12319][119]中贡献.)

### idlelib and IDLE

对idle包做了现代化的改进与重构，使得IDLE更美观、更好用的同时令编程更易于理解、测试与改进。在IDLE的美化方面，特备针对Linux和Mac用户，我们在大多数对话框上应用了ttk插件。总之，IDLE将不再支持tcl/tk 8.4。现在要求有 tcl/tk 8.5 或 8.6。我们建议在使用时运行最新的版本.

“现代化”包括对idlelib模块的重命名和整合.重命名文件部分大写的名字是与之前版本类似的命名，例如，Tkinter和TkFont对应3.0版本中的Tkinter和tkinter.font 。因此，对idlelib在3.5环境下导入的文件通常不会工作在3.6。至少一个模块的名称需要改变（见idlelib / readme.txt），有时甚至会更多。(名称变更由Al Swiegart 与 Terry Reedy 在
[issue 24225][120]中贡献. 大多数idle补丁均会加入这一改进。)

做点补充，最终的结果是，一些idlelib类会更容易使用，将具有更好的API文档与字符串的解释。其他有用的信息会在可用时被及时添加到idlelib。

### importlib

当无法找到被导入模块时会跳出一个新的异常提示 [`ModuleNotFoundError`](https://docs.pytho
n.org/3.6/library/exceptions.html#ModuleNotFoundError "ModuleNotFoundError" )
([`ImportError`](https://docs.python.org/3.6/library/exceptions.ht
ml#ImportError "ImportError" )的一个子类) . 检测`ImportError`的代码（try-except）依然会工作。 (由 Eric Snow 在 [issue 15767][121]中贡献.)

[`importlib.util.LazyLoader`](https://docs.python.org/3.6/library/importlib.ht
ml#importlib.util.LazyLoader "importlib.util.LazyLoader" )在打包好的装载器上更名为 [\`create\_
module()\`](https://docs.python.org/3.6/library/importlib.html#importlib.abc.Lo
ader.create\_module "importlib.abc.Loader.create\_module" ) , 去除了 [`importlib.machinery.BuiltinImporter`](
https://docs.python.org/3.6/library/importlib.html#importlib.machinery.Builtin
Importer "importlib.machinery.BuiltinImporter" )与 [\`importlib.machinery.Ext
ensionFileLoader\`](https://docs.python.org/3.6/library/importlib.html#importli
b.machinery.ExtensionFileLoader "importlib.machinery.ExtensionFileLoader" )
不能同 [`importlib.util.LazyLoader`](https://docs.python.org/3.
6/library/importlib.html#importlib.util.LazyLoader "importlib.util.LazyLoader"
)一起使用的限制.

[`importlib.util.cache_from_source()`](https://docs.python.org/3.6/library/imp
ortlib.html#importlib.util.cache\_from\_source
"importlib.util.cache\_from\_source" ), [`importlib.util.source_from_cache()`](h
ttps://docs.python.org/3.6/library/importlib.html#importlib.util.source\_from\_c
ache "importlib.util.source\_from\_cache" ), 和 [\`importlib.util.spec\_from\_file
\_location()\`](https://docs.python.org/3.6/library/importlib.html#importlib.uti
l.spec\_from\_file\_location "importlib.util.spec\_from\_file\_location" ) 现在可接受[path-like object](https://docs.python.org/3.6/glossary.html#term-
path-like-object).

### inspect

[`inspect.signature()`](https://docs.python.org/3.6/library/inspect.html#inspect.signature "inspect.signature" )函数现在支持报告由编译器为推导式和生成器表达式范围生成的隐式`.0`参数，就好像它们是名为`implicit0`的仅位置参数。 (由Jelle Zijlstra在[issue 19611](https://bugs.python.org/issue19611)中贡献。)

为了减少从Python 2.7和旧版[\`inspect.getargspec()\`](https://docs.python.org/3.6/library/inspect.html#inspect.getargspec "inspect.getargspec" ) API升级时的代码改动，先前记录的对[\`inspect.getfullargspec()\`](https://docs.python.org/3.6/library/inspect.html#inspect.getfullargspec "inspect.getfullargspec" )对弃用已经撤销。虽然这个函数对于单个/源Python 2/3代码库是很方便的，但是更丰富的[\`inspect.signature()\`](https://docs.python.org/3.6/library/inspect.html#inspect.signature "inspect.signature" )接口仍然是新代码的推荐方法。(由Nick Coghlan在[issue 27172](https://bugs.python.org/issue27172)中贡献)

### json

[`json.load()`](https://docs.python.org/3.6/library/json.html#json.load "json.load" )和[`json.loads()`](https://docs.python.org/3.6/library/json.html#json.loads "json.loads" )现在支持二进制输入。已编码的JSON应该用UTF-8, UTF-16, 或者UTF-32来表示。(由Serhiy Storchaka在[issue 17909](https://bugs.python.org/issue17909)中贡献。)

### logging

已添加新的[`WatchedFileHandler.reopenIfNeeded()`](https://docs.python.org/3.6/library/logging.handlers.html#logging.handlers.WatchedFileHandler.reopenIfNeeded "logging.handlers.WatchedFileHandler.reopenIfNeeded" )方法来增加检查日志文件是否需要被重新打开的能力。(由Marian Horban在[issue 24884](https://bugs.python.org/issue24884)中贡献。)

### math

已添加tau (τ)常量到[`math`](https://docs.python.org/3.6/library/math.html#module-math)和[`cmath`](https://docs.python.org/3.6/library/cmath.html#module-cmath)模块中。(由Lisa Roach在[issue 12345](https://bugs.python.org/issue12345)中贡献，见[\*\*PEP 628\*\*](https://www.python.org/dev/peps/pep-0628)以了解详情。)

### multiprocessing

`multiprocessing.Manager()`返回的[Proxy对象](https://docs.python.org/3.6/library/multiprocessing.html#multiprocessing-proxy-objects)现在可以被嵌套。(由Davin Potts在[issue 6766](https://bugs.python.org/issue6766)中贡献。)

### os

见[PEP 519](https://docs.python.org/3.6/whatsnew/3.6.html#whatsnew36-pep519)的摘要，以获取关于[`os`](https://docs.python.org/3.6/library/os.html#module-os)和[`os.path`](https://docs.python.org/3.6/library/os.path.html#module-os.path "os.path: Operations on pathnames." )模块现在如何支持[类路径（path-like）对象](https://docs.python.org/3.6/glossary.html#term-path-like-object)的详情。

[`scandir()`](https://docs.python.org/3.6/library/os.html#os.scandir "os.scandir" )现在支持Windows上的[`bytes`](https://docs.python.org/3.6/library/functions.html#bytes)路径。

一个新的[`close()`](https://docs.python.org/3.6/library/os.html#os.scandir.close "os.scandir.close")方法允许显式关闭一个[`scandir()`](https://docs.python.org/3.6/library/os.html#os.scandir "os.scandir" )迭代器。[`scandir()`](https://docs.python.org/3.6/library/os.html#os.scandir "os.scandir" )迭代器现在支持[上下文管理器](https://docs.python.org/3.6/glossary.html#term-context-manager)协议。如果一个`scandir()`迭代器既不是被耗尽，也不是被显式关闭，那么在其析构函数中将会抛出一个[`ResourceWarning`](https://docs.python.org/3.6/library/exceptions.html#ResourceWarning "ResourceWarning" )。(由Serhiy Storchaka在[issue 25994](https://bugs.python.org/issue25994)中贡献。)

在Linux上，[`os.urandom()`](https://docs.python.org/3.6/library/os.html#os.urandom "os.urandom" )现在会阻塞，直到系统的urandom entropy池被初始化，以提高安全性。见[\*\*PEP 524\*\*](https://www.python.org/dev/peps/pep-0524)以了解缘由。

Linux的`getrandom()`系统调用(获取随机字节)现在作为新的[`os.getrandom()`](https://docs.python.org/3.6/library/os.html#os.getrandom "os.getrandom" )函数公开。(由Victor Stinner贡献，属于[\*\*PEP 524\*\*](https://www.python.org/dev/peps/pep-0524)部分)

### pathlib

[`pathlib`](https://docs.python.org/3.6/library/pathlib.html#module-pathlib "pathlib: Object-oriented filesystem paths" )现在支持[类路径（path-like）对象](https://docs.python.org/3.6/glossary.html#term-path-like-object)。
(由Brett Cannon在[issue 27186](https://bugs.python.org/issue27186)中贡献。)

见PEP 519的摘要了解详情。

### pdb

[`Pdb`](https://docs.python.org/3.6/library/pdb.html#pdb.Pdb)类构造器有一个新的可选_readrc_参数，用来控制是否应该读取`.pdbrc`文件。

### pickle

对象，现在可以使用关键参数 `__new__`进行持久存储[pickle协议](https://docs.python.org/3.6/library/pickle.html
# pickle-protocols)超过已有的协议版本4.
支持这种情况. (由Serhiy Storchaka在[issue
24164](https://bugs.python.org/issue24164)的贡献。)

### pickletools

现在[`pickletools.dis()`](https://docs.python.org/3.6/library/pickletools.html#pic
kletools.dis "pickletools.dis" ) 输出的隐含备注是`MEMOIZE`操作码的索引。 (由Serhiy Storchaka在[issue
25382](https://bugs.python.org/issue25382)的贡献。)

### pydoc

[`pydoc`](https://docs.python.org/3.6/library/pydoc.html#module-pydoc
"pydoc: Documentation generator and online help system." )模块已经学会遵守`MANPAGER`环境变量。 (由Matthias Klose
在[issue 8637][130]的贡献。)

[`help()`][131]
和 [`pydoc`](https://docs.python.org/3.6/library/pydoc.html#module-pydoc
"pydoc: Documentation generator and online help system." ) 现在能用指定的元组字段定义的顺序来显示列表，而不是按字母顺序。
(由Raymond Hettinger在[issue
24879](https://bugs.python.org/issue24879)的贡献。)

### random

这个新[`choices()`](https://docs.python.org/3.6/library/random.html#random.choices
"random.choices" ) 函数返回一个指定元素大小的列表，通过可选权重给出它的规模。(由Raymond Hettinger在[issue 18844][132]的贡献。)

### re

在正则表达式中，增加对 spans 修饰符的支持。 示例:
`'(?i:p)ython'` 匹配 `'python'` 和 `'Python'`, 但不匹配 `'PYTHON'`；
`'(?i)g(?-i:v)r'` 匹配 `'GvR'` 和 `'gvr'`, 但不匹配 `'GVR'`。(由Serhiy Storchaka在 [issue 433028][133] 的贡献。)

匹配对象组可通过 `__getitem__`访问, 它就等价于 `group()`。因此， 现在`mo['name']` 就等价于 `mo.group('name')`。
(由 Eric Smith 在[issue
24454](https://bugs.python.org/issue24454)的贡献。)

现在，`Match` 对象支持 [`index-like objects`](https://docs.python.org/3.6
/reference/datamodel.html#object.\_\_index\_\_ "object.\_\_index\_\_" ) 一样的组索引。 (由Jeroen Demeyer and Xiang Zhang 在 [issue
27177](https://bugs.python.org/issue27177)的贡献。)

### readline

增加了 [`set_auto_history()`](https://docs.python.org/3.6/library/readline.html#readline.set_auto_history) 启用或停用
自动把输入加到历史列表中。 (由Tyler Crompton 在[issue 26870][134]的贡献。)

### rlcompleter

现在，除非前缀开始使用下划线，否则私有和特殊属性名称就被忽略。在一些完成的关键字后面添加一个空格或冒号。
(由Serhiy Storchaka 在[issue
25011](https://bugs.python.org/issue25011) 和 [issue
25209](https://bugs.python.org/issue25209)的贡献。)

### shlex

[`shlex`](https://docs.python.org/3.6/library/shlex.html#shlex.shlex
"shlex.shlex" ) 已经大大 [改进shell的兼容性](https://docs.python.org/3.6/library/shlex.html#improved-shell-compatibility) ,通过新的 _punctuation\_chars_ 参数来控制，哪些字符被作为标点符号。 (由Vinay Sajip 在[issue
1521950](https://bugs.python.org/issue1521950)的贡献。)

### site

在一个.pth文件里，当指定路径添加到[`sys.path`](https://docs.python.org/3.6/library/sys.html#sys.path "sys.path"
) 中, 可能你现在要在目录之上指定文件路径 (例如：zip文件)。 (由Wolfgang Langner 在[issue
26587](https://bugs.python.org/issue26587)的贡献)。

### sqlite3

[`sqlite3.Cursor.lastrowid`](https://docs.python.org/3.6/library/sqlite3.html\#
sqlite3.Cursor.lastrowid "sqlite3.Cursor.lastrowid" ) 现在支持
`REPLACE` 声明. (贡献者： Alex LordThorsen 于 [issue
16864](https://bugs.python.org/issue16864).)

### socket

[`ioctl()`](https://docs.python.org/3.6/library/socket.html#socket.socket.
ioctl "socket.socket.ioctl" ) 函数现在支持 [\`SIO\_LOOPBACK\_FAST\_PA
TH\`](https://docs.python.org/3.6/library/socket.html#socket.SIO\_LOOPBACK\_FAST\_
PATH "socket.SIO\_LOOPBACK\_FAST\_PATH" ) 控制代码. (贡献者： Daniel
Stokes 于 [issue 26536][135].)

[`getsockopt()`](https://docs.python.org/3.6/library/socket.html#socket.so
cket.getsockopt "socket.socket.getsockopt" ) 常量 `SO_DOMAIN`,
`SO_PROTOCOL`, `SO_PEERSEC`, 和 `SO_PASSSEC` 现已被支持. (贡献者：Christian Heimes 于 [issue 26907][136].)

[`setsockopt()`](https://docs.python.org/3.6/library/socket.html#socket.so
cket.setsockopt "socket.socket.setsockopt" ) 现在支持
`setsockopt(level, optname, None, optlen: int)` 形式. (贡献者：
Christian Heimes 于 [issue 27744][137].)

socket 模块现在支持地址族
[`AF_ALG`](https://docs.python.org/3.6/library/socket.html#socket.AF\_ALG
"socket.AF\_ALG" ) 来和Linux Kernel crypto API连接. 添加了`ALG_*`,
`SOL_ALG` 和 [`sendmsg_afalg()`](https://docs.python.org/3.6/library/socket.h
tml#socket.socket.sendmsg\_afalg "socket.socket.sendmsg\_afalg" ).
(贡献者： Christian Heimes 于 [issue
27744](https://bugs.python.org/issue27744) with support from Victor Stinner.)

### socketserver

基于
[`socketserver`](https://docs.python.org/3.6/library/socketserver.html#module-
socketserver "socketserver: A framework for network servers." ) 模块,
包括了
[`http.server`](https://docs.python.org/3.6/library/http.server.html#module-
http.server "http.server: HTTP server and request handlers." ),
[`xmlrpc.server`](https://docs.python.org/3.6/library/xmlrpc.server.html#module-xmlrpc.server "xmlrpc.server: Basic XML-RPC server implementations." )
和 [`wsgiref.simple_server`](https://docs.python.org/3.6/library/wsgiref.html#module-wsgiref.simple_server)的Servers 现在支持 [context
manager](https://docs.python.org/3.6/glossary.html#term-context-manager)
protocol. (Contributed by Aviv Palivoda in [issue
26404](https://bugs.python.org/issue26404).)

[`StreamRequestHandler`](https://docs.python.org/3.6/
library/socketserver.html#socketserver.StreamRequestHandler
"socketserver.StreamRequestHandler" ) 类的 `wfile` 属性 现在实现 the [\`io.Buffered
IOBase\`](https://docs.python.org/3.6/library/io.html#io.BufferedIOBase
"io.BufferedIOBase" ) 可写接口. 特别地, 调用 [`write()`](h
ttps://docs.python.org/3.6/library/io.html#io.BufferedIOBase.write
"io.BufferedIOBase.write" ) 现在可以保证完全发送数据.
(贡献者： Martin Panter 于 [issue
26721](https://bugs.python.org/issue26721).)

### ssl

[`ssl`][139] 已支持 OpenSSL 1.1.0. 最低推荐版本号是 1.0.2. (贡献者： Christian Heimes 于 [issue
26470](https://bugs.python.org/issue26470).)

3DES 已从默认密码套件default cipher suites中删除
添加了密码套件ChaCha20 Poly1305. (贡献者： Christian Heimes 于 [issue
27850](https://bugs.python.org/issue27850) 和 [issue
27766](https://bugs.python.org/issue27766).)

[`SSLContext`](https://docs.python.org/3.6/library/ssl.html#ssl.SSLContext
"ssl.SSLContext" ) 有更好的默认配置选项和密码.
(贡献者： Christian Heimes 于 [issue
28043](https://bugs.python.org/issue28043).)

SSL session 可以使用新的 [`SSLSession`](https://docs.python.org/3.6/library/ssl.html#ssl.SSLSession
"ssl.SSLSession" ) 类从一个客户端连接复制到另一个客户端连接. TLS会话恢复可以加速初始握手，减少延迟并提高性能 (贡献者： Christian
Heimes 于 [issue 19500][138] ，基于Alex Warhawk的草案.)


新的 [`get_ciphers()`](https://docs.python.org/3.6/library/ssl.html#ssl.SSL
Context.get\_ciphers "ssl.SSLContext.get\_ciphers" ) 方法可以用来得到一个
按密码优先级顺序的已启用密码列表.

所有的 constants and flags 已转换为
[`IntEnum`](https://docs.python.org/3.6/library/enum.html#enum.IntEnum
"enum.IntEnum" ) 和 `IntFlags`. (贡献者 ：Christian Heimes 于 [issue
28025](https://bugs.python.org/issue28025).)

Server 和 client-side 添加 [`SSLContext`](https://docs.python.org/3.6/library/ssl.html#ssl.SSLContext
"ssl.SSLContext" ) 特定 TLS 协议. (贡献者： Christian Heimes 于 [issue
28085](https://bugs.python.org/issue28085).)

### statistics

已添加一个新的[`harmonic_mean()`](https://docs.python.org/3.6/library/statistics.html\#statistics.harmonic\_mean "statistics.harmonic\_mean" )函数。 (由Steven D'Aprano在[issue 27181](https://bugs.python.org/issue27181)中贡献。)

### struct

通过`'e'`格式限定符，[`struct`](https://docs.python.org/3.6/library/struct.html#module-struct "struct: Interpret bytes as packed binary data." )现在支持IEEE 754半精度浮点。(由Eli Stevens, Mark Dickinson在[issue 11734][139]中贡献。)

### subprocess

当子系统仍然运行着的时候，[`subprocess.Popen`](https://docs.python.org/3.6/library/subprocess.html#subprocess.Popen "subprocess.Popen" )析构函数现在发出一个[`ResourceWarning`](https://docs.python.org/3.6/library/exceptions.html#ResourceWarning "ResourceWarning" )警告。使用上下文管理器协议(`with proc: ...`)或者明确调用[`wait()`](https://docs.python.org/3.6/library/subprocess.html#subprocess.Popen.wait "subprocess.Popen.wait" )方法来读取子进程的退出状态。
(由Victor Stinner在[issue 26741](https://bugs.python.org/issue26741)中贡献。)

[`subprocess.Popen`](https://docs.python.org/3.6/library/subprocess.html#subprocess.Popen "subprocess.Popen" )构造函数和所有传给它的函数现在接受_encoding_和_errors_参数。指定其中一个将会为_stdin_, _stdout_和_stderr_流启用文本模式。(由Steve Dower在[issue 6135](https://bugs.python.org/issue6135)中贡献。)

### sys

新的[`getfilesystemencodeerrors()`](https://docs.python.org/3.6/library/sys.html#sys.getfilesystemencodeerrors "sys.getfilesystemencodeerrors" )函数返回用来在Unicode文件名和字节文件名之间转换的错误模式的名字。(由Steve Dower在[issue 27781](https://bugs.python.org/issue27781)中贡献。)

在Windows上，[`getwindowsversion()`](https://docs.python.org/3.6/library/sys.html#sys.getwindowsversion "sys.getwindowsversion" )函数的返回值现在包含了_platform\_version_字段，它包含当前操作系统准确的最大版本、最小版本和构建版本，而不是模拟进程的版本 (由Steve Dower在[issue 27932][140]中贡献。)

### telnetlib

[`Telnet`](https://docs.python.org/3.6/library/telnetlib.html#telnetlib.Telnet "telnetlib.Telnet" )现在是一个上下文管理器了 (由Stéphane Wirtel在[issue 25485][141]中贡献)。

### time

[`struct_time`](https://docs.python.org/3.6/library/time.html#time.struct_time "time.struct_time" )的属性`tm_gmtoff`和`tm_zone`现在在所有平台上都可用了。

### timeit

已添加新的[`Timer.autorange()`](https://docs.python.org/3.6/library/timeit.html\#timeit.Timer.autorange "timeit.Timer.autorange" )便捷方法来反复调用[`Timer.timeit()`](https://docs.python.org/3.6/library/timeit.html#timeit.Timer.timeit "timeit.Timer.timeit" )，以便总运行时间大于或等于200毫秒(由Steven D'Aprano在[issue 6422][142]中贡献。)

当在最佳和最差时间之间存在一个很大（4x）的差时，[`timeit`](https://docs.python.org/3.6/library/timeit.html#module-timeit "timeit: Measure the execution time of small code snippets." )现在会告警。(由Serhiy Storchaka在[issue 23552][143]中贡献。)

### tkinter

在`tkinter.Variable`类中添加了方法 `trace_add（）`，`trace_remove（）`和`trace_info（）`。 它们代替了之前版本中的 `trace_variable（）`，`trace（）`，`trace_vdelete（）`和 `trace_vinfo（）`方法，这些方法使用过时的Tcl命令，而在未来版本的Tcl中，这些Tcl命令可能不起作用。（由Serhiy Storchaka在[issue 22115](https://bugs.python.org/issue22115) 提供）。

### traceback

traceback模块和解释器内置的异常展示现在都省略回溯中重复行的长串，如以下例子所示：

```

    >>> def f(): f()
    ...
    >>> f()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 1, in f
      File "<stdin>", line 1, in f
      File "<stdin>", line 1, in f
      [Previous line repeated 995 more times]
    RecursionError: maximum recursion depth exceeded

```

(由Emanuel Barry在[issue 26823](https://bugs.python.org/issue26823)中贡献。)

### tracemalloc

[`tracemalloc`](https://docs.python.org/3.6/library/tracemalloc.html#module-tracemalloc)模块限制支持跟踪多个不同地址空间内的内存分配。

已添加新的[`DomainFilter`](https://docs.python.org/3.6/library/tracemalloc.html#tracemalloc.DomainFilter "tracemalloc.DomainFilter" )过滤器类来根据地址空间（域）过滤块跟踪。

(由Victor Stinner在[issue 26588](https://bugs.python.org/issue26588)中贡献。)

### typing

从Python 3.6起，[`typing`](https://docs.python.org/3.6/library/typing.html#module-typing "typing: Support for type hints \(see PEP 484\)." )模块不再是临时的了，可以把它的API当成稳定版本使用。

由于[`typing`](https://docs.python.org/3.6/library/typing.html#module-typing "typing: Support for type hints \(see PEP 484\)." )模块在Python 3.5中是[临时的](https://docs.python.org/3.6/glossary.html#term-provisional-api)，因此Python 3.6中引入的所有变动也已向后移植到Python 3.5.x。

[`typing`](https://docs.python.org/3.6/library/typing.html#module-typing "typing: Support for type hints \(see PEP 484\)." )模块很大提高了对范型别名的支持。例如，`Dict[str, Tuple[S, T]]`现在是一个有效的类型注释了。(由Guido van Rossum在[Github #195](https://github.com/python/typing/pull/195)中贡献。)

已添加[`typing.ContextManager`](https://docs.python.org/3.6/library/typing.html#typing.ContextManager "typing.ContextManager" )类来展示[`contextlib.AbstractContextManager`](https://docs.python.org/3.6/library/contextlib.html#contextlib.AbstractContextManager "contextlib.AbstractContextManager" )。 (由Brett Cannon在[issue 25609](https://bugs.python.org/issue25609)中贡献。)

已添加[`typing.Collection`](https://docs.python.org/3.6/library/typing.html#typing.Collection "typing.Collection" )类来展示[`collections.abc.Collection`](https://docs.python.org/3.6/library/collections.abc.html#collections.abc.Collection "collections.abc.Collection" )。 (由Ivan Levkivskyi在[issue 27598](https://bugs.python.org/issue27598)中贡献。)

已添加[`typing.ClassVar`](https://docs.python.org/3.6/library/typing.html#typing.ClassVar "typing.ClassVar" )类型构造，来标识类变量。如[\*\*PEP 526\*\*](https://www.python.org/dev/peps/pep-0526)中所述，封装在ClassVar中的一个变量注释暗示着一个给定的属性打算作为一个类变量使用，并且不应该在那个类的实例上设置它。(由Ivan Levkivskyi在[Github #280](https://github.com/python/typing/issues/280)中贡献。)

一个新的[`TYPE_CHECKING`](https://docs.python.org/3.6/library/typing.html#typing.TYPE\_CHECKING "typing.TYPE\_CHECKING" )常量被静态类型检查器假设为`True`，但在运行时则为`False`。 (由Guido van Rossum在[Github #230](https://github.com/python/typing/issues/230)中贡献。)

已添加一个新的[`NewType()`](https://docs.python.org/3.6/library/typing.html#typing.NewType
"typing.NewType" )辅助函数来为注释创建轻量单值类型：

```

    from typing import NewType

    UserId = NewType('UserId', int)
    some_id = UserId(524313)

```

静态类型检查器将会把新的类型当成原始类型的一个子类。 (由Ivan Levkivskyi在[Github #189](https://github.com/python/typing/issues/189)中贡献。)

### unicodedata

[`unicodedata`](https://docs.python.org/3.6/library/unicodedata.html#module-unicodedata "unicodedata: Access the Unicode Database.")模块现在使用来自[Unicode 9.0.0][148]的数据。
(由Benjamin Peterson贡献)

### unittest.mock

[`Mock`](https://docs.python.org/3.6/library/unittest.mock.html#unittest.mock.Mock "unittest.mock.Mock" )类有以下改进：

  * 两个新的方法，[`Mock.assert_called()`][149]和[`Mock.assert_called_once()`][150]，用来检测是否调用了mock对象。(由Amit Saha在[issue 26323][151]中贡献。)
  * [`Mock.reset_mock()`][152]方法现在拥有两个可选的仅关键字参数： _return\_value_ 和 _side\_effect_。 (由Kushal Das在[issue 21271][153]中贡献。)

### urllib.request

如果一个HTTP请求具有一个文件或者可迭代请求体 (不同于一个bytes对象)，但是没有`Content-Length`头，那么现在`AbstractHTTPHandler`会回退到使用分块传输编码，而不是抛出一个错误。
(由Demian Brecht和Rolf Krahl在[issue 12319](https://bugs.python.org/issue12319)中贡献。)

### urllib.robotparser

[`RobotFileParser`](https://docs.python.org/3.6/library/urllib.robotparser.html#urllib.robotparser.RobotFileParser"urllib.robotparser.RobotFileParser" )现在支持`Crawl-delay`和`Request-rate`扩展。(由Nikolay Bogoychev在[issue 16099][154]中贡献。)

### venv

[`venv`][155]接受一个新的参数`--prompt`。这个参数为虚拟环境提供一个可替换的前缀。(在[issue22829](https://bugs.python.org/issue22829)中，由Łukasz Balcerzak提出，并由Stéphane Wirtel移植到3.6。)

### warnings

增加一个可选参数到 [`warnings.warn_explicit（）`](https://docs.python.org/3.6/library/warnings.html#warnings.warn_explici%0At)函数中：引发 [`ResourceWarning` ](https://docs.python.org/3.6/library/exceptions.html#Resource%0AWarning)的已销毁对象。 同时，属性也添加到 `warnings.WarningMessage` 中。（由Victor Stinner在[issue 26568](https://bugs.python.org/issue26568)和[issue 26567](https://bugs.python.org/issue26567)中提供）。

当引发[ `ResourceWarning` ](https://docs.python.org/3.6/library/exceptions.html)警告时，[`tracemalloc`](https://docs.python.org/3.6/library/tracemalloc.html#module-%0Atracemalloc) 模块就尝试检索分配了销毁对象的跟踪。

一个名为 `example.py`的例子
```
    import warnings

    def func():
        return open(__file__)

    f = func()
    f = None
```
输出命令 `python3.6 -Wd -X tracemalloc=5 example.py` :
```
    example.py:7: ResourceWarning: unclosed file <_io.TextIOWrapper name='example.py' mode='r' encoding='UTF-8'>
      f = None
    Object allocated at (most recent call first):
      File "example.py", lineno 4
          return open(__file__)
      File "example.py", lineno 6
          f = func()
```
“对象分配”跟踪是新的，并且只有当[`tracemalloc`](https://docs.python.org/3.6/library/tracemalloc.html#module-%0Atracemalloc)正在跟踪Python内存分配，并且[ `warnings`](https://docs.python.org/3.6/library/warnings.html#module-warnings) 模块已经导入时才会显示。

### winreg

新增64位整数类型
[`REG_QWORD`](https://docs.python.org/3.6/library/winreg.html#winreg.REG\_QWORD
"winreg.REG\_QWORD" ). (Contributed by Clement Rouault in [issue
23026](https://bugs.python.org/issue23026).)

### winsound

允许传递关键词参数到 [`Beep`](https://docs.python.org/3.6/library/winsound.html#winsound.Beep"winsound.Beep" ), [`MessageBeep`](https://docs.python.org/3.6/library/winsoun
d.html#winsound.MessageBeep "winsound.MessageBeep" ), 和 [`PlaySound`](https:
//docs.python.org/3.6/library/winsound.html#winsound.PlaySound"winsound.PlaySound" ) 方法。 ([issue 27982][156]).

### xmlrpc.client

[`xmlrpc.client`](https://docs.python.org/3.6/library/xmlrpc.client.html# module-xmlrpc.client "xmlrpc.client: XML-RPC clientaccess." ) 模块现支持解压由apache XML-RPC实现用于数字和'None'的附加数据类型。(Contributed by Serhiy Storchaka in
[issue 26885][157].)

### zipfile

一个新的类方法 [`ZipInfo.from_file()`](https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipInfo.from\_file "zipfile.ZipInfo.from\_file") 允许从文件系统文件中生成一个
[`ZipInfo`](https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipInfo
"zipfile.ZipInfo" ) 实例。
一个新的方法 [`ZipInfo.is_dir()`](https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipInfo.is\_dir"zipfile.ZipInfo.is\_dir" ) 能够用来检查[`ZipInfo`](https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipInfo"zipfile.ZipInfo" ) 实例是否表示一个目录。 (Contributed by Thomas Kluyver in [issue 26039][158].)

[`ZipFile.open()`](https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipFile.open "zipfile.ZipFile.open" ) 方法现在能够将数据写入到一个ZIP文件，也能够从中提取数据。 (Contributed by Thomas Kluyver in [issue 26039][159].)

### zlib

[`compress()`](https://docs.python.org/3.6/library/zlib.html#zlib.compress
"zlib.compress" ) 以及
[`decompress()`](https://docs.python.org/3.6/library/zlib.html#zlib.decompress
"zlib.decompress" ) 函数现接受关键词参数。 (Contributed by
Aviv Palivoda in [issue 26243][160] and Xiang
Zhang in [issue 16764][161] respectively.)

## 优化

  * Python解释器现在使用16位编码而不是字节码，这使得你可以实现多种操作码的优化。(由Demur Rumed，Serhiy Storchaka和Victor Stinner在[issue 26647][162] 以及 [issue 28050][163]中贡献。)
  * [`asyncio.Future`][164]类现在有一种优化的C实现。(由Yury Selivanov和INADA Naoki在[issue 26081][165]中贡献。)
  * [`asyncio.Task`][166]类现在有一种优化的C实现。(由Yury Selivanov在[issue 28544][167]中贡献。)
  * 在[`typing`][168]模块中通过各种改进的实施(如泛型类型的缓存)，使得其允许高达30倍的性能优化，并且减少了内存占用。
  * ASCII解码器的错误处理程序 `surrogateescape`, `ignore` 以及 `replace` 速度提高了60倍。(由Victor Stinner在[issue 24870][169]中贡献)。
  * ASCII和Latin1解码器的错误处理程序 `surrogateescape` 速度提高了3倍。(由Victor Stinner在[issue 25227][170]中贡献)。
  * UTF-8解码器的错误处理程序 `ignore`, `replace`, `surrogateescape`, `surrogatepass`速度提高了75倍。(由Victor Stinner在[issue 25267][171]中贡献)。
  * UTF-8解码器的错误处理程序`ignore`, `replace` and `surrogateescape`速度提高了15倍。由Victor Stinner在[issue 25301][172]中贡献)。
  * `bytes % args`现在速度可以提高到2倍。(由Victor Stinner在[issue 25349][173]中贡献)。
  * `bytearray % args` 现在速度是以前的2.5到5倍。(由Victor Stinner在[issue 25399][174]中贡献)。
  * 对[`bytes.fromhex()`][175]和[`bytearray.fromhex()`][176]进行优化:现在比以前快2x到3.5x倍。(由Victor Stinner在[issue 25401][177]中贡献)。
  * 对`bytes.replace(b'', b'.')`和`bytearray.replace(b'', b'.')`进行优化:速度可提高80%。 (由Josh Snider在[issue 26574][178]中贡献)。
  * 现在[`PyMem_Malloc()`][179]域([`PYMEM_DOMAIN_MEM`][180])的分配函数使用[pymalloc memory allocator][181]，而不是C库中的`malloc()`函数。 pymalloc分配器针对大小小于等于512字节、生命周期较短的对象进行了优化，而当需要占用较大内存块时则使用`malloc()`。 (由Victor Stinner在[issue 26249][182]中贡献)。
  * 当对大量小对象进行反序列化时，[`pickle.load()`][183]和[`pickle.loads()`][184]的速度可提高10%。(由Victor Stinner在[issue 27056][185]中贡献)。
  * 与传递[位置参数][187]相比，将[关键字参数][186]传递给函数会带来额外的开销。 现在在通过使用Argument Clinic实现的扩展功能中，这种开销将会显著的降低。(由Serhiy Storchaka在[issue 27574][188]中贡献)。
  * 对[`glob`][191]模块中的[`glob()`][189]及[`iglob()`][190]进行优化;使得它们现在大概快了3-6倍。(由Serhiy Storchaka在[issue 25596][192]中贡献)。
  * 使用[`os.scandir()`][194]对[`pathlib`][193]中的glob进行优化;使它大概快了1.5-4倍。(由Serhiy Storchaka在[issue 26032][195]中贡献)。
  * [`xml.etree.ElementTree`][196]中解析、迭代和深拷贝的性能有了显著的提高。 (由Serhiy Storchaka在[issue 25638][197]、[issue 25873][198]及[issue 25869][199]中贡献。)
  * 由浮点数和小数创建[`fractions.Fraction`][200]实例的速度提高了2到3倍。(由Serhiy Storchaka在[issue 25971][201]中贡献。)

## 构建和C API变动

  * Python现在构建需要工具链中的一些C99支持。最值得注意的是，Python现在使用标准的整型类型和宏来替代诸如`PY_LONG_LONG`的自定义宏。欲知详情，见[**PEP 7**](https://www.python.org/dev/peps/pep-0007)和[issue 17884](https://bugs.python.org/issue17884)。
  * 带Android NDK和Android API level 21 (Android 5.0 Lollilop) 或者更高版本交叉编译CPython可以成功运行。虽然Android尚未是一个支持的平台，但是Python测试套件在Android模拟器上运行只有大约16个测试错误。见Android元问题[issue 26865](https://bugs.python.org/issue26865)。
  * 已添加`--enable-optimizations`配置标志。打开它将会激活昂贵的优化，例如PGO。(原始补丁由Intel的Alecsandru Patrascu在[issue 26539](https://bugs.python.org/issue26539)中提出。)
  * 当[`PYMEM_DOMAIN_OBJ`](https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_OBJ) (例如：`PyObject_Malloc()`)和[`PYMEM_DOMAIN_MEM`](https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_MEM) (例如：[`PyMem_Malloc()`](https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc))域的分配函数被调用的时候，现在必须使用[GIL](https://docs.python.org/3.6/glossary.html#term-global-interpreter-lock)。
  * 新的[`Py_FinalizeEx()`](https://docs.python.org/3.6/c-api/init.html#c.Py_FinalizeEx) API，它表示是否清除缓冲数据失败。 (由Martin Panter在[issue 5319](https://bugs.python.org/issue5319)中贡献。)
  * [`PyArg_ParseTupleAndKeywords()`](https://docs.python.org/3.6/c-api/arg.html#c.PyArg_ParseTupleAndKeywords)现在支持[仅位置参数](https://docs.python.org/3.6/glossary.html#positional-only-parameter)。仅位置参数是由空名称定义的。 (由Serhiy Storchaka在[issue 26282](https://bugs.python.org/issue26282)中贡献)。
  * `PyTraceback_Print`方法现在省略了重复行的长串，变成`"[Previous line repeated {count} more times]"`。 (由Emanuel Barry在[issue 26823](https://bugs.python.org/issue26823)中贡献。)
  * 新的[`PyErr_SetImportErrorSubclass()`](https://docs.python.org/3.6/c-api/exceptions.html#c.PyErr_SetImportErrorSubclass)函数指定要引发的[`ImportError`](https://docs.python.org/3.6/library/exceptions.html#ImportError)的子类。 (由Eric Snow在[issue 15767](https://bugs.python.org/issue15767)中贡献。)
  * 新的[`PyErr_ResourceWarning()`](https://docs.python.org/3.6/c-api/exceptions.html#c.PyErr_ResourceWarning)函数可以被用来生成一个[`ResourceWarning`](https://docs.python.org/3.6/library/exceptions.html#ResourceWarning)，提供资源分配的来源。(由Victor Stinner在[issue 26567](https://bugs.python.org/issue26567)中贡献。)
  * 新的[`PyOS_FSPath()`](https://docs.python.org/3.6/c-api/sys.html#c.PyOS_FSPath)函数返回一个[类路径(path-like)对象](https://docs.python.org/3.6/glossary.html#term-path-like-object)的文件系统表示。(由Brett Cannon在[issue 27186](https://bugs.python.org/issue27186)中贡献。)
  * [`PyUnicode_FSConverter()`](https://docs.python.org/3.6/c-api/unicode.html#c.PyUnicode_FSConverter)和[`PyUnicode_FSDecoder()`](https://docs.python.org/3.6/c-api/unicode.html#c.PyUnicode_FSDecoder)函数现在会接受[类路径(path-like)对象](https://docs.python.org/3.6/glossary.html#term-path-like-object)。

## 其他改进
  * 当使用[`--version`](https://docs.python.org/3.6/using/cmdline.html#cmdoption--version) (短格式：[`-V`](https://docs.python.org/3.6/using/cmdline.html#cmdoption-V))两次的时候，Python打印[`sys.version`](https://docs.python.org/3.6/library/sys.html#sys.version)以获得详细信息。
```
$ ./python -VV

    Python 3.6.0b4+ (3.6:223967b49e49+, Nov 21 2016, 20:55:04)
    [GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.42.1)]
```


## 弃用的模块，包以及函数

### 新的关键词


在后续的Python 3.7 中，`async` 和 `await` 将作为关键字使用， 因此不再建议使用它们作为变量名,类名，函数名以及模块名称。
详见Python 3.5[\*\*PEP492\*\*](https://www.python.org/dev/peps/pep-0492).
从 Python 3.6 开始, `async`或`await`的使用将会引发[`DeprecationWarning`](https://doc
s.python.org/3.6/library/exceptions.html#DeprecationWarning
"DeprecationWarning" )

### 已弃用的 Python 行为


Python 3.7 中, 在生成器中引发[`StopIteration`](https://docs.python.org/3.6/library/exceptions.html#StopIteration "StopIteration")异常，现在将会导致[`DesprecationWarning`](https://docs.python.org/3.6/library/exceptions.html#DeprecationWarning "DeprecationWarning" ),
并且触发[\`RuntimeError\`](https://docs.python.org/3.6/library/exceptions.html#RuntimeError
"RuntimeError" ). 查阅[PEP 479: Change StopIteration handlinginside generators](https://docs.python.org/3.6/whatsnew/3.5.html#whatsnew-
pep-479)获得相关细节.



[`__aiter__()`](https://docs.python.org/3.6/reference/datamodel.html#objec
t.\_\_aiter\_\_ "object.\_\_aiter\_\_" )方法现在被期望返回异步迭代器，
而不是之前版本中的可等待迭代器，在 Python 3.6 中调用`__aiter__()`将会触发[`DeprecationWarning`](https://doc
s.python.org/3.6/library/exceptions.html#DeprecationWarning
"DeprecationWarning" )。 Python 3.7 将会移除其的后向兼容性。(Contributed by Yury Selivanov in [issue27243](https://bugs.python.org/issue27243).)


反斜杠对现在不能再作为有效的转义字符串使用，其会导致
[`DeprecationWarning`](https://docs.python.org/3.6/library/exceptions.html#Dep
recationWarning "DeprecationWarning" ). 在接下来的几个Python版本，其最终会成为一个语法错误
[`SyntaxError`](https://docs.python.org/3.6/library/exceptions.html#SyntaxErro
r "SyntaxError" )。(Contributed
by Emanuel Barry in [issue 27364][231].)



在执行相对引用, 且`__spec__` 或 `__package__`未定义时，转而调用`__name__` 和 `__path__`模块的行为
, 现在将会引发[`ImportWarning`](https://docs.python.org/3.6/library/exceptions.htm
l#ImportWarning "ImportWarning" )异常.  (Contributed by Rose Ames in [issue
25791](https://bugs.python.org/issue25791).)

### 弃用的 Python 模块，函数和方法

#### asynchat


为了支持[`asyncio`](https://docs.python.org/3.6/library/asyncio.html#module-asyncio
"asyncio: Asynchronous I/O, event loop, coroutines and tasks." )，
 [`asynchat`](https://docs.python.org/3.6/library/asynchat.html#module-asynchat
 "asynchat: Support for asynchronous command/response protocols." )已被弃用。(Contributed
 by Mariatta in [issue 25002][232].)

#### asyncore


为了支持[`asyncio`](https://docs.python.org/3.6/library/asyncio.html#module-asyncio
"asyncio: Asynchronous I/O, event loop, coroutines and tasks." )模块. [`asyncore`](https://docs.python.org/3.6/library/asyncore.html#module-
asyncore "asyncore: A base class for developing asynchronous socket handling
services." )模块已被弃用. (Contributed
by Mariatta in [issue 25002][233].),

#### dbm


与其他[`dbm`](https://docs.python.org/3.6/library/dbm.html#module-dbm
"dbm: Interfaces to various Unix "database" formats." )模块的的实现方式不同，
[`dbm.dumb`](https://docs.python.org/3.6/library/dbm.html#module-dbm.dumb
"dbm.dumb: Portable implementation of the simple DBM interface." )模块可以通过`'rw'`模式创建数据库
并允许通过`'r'`模式来修改数据库，不过这种行为现在将被弃用，在Python 3.8 将会被移除。(Contributed by Serhiy Storchaka in [issue
21708](https://bugs.python.org/issue21708).)

#### distutils

`Distribution`构造器中`extra_path`参数现在被认为是过时的，如果在 Python 3.6 中对其进行
设置的话将会引发一个警告。
对这个参数的支持将会在后续 Python 版本中移除。详情请参阅[issue
27919](https://bugs.python.org/issue27919)

#### grp


[`getgrgid()`](https://docs.python.org/3.6/library/grp.html#grp.getgrgid
"grp.getgrgid" )中对非整数参数的支持，现在已被移除。(Contributed by Serhiy Storchaka in
[issue 26129][234].)


#### importlib

为了用来在之前的版本中支持
[\`importlib.abc.Loader.exec\_module()\`](https://docs.python.org/3.6/library/importlib.html#im
portlib.abc.Loader.exec\_module "importlib.abc.Loader.exec\_module" )，
[`importlib.machinery.SourceFileLoader.load_module()`](https://docs.python
.org/3.6/library/importlib.html#importlib.machinery.SourceFileLoader.load\_modu
le "importlib.machinery.SourceFileLoader.load\_module" ) 和 [\`importlib.machin
ery.SourcelessFileLoader.load\_module()\`](https://docs.python.org/3.6/library/i
mportlib.html#importlib.machinery.SourcelessFileLoader.load\_module
"importlib.machinery.SourcelessFileLoader.load\_module" )，所以他们是
[`importlib`](https://docs.python.org/3.6/library/importlib.html#module-
importlib "importlib: The implementation of the import machinery." ) 中[\`importlib.abc.Loader.load\_module()\`](https://docs.python.org/3.6/library/importlib.html#importlib.abc.Loader.load\_module "importlib.abc.Loader.load\_module" ) 仅存的未被弃用的实现方式，
不过在 Python 3.6 中，其也已经被弃用。


[`importlib.machinery.WindowsRegistryFinder`](https://docs.python.org/3.6/
library/importlib.html#importlib.machinery.WindowsRegistryFinder
"importlib.machinery.WindowsRegistryFinder" )类已被弃用。 在 Python 3.6.0 中，它被移动到
[`sys.meta_path`](https://docs.python.org/3.6/library/sys.html#sys.meta\_path
"sys.meta\_path" )中， 不过在将来这可能会发生变化。

#### os


在[`os`][235]中对作为路径的通用[bytes-like
objects](https://docs.python.org/3.6/glossary.html#term-bytes-like-object)，[`compile()`](https://docs.python.org/3.6/library/functions.html#compile
"compile" )以及对类函数的非正式支持，现在已经弃用。


#### re


对正则式中间的内联标志 `(?letters)`的支持已被弃用，并将在后续 Python 版本中移除。对正则式开头的标志的支持仍然被保留。
(Contributed by Serhiy Storchaka in [issue 22493][237].)

#### ssl


OpenSSL 0.9.8, 1.0.0 和 1.0.1 已被弃用，不再支持。以后[`ssl`](https://docs.python.org/3.6/library/ssl.html#module-ssl
"ssl: TLS/SSL wrapper for socket objects" )模块至少需要 OpenSSL 1.0.2 或者 OpenSSL 1.1.0

为了支持`context`，
SSL的相关参数如：`certfile`, `keyfile` and `check_hostname`[`ftplib`](https://docs.python.org/3.6/library/ftplib.html#module-ftplib
"ftplib: FTP protocol client \(requires sockets\)." ),
[`http.client`](https://docs.python.org/3.6/library/http.client.html#module-
http.client "http.client: HTTP and HTTPS protocol client \(requires
sockets\)." ), [`imaplib`](https://docs.python.org/3.6/library/imaplib.html#module-imaplib "imaplib: IMAP4 protocol client \(requires sockets\)." ),
[`poplib`](https://docs.python.org/3.6/library/poplib.html#module-poplib
"poplib: POP3 protocol client \(requires sockets\)." ), 和
[`smtplib`](https://docs.python.org/3.6/library/smtplib.html#module-smtplib
"smtplib: SMTP protocol client \(requires sockets\)." ) 已被弃用。(Contributed by Christian Heimes in [issue
28022](https://bugs.python.org/issue28022).)


[`ssl`][238]的部分协议及函数，现已被弃用。后续的 OpenSSL 版本中，部分现有特征将不再可用。
为了引入新的API，其他的特征也已经被弃用。 (Contributed by Christian Heimes in
[issue 28022][239] 和 [issue26470](https://bugs.python.org/issue26470).)


#### tkinter


[`tkinter.tix`](https://docs.python.org/3.6/library/tkinter.tix.html#module-tkinter.tix "tkinter.tix: Tk Extension Widgets for Tkinter")模块已被弃用。
原[`tkinter`](https://docs.python.org/3.6/library/tkinter.html#module-tkinter
"tkinter: Interface to Tcl/Tk for graphical user interfaces" )的使用者，可以使用
[`tkinter.ttk`](https://docs.python.org/3.6/library/tkinter.ttk.html#module-tkinter.ttk "tkinter.ttk: Tk themed widget set")来代替。

#### venv

为了引入`python3 -m venv`命令，`pyvenv` 已被弃用。 这可以防止混淆何种 Python 解释器将被`pyvenv`连接
以及何种 Python 解释器将被用在虚拟环境中。(Contributed by Brett Cannon in [issue
25154](https://bugs.python.org/issue25154).)

### 弃用C API的函数和类型

现在，弃用未公开函数`PyUnicode_AsEncodedObject()`，`PyUnicode_AsDecodedObject()`，`PyUnicode_AsEncodedUnicode()`和`PyUnicode_AsDecodedUnicode()`。使用[通用的基于codec的API](https://docs.python.org/3.6/c-api/codec.html#codec-registry)来代替。

### 弃用构建选项

在非macOS UNIX平台上，现在默认打开`--with-system-ffi`配置标志。它也许可以用`--without-system-ffi`来禁用，但是这个标志的使用已经弃用了，并且在Python 3.7中将不会接受这个标志。macOS不受这个改动的影响。注意，许多OS发行版在构建它们的系统Python时已经使用了`--with-system-
ffi`标志。

## 已移除

### API和特性移除

  * 正则表达式中`'\'`和一个ASCII字母组成的未知转义现在会触发错误。在[`re.sub()`][240]上的替换模板中，仍然允许使用它们，但是已经弃用了。[`re.LOCALE`][241]标志现在只能用于二进制模式。
  * 移除`inspect.getmoduleinfo()` (自CPython 3.3起弃用)。[`inspect.getmodulename()`][242]应该被用于获取一个给定路径的模块名。(由Yury Selivanov在[issue 13248][243]中贡献。)
  * `traceback.Ignore`类和`traceback.usage`，`traceback.modname`，`traceback.fullmodname`，`traceback.find_lines_from_code`，`traceback.find_lines`，`traceback.find_strings`，`traceback.find_executable_lines`方法已从[`traceback`][244]模块移除。它们是自Python 3.2起弃用的未公开方法，并且可以从私有方法获得等价的功能。
  * 移除[`tkinter`][245] widget类中的`tk_menuBar()`和`tk_bindForTraversal()`哑方法 (自Tk 4.0起，相应的Tk命令已废弃)。
  * [`zipfile.ZipFile`][247]类的[`open()`][246]方法不再支持`'U'`模式 (自Python 3.4起弃用)。使用[`io.TextIOWrapper`][248]来在[universal newlines][249]模式下读取压缩文本文件。
  * 已移除未公开的`IN`, `CDROM`, `DLFCN`, `TYPES`, `CDIO`, 和`STROPTS`模块。它们在平台特定的`Lib/plat-*/`目录中可用，但是慢慢过时了，跨平台不一定可用，并且未维护。创建这些模块的脚本在源代码发行版中仍然可以找到，位于[Tools/scripts/h2py.py][250]中。
  * 已移除弃用的`asynchat.fifo`类。

## 移植到Python 3.6

本节列出了与之前版本相比，一些特性的更改和bug的修复，这些可能会影响到你代码的编写。

###'python'命令行操作变更 

  * 默认情况下，用`COUNT_ALLOCS`，`SHOW_ALLOC_COUNT`或`SHOW_TRACK_COUNT`等宏定义的特殊python输出是关闭的。 它可以使用`-X showalloccount`选项重新启用. 它现在输出到`stderr`而不是`stdout`。 (由Serhiy Storchaka撰写在 [issue 23034][251].)

### Changes in the Python API

  * [`open()`][252] `'U'` 模式不再允许使用`'+'`拼接. (由Jeff Balogh and John O'Connor撰写在 [issue 2091][253].)

  * [`sqlite3`][254] 不允许在DDL语句之前隐式地提交打开的事务。

  * 在Linux系统上, [`os.urandom()`][255] 现在为了增强安全性，系统会一直阻塞直到系统urandom熵池被初始化。

  * 当[`importlib.abc.Loader.exec_module()`][256] 被定义时, [`importlib.abc.Loader.create_module()`][257] 也必须被定义.

  * [`PyErr_SetImportError()`][258] 现在被设置为[`TypeError`] [259]当它的** msg **参数没有设置。 以前只返回“NULL”。

  * The format of the `co_lnotab` attribute of code objects changed to support a negative line number delta. By default, Python does not emit bytecode with a negative line number delta. Functions using `frame.f_lineno`, `PyFrame_GetLineNumber()` or `PyCode_Addr2Line()` are not affected. Functions directly decoding `co_lnotab` should be updated to use a signed 8-bit integer type for the line number delta, but this is only required to support applications using a negative line number delta. See `Objects/lnotab_notes.txt` for the `co_lnotab` format and how to decode it, and see the [**PEP 511**][260] for the rationale.

  * [`compileall`] [261]模块中的函数现在返回布尔类型的值，而不是用`1`或`0`来表示成功或失败. 由于布尔值是整数的子类，如果你对'1'或'0'进行身份检查，这只应该是一个问题。 查看[issue 25768][262].

  * 读取[`urllib.parse.urlsplit（）`] [263]和[`urlparse（）`]的`port`属性现在引发超出范围值的[`ValueError`] [265] ，而不是返回[`None`][266]. 查看 [issue 20059][267].

  * [`imp`][268] 模块现在代替原来的[`PendingDeprecationWarning`][270]抛出[`DeprecationWarning`][269]告警 .

  * 以下模块已将缺少的API添加到其 `__all__` 属性中，匹配文档说明API: [`calendar`][271], [`cgi`][272], [`csv`][273], [`ElementTree`][274], [`enum`][275], [`fileinput`][276], [`ftplib`][277], [`logging`][278], [`mailbox`][279], [`mimetypes`][280], [`optparse`][281], [`plistlib`][282], [`smtpd`][283], [`subprocess`][284], [`tarfile`][285], [`threading`][286] and [`wave`][287].  这意味着在使用`import *`引入时，有一定的变化 . (由Joel Taddei and Jacek Kołodziej i撰写在[issue 23883][288].)

  * 当执行相对导入时，如果`__package__`不等于`__spec __。parent`，那么 [`ImportWarning`][289] 警告将被抛出. (由 Brett Cannon 撰写在[issue 25791][290].)

  * 当执行相对导入并且没有已知父包时，则将抛出[`ImportError`] [291]异常。 以前，是抛出[`SystemError`] [292]异常。 (由Brett Cannon 撰写在 [issue 18018][293].)

  * 基于[`socketserver`] [294]模块的服务器，包括[`http.server`] [295]，[`xmlrpc.server`] [296]和[`wsgiref.simple_server`] ，现在只捕获从[`Exception`] [298]派生的异常。 因此，如果一个请求处理程序引发了[[SystemExit]] [299]或[KeyboardInterrupt]] [300]异常，则[`handle_error（）`] [301] 异常不再被调用，同时异常会终止单线程的服务器。 (由Martin Panter 撰写在[issue 23430][302].)

  * 如果用户没有相关权限，[`spwd.getspnam()`][303]现在抛出 [`PermissionError`][304] 异常，代替原来的[`KeyError`][305].

  * (如 `EBADF`) 这样的底层调用错误，[`socket.socket.close()`][306]方法现在会抛出异常。 (由Martin Panter撰写于[issue 26685][307].)

  * The _decode\_data_ argument for the [`smtpd.SMTPChannel`][308] and [`smtpd.SMTPServer`][309] constructors is now `False` by default. This means that the argument passed to [`process_message()`][310] is now a bytes object by default, and `process_message()` will be passed keyword arguments. Code that has already been updated in accordance with the deprecation warning generated by 3.5 will not be affected.

  * All optional arguments of the [`dump()`][311], [`dumps()`][312], [`load()`][313] and [`loads()`][314] functions and [`JSONEncoder`][315] and [`JSONDecoder`][316] class constructors in the [`json`][317] module are now [keyword-only][318]. (Contributed by Serhiy Storchaka in [issue 18726][319].)

  * Subclasses of [`type`][320] which don't override `type.__new__` may no longer use the one-argument form to get the type of an object.

  * As part of [**PEP 487**][321], the handling of keyword arguments passed to [`type`][322] (other than the metaclass hint, `metaclass`) is now consistently delegated to [`object.__init_subclass__()`][323]. This means that `type.__new__()` and `type.__init__()` both now accept arbitrary keyword arguments, but [`object.__init_subclass__()`][324] (which is called from `type.__new__()`) will reject them by default. Custom metaclasses accepting additional keyword arguments will need to adjust their calls to `type.__new__()` (whether direct or via [`super`][325]) accordingly.

  * In `distutils.command.sdist.sdist`, the `default_format` attribute has been removed and is no longer honored. Instead, the gzipped tarfile format is the default on all platforms and no platform-specific selection is made. In environments where distributions are built on Windows and zip distributions are required, configure the project with a `setup.cfg` file containing the following:
```     [sdist]

    formats=zip

```

This behavior has also been backported to earlier Python versions by
Setuptools 26.0.0.

  * In the [`urllib.request`][326] module and the [`http.client.HTTPConnection.request()`][327] method, if no Content-Length header field has been specified and the request body is a file object, it is now sent with HTTP 1.1 chunked encoding. If a file object has to be sent to a HTTP 1.0 server, the Content-Length value now has to be specified by the caller. (Contributed by Demian Brecht and Rolf Krahl with tweaks from Martin Panter in [issue 12319][328].)

  * The [`DictReader`][329] now returns rows of type [`OrderedDict`][330]. (Contributed by Steve Holden in [issue 27842][331].)

  * The [`crypt.METHOD_CRYPT`][332] will no longer be added to `crypt.methods` if unsupported by the platform. (Contributed by Victor Stinner in [issue 25287][333].)

  * The _verbose_ and _rename_ arguments for [`namedtuple()`][334] are now keyword-only. (Contributed by Raymond Hettinger in [issue 25628][335].)

  * On Linux, [`ctypes.util.find_library()`][336] now looks in `LD_LIBRARY_PATH` for shared libraries. (Contributed by Vinay Sajip in [issue 9998][337].)

  * The [`imaplib.IMAP4`][338] class now handles flags containing the `']'` character in messages sent from the server to improve real-world compatibility. (Contributed by Lita Cho in [issue 21815][339].)

  * The `mmap.write()` function now returns the number of bytes written like other write methods. (Contributed by Jakub Stasiak in [issue 26335][340].)

  * The [`pkgutil.iter_modules()`][341] and [`pkgutil.walk_packages()`][342] functions now return [`ModuleInfo`][343] named tuples. (Contributed by Ramchandra Apte in [issue 17211][344].)

  * [`re.sub()`][345] now raises an error for invalid numerical group references in replacement templates even if the pattern is not found in the string. The error message for invalid group references now includes the group index and the position of the reference. (Contributed by SilentGhost, Serhiy Storchaka in [issue 25953][346].)

  * [`zipfile.ZipFile`][347] will now raise [`NotImplementedError`][348] for unrecognized compression values. Previously a plain [`RuntimeError`][349] was raised. Additionally, calling [`ZipFile`][350] methods on a closed ZipFile or calling the [`write()`][351] method on a ZipFile created with mode `'r'` will raise a [`ValueError`][352]. Previously, a [`RuntimeError`][353] was raised in those scenarios.

  * when custom metaclasses are combined with zero-argument [`super()`][354] or direct references from methods to the implicit `__class__` closure variable, the implicit `__classcell__` namespace entry must now be passed up to `type.__new__` for initialisation. Failing to do so will result in a [`DeprecationWarning`][355] in 3.6 and a [`RuntimeWarning`][356] in the future.


### 在C API上的改动

  * 旧的[`PyMem_Malloc()`][357]分配算符族将使用新的[pymalloc分配算符][358]，替换掉了系统原有的’malloc()’。应用程序在没有捕获GIL时调用[`PyMem_Malloc()`][359]会导致崩溃。可通过设置[pythonmalloc][360]环境变量去’debug’来实现应用程内存的分配。详情请看[问题26429][361]。
  * [`Py_Exit()`][362]（和新的主注释器），如果刷新缓存失败，将覆盖当前状态。详情请看[问题5319][363]

### CPython 字节码的更改

在Python 3.6中将会出现几个关于[字节码][364]要更新。
3.6.

  * 新的Python注释器将使用16位词码去替代老的字符码。（感谢Demur Rumed在Serhiy Storchaka和Victor Stinner[问题26647][365]和[问题28050][366]中做出贡献后的审查）
  * 将启用新的[`FORMAT_VALUE`][367]和[`BUILD_STRING`][368] 操作码作为一部分格式化字符串。（感谢Eric Smith在[问题25483][369]和Serhiy Storchaka在[问题27078][370]的贡献）
  * 将启用新的[`BUILD_CONST_KEY_MAP`][371]操作码并伴随常量关键字优化创建字典的过程。（感谢Serhiy Storchaka 提出的[问题27140][372].）
  * 为了更好的表现和更快捷的启动, 已经对调用操作码的功能进行了深度重做。[`MAKE_FUNCTION`][373], [`CALL_FUNCTION`][374],[` CALL_FUNCTION_KW`][375] 以及 `BUILD_MAP_UNPACK_WITH_CALL` 这些操作码已经做了修改，并添加了新的`CALL_FUNCTION_EX` 和 `BUILD_TUPLE_UNPACK_WITH_CALL`。而`CALL_FUNCTION_VAR`, `CALL_FUNCTION_VAR_KW` 和`MAKE_CLOSURE`则在新的Python中被移除。（感谢Demur Rumed在[问题27905][376]中的贡献以及Serhiy Storchaka在[问题27213][377]、[问题28257][378]中的贡献）
  * 添加了[`SETUP_ANNOTATIONS`][379] 和 [`STORE_ANNOTATION`][380]操作码，用于支持新的[变量注释语法][381]。（感谢Ivan Levkivskyi在[问题27985][382]中的贡献）

----
(C) [Copyright][383] 2001-2016, Python
软件基金会版权所有.
Python软件基金会是一个非营利性组织. [欢迎捐赠](https://www.python.org/psf/donations/)

[1]:	https://docs.python.org/3.6/whatsnew/3.6.html
[2]:	mailto:elvis@magic.io
[3]:	mailto:yury@magic.io
[4]:	https://www.python.org/dev/peps/pep-0494
[5]:	https://docs.python.org/3.6/library/secrets.html#module-secrets "secrets: Generate secure random numbers for managing secrets."
[6]:	https://docs.python.org/3.6/library/stdtypes.html#typesmapping
[7]:	https://morepypy.blogspot.com/2015/01/faster-more-memory-efficient-and-more.html
[8]:	https://docs.python.org/3.6/library/asyncio.html#module-asyncio "asyncio: Asynchronous I/O, event loop, coroutines and tasks."
[9]:	https://docs.python.org/3.6/glossary.html#term-path-like-object
[10]:	https://docs.python.org/3.6/library/datetime.html#module-datetime "datetime: Basic date and time types."
[11]:	https://docs.python.org/3.6/library/typing.html#module-typing "typing: Support for type hints \(see PEP 484\)."
[12]:	https://docs.python.org/3.6/library/tracemalloc.html#module-tracemalloc "tracemalloc: Trace memory allocations."
[13]:	https://docs.python.org/3.6/library/secrets.html#module-secrets "secrets: Generate secure random numbers for managing secrets."
[14]:	https://docs.python.org/3.6/library/os.html#os.urandom "os.urandom"
[15]:	https://www.python.org/dev/peps/pep-0524
[16]:	https://docs.python.org/3.6/library/hashlib-blake2.html#module-hashlib "hashlib: BLAKE2 hash function for Python"
[17]:	https://docs.python.org/3.6/library/ssl.html#module-ssl "ssl: TLS/SSL wrapper for socket objects"
[18]:	https://docs.python.org/3.6/library/ssl.html#module-ssl "ssl: TLS/SSL wrapper for socket objects"
[19]:	https://docs.python.org/3.6/library/hashlib-blake2.html#module-hashlib "hashlib: BLAKE2 hash function for Python"
[20]:	https://docs.python.org/3.6/library/hashlib.html#hashlib.scrypt "hashlib.scrypt"
[21]:	https://docs.python.org/3.6/using/windows.html#max-path
[22]:	https://docs.python.org/3.6/using/windows.html#finding-modules
[23]:	https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONHOME
[24]:	https://docs.python.org/3.6/using/windows.html#finding-modules
[25]:	https://www.python.org/dev/peps/pep-0498
[26]:	https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings
[27]:	https://docs.python.org/3.6/library/stdtypes.html#str.format "str.format"
[28]:	https://docs.python.org/3.6/library/functions.html#format "format"
[29]:	https://www.python.org/dev/peps/pep-0498
[30]:	https://docs.python.org/3.6/reference/lexical_analysis.html#f-strings
[31]:	https://www.python.org/dev/peps/pep-0484
[32]:	https://www.python.org/dev/peps/pep-0526
[33]:	http://github.com/python/mypy
[34]:	http://github.com/google/pytype
[35]:	https://www.python.org/dev/peps/pep-0515
[36]:	https://docs.python.org/3.6/library/string.html#formatspec
[37]:	https://www.python.org/dev/peps/pep-0515
[38]:	https://www.python.org/dev/peps/pep-0492
[39]:	https://www.python.org/dev/peps/pep-0525
[40]:	https://www.python.org/dev/peps/pep-0530
[41]:	https://www.python.org/dev/peps/pep-0530
[42]:	https://docs.python.org/3.6/library/functions.html#super "super"
[43]:	https://www.python.org/dev/peps/pep-0487
[44]:	https://www.python.org/dev/peps/pep-0487
[45]:	https://www.python.org/dev/peps/pep-0487
[46]:	https://docs.python.org/3.6/library/stdtypes.html#str "str"
[47]:	https://docs.python.org/3.6/library/functions.html#bytes "bytes"
[48]:	https://docs.python.org/3.6/library/functions.html#int "int"
[49]:	https://docs.python.org/3.6/library/stdtypes.html#str "str"
[50]:	https://docs.python.org/3.6/library/functions.html#bytes "bytes"
[51]:	https://docs.python.org/3.6/library/stdtypes.html#str "str"
[52]:	https://docs.python.org/3.6/library/functions.html#bytes "bytes"
[53]:	https://docs.python.org/3.6/library/functions.html#open "open"
[54]:	https://docs.python.org/3.6/library/os.html#module-os "os: Miscellaneous operating system interfaces."
[55]:	https://www.python.org/dev/peps/pep-0519
[56]:	https://www.python.org/dev/peps/pep-0495
[57]:	https://www.python.org/dev/peps/pep-0495
[58]:	https://www.python.org/dev/peps/pep-0529
[59]:	https://www.python.org/dev/peps/pep-0528
[60]:	https://www.python.org/dev/peps/pep-0520
[61]:	https://www.python.org/dev/peps/pep-0468
[62]:	https://docs.python.org/3.6/library/stdtypes.html#typesmapping
[63]:	https://docs.python.org/3.6/library/stdtypes.html#typesmapping
[64]:	https://docs.python.org/3.6/library/stdtypes.html#dict "dict"
[65]:	https://www.python.org/dev/peps/pep-0523
[66]:	https://www.python.org/dev/peps/pep-0523
[67]:	https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc "PyMem_Malloc"
[68]:	https://docs.python.org/3.6/glossary.html#term-global-interpreter-lock
[69]:	https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_OBJ "PYMEM_DOMAIN_OBJ"
[70]:	https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_MEM "PYMEM_DOMAIN_MEM"
[71]:	https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc "PyMem_Malloc"
[72]:	https://docs.python.org/3.6/reference/datamodel.html#specialnames
[73]:	https://docs.python.org/3.6/reference/datamodel.html#object.__iter__ "object.__iter__"
[74]:	https://bugs.python.org/issue25958
[75]:	https://bugs.python.org/issue26823
[76]:	https://docs.python.org/3.6/library/exceptions.html#ModuleNotFoundError "ModuleNotFoundError"
[77]:	https://docs.python.org/3.6/library/exceptions.html#ImportError "ImportError"
[78]:	https://bugs.python.org/issue15767
[79]:	https://bugs.python.org/issue23722
[80]:	https://www.python.org/dev/peps/pep-0506
[81]:	https://docs.python.org/3.6/library/asyncio-eventloops.html#asyncio.get_event_loop "asyncio.get_event_loop"
[82]:	https://bugs.python.org/issue28613
[83]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.ensure_future "asyncio.ensure_future"
[84]:	https://docs.python.org/3.6/glossary.html#term-awaitable
[85]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.run_coroutine_threadsafe "asyncio.run_coroutine_threadsafe"
[86]:	https://docs.python.org/3.6/library/asyncio-protocol.html#asyncio.BaseTransport.is_closing "asyncio.BaseTransport.is_closing"
[87]:	https://github.com/MagicStack/uvloop
[88]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Future "asyncio.Future"
[89]:	https://bugs.python.org/issue27041
[90]:	https://bugs.python.org/issue27040
[91]:	https://docs.python.org/3.6/library/asyncio-stream.html#asyncio.StreamReader.readuntil "asyncio.StreamReader.readuntil"
[92]:	https://docs.python.org/3.6/library/asyncio-stream.html#asyncio.StreamReader.readexactly "asyncio.StreamReader.readexactly"
[93]:	https://bugs.python.org/issue28370
[94]:	https://bugs.python.org/issue25593
[95]:	https://docs.python.org/3.6/library/exceptions.html#TypeError "TypeError"
[96]:	https://docs.python.org/3.6/library/exceptions.html#StopIteration "StopIteration"
[97]:	https://bugs.python.org/issue26221
[98]:	https://docs.python.org/3.6/library/asyncio-eventloop.html#asyncio.BaseEventLoop.connect_accepted_socket "asyncio.BaseEventLoop.connect_accepted_socket"
[99]:	https://bugs.python.org/issue27392
[100]:	https://bugs.python.org/issue27456
[101]:	https://docs.python.org/3.6/library/asyncio-eventloop.html#asyncio.AbstractEventLoop.shutdown_asyncgens "asyncio.AbstractEventLoop.shutdown_asyncgens"
[102]:	https://bugs.python.org/issue28003
[103]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Future "asyncio.Future"
[104]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Task "asyncio.Task"
[105]:	https://bugs.python.org/issue26081
[106]:	https://bugs.python.org/issue28544
[107]:	https://bugs.python.org/issue12345
[108]:	https://bugs.python.org/issue23229
[109]:	https://bugs.python.org/issue28720
[110]:	https://bugs.python.org/issue17941
[111]:	https://bugs.python.org/issue25628
[112]:	https://bugs.python.org/issue27664
[113]:	https://bugs.python.org/issue24277
[114]:	https://docs.python.org/3.6/library/enum.html#module-enum "enum:Implementation of an enumeration class."
[115]:	https://docs.python.org/3.6/library/enum.html#enum.Flag "enum.Flag"
[116]:	https://bugs.python.org/issue26470
[117]:	https://bugs.python.org/issue26798
[118]:	https://bugs.python.org/issue27928
[119]:	https://bugs.python.org/issue12319
[120]:	https://bugs.python.org/issue24225
[121]:	https://bugs.python.org/issue15767
[122]:	https://bugs.python.org/issue17909
[123]:	https://bugs.python.org/issue24884
[124]:	https://docs.python.org/3.6/library/math.html#module-math "math:Mathematical functions \(sin\(\) etc.\)."
[125]:	https://docs.python.org/3.6/library/cmath.html#module-cmath "cmath:Mathematical functions for complex numbers."
[126]:	https://bugs.python.org/issue12345
[127]:	https://docs.python.org/3.6/library/os.html#module-os "os:Miscellaneous operating system interfaces."
[128]:	https://docs.python.org/3.6/library/functions.html#bytes "bytes"
[129]:	https://docs.python.org/3.6/library/pdb.html#pdb.Pdb "pdb.Pdb"
[130]:	https://bugs.python.org/issue8637
[131]:	https://docs.python.org/3.6/library/functions.html#help "help"
[132]:	https://bugs.python.org/issue18844
[133]:	https://bugs.python.org/issue433028
[134]:	https://bugs.python.org/issue26870
[135]:	https://bugs.python.org/issue26536
[136]:	https://bugs.python.org/issue26907
[137]:	https://bugs.python.org/issue27744
[138]:	https://bugs.python.org/issue19500
[139]:	https://bugs.python.org/issue11734
[140]:	https://bugs.python.org/issue27932
[141]:	https://bugs.python.org/issue25485
[142]:	https://bugs.python.org/issue6422
[143]:	https://bugs.python.org/issue23552
[144]:	https://bugs.python.org/issue22115
[145]:	https://docs.python.org/3.6/glossary.html#term-provisional-api
[146]:	https://bugs.python.org/issue27598
[147]:	https://github.com/python/typing/issues/230
[148]:	http://unicode.org/versions/Unicode9.0.0/
[149]:	https://docs.python.org/3.6/library/unittest.mock.html#unittest.mock.Mock.assert_called "unittest.mock.Mock.assert_called"
[150]:	https://docs.python.org/3.6/library/unittest.mock.html#unittest.mock.Mock.assert_called_once "unittest.mock.Mock.assert_called_once"
[151]:	https://bugs.python.org/issue26323
[152]:	https://docs.python.org/3.6/library/unittest.mock.html#unittest.mock.Mock.reset_mock "unittest.mock.Mock.reset_mock"
[153]:	https://bugs.python.org/issue21271
[154]:	https://bugs.python.org/issue16099
[155]:	https://docs.python.org/3.6/library/venv.html#module-venv "venv:Creation of virtual environments."
[156]:	https://bugs.python.org/issue27982
[157]:	https://bugs.python.org/issue26885
[158]:	https://bugs.python.org/issue26039
[159]:	https://bugs.python.org/issue26039
[160]:	https://bugs.python.org/issue26243
[161]:	https://bugs.python.org/issue16764
[162]:	https://bugs.python.org/issue26647
[163]:	https://bugs.python.org/issue28050
[164]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Future "asyncio.Future"
[165]:	https://bugs.python.org/issue26081
[166]:	https://docs.python.org/3.6/library/asyncio-task.html#asyncio.Task "asyncio.Task"
[167]:	https://bugs.python.org/issue28544
[168]:	https://docs.python.org/3.6/library/typing.html#module-typing "typing: Support for type hints \(see PEP 484\)."
[169]:	https://bugs.python.org/issue24870
[170]:	https://bugs.python.org/issue25227
[171]:	https://bugs.python.org/issue25267
[172]:	https://bugs.python.org/issue25301
[173]:	https://bugs.python.org/issue25349
[174]:	https://bugs.python.org/issue25399
[175]:	https://docs.python.org/3.6/library/stdtypes.html#bytes.fromhex "bytes.fromhex"
[176]:	https://docs.python.org/3.6/library/stdtypes.html#bytearray.fromhex "bytearray.fromhex"
[177]:	https://bugs.python.org/issue25401
[178]:	https://bugs.python.org/issue26574
[179]:	https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc "PyMem_Malloc"
[180]:	https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_MEM "PYMEM_DOMAIN_MEM"
[181]:	https://docs.python.org/3.6/c-api/memory.html#pymalloc
[182]:	https://bugs.python.org/issue26249
[183]:	https://docs.python.org/3.6/library/pickle.html#pickle.load "pickle.load"
[184]:	https://docs.python.org/3.6/library/pickle.html#pickle.loads "pickle.loads"
[185]:	https://bugs.python.org/issue27056
[186]:	https://docs.python.org/3.6/glossary.html#term-keyword-argument
[187]:	https://docs.python.org/3.6/glossary.html#term-positional-argument
[188]:	https://bugs.python.org/issue27574
[189]:	https://docs.python.org/3.6/library/glob.html#glob.glob "glob.glob"
[190]:	https://docs.python.org/3.6/library/glob.html#glob.iglob "glob.iglob"
[191]:	https://docs.python.org/3.6/library/glob.html#module-glob "glob: Unix shell style pathname pattern expansion."
[192]:	https://bugs.python.org/issue25596
[193]:	https://docs.python.org/3.6/library/pathlib.html#module-pathlib "pathlib: Object-oriented filesystem paths"
[194]:	https://docs.python.org/3.6/library/os.html#os.scandir "os.scandir"
[195]:	https://bugs.python.org/issue26032
[196]:	https://docs.python.org/3.6/library/xml.etree.elementtree.html#module-xml.etree.ElementTree "xml.etree.ElementTree: Implementation of the ElementTree API."
[197]:	https://bugs.python.org/issue25638
[198]:	https://bugs.python.org/issue25873
[199]:	https://bugs.python.org/issue25869
[200]:	https://docs.python.org/3.6/library/fractions.html#fractions.Fraction "fractions.Fraction"
[201]:	https://bugs.python.org/issue25971
[202]:	https://www.python.org/dev/peps/pep-0007
[203]:	https://bugs.python.org/issue17884
[204]:	https://bugs.python.org/issue26865
[205]:	https://bugs.python.org/issue26539
[206]:	https://docs.python.org/3.6/glossary.html#term-global-interpreter-lock
[207]:	https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_OBJ "PYMEM_DOMAIN_OBJ"
[208]:	https://docs.python.org/3.6/c-api/memory.html#c.PYMEM_DOMAIN_MEM "PYMEM_DOMAIN_MEM"
[209]:	https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc "PyMem_Malloc"
[210]:	https://docs.python.org/3.6/c-api/init.html#c.Py_FinalizeEx "Py_FinalizeEx"
[211]:	https://bugs.python.org/issue5319
[212]:	https://docs.python.org/3.6/c-api/arg.html#c.PyArg_ParseTupleAndKeywords "PyArg_ParseTupleAndKeywords"
[213]:	https://docs.python.org/3.6/glossary.html#positional-only-parameter
[214]:	https://bugs.python.org/issue26282
[215]:	https://bugs.python.org/issue26823
[216]:	https://docs.python.org/3.6/c-api/exceptions.html#c.PyErr_SetImportErrorSubclass "PyErr_SetImportErrorSubclass"
[217]:	https://docs.python.org/3.6/library/exceptions.html#ImportError "ImportError"
[218]:	https://bugs.python.org/issue15767
[219]:	https://docs.python.org/3.6/c-api/exceptions.html#c.PyErr_ResourceWarning "PyErr_ResourceWarning"
[220]:	https://docs.python.org/3.6/library/exceptions.html#ResourceWarning "ResourceWarning"
[221]:	https://bugs.python.org/issue26567
[222]:	https://docs.python.org/3.6/c-api/sys.html#c.PyOS_FSPath "PyOS_FSPath"
[223]:	https://docs.python.org/3.6/glossary.html#term-path-like-object
[224]:	https://bugs.python.org/issue27186
[225]:	https://docs.python.org/3.6/c-api/unicode.html#c.PyUnicode_FSConverter "PyUnicode_FSConverter"
[226]:	https://docs.python.org/3.6/c-api/unicode.html#c.PyUnicode_FSDecoder "PyUnicode_FSDecoder"
[227]:	https://docs.python.org/3.6/glossary.html#term-path-like-object
[228]:	https://docs.python.org/3.6/using/cmdline.html#cmdoption--version
[229]:	https://docs.python.org/3.6/using/cmdline.html#cmdoption-V
[230]:	https://docs.python.org/3.6/library/sys.html#sys.version "sys.version"
[231]:	https://bugs.python.org/issue27364
[232]:	https://bugs.python.org/issue25002
[233]:	https://bugs.python.org/issue25002
[234]:	https://bugs.python.org/issue26129
[235]:	https://docs.python.org/3.6/library/os.html#module-os "os:Miscellaneous operating system interfaces."
[236]:	https://bugs.python.org/issue25791
[237]:	https://bugs.python.org/issue22493
[238]:	https://docs.python.org/3.6/library/ssl.html#module-ssl "ssl: TLS/SSLwrapper for socket objects"
[239]:	https://bugs.python.org/issue28022
[240]:	https://docs.python.org/3.6/library/re.html#re.sub "re.sub"
[241]:	https://docs.python.org/3.6/library/re.html#re.LOCALE "re.LOCALE"
[242]:	https://docs.python.org/3.6/library/inspect.html#inspect.getmodulename "inspect.getmodulename"
[243]:	https://bugs.python.org/issue13248
[244]:	https://docs.python.org/3.6/library/traceback.html#module-traceback "traceback: Print or retrieve a stack traceback."
[245]:	https://docs.python.org/3.6/library/tkinter.html#module-tkinter "tkinter: Interface to Tcl/Tk for graphical user interfaces"
[246]:	https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipFile.open "zipfile.ZipFile.open"
[247]:	https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipFile "zipfile.ZipFile"
[248]:	https://docs.python.org/3.6/library/io.html#io.TextIOWrapper "io.TextIOWrapper"
[249]:	https://docs.python.org/3.6/glossary.html#term-universal-newlines
[250]:	https://hg.python.org/cpython/file/3.6/Tools/scripts/h2py.py
[251]:	https://bugs.python.org/issue23034
[252]:	https://docs.python.org/3.6/library/functions.html#open "open"
[253]:	https://bugs.python.org/issue2091
[254]:	https://docs.python.org/3.6/library/sqlite3.html#module-sqlite3 "sqlite3: A DB-API 2.0 implementation using SQLite 3.x."
[255]:	https://docs.python.org/3.6/library/os.html#os.urandom "os.urandom"
[256]:	https://docs.python.org/3.6/library/importlib.html#importlib.abc.Loader.exec_module "importlib.abc.Loader.exec_module"
[257]:	https://docs.python.org/3.6/library/importlib.html#importlib.abc.Loader.create_module "importlib.abc.Loader.create_module"
[258]:	https://docs.python.org/3.6/c-api/exceptions.html#c.PyErr_SetImportError "PyErr_SetImportError"
[259]:	https://docs.python.org/3.6/library/exceptions.html#TypeError "TypeError"
[260]:	https://www.python.org/dev/peps/pep-0511
[261]:	https://docs.python.org/3.6/library/compileall.html#module-compileall "compileall: Tools for byte-compiling all Python source files in a directory tree."
[262]:	https://bugs.python.org/issue25768
[263]:	https://docs.python.org/3.6/library/urllib.parse.html#urllib.parse.urlsplit "urllib.parse.urlsplit"
[264]:	https://docs.python.org/3.6/library/urllib.parse.html#urllib.parse.urlparse "urllib.parse.urlparse"
[265]:	https://docs.python.org/3.6/library/exceptions.html#ValueError "ValueError"
[266]:	https://docs.python.org/3.6/library/constants.html#None "None"
[267]:	https://bugs.python.org/issue20059
[268]:	https://docs.python.org/3.6/library/imp.html#module-imp "imp: Access the implementation of the import statement. \(deprecated\)"
[269]:	https://docs.python.org/3.6/library/exceptions.html#DeprecationWarning "DeprecationWarning"
[270]:	https://docs.python.org/3.6/library/exceptions.html#PendingDeprecationWarning "PendingDeprecationWarning"
[271]:	https://docs.python.org/3.6/library/calendar.html#module-calendar "calendar: Functions for working with calendars, including some emulation of the Unix cal program."
[272]:	https://docs.python.org/3.6/library/cgi.html#module-cgi "cgi: Helpers for running Python scripts via the Common Gateway Interface."
[273]:	https://docs.python.org/3.6/library/csv.html#module-csv "csv: Write and read tabular data to and from delimited files."
[274]:	https://docs.python.org/3.6/library/xml.etree.elementtree.html#module-xml.etree.ElementTree "xml.etree.ElementTree: Implementation of the ElementTree API."
[275]:	https://docs.python.org/3.6/library/enum.html#module-enum "enum: Implementation of an enumeration class."
[276]:	https://docs.python.org/3.6/library/fileinput.html#module-fileinput "fileinput: Loop over standard input or a list of files."
[277]:	https://docs.python.org/3.6/library/ftplib.html#module-ftplib "ftplib: FTP protocol client \(requires sockets\)."
[278]:	https://docs.python.org/3.6/library/logging.html#module-logging "logging: Flexible event logging system for applications."
[279]:	https://docs.python.org/3.6/library/mailbox.html#module-mailbox "mailbox: Manipulate mailboxes in various formats"
[280]:	https://docs.python.org/3.6/library/mimetypes.html#module-mimetypes "mimetypes: Mapping of filename extensions to MIME types."
[281]:	https://docs.python.org/3.6/library/optparse.html#module-optparse "optparse: Command-line option parsing library. \(deprecated\)"
[282]:	https://docs.python.org/3.6/library/plistlib.html#module-plistlib "plistlib: Generate and parse Mac OS X plist files."
[283]:	https://docs.python.org/3.6/library/smtpd.html#module-smtpd "smtpd: A SMTP server implementation in Python."
[284]:	https://docs.python.org/3.6/library/subprocess.html#module-subprocess "subprocess: Subprocess management."
[285]:	https://docs.python.org/3.6/library/tarfile.html#module-tarfile "tarfile: Read and write tar-format archive files."
[286]:	https://docs.python.org/3.6/library/threading.html#module-threading "threading: Thread-based parallelism."
[287]:	https://docs.python.org/3.6/library/wave.html#module-wave "wave: Provide an interface to the WAV sound format."
[288]:	https://bugs.python.org/issue23883
[289]:	https://docs.python.org/3.6/library/exceptions.html#ImportWarning "ImportWarning"
[290]:	https://bugs.python.org/issue25791
[291]:	https://docs.python.org/3.6/library/exceptions.html#ImportError "ImportError"
[292]:	https://docs.python.org/3.6/library/exceptions.html#SystemError "SystemError"
[293]:	https://bugs.python.org/issue18018
[294]:	https://docs.python.org/3.6/library/socketserver.html#module-socketserver "socketserver: A framework for network servers."
[295]:	https://docs.python.org/3.6/library/http.server.html#module-http.server "http.server: HTTP server and request handlers."
[296]:	https://docs.python.org/3.6/library/xmlrpc.server.html#module-xmlrpc.server "xmlrpc.server: Basic XML-RPC server implementations."
[297]:	https://docs.python.org/3.6/library/wsgiref.html#module-wsgiref.simple_server "wsgiref.simple_server: A simple WSGI HTTP server."
[298]:	https://docs.python.org/3.6/library/exceptions.html#Exception "Exception"
[299]:	https://docs.python.org/3.6/library/exceptions.html#SystemExit "SystemExit"
[300]:	https://docs.python.org/3.6/library/exceptions.html#KeyboardInterrupt "KeyboardInterrupt"
[301]:	https://docs.python.org/3.6/library/socketserver.html#socketserver.BaseServer.handle_error "socketserver.BaseServer.handle_error"
[302]:	https://bugs.python.org/issue23430
[303]:	https://docs.python.org/3.6/library/spwd.html#spwd.getspnam "spwd.getspnam"
[304]:	https://docs.python.org/3.6/library/exceptions.html#PermissionError "PermissionError"
[305]:	https://docs.python.org/3.6/library/exceptions.html#KeyError "KeyError"
[306]:	https://docs.python.org/3.6/library/socket.html#socket.socket.close "socket.socket.close"
[307]:	https://bugs.python.org/issue26685
[308]:	https://docs.python.org/3.6/library/smtpd.html#smtpd.SMTPChannel "smtpd.SMTPChannel"
[309]:	https://docs.python.org/3.6/library/smtpd.html#smtpd.SMTPServer "smtpd.SMTPServer"
[310]:	https://docs.python.org/3.6/library/smtpd.html#smtpd.SMTPServer.process_message "smtpd.SMTPServer.process_message"
[311]:	https://docs.python.org/3.6/library/json.html#json.dump "json.dump"
[312]:	https://docs.python.org/3.6/library/json.html#json.dumps "json.dumps"
[313]:	https://docs.python.org/3.6/library/json.html#json.load "json.load"
[314]:	https://docs.python.org/3.6/library/json.html#json.loads "json.loads"
[315]:	https://docs.python.org/3.6/library/json.html#json.JSONEncoder "json.JSONEncoder"
[316]:	https://docs.python.org/3.6/library/json.html#json.JSONDecoder "json.JSONDecoder"
[317]:	https://docs.python.org/3.6/library/json.html#module-json "json: Encode and decode the JSON format."
[318]:	https://docs.python.org/3.6/glossary.html#keyword-only-parameter
[319]:	https://bugs.python.org/issue18726
[320]:	https://docs.python.org/3.6/library/functions.html#type "type"
[321]:	https://www.python.org/dev/peps/pep-0487
[322]:	https://docs.python.org/3.6/library/functions.html#type "type"
[323]:	https://docs.python.org/3.6/reference/datamodel.html#object.__init_subclass__ "object.__init_subclass__"
[324]:	https://docs.python.org/3.6/reference/datamodel.html#object.__init_subclass__ "object.__init_subclass__"
[325]:	https://docs.python.org/3.6/library/functions.html#super "super"
[326]:	https://docs.python.org/3.6/library/urllib.request.html#module-urllib.request "urllib.request: Extensible library for opening URLs."
[327]:	https://docs.python.org/3.6/library/http.client.html#http.client.HTTPConnection.request "http.client.HTTPConnection.request"
[328]:	https://bugs.python.org/issue12319
[329]:	https://docs.python.org/3.6/library/csv.html#csv.DictReader "csv.DictReader"
[330]:	https://docs.python.org/3.6/library/collections.html#collections.OrderedDict "collections.OrderedDict"
[331]:	https://bugs.python.org/issue27842
[332]:	https://docs.python.org/3.6/library/crypt.html#crypt.METHOD_CRYPT "crypt.METHOD_CRYPT"
[333]:	https://bugs.python.org/issue25287
[334]:	https://docs.python.org/3.6/library/collections.html#collections.namedtuple "collections.namedtuple"
[335]:	https://bugs.python.org/issue25628
[336]:	https://docs.python.org/3.6/library/ctypes.html#ctypes.util.find_library "ctypes.util.find_library"
[337]:	https://bugs.python.org/issue9998
[338]:	https://docs.python.org/3.6/library/imaplib.html#imaplib.IMAP4 "imaplib.IMAP4"
[339]:	https://bugs.python.org/issue21815
[340]:	https://bugs.python.org/issue26335
[341]:	https://docs.python.org/3.6/library/pkgutil.html#pkgutil.iter_modules "pkgutil.iter_modules"
[342]:	https://docs.python.org/3.6/library/pkgutil.html#pkgutil.walk_packages "pkgutil.walk_packages"
[343]:	https://docs.python.org/3.6/library/pkgutil.html#pkgutil.ModuleInfo "pkgutil.ModuleInfo"
[344]:	https://bugs.python.org/issue17211
[345]:	https://docs.python.org/3.6/library/re.html#re.sub "re.sub"
[346]:	https://bugs.python.org/issue25953
[347]:	https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipFile "zipfile.ZipFile"
[348]:	https://docs.python.org/3.6/library/exceptions.html#NotImplementedError "NotImplementedError"
[349]:	https://docs.python.org/3.6/library/exceptions.html#RuntimeError "RuntimeError"
[350]:	https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipFile "zipfile.ZipFile"
[351]:	https://docs.python.org/3.6/library/zipfile.html#zipfile.ZipFile.write "zipfile.ZipFile.write"
[352]:	https://docs.python.org/3.6/library/exceptions.html#ValueError "ValueError"
[353]:	https://docs.python.org/3.6/library/exceptions.html#RuntimeError "RuntimeError"
[354]:	https://docs.python.org/3.6/library/functions.html#super "super"
[355]:	https://docs.python.org/3.6/library/exceptions.html#DeprecationWarning "DeprecationWarning"
[356]:	https://docs.python.org/3.6/library/exceptions.html#RuntimeWarning "RuntimeWarning"
[357]:	https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc "PyMem_Malloc"
[358]:	https://docs.python.org/3.6/c-api/memory.html#pymalloc
[359]:	https://docs.python.org/3.6/c-api/memory.html#c.PyMem_Malloc "PyMem_Malloc"
[360]:	https://docs.python.org/3.6/using/cmdline.html#envvar-PYTHONMALLOC
[361]:	https://bugs.python.org/issue26249
[362]:	https://docs.python.org/3.6/c-api/sys.html#c.Py_Exit "Py_Exit"
[363]:	https://bugs.python.org/issue5319
[364]:	https://docs.python.org/3.6/glossary.html#term-bytecode
[365]:	https://bugs.python.org/issue26647
[366]:	https://bugs.python.org/issue28050
[367]:	https://docs.python.org/3.6/library/dis.html#opcode-FORMAT_VALUE
[368]:	https://docs.python.org/3.6/library/dis.html#opcode-BUILD_STRING
[369]:	https://bugs.python.org/issue25483
[370]:	https://bugs.python.org/issue27078
[371]:	https://docs.python.org/3.6/library/dis.html#opcode-BUILD_CONST_KEY_MAP
[372]:	https://bugs.python.org/issue27140
[373]:	https://docs.python.org/3.6/library/dis.html#opcode-MAKE_FUNCTION
[374]:	https://docs.python.org/3.6/library/dis.html#opcode-CALL_FUNCTION
[375]:	https://docs.python.org/3.6/library/dis.html#opcode-CALL_FUNCTION_KW
[376]:	https://bugs.python.org/issue27095
[377]:	https://bugs.python.org/issue27213
[378]:	https://bugs.python.org/issue28257
[379]:	https://docs.python.org/3.6/library/dis.html#opcode-SETUP_ANNOTATIONS
[380]:	https://docs.python.org/3.6/library/dis.html#opcode-STORE_ANNOTATION
[381]:	https://docs.python.org/3.6/glossary.html#term-variable-annotation
[382]:	https://bugs.python.org/issue27985
[383]:	https://docs.python.org/3.6/copyright.html
[384]:	http://sphinx.pocoo.org/
