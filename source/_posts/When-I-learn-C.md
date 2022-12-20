---
title: When I learn C
date: 2022-12-17 11:31:00
tags: C notes
---

# HERE IS NOTE FOR *Head First C* 😃

## 0 关于

这里是符号说明，形如

- 这是一条个人笔记

>这是小拓展

## 1 入门

### 运行

`gcc -o` 设置文件名

`./name` 类Unix操作系统中运行程序必须指定程序所在的目录，除非该程序目录已在PATH环境变量中，所以运行命令

### 字符串

C不支持现成的字符串

定义字符串数组需额外加一个字符的空间来容纳`/0`（NULL字符），字符的索引值为偏移量

单引号通常用来表示单个字符，而双引号通常用来表示字符串。通常应该用双引号来定义字符串。用双引号定义的字符串叫字符串字面值（string literal），比起字符数组，它输入起来也更方便

总线错误（bus error）意味着程序无法更新那一块存储器空间

### 等号

`num = 1` 等号用来赋值（assignment）

`num == 1` 检查值是否相等

`nunm += 2 or num -= 2` 加减2

`num++ or num--` 加减1

### 布尔运算

`&& || !` 与或非

`&`和`|`操作符总是计算两个条件，而`&&`和`||`可以跳过第二个条件

位运算 `6 & 4` 等于4

### switch

switch语句检查一个单独的值。计算机会在第一个匹配的case语句处开始执行代码。在遇到break或到达switch语句的末尾前，代码会一直运行。

```c
switch(train) {
case 37:
	winnings = winnings + 50;
	break;
case 65:
	puts("头等奖!");
	winnings = winnings + 80;		//执行完这条接着再+20这条，直到break
case 12:
	winnings = winnings + 20;
	break;
default:
	winnings = 0;
}
```

### for

```c
int counter;
for (counter = 1; counter < 11; counter++) {	//(初始化循环变量；循环运行前检查；循环后执行)
printf("%i个枣\n", counter);
}
```

可以用`break`在任意时刻退出循环

可以用`continue`随时跳到循环条件处

### 链式赋值

`x = y = 4;`给多个变量赋相同值

 ## 2 指针

使用指针的主要目的之一就是让函数共享存储器，指针只是一个保存存储器地址的变量，它们是进程存储器中真实编号的地址

### 储存器

局部变量保存在栈（Stack），全局变量保存在全局量区（Globals）

`&x`指x的地址 `%p`来格式化输出

> 栈
> 这是存储器用来保存局部变量的部分。每当调用函数，函数的所有局部变量都在栈上创建。它之所以叫栈是因为它看起来就像堆积而成的栈板：当进入函数时，变量会放到栈顶；离开函数时，把变量从栈顶拿走。奇怪的是，栈做起事来颠三倒四，它从存储器的顶部开始，向下增长。
>
> 堆
> 堆用于动态存储：程序在运行时创建一些数据，然后使用很长一段时间。
>
> 全局量
> 全局量位于所有函数之外，并对所有函数可见。程序一开始运行时就会创建全局量，可以修改它们，不像常量。
>
> 常量
> 常量也在程序一开始运行时创建，但它们保存在只读存储器中。常量是一些在程序中要用到的不变量，你不会想修改它们的值，例如字符串字面值。
>
> 代码
> 最后是代码段，很多操作系统都把代码放在存储器地址的低位。代码段也是只读的，它是存储器中用来加载机器代码的部分。

### int *

`int *address_of_x = &x;`

`*`来解引用，如`int value_stored = *address_of_x;` 、`*address_of_x = 99;`

### sizeof（）

这是个运算符

```c
sizeof(int);
sizeof("Turtles!");		//返回9
```

### 数组变量

数组变量可以被用作指针，指向数组中的第一个元素。函数参数声明如为数组，则其会被当作指针处理

计算机不会为数组变量分配任何空间，编译器仅在出现它的地方把它替换成数组的起始地址，所以不能把它指向任何其他地方

> 指针退化
>
> 假如把数组赋给指针变量，指针变量只会包含数组的地址信息，而对数组的长度一无所知，相当于指针丢失了一些信息，也就是指针退化。
>
> 只要把数组传递给函数，数组免不了退化为指针，但需要记清楚代码中有哪些地方发生过数组退化，因为它们会引发一些不易察觉的错误。

```c
int drinks[] = {4, 2, 3};		//drinks[i] == *(drinks + i)
doses[3] == *(doses + 3) == *(3 + doses) == 3[doses]
```

### 指针类型

指针之所以有类型，是因为编译器在指针算术运算时需要知道加几

如`int nums[] = {1, 2, 3};`中`nums`与`nums+1`的地址间隔4个字节（如果int通常占4个字节）

- 这里的数组内的每一个值都是int类型，都占4字节

### scanf() vs fgets()

`scanf()`会导致缓冲区溢出,引发段错误（abort trap）,应限制`scanf()`读取字符串的长度

`fgets(char指针，sizeof(char指针),stdin)` stdin表示数据来自键盘

`fgets()`缓冲区大小把`\0`字符也算了进去，所以不必像`scanf()`那样把长度减1

如果要向`fgets()`函数传递数组变量，就用`sizeof`，如果只是传指针，就应该输入想要的长度。

`scanf()`不但允许输入多个字段，而且允许输入结构化数据，可以指定两个字段之间以什
么字符分割，`fgets()`只允许向缓冲区中输入一个字符串，而且只能是字符串，不能是其他数据类型，只能有一个缓冲区

当`scanf()`用`%s`读取字符串时，遇到空格就会停止。如果想要输入多个单词，需要多次调用`scanf()`，或使用一些复杂的正则表达式技巧，`fgets()`总能读取整个字符串

**Conclusion**:	如果需要输入由多个字段构成的结构化数据，可以使用`scanf()`；而如果想要输入一个非结构化的字符串，`fgets()`将是不二之选

### cards[ ]还是*cards

字符串字面值保存在只读存储器中。如果想要修改字符串，需要在新的数组中创建副本。可以将char指针声明成为`const char *`，以防代码用它修改字符串

## 2.5 字符串原理

### 创建数组的数组

```c
char tracks[][80] = {			//第一对方括号用来访问由所有字符串组成的数组
    						  //第二对方括号用来访问每个单独的字符串
	"I left my heart in Harvard Med School",
	"Newark, Newark - a wonderful town",
	"Dancing with a Dork",
	"From here to maternity",
	"The girl from Iwo Jima",
};
```

可以用`char strings[...][...]`来创建数组的数组。
第一组方括号用来访问外层数组
第二组方括号用来访问每个内层数组中的元素

### 使用string.h

```c
#include <stdio.h>
#include <string.h>
strcmp()
可以比较字符串
strchr()
用来在字符串中找到某个字符的位置
strlen()
可以得到字符串的长度
strcpy()
可以复制字符串
```

使用以上代码引用头文件

### 使用strstr（）

```c
char s0[] = "dysfunctional";
char s1[] = "fun";
if (strstr(s0, s1))
	puts("我在dysfunctional中找到fun了!");
```

> 全局变量位于任何函数之外，所有函数都可以调用它们。

### 指针的数组

```c
char *names_for_dog[] = {"Bowser", "Bonza", "Snodgrass"};
//可以像访问数组的数组那样访问指针的数组,一个字符串字面值配一个指针
```



## 3 小工具

小工具设计遵循以下原则：

- 从标准输入读取数据
- 在标准输出显示数据
- 处理文本数据，而不是难以阅读的二进制格式
- 只做一件简单的事
- 如果想完成一个不同的任务，应该另外写一个小工具
- 小工具通常读写文本数据

### 过滤器（filter）

这是一种小工具，它逐行读取数据，对数据进行处理，再把数据写到某个地方。如果你的计算机是Unix，或你在Windows上安装了Cygwin，就已经拥有很多过滤器工具了。

> head：显示文件前几行的内容
> tail：显示文件最后几行的内容
> sed：流编辑器（stream editor），用来搜索和替换文本

### 重定向

可以重定向标准输入、标准输出，让程序从键盘以外的地方读数据、往显示器以外的地方写数据，如文件。可用命令

```bash
$ ./geo2json < gpsdata.csv > output.json
```

### 错误状态码

程序在数据中发现错误就会退出，并把退出状态置为2。怎么在程序结束后检查错误状态呢？

要看操作系统

如果你的计算机是Mac、Linux、其他UNIX，或你在Windows上使用Cygwin，可以用以下命令显示错误状态：

```bash
$ echo $?
```

如果用的是Windows的命令提示符，则可以输入：

```bash
C:\> echo %ERRORLEVEL%
```

这两条命令做了相同的事：显示程序结束时返回的那个数字。

### 标准错误

标准错误——一个用来发送错误消息的二号输出

人有两只耳朵和一张嘴，但进程有一只耳朵（标准输入）和两张嘴（标准输出和标准错误）

### fprintf()打印到数据流

printf()函数可以将数据发送到标准输出，但printf()只是fprintf()一个的特例

scanf()  fscanf(stdin, ...)

```c
printf("我喜欢乌龟！");
fprintf(stdout, "我喜欢乌龟！");
//以上命令等价
```

stdout 标准输出

stderr  标准错误

stdin 标准输入

`>`重定向标准输出	`<`重定向标准输入	`2>`重定向标准错误

### 用管道连接输入与输出

符号 | 表示管道（pipe），它能连接一个进程的标准输出与另一个进程的标准输入

`bermuda | geo2json`

```bash
> (./bermuda | ./geo2json) < spooky.csv > output.json
```

只要在每个程序前加上一个`|`就可以连接多个程序，一连串相连的进程就叫流水线（pipeline），这时`<`会把文件内容发送到流水线中第一个进程的标准输入，`>`会捕获流水线中最后一个进程的标准输出

外面的括号是必需的,这对括号保证了数据文件由bermuda程序的标准输入来读取

### 创建自己的数据流

每条数据流用一个指向文件的指针来表示，可以用`fopen()`函数创建新数据流

```c
FILE *in_file = fopen("input.txt", "r");	//将创建一条数据流，从文件中读取数据
FILE *out_file = fopen("output.txt", "w");	//将创建一条数据流，向文件写数据
```

fopen()函数接收两个参数：文件名和模式。共有三种模式

“w”= 写（write）	写文件
“r”= 读（read）	读文件
“a”= 追加（append）	在文件末尾追加数据

创建数据流后，可以用fprintf()往数据流中打印数据
可以用fscanf()函数从文件中读取数据

```c
fprintf(out_file, "%s 和 %s ", "红 ", "绿");
fscanf(in_file, "%79[^\n]\n", sentence);
```

最当用完数据流，别忘了关闭它。虽然所有的数据流在程序结束后都会自动关闭，但你仍应该自己关闭它们

```c
fclose(in_file);
fclose(out_file);
```

通常情况下，一个进程最多可以有256条数据流。但数据流的数量是有限的，用完后应该关闭它们。

最早FILE是用宏定义的，而宏的名字通常都要大写。

### 首选项

对GUI程序来说，可以修改程序的首选项；而对于categorize这样的命令行程序，可以传给它命令行参数

```c
int main(int argc, char *argv[])
{
.... 做事情....
}

```

main()函数有两个版本，一个有命令行参数，一个没有。命令行参数通过两个变量传递给main()函数，一个是参数的计数（argc），另一个是指针（指向参数字符串）数组

用户运行程序时，命令行中第一个参数是程序名。也就是说，第一个命令行参数其实是argv[1]

```bash
>./categorize mermaid mermaid.csv Elvis elvises.csv the_rest.csv
   argv[0]	 argv[1]				...				argv[5]
```

在程序中打开文件准备读写时，最好检查一下有没有错误发生。好在如果数据流打开失败，fopen()函数会返回0，也就是说如果想检查错误，可以将下面这段代码：

```c
FILE *in = fopen("我不存在.txt", "r");
```

改成这样：

```c
FILE *in;
if (!(in = fopen("我不存在.txt", "r"))) {
	fprintf(stderr, "无法打开文件.\n");
	return 1;
}
```

### 使用库 getopt()

unistd.h头文件不属于C标准库，而是POSIX库中的一员。POSIX的目标是创建一套能够在所有主流操作系统上使用的函数。

getop()使用范例

```bash
> rocket_to -e 4 -a Brasilia Tokyo London
```

```c
#include <unistd.h>
...
while ((ch = getopt(argc, argv, "ae:")) != EOF)
//ae:告诉getopt()函数“a和e是有效选项”，e后面的冒号表示“-e后面需要再跟一个参数”，getopt()会用optarg变量指向这个参数
	switch(ch) {
	...
	case 'e':
		engine_count = optarg;
	...
	}
argc -= optind;		//最后这两行用来跳过已读取的选项
argv += optind;		//optind保存了“getopt()函数从命令行读取了几个选项”
```

经过一番处理，0号参数不再是程序名了。argv[0]会指向选项后的第一个命令行参数。循环结束以后，为了让程序读取命令行参数，需要调整一下argv和argc变量，跳过所有选项。

```bash
> rocket_to -e 4 -a Brasilia  Tokyo   London
					argv[0]  argv[1]  argv[2]
```

Tips：

- 可以多个选项`abc:d`
- 可以合并命令行里的选项，如：`-td now` 与 `-d now -t`
- 可以改变选项之间顺序，因为我们用循环读取选项，所以 `-d now -t`、`-t -d now`、`-td now`都一样(
- 只要程序在命令行看到一个前缀为－值，就会把它当成选项处理，前提是它必须在命令行参数之前出现
- 为了避免歧义，可以用`--`隔开参数和选项，比如`set_temper-ature -c -- -4`。getopt()看到`--`就会停止读取选项，程序会把后面的内容当成普通的命令行参数读取



## 4 使用多个源文件

### 简明数据类型指南

- char
  字符在计算机的存储器中以字符编码的形式保存，字符编码是一个数字，因此在计算机看来，A与数字65完全一样（ASCII码）
- int
  如果你要保存一个整数，通常可以使用int。不同计算机中int的大小不同，但至少应该有16位。一般而言，int可以保存几万以内的数字
- long
  但如果想保存一个很大的计数呢？long数据类型就是为此而生的。在某些计算机中，long的大小是int的两倍，所以可以保存几十亿以内的数字；但大部分计算机的long和int一样大，因为在这些计算机中int本身就很大。long至少应该有32位
- float
  float是保存浮点数的基本数据类型。平时你会碰到很多浮点数，比如一杯香橙摩卡冰乐有多少毫升，就可以用float保存
- double
  但如果想表示很精确的浮点数呢？如果想让计算结果精确到小数点以后很多位，可以使用double。double比float多占一倍空间，可以保存更大、更精确的数字

>为什么把一个很大的数保存到short中会变成负数？
>
>数字以二进制保存，二进制的100 000看起来像这样：
>x <- 0001 1000 0110 1010 0000
>当计算机想把这个值保存到short时，发现只能保存2个字节，所以只保存了数字右半边：
>y <- 1000 0110 1010 0000
>最高位是1的二进制有符号数会被当成负数处理，它等价于下面的十进制数：
>-31072

### 使用类型转换临时转换数值的类型

```c
int x = 7;
int y = 2;
//float z = (float)x / (float)y;
float z = (float)x / y
//如果编译器发现有整数在加、减、乘、除浮点数，会自动完成转换，因此可以减少代码中显式类型转换的次数：
printf("z = %f\n", z);
```

### 两个关键字

unsigned
用unsigned修饰的数值只能是非负数。由于无需记录负数，无符号数有更多的位可以使用，因此它可以保存更大的数。unsigned int可以保存0到最大值的数。这个最大值是int可以保存最大值的两倍左右。还有signed关键字，但你几乎从没见过，因为所有数据类型默认都是有符号的。
`unsigned char c;`

long
没错，你可以在数据类型前加long，让它变长。longint是加长版的int；long int可以保存范围更广的数字；long long比long更长；还可以对浮点数用long。(c99和c11支持long long)
`long double d;`

>%.2f把浮点数格式化为小数点后两位
>
>%hi用来格式化short

```c
#include <stdio.h>
#include <limits.h> //含有表示整型（比如int和char）大小的值
#include <float.h> //含有表示float和double类型大小的值
int main()
{
printf("The value of INT_MAX is %i\n", INT_MAX);
printf("The value of INT_MIN is %i\n", INT_MIN);
printf("An int takes %z bytes\n", sizeof(int));
printf("The value of FLT_MAX is %f\n", FLT_MAX);
printf("The value of FLT_MIN is %.50f\n", FLT_MIN);
printf("A float takes %z bytes\n", sizeof(float));
    //可把INT和FLT替换成CHAR（char）、DBL（double）、SHRT（short）或LNG（long）
return 0;
}
```

>位数是计算机能够处理的数值长度

### 声明与定义分离

`float add_with_tax()(float f);`

声明只是一个函数签名：一条包含函数名、形参类型与返回类型的记录

### 创建头文件

两步

1. 创建一个扩展名为.h的文件，把你的声明写在里面，不用在头文件中包含main()函数，反正也没有函数会调用它
2. 在主代码中包含头文件，应该在代码的顶部加一句include

`#include 'asdf.h'` `#include <stdio.h>`

通常情况下，引号表示以相对路径查找头文件，如果不加目录名，只包含一个文件名，编译器就会在当前目录下查找头文件；如果用了尖括号，编译器就会以绝对路径查找头文件

当编译器看到尖括号，就会到标准库代码所在目录查找头文件，但现在你的头文件和.c文件在同一目录下，用引号把文件名括起来，编译器就会在本地查找文件。本地头文件也可以带目录名，但通常会把它和C文件放在相同目录中。

当编译器在代码中读到#include，就会读取头文件中的内容，仿佛它们本来就在代码中。

> 如果编译器发现你调用了一个它没见过的函数，就会假设这个函数返回int

### 保留字

|   1    |    2     |    3     |
| :----: | :------: | :------: |
|  auto  |    if    |  break   |
|  int   |   case   |   long   |
|  char  | register | continue |
| return | default  |  short   |
|   do   |  sizeof  |  double  |
| static |   else   |  struct  |
| entry  |  switch  |  extern  |
| typeof |  float   |  union   |
|  for   | unsigned |   goto   |
| while  |   enum   |   void   |
| const  |  signed  | volatile |

### 共享代码

为了共享代码，可以把代码放到一个单独的C文件中。
需要把函数声明放到一个单独的.h头文件中。
在所有需要使用共享代码的C文件中包含这个头文件。
在编译的命令中列出所有C文件。

```c
//encrypt.h
void encrypt(char *message);
```

```c
//encrypt.c
#include "encrypt.h"
void encrypt(char *message){
	char c;
	while (*message) {
	*message = *message ^ 31;
	message++;
	}
}
```

```c
//mainprog.c
#include <stdio.h>
#include "encrypt.h"
int main(){
	char msg[80];
	while (fgets(msg, 80, stdin)) {
	encrypt(msg);
	printf("%s", msg);
	}
}
```

```bash
./ gcc message_hider.c encrypt.c -o message_hider
```

> 共享变量
> 为了防止两个源文件中的同名变量相互干扰，变量的作用域仅限于某个文件内。如果你想共享变量，就应该在头文件中声明，并在变量名前加上extern关键字：
> `extern int passcode;`

### gccの编译

gcc -c会编译代码，但不会链接目标文件

```bash
./ gcc -c *.c
```

gcc -o 链接，在例子中把目标文件链接为一个叫launch的可执行程序

```bash
./ gcc *.o -o launch
```

### Make

如果你掌握了某样东西的简单规则，别多想，自动化它

make编译的文件叫目标（target）。目标可以是任何用其他文件生成的文件，也就是说目标可以是一批文件压缩而成的压缩文档

make需要知道：依赖项、生成方法。依赖项和生成方法合在一起构成了一条规则。有了规则，make就知道如何生成目标

>versions :
>
>UNIX make
>
>MinGW mingw32-make
>
>Microsoft  NMAKE

makefile文件书写

```makefile
launch.o: launch.c launch.h thruster.h		//目标：依赖项
	gcc -c launch.c							//生成方法（必须以tab开头）
											//这是规则
thruster.o: thruster.h thruster.c
	gcc -c thruster.c
launch: launch.o thruster.o
	gcc launch.o thruster.o -o launch
```

之后直接

```bash
./ make launch
```

>更自动化的工具：autoconf

-> [GUN Make Manual](http://tinyurl.com/yczmjx)

## 5 结构、联合与位字段

### 结构 Struct

结构化数据结构 structured data type

结构是一种由一系列其他数据类型组成的数据类型。

```c
struct fish {
	const char *name;
	const char *species;
	int teeth;
	int age;
};
```

创建一个新自定义的数据类型，由其它一批数据组成。

- 结构的大小固定
- 结构中的数据都有名字

创建数据：

```c
struct fish snappy = {"Snappy", "Piranha", 69, 4}
```

把参数封装在结构中，代码会更稳定

结构变量是结构本身的名字

读取时只能按名访问，使用“.”运算符读取结构字段：<结构>.<字段名>语法（也叫“点表示法”）

```c
struct fish snappy = {"Snappy", "piranha", 69, 4};
printf("Name = %s\n", snappy.name);
```

为结构变量赋值相当于叫计算机复制数据

### 结构中的结构

为什么要嵌套定义结构？

之所以要这么做是为了对抗复杂性 。通过使用结构，我们可以建立更大的数据块。通过把结构组合在一起，我们可以创建更大的数据结构。本来你只能用int、short，但有了结构以后，就可以描述十分复杂的东西，比如网络流和视频图像。

```c
struct preferences {
	const char *food;
	float exercise_hours;
};
struct fish {
	const char *name;
	const char *species;
	int teeth;
	int age;
	struct preferences care; //nesting 嵌套
   struct fish snappy = {"Snappy", "Piranha", 69, 4, {"Meat", 7.5}};
   printf("Snappy 喜欢吃 %s", snappy.care.food);
	printf("Snappy 喜欢锻炼 %f hours", snappy.care.exercise_hours); //访问
};
```

### typedef

在C语言中可以为结构创建别名，你只要在struct关键字前加上typedef，并在右花括号后写上类型名，就可以在任何地方使用这种新类型。

```c
typedef struct cell_phone {
	int cell_no;
	const char *wallpaper;
	float minutes_of_charge;
} phone;
```

当你用typedef为结构创建别名，需要决定别名叫什么。别名其实就是类型名，也就是说结构有两个名字：一个是结构名（struct cell_phone），另一个是类型名（phone）。为什么要有两个名字？一般一个就够了。如果只写类型名而不写结构名，编译器也没意见：

```c
typedef struct {
	int cell_no;
	const char *wallpaper;
	float minutes_of_charge;
} phone;
phone p = {5557879, "s.png", 1.35};
```

这样的结构称为匿名结构。

### 更新结构

```c
fish snappy = {"Snappy", "piranha", 69, 4};
printf("Hello %s\n", snappy.name);
snappy.teeth = 68;
```

计算机通过把值赋给函数形参的方式向函数传值，所有赋值都会复制

如果想让函数更新结构变量，就不能把结构作为参数传递，因为这样做仅仅是将数据的副本复制给
了函数。取而代之，可以传递结构的地址

还有一种表示结构指针的方法，它更易于阅读。

`(*t).age` 和 `t->age` 等价

“指针->字段”等于“(*指针).字段”	“->”表示法省掉了括号，代码更易阅读。

### 联合？

每次创建结构实例，计算机都会在存储器中相继摆放字段

联合则不同。当定义联合时，计算机只为其中一个字段分配空间，并且计算机会为其中最大的字段分配空间，然后由你决定里面保存什么值

>计算机需要保证联合的大小固定。唯一的办法就是让它足够大，任何一个字段都能装得下

```c
typedef union {			//这里的关键字是union
	short count;
	float weight;
	float volume;
} quantity;
```

### 使用联合

- C89 方式

  把值赋给联合中第一个字段

  ```c
  quantity q = {4};
  ```

- 指定初始化器（designated initializer）

  ```c
  quantity q = { .weight = 1.5 };
  ```

- 点 表示法

  在第一行创建变量，然后在第二行设置字段的值

  ```c
  quantity q;
  q.volume = 3.7;
  ```

无论用哪种方法设置联合的值，都只会保存一条数据。联合只是提供了一种创建支持不同数据类型的变量的方法

“指定初始化器”也可以用来设置结构字段的初值，并提高代码的可读性

```c
typedef struct {
	const char *color;
	int gears;
	int height;
} bike;

bike b = {.height=17, .gears=21};
```

### 联合与结构

```c
typedef struct {
    const char *name ;
    const char *country;
    quantity amount;
} fruit_order;

fruit_order apples = {"apples","English",.amount.weight = 4.2}
printf("This order contains %2.2f lbs of %s\n",apples.amount.weight, apples.name);
```

### 枚举变量保存符号

你需要某种方法记录我们在联合中保存了什么值。

结构与联合用分号（;）来分割数据项，而枚举用逗号。

```c
enum colors {RED, GREEN, PUCE};		//可以用typedef为类型起个名字
enum colors favorite = PUCE;
```

***so？枚举好处？？？*** 限制我能给的值？ 实例感受下：

```c
typedef enum {
    COUNT,
    POUNDS,
    PINTS
}unit_of_measure;

typedef struct {
    const char *name;
    const char *country;
    quantity amount;
    unit_of_measure units;
}fruit_order;

void display(fruit_order order) {
    printf("This order contains ");
    if (order.units==PINTS) printf("%2.2f pints of %s\n", order.amount.volume, order.name);

    else if (order.units==POUNDS) printf("%2.2f lbs of %s\n", order.amount.weight, order.name);
    else printf("%i %s\n", order.amount.count, order.name);
}

int main() {
    fruit_order strawberries= {
        "strawberries",
        "Spain",
        .amount.weight=17.6,
        POUNDS
    };
    display(strawberries);
    return 0;
}
```

### 位字段（bitfield）

C语言不支持二进制字面值，不过它支持十六进制字面值。每当C语言看到0x开头的数字，就认为它是以16为基数的数字（0x54）

可以用位字段指定一个字段有多少位

```c
typedef struct {
	unsigned int low_pass_vcf:1;		//位字段应当声明为unsigned int
	unsigned int filter_coupler:1;		//表示该字段只使用1位存储空间
	unsigned int reverb:1;
	unsigned int sequential:4;
	...
} synth;
```

如果你有一连串的位字段，计算机会放在一起，以节省空间，也就是说如果有8个1位的位字段，计算机就会把它们保存在一个字节中

如果编译器发现结构中只有一个位字段，还是会把它填充成一个字，这就是为什么位字段总是组合在一起

## 6 数据结构与动态存储

### 链表

保存可变数量的数据，插入数据非常快

链表是一种抽象数据结构。链表是通用的，可以用来保存很多不同类型的数据

链表保存了一条数据和一个链向另一条数据的链接

如果一个结构包含一个链向同种结构的链接，那么这个结构就被称为递归结构

只要在结构中保存指针，island数据就含有下一个我们将游览的island的地址。只要我们的代码能访问一个island，就能够跳到下一个island。

在递归结构中，需要包含一个相同类型的指针， C语言的语法不允许用typedef别名来声明它，因此必须为结构起一个名字

```c
typedef struct island {
char *name;
char *opens;
char *closes;
struct island *next;
} island;

island amity = {"Amity", "09:00", "17:00", NULL};
island craggy = {"Craggy", "09:00", "17:00", NULL};
island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
island shutter = {"Shutter", "09:00", "17:00", NULL};

amity.next = &craggy;
craggy.next = &isla_nublar;
isla_nublar.next = &shutter;

\\插入数据
island skull = {"Skull", "09:00", "17:00", NULL};
isla_nublar.next = &skull;
skull.next = &shutter;

void display(island *start){
    island *i = start;
    for(; i != NULL; i = i->next){
	printf("Name:%s\n open:%s-%s\n",i->name,i->open,i->closes);        
    }
}
```

在C语言中，NULL的值实际上为0，NULL专门用来把某个指针设为0。

想快速地插入数据，就需要**链表**。想直接访问元素，就应该用**数组**

### 栈—动态存储



## 11 网络与套接字









## 十大遗漏知识点

### 运算符

- 递增与递减

  ++和--的位置决定了表达式返回i的原始值还是新值，前新后旧

- 三目运算符

  ```c
  if (x == 1)
  	return 2;
  else
  	return 3;

  return (x == 1) ? 2 : 3;	//这里上下等价
  ```

- 位运算

  C语言可以用来编写底层代码，为此它提供了一组位运算符：

  | 运算符 | 说明                 |
  | :----: | -------------------- |
  |   ~a   | a中所有位都取反      |
  | a & b  | a中的位“与”b中的位   |
  | a \| b | a中的位“或”b中的位   |
  | a ^ b  | a中的位“异或”b中的位 |
  |   <<   | 位左移（值增加）     |
  |   >>   | 位右移（值减小）     |

  <<运算符可以用来快速地将某个整型值乘以2的幂，但小心千万别溢出

- 用逗号分割表达式
  for循环在每次循环的末尾都会出现递增操作。
  但如果你想在循环末尾执行多个运算怎么办？可以使用逗号运算符：
  `for (i = 0; i < 10; i++, j++)` 递增i和j。
  之所以要有逗号运算符是因为有时你不想用分号来分割表达式

### 预处理指令



