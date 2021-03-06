# 学习使用cmake

## make和Makefile

  Makefile: 记录项目构建规则流程的普通文件

  make: Makefile解释器，当前命令行下执行make,这时候这个解释程序会到当前路径下寻找名字叫Makefile的文件，对其内部的内容进行解释执行找到Makefile中第一个目标对象，通过依赖对象的时间关系判断是否需要重新生成，若需要则执行命令，完毕后退出；

- Makefile规则：

  ``` Makefile
  <target> : <prerequisites>
  [tab] <command>
  ```

  第一行冒号前面的部分，叫做“目标”，冒号后面的部分叫做“依赖条件”；
  第二行必须由一个tab键开始，后面跟着“命令”。

- Makefile模式规则：

  可以使用模式规则来定义一个隐含规则。一个模式规则像一个一般的规则，只是在规则中，目标的定义需要有“%”字符。它的意思是表示一个或多个任意字符。在依赖目标中同样可以使用“%”，只是依赖目标中的“%”的取值，取决于其目标。也就是说，目标中的模式的“%”决定了依赖目标中“%”的样子。

  ``` Makefile
  %.o : %.c
  ```

  “%.c”就是所有的以.c 结尾的文件

- Makefile自动化变量：

  **目标:**

  `$@`
  表示规则中的目标文件集。在模式规则中，如果有多个目标，那么，"$@"就是匹配与目标中模式定义的集合。

  `$%`
  仅当目标是函数库文件时，"$%"表示规则中的目标成员名。如：如果目标是"foo.a(bar.o)"，那么，"$%"就是"bar.o","%@"就是"foo.a"。如果目标不是函数库文件，那么其值为空。

  `$<`
  依赖目标中的第一个目标名字。如果依赖目标是以模式（即"%"）定义的，那么"%<"将是符合模式的一系列的文件集。注意，其是一个个取出来的。

  **依赖目标:**

  `$?`
  所有比目标新的依赖目标的集合。以空格分隔。

  `$^`
  所有的依赖目标的集合。以空格分隔。如果在依赖目标中有多个重复的，那么这个变量会取出重复的依赖目标，只保留一份。

  `$+`
  这个变量很像"$^"，也是所有依赖目标的几何。只是它不去除重复的依赖目标。

  `$*`
  这个变量表示目标模式中"%"及之前的部分。如果目标的模式是"a.%.b"，那么，"$*"的值是"dir/a.foo"。这个变量对于构造有关联的文件名是比较有效。

- Makefile伪目标

  使用 `.PHONY : xxx` 指定 xxx 为目标，而不会因为存在文件而执行
  ，即假如文件夹中存在 xxx 文件，也会继续执行后续指令

- [Makefile实例](./makefile_demo/Makefile)

- Makefile条件判断

  ``` Makefile
  ifeq/ifneq/ifdef/ifndef <参数/变量名>
    <条件为真时执行语句>
  else
    <条件为假时执行语句>
  endif
  ```

  ifeq 用来判断是否相等，ifneq 就是判断是否不相等，ifdef 用来判断是否存在，ifndef 就是判断是否不存在

- Makefile常见函数使用

  `$(subst <from>,<to>,<text>)`
  此函数的功能是将字符串`<text>`中的`<from>`内容替换为`<to>`，函数返回被替换以后的字符串

  `$(patsubst <pattern>,<replacement>,<text>)`
  此函数查找字符串`<text>`中的单词是否符合模式`<pattern>`，如果匹配就用`<replacement>`来替换掉，`<pattern>`可以使用包括通配符“%”，表示任意长度的字符串，函数返回值就是替换后的字符串。如果`<replacement>`中也包涵“%”，那么`<replacement>`中的“%”将是`<pattern>`中的那个“%”所代表的字符串

  `$(dir <names…>)`
  此函数用来从文件名序列`<names>`中提取出目录部分，返回值是文件名序列`<names>`的目录部分

  `$(notdir <names…>)`
  此函数用与从文件名序列`<names>`中提取出文件名非目录部分

  `$(foreach <var>, <list>,<text>)`
  此函数的意思就是把参数`<list>`中的单词逐一取出来放到参数`<var>`中，然后再执行`<text>`所包含的表达式。每次`<text>`都会返回一个字符串，循环的过程中，`<text>`中所包含的每个字符串会以空格隔开，最后当整个循环结束时，`<text>`所返回的每个字符串所组成的整个字符串将会是函数 foreach 函数的返回值。

  `$(wildcard PATTERN…)`
  通配符“%”只能用在规则中，只有在规则中它才会展开，如果在变量定义和函数使用时，通配符不会自动展开，这个时候就要用到函数 wildcard

## cmake

  待补充
