---
title: When I learn C
date: 2023-1-25 16:23:53
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

`./name` 类Unix操作系统中运行程序必须指定程序所在的目录，除非该程序目录已在PATH环境变量中

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

`scanf()`不但允许输入多个字段，而且允许输入结构化数据，可以指定两个字段之间以什么字符分割

当`scanf()`用`%s`读取字符串时，遇到空格就会停止。如果想要输入多个单词，需要多次调用`scanf()`，或使用一些复杂的正则表达式技巧

`fgets(char指针，sizeof(char指针),stdin)` stdin表示数据来自键盘

`fgets()`缓冲区大小把`\0`字符也算了进去，所以不必像`scanf()`那样把长度减1

如果要向`fgets()`函数传递数组变量，就用`sizeof`，如果只是传指针，就应该输入想要的长度。

`fgets()`只允许向缓冲区中输入一个字符串，而且只能是字符串，不能是其他数据类型，只能有一个缓冲区

`fgets()`总能读取整个字符串

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

### 堆

栈是存储器用来保存局部变量的区域。数据保存在局部变量中，一旦离开函数，变量就会消失。堆是程序中用来保存长期使用数据的地方。堆上的数据不会自动清除，因此堆是保存数据结构的绝佳场所

### malloc() 与 free()

`malloc()`，memory allocation 申请储存器。接收一个参数：所需要的字节数。常与`sizeof`一起使用。`sizeof`告知某种数据类型在系统中占了多少字节。这种数据类型可以是结构，也可以是int或double这样的基本数据类型。返回一个指针，指针中保存了存储器块的起始地址。`free()`需要接收`malloc()`创建的存储器的地址。只要告诉C标准库存储器块从哪里开始，它就能查阅记录，知道要释放多少存储器。

```c
# include <stdlib.h>	//使用malloc()和free()需要包含这个头文件

malloc(sizeof(island));
island *p = malloc(sizeof(island));		//返回的通用指针，即void*类型


free(p);
```

虽然程序结束以后，所有堆空间会自动释放，但用free()显式释放创建的所有动态存储器是一种好的做法

### 字符串复制

`string.h`的头文件中有一个函数`strdup()`。它可以把字符串复制到堆上

```c
char *s = "MONA LISA"
char *copy = strdup(s);
```

作用机理为： 计算出字符串的长度，然后调用malloc()函数在堆上分配相应的空间。再把所有字符复制到堆上的新空间。

strdup()总是在堆上创建空间，而不是在栈上，因为栈用来保存局部变量，而局部变量很快就会被清除。

并且因为strdup()把新字符串放在堆上，所以千万记得要用free()函数释放空间。

有了动态分配存储器，就能在运行时创建需要的存储器。使用malloc()与free()，可以访问动态堆存储器。

### 垃圾收集

一些语言会跟踪程序在堆上分配的数据，当程序不再使用这些数据时，就会释放它们。

C语言非常古老，发明它的时候，绝大多数语言都没有自动 “垃圾回收”机制。

操作系统会在程序结束时清除所有存储器。不过，还是应该显式释放你创建的每样东西，这是一种好的习惯。

### 数据结构

一些举例：

- 关联数组（也叫映射）
  - 连接“键”（key）信息与“值”（value）信息。可以用来连接两种不同类型的数据。
- 双向链表
  - 和普通链表很像，但双向连接，可以双向处理。
- 链表
  - 可以用来保存一串数据项，并使插入新数据项变得简单，但只能沿着一个方向处理。
- 二叉树
  - 每一项都与其他两项相连，可以用来保存层次信息。

### valgrind

valgrind通过伪造malloc()可以监控分配在堆上的数据。当程序想分配堆存储器时，valgrind将会拦截对malloc()和free()的调用，然后运行自己的malloc()和free()。valgrind
的malloc()会记录调用它的是哪段代码和分配了哪段存储器。程序结束时，valgrind会汇报堆上有哪些数据，并告诉这些数据是由哪段代码创建的。

调试信息是编译时打包到可执行文件中的附加数据，比如某段代码在源文件中的行号。只要有调试信息，valgrind就能提供更多有助于发现存储器泄漏的信息。

为了在可执行文件中加入调试信息，需要加上-g开关，并重新编译源代码。

```bash
$ gcc -g spies.c -o spies		// -g 开关告诉编译器要记录要编译代码的行号
```

存储器泄漏是C程序中最难发现的错误。

valgrind工具可以发现泄漏、定位泄漏、检验泄漏是否修复

## 7 高级函数

### 向函数传递函数

```c
int sports_no_bieber(char *s){
	return strstr(s, "sports") && !strstr(s, "bieber");
}
void find(int(*match)(char*)){
	int i;
	puts("Search results:");
	puts("------------------------------------");
	for (i = 0; i < NUM_ADS; i++) {
		if (match(ADS[i])) {
			printf("%s\n", ADS[i]);
		}
	}
	puts("------------------------------------");
}

find(sports_no_bieber);
```

函数名是指向函数的指针，创建函数的同时也创建了一个同名函数指针，指针中保存了函数的地址，当调用函数时，你在使用函数指针。

>两者并不完全相同，函数名是L-value，而指针变量是R-value，因此函数名不能像指针变量那样自加或自减。

函数有不同的返回类型和形参，所以它有许多不同的类型，没有`function*`的说法。

### 创建函数指针

需要把函数的返回类型和接收参数类型告诉C编译器。

```c
int (*warp_fn)(int);
warp_fn = go_to_warp_speed;		
//创建一个叫warp_fn的变量，用来保存go_to_warp_speed()函数的地址。相当于调go_to_warp_speed(4)
warp_fn(4);

char** (*names_fn)(char*,int);
names_fn = album_names;
//创建一个叫names_fn的变量，用来保存album_names()函数的地址。
char** results = names_fn("Sacha Distel", 1972);
```

一旦声明了函数指针变量，就可以像其他变量一样使用它，可以对它赋值，也可以把它加到数组中，还可以把它传给函数

>char**是一个指针，通常用来指向字符串数组

函数指针是C语言最强大的特性之一

```c
char** 		(*	names_fn	)(	char*,int	)
返回类型	       指针变量		    参数类型
    		在这里声明形参的名称
```

>`match(ADS[i])`可以换成`(*match)(ADS[i])`
>
>`find(sports_or_workout)`可以写成`find(&sports_or_workout)`
>
>即使省略*和&，C编译器也能识别它们，这样代码更好读

### 排序qsort()

>void指针(void *)可以保存任何类型数据的地址，但使用前必须把它转换为具体类型

```c
qsort(void *array, size_t length, size_t item_size, int (*compar)(const void *, const void *));
//qsort(数组指针，数组长度，数组中每个元素长度，比较器函数指针（参数）)
```

比较器会返回给qsort()三种值

如果第一个值比第二个值大，就返回正数；如果第一个值比第二个值小，就返回负数；如果两个值相等，就返回0

```c
int compare_scores(const void* score_a, const void* score_b){
	int a = *(int*)score_a;
	int b = *(int*)score_b;	//这样将void指针转换为整型指针
    	...
}
```

字符串是字符指针，指向字符串的指针是指针的指针（绕口令哈哈哈）

```c
int compare_names(const void* a, const void* b){
	char** sa = (char**)a;
	char** sb = (char**)b;
	return strcmp(*sa, *sb); //需要用*来取得字符串
}
```

### 创建函数指针数组

如果想在数组中保存函数，就必须告诉编译器函数的具体特征：函数返回什么类型以及接收什么参数

```c
void (*replies[])(response) = {dump, second_chance, marriage};
//返回类型（*指针变量）（参数类型）
```

> C语言在创建枚举时会给每个符号分配一个从0开始的数字

函数指针数组让代码易于管理，它们让代码变得更短、更易于扩展，从而可以伸缩

### 可变参数函数

参数数量可变的函数被称为可变参数函数。C标准库`stdarg.h`中有一组宏（macro）可以帮助建立可变参数函数

>可以把宏想象成一种特殊类型的函数，它可以修改源代码

```c
\\打印一连串int函数
#include <stdarg.h>
void print_ints(int args, ...){		\\在C语言中，函数参数后的省略号“…”表示还有更多参数
	va_list ap;				\\保存传给函数的其他参数
	va_start(ap, args);		    \\需要把最后一个普通参数写明(至少需要一个普通参数)
	int i;
	for (i = 0; i < args; i++) {
		printf("argument: %i\n", va_arg(ap, int));		\\用va_arg读取保存在va_list中的参数
	}
	va_end(ap);								   \\当读完了所有参数，要用va_end宏来让C结束
}
```

>va_arg接收两个值：va_list和要读取参数的类型。本例中所有参数都是int
>
>宏用来在编译前重写代码，这里的几个宏`va_start`、`va_arg`和`va_end`看起来很像函数，但实际上隐藏在它们背后的是一些神秘的指令。在编译前，预处理器会根据这些指令在程序中插入巧妙的代码

- va_arg 会返回下一个额外参数，那么一定要搭配循环遍历吗？🤔



## 8 静态库与动态库

### 库文件引用

<stdio.h>尖括号代表标准头文件，编译器就会在标准头文件目录中查找文件

"encrypt.h"引号代表本地头文件，与程序在同一目录中

> 通常类UNIX操作系统（如Mac或Linux）中，编译器会在以下目录查找头文件：
> /usr/local/include
> /usr/include
>
> 如果是MinGW版的gcc，编译器会在\MinGW\include中查找

### 共享代码

会希望在程序之间共享两类代码：.h头文件和.o目标文件

#### .h头文件

1. 把头文件保存在标准目录中

   可以在源代码中用尖括号包含它们

2. 在include语句中使用完整路径名

   ```c
   # include "/my_header_files/encrypt.h"
   ```

3. 告诉编译器去哪找头文件

   可以使用gcc的`-I`选项
   
   ```bash
   $ gcc -I/my_header_files test_code.c  ...   -o test_code
   ```
编译器会先检查`-I`选项中的目录，然后像往常一样检查所有标准目录

#### .o目标文件

可以把.o目标文件放在一个类似共享目录的地方，用完整路径名共享

```bash
$ gcc -I/my_header_files test_code.c
	   /my_object_files/encrypt.o
	   /my_object_files/checksum.o -o test_code
```

#### 存档

把一批目标文件打包在一起就成了存档文件。只要创建目标文件存档，就可以一次告诉编译器一
批目标文件

可以使用`nm`命令查看存档中的内容，列出存档中保存文件的名字

可以使用`ar`命令来存档

```bash
$ ar -rcs libhfsecurity.a encrypt.o checksum.o
```

参数r表示如果.a文件存在就更新它，参数c表示创建存档时不显示反馈信息，参数s表示需要ar在.a文件开头建立索引。接着是.a文件的文件名，以及需要存档的文件

所有.a文件名都是libXXX.a的形式。这是命名存档的标准方式，存档是静态库（static library），所以要以lib开头，否则编译器找不到它们

接着可以把存档保存在库目录中，并不同情况下进行编译

1. 把.a文件保存在标准目录中，如/usr/local/lib

   确保代码能正确运行之后会把存档安装在标准目录，这个目录专门用来放本地自定义库

   用-I开关编译代码

   ```bash
   $ gcc test_code.c -lhfsecurity -o test_code
   ```

2. 把.a文件放在其他目录中

   如果还处于开发阶段，或者在系统目录中安装代码不合适，也可以创建自己的库目录`/my_lib`

   用-L选项告诉编译器去哪找存档

   ```bash
   $ gcc test_code.c -L/my_lib -lhfsecurity -o test_code
   ```

如果要使用多个存档，可以设置多个-l选项，`hfsecurity`叫编译器去找一个叫`libhfsecurity.a`的存档，-l选
项后的名字必须与存档名的一部分匹配，如果存档叫libawesome.a，可以用-lawesome开关编译程序

> 不同机器库目录的内容可以相差很多。因为不同操作系统提供了不同的服务。每个.a文件都是一个独立的库，有的库用来连接网络，有的用来创建GUI程序
>
> 找几个.a文件来试用一下nm命令。每个模块都列出了很多名字，它们是一些已经编译好了的函数，可以在程序中使用它们：0000000000000000 T _yywrap，T代表文本(Text)，说明这是一个函数，函数名为yywrap
>
> nm命令会告诉你每个.o目标文件的名字，然后列出所有目标文件中的名字，如果某个名字前出现了T，就说明它是目标文件中某个函数的名字

### 动态库

带有元信息的可重定位目标文件，由一个或多个.o文件创建，核心是一段目标代码

#### 创建

首先创建目标文件

```bash
$ gcc -I/includes -fPIC -c hfcal.c -o hfcal.o
```

`-fPIC`表示创建位置无关代码。有的操作系统和处理器要用位置无关代码创建库，这样它们才能在运行时决定把代码加载到存储器的哪个位置，事实上在大多数操作系统中都不需要加这个选择

> **位置无关代码**
>
> 位置无关代码就是无论计算机把它加载到存储器的哪个位置都可以运行的代码。想象你有一个动态库，它要使用加载点500个字节以外的某个全局变量的值，那么如果操作系统把库加载到其他地方就会出错。只要让编译器创建位置无关的代码，就可以避免这种问题。
> 包括Windows在内的一些操作系统在加载动态库时会使用一种叫存储器映射的技术，也就是说所有代码其实都是位置无关的。若你在Windows上用刚刚那条命令编译代码，gcc可能会给出一条警告，告诉你不需要-fPIC选项。你既可以奉命删除它，也可以当作没看见。

然后生成动态库

绝大部分操作系统都支持动态库，它们的工作方式也大抵相同，但称呼却大相径庭

Windows 动态链接库；Linux和Unix 共享目标文件；Mac 动态库

```bash
$ gcc -shared hfcal.o -o	C:\libs\hfcal.dll	//Windows 上的MinGW
					   /libs/libhfcal.dll.a   //Windows上的Cygwin
					   /libs/libhfcal.so      //Linux或Unix
					   /libs/libhfcal.dylib  //Mac
```

-shared选项告诉gcc你想把.o目标文件转化为动态库

编译器创建动态库时会把库的名字保存在文件中，假设你在Linux中创建了一个叫libhfcal.so的库，那么libhfcal.so文件就会记住它的库名叫hfcal。也就是说，一旦你用某个名字编译了库，就不能再修改文件名了。若想重命名库，就必须用新的名字重新编译一次

> 在一些古老的Mac系统中没有-shared 选项，但是可以用-dynamiclib代替

最后编译程序

一旦创建了动态库，你就可以像静态库那样使用它

```bash
$ gcc -I/include -c elliptical.c -o elliptical.o
$ gcc elliptical.o -L/libs -lhfcal -o elliptical
```

> 在MinGW和Cygwin上，库名的格式有很多种，hfcal的库名可以是: libhfcal.dll.a、libhfcal.dll、hfcal.dll

尽管使用的命令和静态存档一模一样，但两者编译的方式不同。因为库是动态的，所以编译器不会在可执行文件中包含库代码，而是插入一段用来查找库的“占位符”代码，并在运行时链接库

可以运行程序了

Mac可以直接运行当你在Mac中编译程序时，文件的完整路径/libs/libhfcal.dylib保存在可执行文件中，程序启动时知道去哪里找它

在Linux和大部分Unix中，编译器只会记录libhfcal.so库的文件名，而不会包含路径名。也就是说如果不把hfcal库保存到标准目录（如/usr/lib），程序就找不到它。为了解决这个问题，Linux会检查保存在LD_LIBRARY_PATH变量中的附加目录。只要把库目录添加到LD_LIBRARY_PATH中，并export它，elliptical就能找到libhfcal.so

```bash
> export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/libs
```

Windows同Linux一样，它的可执行文件也只保存hfcal库的名字，不保存目录名。不过Windows没有用LD_LIBRARY_PATH变量去找hfcal库。Windows程序会先在当前目录下查找，如果没找到就去查找保存在PATH变量中的目录

```bash
//Cygwin
PATH="$PATH:/libs"
//MinGW
PATH="%PATH%:C:\libs"
```

在Linux和Mac中，动态库通常保存在/usr/lib或/usr/local/lib中；而在Windows中，程序员通常把.DLL和可执行文件保存在同一个目录下

- 动态库在修改库文件后不用再重新编译项目文件

有了动态库，就能在运行时替换代码。不用重新编译程序，你就能修改它。如果你有很多程序，它们共享一段相同的代码，通过建立动态库，就可以同时更新所有程序

## 9 进程与系统调用

C程序无论做什么事都要靠操作系统。如果它想与硬件打交道，就要进行系统调用。系统调用是操作系统内核中的函数，C标准库中大部分代码都依赖于它们

### system()

接收一个字符串参数，并把它当作命令执行

```c
system("dir D:");	//打印D盘内容
system("gedit");       //在Linux中启动编辑器
system("say 'End of Line' ");	//在Mac上朗读文本
```

在一些操作系统中，系统调用的代码位于操作系统内核。而对其他操作系统而言，系统调用可能保存在动态库中

> **内核**？
>
> 在大部分计算机上，系统调用就是操作系统内核中的函数。什么是内核？虽然你从来没在屏幕上看到过它，但内核其实一直都在那里控制计算机。内核是计算机中最重要的程序，它主管三样东西：
> 	进程
> 只有当内核把程序加载到存储器时程序才能运行。内核创建进程，并确保它们得到了所需资源。内核同时也会留意那些变得贪得无厌或者已经崩溃的进程。
> 	存储器
> 计算机所能提供的存储器资源是有限的，因此内核必须小心翼翼地分配每个进程所能使用的存储器大小。内核还能把部分存储器交换到磁盘从而增加虚拟存储器空间。
> 	硬件
> 内核利用设备驱动与连接到计算机上的设备交互。你的程序在不了解键盘、屏幕和图形处理器的情况下就能使用它们，因为内核会代表你与它们交涉。系统调用是程序用来与内核对话的函数。

### exec()

进程是存储器中运行的程序。操作系统用一个数字来标识进程，它叫进程标识符（process identifier，简称PID）

exec()函数通过运行其他程序来替换当前进程。你可以告诉exec()函数要使用哪些命令行参数和环境变量。新程序启动后PID和老程序一样。

exec()函数在unistd.h中，它版本众多，但可以分为列表函数和数组函数。

#### 列表函数

execl()、execlp()、execle()

列表函数以参数列表的形式接收命令行参数：

1. 程序

   告诉exec()函数将运行什么程序。对execl()或execle()来说，它是程序的完整路径名；对execlp()来讲就是命令的名字，execlp()会根据它去查找程序

2. 命令行参数

   需要依次列出想使用的命令行参数。别忘了，第一个命令行参数必须是程序名，也就是说列表版exec()的前两个参数是相同字符串

   > **命令行参数之间的空格会把MinGW弄糊涂**
   >
   > 如果把“I like”和“turles”这两个参数传给exec()，MinGW程序可能会发送三个参数：“I”、“like”和“turtle”。

3. NULL

   需要在最后一个命令行参数后加上NULL，告诉函数没有其他参数了

4. 环境变量（如果有的话）

   如果调用了以 ...e() 结尾的 exec() 函数，还可以传递环境变量数组，像"POWER=4","PORT=OPEN"……那样的字符数组

```c
execl("/home/flynn/clu", "/home/flynn/clu", "paranoids", "contract", NULL)	//execL = 参数列表(List)
execlp("clu", "clu", "paranoids", "contract", NULL)		//execLP = 参数列表（List） + 在PATH中查找程序
execle("/home/flynn/clu", "/home/flynn/clu", "paranoids", "contract", NULL, env_vars)
//execLE = 参数列表（List） + 环境变量（Environment）
```

#### 数组函数

execv()、execvp()、execve()

如果已经把命令行参数保存在了数组中，就会发现这两个版本用起来更容易

```c
execv("/home/flynn/clu", my_args);	//execV=参数数组或参数向量（Vector）
execvp("clu", my_args);		//execVP = 参数数组/向量（Vector） + 在PATH中查找
//参数需要保存在字符串数组my_args中
//以上两函数唯一区别是execvp会用PATH变量查找程序
```

| 使用          | 字符 |
| ------------- | ---- |
| 参数列表      | l    |
| 参数数组/向量 | v    |
| 根据PATH查找  | p    |
| 环境变量      | e    |

l、v总是在p、e之前出现；p、e是可选的

### 环境变量（具体做法见练习文件）

每个进程都有一组环境变量。你可以在命令行中输入set或env查看它们的值，它们一般会告诉进程一些有用的信息，比如用户主目录的位置，或去哪里找命令。C程序可以用getenv()系统调用读取环境变量

可以用字符串指针数组的形式创建一组环境变量。环境变量的格式是“变量名=值”。数组最后一项必须是NULL

```c
char *my_env[] = {"JUICE=peach and apple", NULL};
execle("diner_info", "diner_info", "4", NULL, my_env);
```

> **在Cygwin中传递环境变量时一定要包含PATH变量**
>
> 在Cygwin中，加载程序时需要用PATH变量，因此在Cygwin上传递环境变量时一定要包含PATH=/usr/bin

### 系统调用错误

如果在调用程序时发生错误，当前进程会继续运行，于是可以向用户报告错误信息

```c
execle("diner_info", "diner_info", "4", NULL, my_env);
puts("哥们，diner_info程序肯定发生了什么问题");
```

所有的系统调用都遵循“失败黄金法则”

1. 尽可能收拾残局
2. 把errno变量设为错误码
3. 返回-1

errno变量是定义在errno.h中的全局变量，还一并定义了许多标准错误码

| 标准错误码 | 错误消息         |
| ---------- | ---------------- |
| EPERM=1    | 不允许操作       |
| ENOENT=2   | 没有该文件或目录 |
| ESRCH=3    | 没有该进程       |

可以拿errno和这些值比较，也可以用string.h中的`strerror()`的函数查询标准错误消息

```c
puts(strerror(errno));	//strerror()将错误码转换为一条消息
```

### 用fork()克隆进程

exec() 是程序中最后一行代码

fork()会克隆当前进程。新建副本将从同一行开始运行相同程序，变量和变量中的值完全一样，只有进程标识符（PID）和原进程不同。原进程叫父进程，而新建副本叫子进程

> 注意！与Unix和Mac不同，**Windows天生不支持fork()**，如果想在Windows中使用fork()，必须先要安装Cygwin
>
> Windows有一个`Create-Process()`函数。它是一个加强版的system()
>
> 此处[官方文档](https://learn.microsoft.com/en-us/windows/win32/procthread/creating-processes)

用fork()+exec()运行子进程，在子进程中调用exec()函数，这样原来的父进程就能继续运行了

复制进程后，为区分父子进程，fork()函数向子进程返回0，向父进程返回非零值

> **fork()**
>
> 你可以像这样调用fork()：`pid_t pid = fork();`
> fork()会返回一个整型值：为子进程返回0，为父进程返回一个正数。父进程将接收到子进程的进程标识符
> 什么是pid_t？不同操作系统用不同的整数类型保存进程ID，有的用short，有的用int，操作系统使用哪种类型，pid_t就设为哪个

- 这样的设计中一般现在要考虑可能出现错误了，可以这样写

- ```c
  pid_t pid = fork();
  if (pid == -1) {
      fprintf(stderr, "Can't fork process: %s\n", strerror(errno));
      return 1;
  }
  if (!pid) {
        ...   
  } 

> 在用exec()替换子进程前可以不用等fork()复制完整个进程，操作系统会采用写时复制（copy-on-write）技术等技巧。比如操作系统不会真的复制父进程的数据，而是让父子进程共享数据。如果操作系统发现子进程要修改存储器，就会为它复制一份

## 10 进程间通信

在命令行中，重定向是非常有用的命令，也可以让进程重定向自己

### 进程内部

进程含有它正在运行的程序，还有栈和堆数据空间。进程还需要记录数据流的连向，它用文件描述符表示数据流，所谓的描述符其实就是一个数字。进程会把文件描述符和对应的数据流保存在描述符表中（下表仅为示意）描述符表的一列是文件描述符号，另一列是它们对应的数据流

| #    | 数据流     |
| ---- | ---------- |
| 0    | 键盘       |
| 1    | 屏幕       |
| 2    | 屏幕       |
| 3    | 数据库连接 |

描述符表的前三项万年不变：0号标准输入，1号标准输出，2号标准错误。其他项要么为空，要么连接进程打开的数据流。比如程序在打开文件进行读写时，就会占用其中一项

创建进程以后，标准输入连到键盘，标准输出和标准错误连到屏幕。它们会保持这样的连接，直到有人把它们重定向到了其他地方

- 文件描述符描述的不一定是文件

在 类Unix操作系统中，可以用以下命令把标准错误和标准输出重定向到一个地方：

`./myprog 2>&1`

“2>”表示“重定向标准错误”;“&1”表示“到标准输出”

### fileno()返回描述符号

每打开一个文件，操作系统都会在描述符表中新注册一项

```c
FILE *my_file = fopen("guitar.mp3", "r");
```

操作系统会打开*guitar.mp3*文件，然后返回一个指向它的指针，操作系统还会遍历描述符表寻找空项，把新文件注册在其中

调用fileno()函数可以根据文件指针知道它是几号描述符

```c
int descriptor = fileno(my_file);
```

> 在失败时不返回-1的函数很少，fileno()就是其中之一。只要你把打开文件的指针传给fileno()，它就一定会返回描述符编号			😴？？

### dup2()复制数据流

可以用dup2()函数修改某个已经注册过的数据流，dup2()可以复制数据流

```c
dup2(4, 3);		//同时将4号数据流连接到3号描述符
```

### exit()

exit()系统调用是结束程序的最快方式，为了使用exit系统调用，必须包含stdlib.h头文件

```c
#include <stdlib.h>
void error(char *msg){
	fprintf(stderr, "%s: %s\n", msg, strerror(errno));
	exit(1);		//exit(1)会立刻终止程序，并把退出状态置1
}
```

每次程序执行只有一次调用exit()的机会

### 等待子进程

子进程一创建就和父进程没关系了，有时候子进程还未完成任务，父进程就已经结束了，操作系统必须提供一种方式，让你等待子进程完成任务

#### waitpid()函数

waitpid()函数会等子进程结束以后才返回，也就是说可以在父进程中加几行代码，让它等到子进程运行结束以后才退出

需要包含sys/wait.h的头文件(Windows平台似乎并不支持)

```c
#include <sys/wait.h>
```

接收三个参数

``` c
waitpid(pid,pid_status, options )
```

1. pid

   父进程在克隆子进程时会得到子进程的ID

2. pid_status

   pid_status用来保存进程的退出信息。因为waitpid()需要修改pid_status，因此它必须是个指针

3. options

   waitpid()有一些选项，详情可以输入man waitpid查看。如果把选项设为0，函数将等待进程结束

**pid_status**?

waitpid()函数结束等待时会在pid_status中保存一个值，它告诉你进程的完成情况

为了得到子进程的退出状态，可以把pid_status的值传给WEXITSTATUS()宏

```c
if (WEXITSTATUS(pid_status))
    puts("Error status non-zero");
```

pid_status中保存了好几条信息，只有前8位表示进程的退出状态，所以需要用宏来查看

重定向输入、输出，然后让进程相互等待，这就是进程间通信

进程之间通过共享数据和互相可以等待实现更多功能

### 管道连接进程

可以在命令行用管道把一个进程的输出连接到另一个进程的输入

```bash
python rssgossip.py -u 'pajama death' | grep 'http'
	http://www.rock-news.com/exclusive/24.html
	http://www.rolling-stone.com/pdalbum.html
```

grep找出了包含http的那些行

管道两侧的命令是父子关系，如上例中grep命令是rssgossip.py脚本的父进程

1. 命令行创建了父进程
2. 父进程在子进程中克隆出了rssgossip.py脚本
3. 父进程用管道把子进程的输出连接到自己的输入
4. 父进程运行了grep命令

### pipe()打开两条数据流

用pipe()函数建立管道

每当打开数据流时，它都会加入描述符表。pipe()函数也是如此，它创建两条相连的数据流，并把它们加到表中，然后只要往其中一条数据流中写数据，就可以从另一条数据流中读取

pipe()在描述符中创建这两项时，会把它们的文件描述符保存在一个包含两个元素的数组中

```c
int fd[2];
if (pipe(fd) == -1) {
	error("Can't create the pipe");
}
```

pipe()函数创建了管道，并返回了两个描述符： fd[1]用来向管道写数据，fd[0]用来从管道读数据

在子进程中，关闭管道的fd[0]端，修改子进程的标准输出，让它指向描述符fd[1]对应的数据流；在父进程中需要关闭管道的fd[1]端，重定向父进程的标准输入，让它从描述符fd[0]对应的数据流中读取数据

```c
// 子进程
close(fd[0]);
dup2(fd[1], 1);

//父进程
dup2(fd[0], 0);
close(fd[1]);
```

不同操作系统打开网页？较为简单粗暴的system()写法

```c
void open_url(char *url){
	char launch[255];
	sprintf(launch, "cmd /c start %s", url);		//Windows
	system(launch);
	sprintf(launch, "x-www-browser '%s' &", url);	//Linux
	system(launch);
	sprintf(launch, "open '%s'", url);			     //Mac
	system(launch);
}
```

>在类Unix机器或任何使用Cygwin的Windows机器中可以获取以下程序
>
>**curl/wget**
>可以用这两个程序与网络服务器通信，也可以在C代码中使用它们与网络通信
>**mail/mutt**
>可以在命令行用这两个程序发送邮件。如果在机器上装了它们，C程序就能发送邮件
>**convert**
>convert命令可以转换图片格式。你可以写一个C程序，它输出文本格式的SVG图表，然后用convert命令把SVG转化成PNG图片

**关于管道的Q&A**

管道可能是文件。可以创建基于文件的管道，它们通常叫有名管道或FIFO(First In First Out，先进先出)文件。因为基于文件的管道有名字，所以两个进程只要知道管道的名字也能用它来通信，即使它们非父子进程。使用[mkfifo()系统调用](http://tinyurl.com/cdf6ve5)有名管道

如果不用文件来实现管道, 那么通常用存储器。数据写到存储器中的某个位置，然后再从另一
个位置读取

如果试图读取一个空的管道，程序会等管道中出现东西

子进程结束时，管道会关闭。fgets()将收到EOF（End Of File，文件结束符），于是fgets()函数返回0，循环就结束了

管道只能单向通信。不过可以创建两个管道，一个从父进程连到子进程，另一个从子进程连到父进程

### 杀死进程😎Ctrl-C

操作系统会从键盘读取数据，但当它看到用户按了Ctrl-C，就会向程序发送中断信号，进程运行默认中断处理器，调用了exit()

信号是一条短消息，即一个整型值。当信号到来时，进程必须停止手中一切工作去处理信号。进程会查看信号映射表，表中每个信号都对应一个信号处理器函数。中断信号的默认信号处理器会调用exit()函数

既然有这么一张信号映射表，那么就可以修改进程收到信号时运行的函数

### sigaction是一个函数包装器

**以下均需要包含signal.h文件**

sigaction是一个结构体，它有一个函数指针。sigaction告诉操作系统进程收到某个信号时应该调用哪个函数。

创建方法：

```c
struct sigaction action;
action.sa_handler = diediedie;	//指定计算机调用哪个函数，这个被sigaction包装起来的函数就叫处理器
sigemptyset(&action.sa_mask);  //用掩码来过滤sigaction要处理的信号，通常会用一个空的掩码
action.sa_flags = 0;		//一些附加标志位，将它们置0就行了
```

处理器必须接收信号参数，为整型

```c
void diediedie(int sig){
	puts ("Goodbye cruel world....\n");
	exit(1);
}
```

处理器的代码应该短而快，刚好能处理接收到的信号就好

> 在处理器函数中使用标准输出和标准错误时要小心
>
> 之所以会有信号就是因为程序中发生了故障，而故障可能就是这些无法使用

### 用sigaction()来注册sigaction

创建sigaction以后，需要用sigaction()函数来让操作系统知道它的存在

```c
sigaction(signal_no, &new_action, &old_action);
```

接收三个参数：

1. 信号编号

   这个整型值代表了你希望处理的信号。通常会传递SIGINT或SIGQUIT这样的标准信号

2. 新动作

   你想注册的新sigaction的地址

3. 旧动作

   如果你想保存被替换的信号处理器，可以再传一个sigaction指针；如果不想保存，可以设置为NULL

如果sigaction()函数失败，会返回－1，并设置errno变量

以下函数简化了注册过程

```c
int catch_signal(int sig, void (*handler)(int)){
	struct sigaction action;
	action.sa_handler = handler;
	sigemptyset(&action.sa_mask);
	action.sa_flags = 0;
	return sigaction (sig, &action, NULL);
}
catch_signal(SIGINT, diediedie);
```

### 操作系统的信号（部分）

| 信号     | 引起原因                                               |
| -------- | :----------------------------------------------------- |
| SIGINT   | 进程被中断                                             |
| SIGQUIT  | 有人要求停止进程，并把存储器中的内容保存到核心转储文件 |
| SIGFPE   | 浮点错误                                               |
| SIGTRAP  | 调试人员询问进程执行到了哪里                           |
| SIGSEGV  | 进程企图访问非法存储器地址                             |
| SIGWINCH | 终端窗口的大小发生改变                                 |
| SIGTERM  | 有人要求内核终止进程                                   |
| SIGPIPE  | 进程在向一个没有人读的管道写数据                       |

### 使用kill发送信号

在类Unix操作系统中有一个叫kill的命令（在Windows上用Cygwin）

叫kill是因为这个命令通常用来“杀死”进程。事实上，kill只是向进程发送了一个信号，kill默认会向进程发送SIGTERM信号

kill －KILL一定能杀死进程

### raise()给自己发送信号

```c
raise(SIGTERM);
```

通常会在自定义的信号处理函数中使用raise()，这样程序就能在接收到低级别的信号时引发更高级别的信号。这就是信号升级

### SIGALRM

当计算机中发生了进程需要知道的事情时，操作系统就会向进程发送信号，除了在发生错误时使用，有时进程也需要产生自己的信号

```c
alarm(120);			//把闹钟调到120秒以后闹铃
do_important_busy_work();
do_more_busy_work();			//其间代码就会做其他事
```

进程在收到闹钟信号以后默认会结束进程，但通常情况下使用定时器不是为了让它帮你“杀死”程序，而是为了利用闹钟信号的处理器去做另一件事

```c
catch_signal(SIGALRM, pour_coffee);
alarm(120);
```

闹钟信号可以实现多任务。如果需要每隔几秒运行一个任务，或者想限制花费在某个任务上的时间，就可以用闹钟信号让程序打断自己

一个进程只有一个定时器。定时器由操作系统的内核管理，如果一个进程有很多定时器，内核就会变得很慢，因此操作系统需要限制进程能使用的定时器个数

每次调用alarm()函数都会重置定时器

> **不要同时使用alarm()和sleep()**
>
> sleep()函数会让程序沉睡一段时间。和alarm()函数一样，它也使用了间隔计时器，因此同时使用这两个函数会发生冲突

> setitimer()函数可以把进程间隔计时器的单位设为几分之一秒

### 重置信号和忽略信号

如果想还原默认的信号处理器，signal.h头文件中有一个特殊的符号SIG_DFL，它代表以默认方式处理信号

```c
catch_signal(SIGTERM, SIG_DFL);
```

同时，还可以用SIG_IGN符号让进程忽略某个信号

```c
catch_signal(SIGINT, SIG_IGN);
```

在决定忽略某个信号前一定要慎重考虑，信号是控制进程和终止进程的重要方式，如果忽略了它们，程序就很难停下来

## 11 网络与套接字



## 12 线程








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
