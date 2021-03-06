---
date: 2015-03-22
description: ""
tags: ["Python","代码风格","翻译","风格指南"]
title: "Python代码风格指南(Style Guide for Python Code中文翻译)"
topics: []
draft: true
url: /post/2015-03-22
---

翻译自：[PEP 8 - Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)


介绍(Introduction)
============

这篇文档说明了Python主要发行版中标准库代码所遵守的规范。请参考用于实现Python的C代码风格的指南信息PEP。

这篇文档和PEP 257(Docstring Conventions)都改编自Guido(译注：Python之父)最早的Python风格指南文章，并加入了Barry风格指南里的内容。

语言自身在发生着改变，随着新的规范的出现和旧规范的过时，代码风格也会随着时间演变。

很多项目都有自己的一套风格指南。若和本指南有任何冲突，应该优先考虑其项目相关的那套指南。


保持盲目的一致是头脑简单的表现(A Foolish Consistency is the Hobgoblin of Little Minds)
======================================================

(注：标题语出自Ralph Waldo Emerson, Hobgolin意指民间故事中友好但常制造麻烦的动物角色。)

Guido的一个重要观点是代码被读的次数远多于被写的次数。这篇指南旨在提高代码的可读性，使浩瀚如烟的Python代码风格能保持一致。正如PEP 20那首《Zen of Python》的小诗里所说的：“可读性很重要(Readability counts)”。

这本风格指南是关于一致性的。同风格指南保持一致性是重要的，但是同项目保持一致性更加重要，同一个模块和一个函数保持一致性则最为重要。

然而最最重要的是：要知道何时去违反一致性，因为有时风格指南并不适用。当存有疑虑时，请自行做出最佳判断。请参考别的例子去做出最好的决定。并且不要犹豫，尽管提问。

特别的：千万不要为了遵守这篇PEP而破坏向后兼容性！

如果有以下借口，则可以忽略这份风格指南：

1. 当采用风格指南时会让代码更难读，甚至对于习惯阅读遵循这篇PEP的代码的人来说也是如此。

2. 需要和周围的代码保持一致性，但这些代码违反了指南中的风格（可是时历史原因造成的）——尽管这可能也是一个收拾别人烂摊子的机会（True in XP style?）。

3. 若是有问题的某段代码早于引入指南的时间，那么没有必要去修改这段代码。

4. 代码需要和更旧版本的Python保持兼容，而旧版本的Python不支持风格指南所推荐的特性。

代码设计(Code lay-out)
============

缩进(Indentation)
-----------

每个缩进级别采用4个空格。

连续行所包装的元素应该要么采用Python隐式续行，即垂直对齐于圆括号、方括号和花括号，要么采用*悬挂缩进(hanging indent)*。采用悬挂缩进时需考虑以下两点：第一行不应该包括参数，并且在续行中需要再缩进一级以便清楚表示。

正确的例子::

    # 同开始分界符(左括号)对齐
    foo = long_function_name(var_one, var_two,
                             var_three, var_four)

    # 续行多缩进一级以同其他代码区别
    def long_function_name(
            var_one, var_two, var_three,
            var_four):
        print(var_one)

    # 悬挂缩进需要多缩进一级
    foo = long_function_name(
        var_one, var_two,
        var_three, var_four)

错误的例子::

    # 采用垂直对齐时第一行不应该有参数
    foo = long_function_name(var_one, var_two,
        var_three, var_four)

    # 续行并没有被区分开，因此需要再缩进一级
    def long_function_name(
        var_one, var_two, var_three,
        var_four):
        print(var_one)

对于续行来说，4空格的规则可以不遵守。

同样可行的例子::

    # 悬挂缩进可以不采用4空格的缩进方法。
    foo = long_function_name(
      var_one, var_two,
      var_three, var_four)

`多行if语句`

如果``if``语句太长，需要用多行书写，2个字符(例如,``if``)加上一个空格和一个左括号刚好是4空格的缩进，但这对多行条件语句的续行是没用的。因为这会和``if``语句中嵌套的其他的缩进的语句产生视觉上的冲突。这份PEP中并没有做出明确的说明应该怎样来区分条件语句和``if``语句中所嵌套的语句。以下几种方法都是可行的，但不仅仅只限于这几种方法：

    # 不采用额外缩进
    if (this_is_one_thing and
        that_is_another_thing):
        do_something()

    # 增加一行注释，在编辑器中显示时能有所区分
    # supporting syntax highlighting.
    if (this_is_one_thing and
        that_is_another_thing):
        # Since both conditions are true, we can frobnicate.
        do_something()

    # 在条件语句的续行增加一级缩进
    if (this_is_one_thing
            and that_is_another_thing):
        do_something()

多行结束右圆/方/花括号可以单独一行书写，和上一行的缩进对齐：

    my_list = [
        1, 2, 3,
        4, 5, 6,
        ]
    result = some_function_that_takes_arguments(
        'a', 'b', 'c',
        'd', 'e', 'f',
        )

也可以和多行开始的第一行的第一个字符对齐：

    my_list = [
        1, 2, 3,
        4, 5, 6,
    ]
    result = some_function_that_takes_arguments(
        'a', 'b', 'c',
        'd', 'e', 'f',
    )


Tab还是空格？(Tab or space?)
---------------

推荐使用空格来进行缩进。

Tab应该只在现有代码已经使用tab进行缩进的情况下使用，以便和现有代码保持一致。

Python 3不允许tab和空格混合使用。

Python 2的代码若有tab和空格混合使用的情况，应该把tab全部转换为只有空格。

当使用命令行运行Python 2时，使用``-t``选项，会出现非法混用tab和空格的警告。当使用``-tt``选项时，这些警告会变成错误。强烈推荐使用这些选项！

每行最大长度(Maximum Line Length)
-------------------

将所有行都限制在79个字符长度以内。

对于连续的大段文字（文档字符串(docstring)或注释），其结构上的限制更少，这些行应该被限制在72个字符长度内。

限制编辑器的窗口宽度能让好几个文件同时打开在屏幕上显示，在使用代码评审(code review)工具时在两个相邻窗口显示两个版本的代码效果很好。

很多工具的默认自动换行会破坏代码的结构，使代码更难以理解。在窗口大小设置为80个字符的编辑器中，为避免自动换行而需要限制每行字符长度，即使编辑器可能在换行时在最后一列会放置一个记号。一些基于web的工具可能根本没有自动换行的功能。

一些团队强烈偏好更长的行长度。当代码仅仅只由一个团队维护时，可以达成一致让行长度增加到80到100字符(实际上最大行长是99字符)，注释和文档字符串仍然是以72字符换行。

Python标准库比较传统，将行长限制在79个字符以内（文档字符串/注释为72个字符）。

一种推荐的换行方式是利用Python圆括号、方括号和花括号中的隐式续行。长行可以通过在括号内换行来分成多行。应该最好加上反斜杠来区别续行。

有时续行只有反斜杠才适用。例如，长的多个``with``语句不能采用隐式续行，只能接受反斜杠表示换行：

    with open('/path/to/some/file/you/want/to/read') as file_1, \
         open('/path/to/some/file/being/written', 'w') as file_2:
        file_2.write(file_1.read())

（参照前面关于 `多行if语句`的讨论来进一步考虑这里`with`语句的缩进。）

另一个这样的例子是``assert``语句。

要确保续行的缩进适当。逻辑运算符附近的换行处最好是在运算符*之后*，而不是在其之前。来看一些例子：

    class Rectangle(Blob):

        def __init__(self, width, height,
                     color='black', emphasis=None, highlight=0):
            if (width == 0 and height == 0 and
                    color == 'red' and emphasis == 'strong' or
                    highlight > 100):
                raise ValueError("sorry, you lose")
            if width == 0 and height == 0 and (color == 'red' or
                                               emphasis is None):
                raise ValueError("I don't think so -- values are %s, %s" %
                                 (width, height))
            Blob.__init__(self, width, height,
                          color, emphasis, highlight)

空行(Blank line)
-----------

使用2个空行来分隔最高级的函数(function)和类(class)定义。

使用1个空行来分隔类中的方法(method)定义。

（尽量少地）使用额外的空行来分隔一组相关的函数。在一系列相关的只有一行的函数之间，空格也可以被省略，比如一组dummy实现。

在函数内（尽量少地）使用空行使代码逻辑更清晰。

Python支持control-L（如:^L）换页符作为空格；许多工具将这些符号作为分页符，因此你可以使用这些符号来分页或者区分文件中的相关区域。注意，一些编辑器和基于web的代码预览器可能不会将control-L识别为分页符，而是显示成其他符号。

源文件编码(Source File Encoding)
--------------------

Python核心发行版中的代码应该一直使用UTF-8（Python 2中使用ASCII）。

使用ASCII（Python 2）或者UTF-8（Python 3）的文件不应该添加编码声明。

再标准库中，只有用作测试目的，或者注释或文档字符串需要提及作者名字不得不使用非ASCII字符时，才能使用非默认的编码。否则，在字符串文字中包括非ASCII数据时，推荐使用``\x``, ``\u``, ``\U``或``\N``等转义符。

对于Python 3.0及其以后的版本中，标准库遵循以下原则（参见PEP 3131）：Python标准库中的所有标识符都**必须**只采用ASCII编码的标识符，在可行的条件下也**应当**使用英文词（很多情况下，使用的缩写和技术术语词都不是英文）。此外，字符串文字和注释应该只包括ASCII编码。只有两种例外：
(a) 测试情况下为了测试非ASCII编码的特性
(b) 作者名字。作者名字不是由拉丁字母组成的也**必须**提供一个拉丁音译名。

鼓励面向全世界的开源项目都采用类似的原则。


Imports
-------

- Imports should usually be on separate lines, e.g.::
- Imports应该分行写，而不是都写在一行，例如：

      Yes: import os
           import sys

      No:  import sys, os

  It's okay to say this though::
  但这样写也是可以的：

      from subprocess import Popen, PIPE

- Imports are always put at the top of the file, just after any module
  comments and docstrings, and before module globals and constants.
- Imports应该写在代码文件的开头，位于module注释和文档字符串之后，module全局变量(globals)和常量(constants)声明之前。

  Imports should be grouped in the following order:
  Imports应该按照下面的顺序分组来写：

  1. standard library imports
  2. related third party imports
  3. local application/library specific imports
  4. 标准库imports
  5. 相关第三方imports
  6. 本地应用/库的具体imports

  You should put a blank line between each group of imports.
  不同组的imports之前用空格隔开。

  Put any relevant ``__all__`` specification after the imports.
  将任何相关的``__all__``说明(specification)放在imports之后。

- Absolute imports are recommended, as they are usually more readable
  and tend to be better behaved (or at least give better error
  messages) if the import system is incorrectly configured (such as
  when a directory inside a package ends up on ``sys.path``)::
- 推荐使用绝对(absolute)imports，因为这样通常更易读，在import系统没有正确配置（比如中的路径以``sys.path``结束）的情况下，也会有更好的表现（或者至少会给出错误信息）：

    import mypkg.sibling
    from mypkg import sibling
    from mypkg.sibling import example

  However, explicit relative imports are an acceptable alternative to
  absolute imports, especially when dealing with complex package layouts
  where using absolute imports would be unnecessarily verbose::
  然而，除了绝对imports，显式的相对imports也是一种可以接受的替代方式。特别是当处理复杂的包布局(package layouts)时，采用绝对imports会显得啰嗦。

    from . import sibling
    from .sibling import example

  Standard library code should avoid complex package layouts and always
  use absolute imports.
  标准库代码应当避免复杂的包布局，一直使用绝对imports。

  Implicit relative imports should *never* be used and have been removed
  in Python 3.
  隐式的相对imports应该**永不**使用，并且Python 3中已经被去掉了。

- When importing a class from a class-containing module, it's usually
  okay to spell this::
- 当从一个包括类的模块中import一个类时，通常可以这样写：

      from myclass import MyClass
      from foo.bar.yourclass import YourClass

  If this spelling causes local name clashes, then spell them ::
  如果和本地命名的拼写产生了冲突，应当直接import模块：

      import myclass
      import foo.bar.yourclass

  and use "myclass.MyClass" and "foo.bar.yourclass.YourClass".
  然后使用"myclass.MyClass"和"foo.bar.yourclass.YourClass".

- Wildcard imports (``from <module> import *``) should be avoided, as
  they make it unclear which names are present in the namespace,
  confusing both readers and many automated tools. There is one
  defensible use case for a wildcard import, which is to republish an
  internal interface as part of a public API (for example, overwriting
  a pure Python implementation of an interface with the definitions
  from an optional accelerator module and exactly which definitions
  will be overwritten isn't known in advance).
- 避免使用通配符imports(``from <module> import *``)，因为会造成在当前命名空间出现的命名含义不清晰，给读者和许多自动化工具造成困扰。有一个可以正当使用通配符import的情形，即将一个内部接口重新发布成公共API的一部分。比如，使用备选的加速模块中的定义去覆盖纯Python实现的接口，将被覆盖的定义恰好在不能提前知晓。

  When republishing names this way, the guidelines below regarding
  public and internal interfaces still apply.
  当使用这种方式重新发布命名时，以下关于公共和内部接口的指南仍然适用。


String Quotes
字符串引用
=============

In Python, single-quoted strings and double-quoted strings are the
same.  This PEP does not make a recommendation for this.  Pick a rule
and stick to it.  When a string contains single or double quote
characters, however, use the other one to avoid backslashes in the
string. It improves readability.
在Python中，用单引号和双引号来表示字符串是一样的。但在这里不推荐将这两种方式看作一样。选择一种规则并坚持使用。当字符串中包含单引号（双引号）时，采用双引号（单引号）来表示字符串，避免使用反斜杠。这样使代码更易读。

For triple-quoted strings, always use double quote characters to be
consistent with the docstring convention in PEP 257.
对于三引号表示的字符串，永远在其中使用双引号字符来和PEP 257的文档字符串规则保持一致。

Whitespace in Expressions and Statements
表达式和语句中的空格
========================================

Pet Peeves
一些痛点
----------

Avoid extraneous whitespace in the following situations:
在下列情形中避免使用过多的空白：

- Immediately inside parentheses, brackets or braces. ::
- 方括号，圆括号和花括号之后：

      Yes: spam(ham[1], {eggs: 2})
      No:  spam( ham[ 1 ], { eggs: 2 } )

- Immediately before a comma, semicolon, or colon::
- 逗号，分号或冒号之前：

      Yes: if x == 4: print x, y; x, y = y, x
      No:  if x == 4 : print x , y ; x , y = y , x

- However, in a slice the colon acts like a binary operator, and
  should have equal amounts on either side (treating it as the
  operator with the lowest priority).  In an extended slice, both
  colons must have the same amount of spacing applied.  Exception:
  when a slice parameter is omitted, the space is omitted.
- 不过，在分片操作时，冒号和二元运算符是一样的，应该在其左右两边有相同数量的空格（就像对待优先级最低的运算符一样）。在扩展的分片操作中，所有的冒号的左右两边的空格数都应该相等。也有例外：当切片操作中的参数被省略时，应该也忽略空格。
  Yes::

      ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
      ham[lower:upper], ham[lower:upper:], ham[lower::step]
      ham[lower+offset : upper+offset]
      ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
      ham[lower + offset : upper + offset]

  No::

      ham[lower + offset:upper + offset]
      ham[1: 9], ham[1 :9], ham[1:9 :3]
      ham[lower : : upper]
      ham[ : upper]

- Immediately before the open parenthesis that starts the argument
  list of a function call::
- 在调用函数时传递参数list的括号之前：

      Yes: spam(1)
      No:  spam (1)

- Immediately before the open parenthesis that starts an indexing or
  slicing::
- 在索引和切片操作的左括号之前：

      Yes: dct['key'] = lst[index]
      No:  dct ['key'] = lst [index]

- More than one space around an assignment (or other) operator to
  align it with another.
- 赋值(或其他)运算符周围使用多个空格来和其他语句对齐：

  Yes::

      x = 1
      y = 2
      long_variable = 3

  No::

      x             = 1
      y             = 2
      long_variable = 3


Other Recommendations
其他建议
---------------------

- Always surround these binary operators with a single space on either
  side: assignment (``=``), augmented assignment (``+=``, ``-=``
  etc.), comparisons (``==``, ``<``, ``>``, ``!=``, ``<>``, ``<=``,
  ``>=``, ``in``, ``not in``, ``is``, ``is not``), Booleans (``and``,
  ``or``, ``not``).
- 在二元云算符的两边都使用一个空格：赋值运算符(``=``)，增量赋值运算符(``+=``, ``-=``
  etc.)，比较运算符(``==``, ``<``, ``>``, ``!=``, ``<>``, ``<=``,
  ``>=``, ``in``, ``not in``, ``is``, ``is not``)，布尔运算符(``and``,
  ``or``, ``not``)。

- If operators with different priorities are used, consider adding
  whitespace around the operators with the lowest priority(ies). Use
  your own judgment; however, never use more than one space, and
  always have the same amount of whitespace on both sides of a binary
  operator.
- 如果使用了优先级不同的运算符，则在优先级较低的操作符周围增加空白。请你自行判断，不过永远不要用超过1个空格，永远保持二元运算符两侧的空白数量一样。

  Yes::

      i = i + 1
      submitted += 1
      x = x*2 - 1
      hypot2 = x*x + y*y
      c = (a+b) * (a-b)

  No::

      i=i+1
      submitted +=1
      x = x * 2 - 1
      hypot2 = x * x + y * y
      c = (a + b) * (a - b)

- Don't use spaces around the ``=`` sign when used to indicate a
  keyword argument or a default parameter value.
- 使用``=``符号来表示关键字参数或默认参数值时，不要在其周围使用空格。

  Yes::

      def complex(real, imag=0.0):
          return magic(r=real, i=imag)

  No::

      def complex(real, imag = 0.0):
          return magic(r = real, i = imag)

- Do use spaces around the ``=`` sign  of an annotated function definition.
  Additionally, use a single space after the ``:``, as well as a single space
  on either side of the ``->`` sign representing an annotated return value.
- 在带注释的函数定义中需要在``=``符号周围加上空格。此外, 在``:``后使用一个空格，在``->``表示带注释的返回值时，其两侧各使用一个空格。

  Yes::

      def munge(input: AnyStr):
      def munge(sep: AnyStr = None):
      def munge() -> AnyStr:
      def munge(input: AnyStr, sep: AnyStr = None, limit=1000):

  No::

      def munge(input: AnyStr=None):
      def munge(input:AnyStr):
      def munge(input: AnyStr)->PosInt:

- Compound statements (multiple statements on the same line) are
  generally discouraged.
- 复合语句（即将多行语句写在一行）一般是不鼓励使用的。

  Yes::

      if foo == 'blah':
          do_blah_thing()
      do_one()
      do_two()
      do_three()

  Rather not::

      if foo == 'blah': do_blah_thing()
      do_one(); do_two(); do_three()

- While sometimes it's okay to put an if/for/while with a small body
  on the same line, never do this for multi-clause statements.  Also
  avoid folding such long lines!
- 有时也可以将短小的if/for/while中的语句写在一行，但对于有多个分句的语句永远不要这样做。也要避免将多行都写在一起。
  Rather not::

      if foo == 'blah': do_blah_thing()
      for x in lst: total += x
      while t < 10: t = delay()

  Definitely not::

      if foo == 'blah': do_blah_thing()
      else: do_non_blah_thing()

      try: something()
      finally: cleanup()

      do_one(); do_two(); do_three(long, argument,
                                   list, like, this)

      if foo == 'blah': one(); two(); three()

Comments
注释
========

Comments that contradict the code are worse than no comments.  Always
make a priority of keeping the comments up-to-date when the code
changes!
和代码矛盾的注释还不如没有。当代码有改动时，一定要优先更改注释使保持更新。

Comments should be complete sentences.  If a comment is a phrase or
sentence, its first word should be capitalized, unless it is an
identifier that begins with a lower case letter (never alter the case
of identifiers!).
注释应该是完整的几个句子。如果注释是一个短语或一个句子，其首字母应该大写，除非开头是一个以小写字母开头的标识符（永远不要更改标识符的大小写）。

If a comment is short, the period at the end can be omitted.  Block
comments generally consist of one or more paragraphs built out of
complete sentences, and each sentence should end in a period.
如果注释很短，结束的句号可以被忽略。块注释通常由一段或几段完整的句子组成，每个句子都应该以句号结束。

You should use two spaces after a sentence-ending period.
你应该在句尾的句号后再加上2个空格。

When writing English, follow Strunk and White.
使用英文写作，参考Strunk和White的《The Elements of Style》

Python coders from non-English speaking countries: please write your
comments in English, unless you are 120% sure that the code will never
be read by people who don't speak your language.
来自非英语国家的Python程序员们，请使用英文来写注释，除非你120%确定你的代码永远不会被不使用你所用语言的人阅读到。

Block Comments
块注释
--------------

Block comments generally apply to some (or all) code that follows
them, and are indented to the same level as that code.  Each line of a
block comment starts with a ``#`` and a single space (unless it is
indented text inside the comment).
块注释一般写在对应代码之前，并且和对应代码有同样的缩进级别。块注释的每一行都应该以``#``和一个空格开头（除非这在注释内缩进对齐的文本）。

Paragraphs inside a block comment are separated by a line containing a
single ``#``.
块注释中的段落应该用只含有单个``#``的一行隔开。

Inline Comments
行内注释
---------------

Use inline comments sparingly.
尽量少用行内注释。

An inline comment is a comment on the same line as a statement.
Inline comments should be separated by at least two spaces from the
statement.  They should start with a # and a single space.
行内注释是和代码语句写在一行内的注释。行内注释应该至少和代码语句之间有两个空格的间隔，并且以``#``和一个空格开始。

Inline comments are unnecessary and in fact distracting if they state
the obvious.  Don't do this::
行内注释不是必要的，在代码含义很明显时甚至会让人分心。请不要这样做：

    x = x + 1                 # Increment x

But sometimes, this is useful::
但这样做是有用的：

    x = x + 1                 # Compensate for border

Documentation Strings
文档字符串
---------------------

Conventions for writing good documentation strings
(a.k.a. "docstrings") are immortalized in PEP 257.
要知道如何写出好的文档字符串（docstring），请参考PEP 257。

- Write docstrings for all public modules, functions, classes, and
  methods.  Docstrings are not necessary for non-public methods, but
  you should have a comment that describes what the method does.  This
  comment should appear after the ``def`` line.
- 所有的公共模块，函数，类和方法都应该有文档字符串。对于非公共方法，文档字符串不是必要的，但你应该留有注释说明该方法的功能，该注释应当出现在``def``的下一行。

- PEP 257 describes good docstring conventions.  Note that most
  importantly, the ``"""`` that ends a multiline docstring should be
  on a line by itself, e.g.::
- PEP 257描述了好的文档字符应该遵循的规则。其中最重要的是，多行文档字符串以单行``"""``结尾，不能有其他字符，例如：

      """Return a foobang

      Optional plotz says to frobnicate the bizbaz first.
      """

- For one liner docstrings, please keep the closing ``"""`` on
  the same line.
- 对于仅有一行的文档字符串，结尾处的``"""``应该也写在这一行里。


Version Bookkeeping
版本注记
===================

If you have to have Subversion, CVS, or RCS crud in your source file,
do it as follows. ::
如果你必须在源代码中包含Subversion, CVS或RCS crud，请这样做：

    __version__ = "$Revision$"
    # $Source$

These lines should be included after the module's docstring, before
any other code, separated by a blank line above and below.
以上几行的内容应当在模块的文档字符串之后，在其他代码之前，并且在其开始和结束都使用一个空行隔开。


Naming Conventions
命名约定
==================

The naming conventions of Python's library are a bit of a mess, so
we'll never get this completely consistent -- nevertheless, here are
the currently recommended naming standards.  New modules and packages
(including third party frameworks) should be written to these
standards, but where an existing library has a different style,
internal consistency is preferred.
Python标准库的命名约定有一些混乱，所以我们永远都无法保持一致。不过，还是有一些现在推荐的命名标准。新的模块和包（包括第三方框架）应该采用这些标准，但若是已经存在的库有另一套风格的话，还是应当与原有的风格保持内部一致。

Overriding Principle
重写原则
--------------------

Names that are visible to the user as public parts of the API should
follow conventions that reflect usage rather than implementation.
用户可见的公共部分API的命名应当展示其功能而非其实现。

Descriptive: Naming Styles
描述性：命名风格
--------------------------

There are a lot of different naming styles.  It helps to be able to
recognize what naming style is being used, independently from what
they are used for.
有很多不同的命名风格，能够独立地从命名对象的用途看出采用了哪种命名风格，将是很有帮助的。

The following naming styles are commonly distinguished:
以下是常用于区分的命名风格：

- ``b`` (single lowercase letter单个小写字母)
- ``B`` (single uppercase letter单个大写字母)
- ``lowercase``
- ``lower_case_with_underscores``
- ``UPPERCASE``
- ``UPPER_CASE_WITH_UNDERSCORES``
- ``CapitalizedWords`` (or CapWords, or CamelCase -- so named because
  of the bumpy look of its letters [3]_也叫做CapWords或者CamelCase -- 因为单词字母大写看起来很像驼峰).  This is also sometimes known
  as StudlyCaps. 也被称作StudlyCaps。

  Note: When using abbreviations in CapWords, capitalize all the
  letters of the abbreviation.  Thus HTTPServerError is better than
  HttpServerError.
  注意：当CapWords里包含缩写时，将缩写部分的字母都大写。HTTPServerError比HttpServerError要好。
- ``mixedCase`` (differs from CapitalizedWords by initial lowercase
  character!和CapitalizedWords不同在于其首字母小写！)
- ``Capitalized_Words_With_Underscores`` (ugly!超丑！)

There's also the style of using a short unique prefix to group related
names together.  This is not used much in Python, but it is mentioned
for completeness.  For example, the ``os.stat()`` function returns a
tuple whose items traditionally have names like ``st_mode``,
``st_size``, ``st_mtime`` and so on.  (This is done to emphasize the
correspondence with the fields of the POSIX system call struct, which
helps programmers familiar with that.)
也有风格使用短小唯一的前缀来表示一组相关的命名。这在Python中并不常见，但为了完整起见这里也捎带提一下。比如，``os.stat()``函数返回一个tuple，其中的元素名原本为``st_mode``,``st-size``,``st_mtime``等等。（这样做是为了强调和POSIX系统调用结构之间的关系，可以帮助程序员更好熟悉。）

The X11 library uses a leading X for all its public functions.  In
Python, this style is generally deemed unnecessary because attribute
and method names are prefixed with an object, and function names are
prefixed with a module name.
X11库中的公共函数名都以X开头。在Python中这样的风格一般被认为是不必要的，因为属性和方法名之前已经有了对象名的前缀，而函数名前也有了模块名的前缀。

In addition, the following special forms using leading or trailing
underscores are recognized (these can generally be combined with any
case convention):
此外，要区别以下划线开始或结尾的特殊形式（可以和气其它的规则结合起来）：

- ``_single_leading_underscore``: weak "internal use" indicator.
  E.g. ``from M import *`` does not import objects whose name starts
  with an underscore.
- ``_single_leading_underscore``: 以单个下划线开头是"内部使用"的弱指示符。
  E.g. ``from M import *``不会import以下划线开头的对象。

- ``single_trailing_underscore_``: used by convention to avoid
  conflicts with Python keyword, e.g. ::
- ``single_trailing_underscore_``: 以单个下划线结尾用来避免和Python关键词产生冲突，例如:

      Tkinter.Toplevel(master, class_='ClassName')

- ``__double_leading_underscore``: when naming a class attribute,
  invokes name mangling (inside class FooBar, ``__boo`` becomes
  ``_FooBar__boo``; see below).
- ``__double_leading_underscore``: 以双下划线开头的风格命名类属性表示触发命名修饰（在FooBar类中，``__boo``命名会被修饰成``_FooBar__boo``; 见下）。

- ``__double_leading_and_trailing_underscore__``: "magic" objects or
  attributes that live in user-controlled namespaces.
  E.g. ``__init__``, ``__import__`` or ``__file__``.  Never invent
  such names; only use them as documented.
- ``__double_leading_and_trailing_underscore__``: 以双下划线开头和结尾的命名风格表示生存在用户控制的命名空间里“魔法的”对象或属性。
  E.g. ``__init__``, ``__import__`` 或 ``__file__``。请依照文档描述来使用这些命名，千万不要自己发明。

Prescriptive: Naming Conventions
规范性：命名约定
--------------------------------

Names to Avoid
需要避免的命名
~~~~~~~~~~~~~~

Never use the characters 'l' (lowercase letter el), 'O' (uppercase
letter oh), or 'I' (uppercase letter eye) as single character variable
names.
不要使用字符'l'（小写的字母el），'O'（大写的字母oh），或者'I'（大写的字母eye）来作为单个字符的变量名。

In some fonts, these characters are indistinguishable from the
numerals one and zero.  When tempted to use 'l', use 'L' instead.
在一些字体中，这些字符和数字1和0无法区别开来。当想使用'l'时，使用'L'代替。

Package and Module Names
包和模块命名
~~~~~~~~~~~~~~~~~~~~~~~~

Modules should have short, all-lowercase names.  Underscores can be
used in the module name if it improves readability.  Python packages
should also have short, all-lowercase names, although the use of
underscores is discouraged.
模块命名应短小，且为全小写。若下划线能提高可读性，也可以在模块名中使用。Python包命名也应该短小，且为全小写，但不应使用下划线。

Since module names are mapped to file names, and some file systems are
case insensitive and truncate long names, it is important that module
names be chosen to be fairly short -- this won't be a problem on Unix,
but it may be a problem when the code is transported to older Mac or
Windows versions, or DOS.
模块名是对应到文件名的，一些文件系统会区分大小写并且会将长的文件名截断。因此模块名应该尽量短小，这个问题在Unix系统上是不存在的，但把代码移植到较旧的Mac，Windows版本或DOS系统上时，可能会出现问题。

When an extension module written in C or C++ has an accompanying
Python module that provides a higher level (e.g. more object oriented)
interface, the C/C++ module has a leading underscore
(e.g. ``_socket``).
当使用C或C++写的扩展模块有相应的Python模块提供更高级的接口时（e.g. 更加面向对象），C/C++模块名以下划线开头（e.g. ``_sociket``）。

Class Names
类命名
~~~~~~~~~~~

Class names should normally use the CapWords convention.
类命名应该使用单词字母大写（CapWords）的命名约定。

The naming convention for functions may be used instead in cases where
the interface is documented and used primarily as a callable.
当接口已有文档说明且主要是被用作调用时，也可以使用函数的命名约定。

Note that there is a separate convention for builtin names: most builtin
names are single words (or two words run together), with the CapWords
convention used only for exception names and builtin constants.
注意对于内建命名有一个特殊的约定：大部分内建名都是一个单词（或者两个一起使用的单词），单词首字母大写(CapWords)的约定只对异常命名和内建常量使用。

Exception Names
异常命名
~~~~~~~~~~~~~~~

Because exceptions should be classes, the class naming convention
applies here.  However, you should use the suffix "Error" on your
exception names (if the exception actually is an error).
由于异常实际上也是类，因此类命名约定也适用与异常。不同的是，如果异常实际上是抛出错误的话，异常名前应该加上"Error"的前缀。

Global Variable Names
全局变量命名
~~~~~~~~~~~~~~~~~~~~~

(Let's hope that these variables are meant for use inside one module
only.)  The conventions are about the same as those for functions.
（在此之前，我们先假定这些变量都仅在同一个模块内使用。）这些约定同样也适用于函数命名。

Modules that are designed for use via ``from M import *`` should use
the ``__all__`` mechanism to prevent exporting globals, or use the
older convention of prefixing such globals with an underscore (which
you might want to do to indicate these globals are "module
non-public").
对于引用方式设计为``from M import *``的模块，应该使用``__all__``机制来避免引入全局变量，或者采用下划线前缀的旧约定来命名全局变量，从而表明这些变量是“模块非公开的”。

Function Names
函数命名
~~~~~~~~~~~~~~

Function names should be lowercase, with words separated by
underscores as necessary to improve readability.
函数命名应该都是小写，必要时使用下划线来提高可读性。

mixedCase is allowed only in contexts where that's already the
prevailing style (e.g. threading.py), to retain backwards
compatibility.
只有当已有代码风格已经是混合大小写时（比如threading.py），为了保留向后兼容性才使用混合大小写。

Function and method arguments
函数和方法参数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Always use ``self`` for the first argument to instance methods.
实例方法的第一参数永远都是``self``。

Always use ``cls`` for the first argument to class methods.
类方法的第一个参数永远都是``cls``。

If a function argument's name clashes with a reserved keyword, it is
generally better to append a single trailing underscore rather than
use an abbreviation or spelling corruption.  Thus ``class_`` is better
than ``clss``.  (Perhaps better is to avoid such clashes by using a
synonym.)
在函数参数名和保留关键字冲突时，相对于使用缩写或拼写简化，使用以下划线结尾的命名一般更好。比如，``class_``比``clss``更好。（或许使用同义词避免这样的冲突是更好的方式。）


Method Names and Instance Variables
方法命名和实例变量
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the function naming rules: lowercase with words separated by
underscores as necessary to improve readability.
使用函数命名的规则：必要时小写单词使用下划线分开以提高可读性。

Use one leading underscore only for non-public methods and instance
variables.
仅对于非公开方法和变量命名在开头使用一个下划线。

To avoid name clashes with subclasses, use two leading underscores to
invoke Python's name mangling rules.
避免和子类的命名冲突，使用两个下划线开头来触发Python的命名修饰机制。

Python mangles these names with the class name: if class Foo has an
attribute named ``__a``, it cannot be accessed by ``Foo.__a``.  (An
insistent user could still gain access by calling ``Foo._Foo__a``.)
Generally, double leading underscores should be used only to avoid
name conflicts with attributes in classes designed to be subclassed.
Python类名的命名修饰规则：如果类Foo有一个属性叫``__a``，不能使用``Foo.__a``的方式访问该变量。（有用户可能仍然坚持使用``Foo._Foo__a``的方法访问。）一般来说，两个下划线开头的命名方法只应该用来避免设计为子类的属性中的命名冲突。

Note: there is some controversy about the use of __names (see below).
注意：关于__names的使用也有一些争论（见下）。

Constants
常量
~~~~~~~~~

Constants are usually defined on a module level and written in all
capital letters with underscores separating words.  Examples include
``MAX_OVERFLOW`` and ``TOTAL``.
常量通常是在模块级别定义的，使用全部大写并用下划线将单词分开。例如：``MAX_OVERFLOW``和``TOTAL``

Designing for inheritance
继承的设计
~~~~~~~~~~~~~~~~~~~~~~~~~

Always decide whether a class's methods and instance variables
(collectively: "attributes") should be public or non-public.  If in
doubt, choose non-public; it's easier to make it public later than to
make a public attribute non-public.
记得永远区别类的方法和实例变量（属性）应该是公开的还是非公开的。如果有疑虑的话，请选择非公开的；因为之后将非公开属性变为公开属性要容易些。

Public attributes are those that you expect unrelated clients of your
class to use, with your commitment to avoid backward incompatible
changes.  Non-public attributes are those that are not intended to be
used by third parties; you make no guarantees that non-public
attributes won't change or even be removed.
公开属性是那些和你希望和你定义的类无关的客户来使用的，并且承诺避免向后不兼容的改变。非公开属性是那些不希望被第三方使用的部分，你可以不用保证非公开属性不会变化或被移除。

We don't use the term "private" here, since no attribute is really
private in Python (without a generally unnecessary amount of work).
我们在这里没有使用“私有（private）”这个词，因为在Python里没有什么属性是真正私有的（这样设计省略了大量不必要的工作）。

Another category of attributes are those that are part of the
"subclass API" (often called "protected" in other languages).  Some
classes are designed to be inherited from, either to extend or modify
aspects of the class's behavior.  When designing such a class, take
care to make explicit decisions about which attributes are public,
which are part of the subclass API, and which are truly only to be
used by your base class.
另一类属性属于子类API的一部分（在其他语言中经常被称为"protected"）。一些类是为继承设计的，要么扩展要么修改类行为的部分。当设计这样的类时，需要谨慎明确地决定哪些属性是公开的，哪些属于子类API，哪些真的只会被你的基类调用。

With this in mind, here are the Pythonic guidelines:
在大脑中形成这种想法后，下面是Python化的指南说明：

- Public attributes should have no leading underscores.
- 公开属性不应该有开头下划线。

- If your public attribute name collides with a reserved keyword,
  append a single trailing underscore to your attribute name.  This is
  preferable to an abbreviation or corrupted spelling.  (However,
  notwithstanding this rule, 'cls' is the preferred spelling for any
  variable or argument which is known to be a class, especially the
  first argument to a class method.)
- 如果公开属性的名字和保留关键字有冲突，在你的属性名尾部加上一个下划线。这比采用缩写和简写更好。（然而，和这条规则冲突的是，‘cls’对任何变量和参数来说都是一个更好地拼写，因为大家都知道这表示class，特别是在类方法的第一个参数里。）

  Note 1: See the argument name recommendation above for class methods.
  注意 1：对于类方法，参考之前的参数命名建议。

- For simple public data attributes, it is best to expose just the
  attribute name, without complicated accessor/mutator methods.  Keep
  in mind that Python provides an easy path to future enhancement,
  should you find that a simple data attribute needs to grow
  functional behavior.  In that case, use properties to hide
  functional implementation behind simple data attribute access
  syntax.
-对于简单地公共数据属性，最后仅公开属性名字，不要公开复杂的调用或mutator方法。记住在Python中，提供了一条简单的路径来实现未来增强，你应该简单数据属性需要增加功能行为。这种情况下，使用properties来将功能实现隐藏在简单数据属性访问语法之后。

  Note 1: Properties only work on new-style classes.
  注意 1：Properties 仅仅对新风格类有用。

  Note 2: Try to keep the functional behavior side-effect free,
  although side-effects such as caching are generally fine.
  注意 2：尽量保证功能行为没有副作用，尽管副作用比如缓存等并没有什么大问题。

  Note 3: Avoid using properties for computationally expensive
  operations; the attribute notation makes the caller believe that
  access is (relatively) cheap.
  注意 3: 对计算量大的运算避免试用properties；属性的注解会让调用者相信访问的运算量是相对较小的。

- If your class is intended to be subclassed, and you have attributes
  that you do not want subclasses to use, consider naming them with
  double leading underscores and no trailing underscores.  This
  invokes Python's name mangling algorithm, where the name of the
  class is mangled into the attribute name.  This helps avoid
  attribute name collisions should subclasses inadvertently contain
  attributes with the same name.
- 如果你的类是子类的话，你有一些属性并不像让子类访问，考虑将他们命名为两个下划线开头并且结尾处没有下划线。这样会触发Python命名修饰算法，类名会被修饰添加到属性名中。这样可以避免属性命名冲突，以免子类会不经意间包含相同的命名。

  Note 1: Note that only the simple class name is used in the mangled
  name, so if a subclass chooses both the same class name and attribute
  name, you can still get name collisions.
  注意 1：注意命名修饰仅仅是简单地将类名加入到修饰名中，所以如果子类有相同的类名合属性名，你可能仍然会遇到命名冲突问题。

  Note 2: Name mangling can make certain uses, such as debugging and
  ``__getattr__()``, less convenient.  However the name mangling
  algorithm is well documented and easy to perform manually.
  注意 2：命名修饰可以有特定用途，比如在调试时，``__getattr__()``比较不方便。然而命名修饰算法的可以很好地记录，并且容意手动执行。

  Note 3: Not everyone likes name mangling.  Try to balance the
  need to avoid accidental name clashes with potential use by
  advanced callers.
  注意 3：不是所有人都喜欢命名修饰。试着权衡避免偶然命名冲突的需求和试用高级调用者使用的潜在可能性。


Public and internal interfaces
公开和内部接口
------------------------------

Any backwards compatibility guarantees apply only to public interfaces.
Accordingly, it is important that users be able to clearly distinguish
between public and internal interfaces.
任何向后兼容性保证仅对公开接口适用。相应地，用户能够清楚分辨公开接口和内部接口是很重要的。

Documented interfaces are considered public, unless the documentation
explicitly declares them to be provisional or internal interfaces exempt
from the usual backwards compatibility guarantees. All undocumented
interfaces should be assumed to be internal.
文档化的接口被认为是公开的，除非文档中明确申明了它们是临时的或者内部接口，不保证向后兼容性。所有文档中未提到的接口应该被认为是内部的。

To better support introspection, modules should explicitly declare the
names in their public API using the ``__all__`` attribute. Setting
``__all__`` to an empty list indicates that the module has no public API.
为了更好审视公开接口和内部接口，模块应该在``__all``属性中明确申明公开API是哪些。将``__all__``设为空list表示该模块中没有公开API。

Even with ``__all__`` set appropriately, internal interfaces (packages,
modules, classes, functions, attributes or other names) should still be
prefixed with a single leading underscore.
即使正确设置了``__all``属性，内部接口（包，模块，类，函数，属性或其他命名）也应该以一个下划线开头。

An interface is also considered internal if any containing namespace
(package, module or class) is considered internal.
如果接口的任一一个命名空间（包，模块或类）是内部的，那么该接口也应该是内部的。

Imported names should always be considered an implementation detail.
Other modules must not rely on indirect access to such imported names
unless they are an explicitly documented part of the containing module's
API, such as ``os.path`` or a package's ``__init__`` module that exposes
functionality from submodules.
导入的命名应该永远被认为是实现细节。其他模块不应当依赖这些非直接访问的导入命名，除非它们在文档中明确地被写为模块的API，例如``os.path``或者包的``__init__``模块，那些从子模块展现的功能。


Programming Recommendations
编程建议
===========================

- Code should be written in a way that does not disadvantage other
  implementations of Python (PyPy, Jython, IronPython, Cython, Psyco,
  and such).
- 代码实现应该最好不会在其他Python实现版本中存在缺陷。

  For example, do not rely on CPython's efficient implementation of
  in-place string concatenation for statements in the form ``a += b``
  or ``a = a + b``.  This optimization is fragile even in CPython (it
  only works for some types) and isn't present at all in implementations
  that don't use refcounting.  In performance sensitive parts of the
  library, the ``''.join()`` form should be used instead.  This will
  ensure that concatenation occurs in linear time across various
  implementations.
  比如，不要依赖CPython对于in-place字符串连接的高效实现，针对``a += b``或``a = a + b``的语句形式。该优化在CPython中很脆弱（只对几种类型有效）并且在不使用refcounting的实现中根本没用。在库中性能敏感的部分，应当使用``''.join()``形式。这可以保证在各种实现版本中的字符串连接都是线性时间。

- Comparisons to singletons like None should always be done with
  ``is`` or ``is not``, never the equality operators.
- 对于单件（singleton）如None的比较应当永远使用``is``或``is not``，绝不使用等号运算符。

  Also, beware of writing ``if x`` when you really mean ``if x is not
  None`` -- e.g. when testing whether a variable or argument that
  defaults to None was set to some other value.  The other value might
  have a type (such as a container) that could be false in a boolean
  context!
  并且，小心使用``if x``，当你真正想表达的是``if x is not None``。比如，当测试一个变量或参数的默认None是否被赋了其他值时。其他的值的类型（比如作为一个容器container）可能是布尔含义下地false。

- Use ``is not`` operator rather than ``not ... is``.  While both
  expressions are functionally identical, the former is more readable
  and preferred.
- 使用``is not``比使用``not ... is``更好。尽管两种表达的功能是一样的，但前者的可读性更好。

  Yes::

      if foo is not None:

  No::

      if not foo is None:

- When implementing ordering operations with rich comparisons, it is
  best to implement all six operations (``__eq__``, ``__ne__``,
  ``__lt__``, ``__le__``, ``__gt__``, ``__ge__``) rather than relying
  on other code to only exercise a particular comparison.
- 当

  To minimize the effort involved, the ``functools.total_ordering()``
  decorator provides a tool to generate missing comparison methods.

  PEP 207 indicates that reflexivity rules *are* assumed by Python.
  Thus, the interpreter may swap ``y > x`` with ``x < y``, ``y >= x``
  with ``x <= y``, and may swap the arguments of ``x == y`` and ``x !=
  y``.  The ``sort()`` and ``min()`` operations are guaranteed to use
  the ``<`` operator and the ``max()`` function uses the ``>``
  operator.  However, it is best to implement all six operations so
  that confusion doesn't arise in other contexts.

- Always use a def statement instead of an assignment statement that binds
  a lambda expression directly to an identifier.

  Yes::

      def f(x): return 2*x

  No::

      f = lambda x: 2*x

  The first form means that the name of the resulting function object is
  specifically 'f' instead of the generic '<lambda>'. This is more
  useful for tracebacks and string representations in general. The use
  of the assignment statement eliminates the sole benefit a lambda
  expression can offer over an explicit def statement (i.e. that it can
  be embedded inside a larger expression)

- Derive exceptions from ``Exception`` rather than ``BaseException``.
  Direct inheritance from ``BaseException`` is reserved for exceptions
  where catching them is almost always the wrong thing to do.

  Design exception hierarchies based on the distinctions that code
  *catching* the exceptions is likely to need, rather than the locations
  where the exceptions are raised. Aim to answer the question
  "What went wrong?" programmatically, rather than only stating that
  "A problem occurred" (see PEP 3151 for an example of this lesson being
  learned for the builtin exception hierarchy)

  Class naming conventions apply here, although you should add the
  suffix "Error" to your exception classes if the exception is an
  error.  Non-error exceptions that are used for non-local flow control
  or other forms of signaling need no special suffix.

- Use exception chaining appropriately. In Python 3, "raise X from Y"
  should be used to indicate explicit replacement without losing the
  original traceback.

  When deliberately replacing an inner exception (using "raise X" in
  Python 2 or "raise X from None" in Python 3.3+), ensure that relevant
  details are transferred to the new exception (such as preserving the
  attribute name when converting KeyError to AttributeError, or
  embedding the text of the original exception in the new exception
  message).

- When raising an exception in Python 2, use ``raise ValueError('message')``
  instead of the older form ``raise ValueError, 'message'``.

  The latter form is not legal Python 3 syntax.

  The paren-using form also means that when the exception arguments are
  long or include string formatting, you don't need to use line
  continuation characters thanks to the containing parentheses.

- When catching exceptions, mention specific exceptions whenever
  possible instead of using a bare ``except:`` clause.

  For example, use::

      try:
          import platform_specific_module
      except ImportError:
          platform_specific_module = None

  A bare ``except:`` clause will catch SystemExit and
  KeyboardInterrupt exceptions, making it harder to interrupt a
  program with Control-C, and can disguise other problems.  If you
  want to catch all exceptions that signal program errors, use
  ``except Exception:`` (bare except is equivalent to ``except
  BaseException:``).

  A good rule of thumb is to limit use of bare 'except' clauses to two
  cases:

  1. If the exception handler will be printing out or logging the
     traceback; at least the user will be aware that an error has
     occurred.

  2. If the code needs to do some cleanup work, but then lets the
     exception propagate upwards with ``raise``.  ``try...finally``
     can be a better way to handle this case.

- When binding caught exceptions to a name, prefer the explicit name
  binding syntax added in Python 2.6::

      try:
          process_data()
      except Exception as exc:
          raise DataProcessingFailedError(str(exc))

  This is the only syntax supported in Python 3, and avoids the
  ambiguity problems associated with the older comma-based syntax.

- When catching operating system errors, prefer the explicit exception
  hierarchy introduced in Python 3.3 over introspection of ``errno``
  values.

- Additionally, for all try/except clauses, limit the ``try`` clause
  to the absolute minimum amount of code necessary.  Again, this
  avoids masking bugs.

  Yes::

      try:
          value = collection[key]
      except KeyError:
          return key_not_found(key)
      else:
          return handle_value(value)

  No::

      try:
          # Too broad!
          return handle_value(collection[key])
      except KeyError:
          # Will also catch KeyError raised by handle_value()
          return key_not_found(key)

- When a resource is local to a particular section of code, use a
  ``with`` statement to ensure it is cleaned up promptly and reliably
  after use. A try/finally statement is also acceptable.

- Context managers should be invoked through separate functions or methods
  whenever they do something other than acquire and release resources.
  For example:

  Yes::

               with conn.begin_transaction():
                   do_stuff_in_transaction(conn)

  No::

               with conn:
                   do_stuff_in_transaction(conn)

  The latter example doesn't provide any information to indicate that
  the __enter__ and __exit__ methods are doing something other than
  closing the connection after a transaction.  Being explicit is
  important in this case.

- Use string methods instead of the string module.

  String methods are always much faster and share the same API with
  unicode strings.  Override this rule if backward compatibility with
  Pythons older than 2.0 is required.

- Use ``''.startswith()`` and ``''.endswith()`` instead of string
  slicing to check for prefixes or suffixes.

  startswith() and endswith() are cleaner and less error prone.  For
  example::

      Yes: if foo.startswith('bar'):
      No:  if foo[:3] == 'bar':

- Object type comparisons should always use isinstance() instead of
  comparing types directly. ::

      Yes: if isinstance(obj, int):

      No:  if type(obj) is type(1):

  When checking if an object is a string, keep in mind that it might
  be a unicode string too!  In Python 2, str and unicode have a
  common base class, basestring, so you can do::

      if isinstance(obj, basestring):

  Note that in Python 3, ``unicode`` and ``basestring`` no longer exist
  (there is only ``str``) and a bytes object is no longer a kind of
  string (it is a sequence of integers instead)

- For sequences, (strings, lists, tuples), use the fact that empty
  sequences are false. ::

      Yes: if not seq:
           if seq:

      No: if len(seq)
          if not len(seq)

- Don't write string literals that rely on significant trailing
  whitespace.  Such trailing whitespace is visually indistinguishable
  and some editors (or more recently, reindent.py) will trim them.

- Don't compare boolean values to True or False using ``==``. ::

      Yes:   if greeting:
      No:    if greeting == True:
      Worse: if greeting is True:

- The Python standard library will not use function annotations as
  that would result in a premature commitment to a particular
  annotation style.  Instead, the annotations are left for users to
  discover and experiment with useful annotation styles.

  It is recommended that third party experiments with annotations use an
  associated decorator to indicate how the annotation should be
  interpreted.

  Early core developer attempts to use function annotations revealed
  inconsistent, ad-hoc annotation styles.  For example:

  * ``[str]`` was ambiguous as to whether it represented a list of
    strings or a value that could be either *str* or *None*.

  * The notation ``open(file:(str,bytes))`` was used for a value that
    could be either *bytes* or *str* rather than a 2-tuple containing
    a *str* value followed by a *bytes* value.

  * The annotation ``seek(whence:int)`` exhibited a mix of
    over-specification and under-specification: *int* is too
    restrictive (anything with ``__index__`` would be allowed) and it
    is not restrictive enough (only the values 0, 1, and 2 are
    allowed).  Likewise, the annotation ``write(b: bytes)`` was also
    too restrictive (anything supporting the buffer protocol would be
    allowed).

  * Annotations such as ``read1(n: int=None)`` were self-contradictory
    since *None* is not an *int*.  Annotations such as
    ``source_path(self, fullname:str) -> object`` were confusing about
    what the return type should be.

  * In addition to the above, annotations were inconsistent in the use
    of concrete types versus abstract types:  *int* versus *Integral*
    and set/frozenset versus MutableSet/Set.

  * Some annotations in the abstract base classes were incorrect
    specifications.  For example, set-to-set operations require
    *other* to be another instance of *Set* rather than just an
    *Iterable*.

  * A further issue was that annotations become part of the
    specification but weren't being tested.

  * In most cases, the docstrings already included the type
    specifications and did so with greater clarity than the function
    annotations.  In the remaining cases, the docstrings were improved
    once the annotations were removed.

  * The observed function annotations were too ad-hoc and inconsistent
    to work with a coherent system of automatic type checking or
    argument validation.  Leaving these annotations in the code would
    have made it more difficult to make changes later so that
    automated utilities could be supported.


.. rubric:: Footnotes

.. [#fn-hi] *Hanging indentation* is a type-setting style where all
   the lines in a paragraph are indented except the first line.  In
   the context of Python, the term is used to describe a style where
   the opening parenthesis of a parenthesized statement is the last
   non-whitespace character of the line, with subsequent lines being
   indented until the closing parenthesis.


References
==========

.. [^1] PEP 7, Style Guide for C Code, van Rossum

.. [^2] Barry's GNU Mailman style guide
       http://barry.warsaw.us/software/STYLEGUIDE.txt

.. [^3] http://www.wikipedia.com/wiki/CamelCase

.. [^4] PEP 8 modernisation, July 2013
   http://bugs.python.org/issue18472


Copyright
=========

This document has been placed in the public domain.


