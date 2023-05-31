---
title: 程设课笔记
date: 2023-05-31 18:27:00
tags: C , C++
---

## C++对C的扩充

在C++中引入术语stream(流)，指的是来自设备的一个数据流。

在输入操作中，字节从输入设备流向内存

在输出操作中，字节从内存流向输出设备

头文件iostream中定义了输入流cin和输出流cout对象

### 用`cout`进行输出

cout表示输出流对象，与它相关联的设备是显示器。cout必须和输出运算符<<一起使用，表示将其后面的数据插入到输出流中去。

```c++
cout = c + out
//c -> char
```

举例:

```c++
#include <iostream>
using namespace std;
int main(){
    int i =1;
    float f = 3.14;
    char c = 'A', str[]="hello,world\n";
    cout << i;
    cout << f;
    cout << c;
    cout << str;
    cout << i << f << c << str << endl;
}
```

### 用`cin`进行输出

cin表示输入流对象，与它相关联的输入输出设备是键盘。当我们从键盘输入字符串时，形成了输入流（数据流），用提取操作符>>将数据流储存到一个事先定义好的变量中。

```c++
cin = c + in;
```

举例：

```c++
#include <iostream>
using namespace std;
int main(){
    int i;
    float f;
    char c, str[20];
    cin >> i >> f >> c >> str;
}
// 以空白字符（空格、制表符、换行、回车）作为输入数据之间的间隔
//输入一个字符可以使用：
cin.get(c);
//输入一个字符串可用：
cin.getline(str,20);
```

### C++对数据类型的扩充

#### 布尔类型

取值为：true或false

```c++
bool RealMonkey = true;
bool RealMonkey, PrettyWoman;
RealMoney = false;
PrettyWoman =false;
```

#### 引用类型

引用（reference）的作用是为一个变量起一个别名

```c++
int LiBai;
int &LiTaiBai = LiBai; // 必须初始化
int &QingLianJuShi =LiBai;
int &ShiXian = LiBai;
int &JiuXian = LiBai;
LiBai = 49;
LiTaiBai ++;
```

#### 引用作为函数参数

```c++
// 使得在被调用的函数中可以修改作为实参的变量的值
void main(){
    int salary, nCars, nHouses;
    salary = 6000;
    nCars = 0;
    nHouses = 0;
    DayDreaming(salary, nCars, nHouses);
    cout << salary << " " << nCars << " " << nHouses;
}

void DayDreaming(int &salary, int &cars, int &houses){
    salary = salary * 3;
    cars += 2;
    houses ++;
}
```

## 类和对象

### 对象（Object）

什么是对象

- 在认知心理学中，对象是指可触摸、可见或思维可理解的东西
- 软件工程中的定义：对象是一个具有状态、行为和标识的实体。相似对象的结构和行为在它们的共有的类中定义

对象的属性：对象所具有的一些特征成为属性。这些属性会有其对应的值

对象的状态：一个对象的状态包括该对象的所有属性及每个属性的值

对象的行为：对象不是孤立存在的，一个对象可以作用于其他对象，也可被其他对象所作用，从而导致状态的变化

对象的操作：一个对象（类）对外提供的服务

### 类（class）

一组具有类似属性和行为的对象

#### 类与对象

类：抽象地定义了该类对象地本质特征（属性和操作），类型定义、模板

对象：类的实例，具有各自的属性值，占用存储空间

## 类的定义与使用

```c++
//类定义
class Baby{
    //成员变量
    
    //成员函数  
    
};

//类实例
Baby dawa, erwa;
```

### 成员变量的初始化

- 在**定义类时**即可确定，用构造函数，且无需参数
- 在**创建对象时**确定，用带参数的构造函数
- 在**创建对象后**才能确定，用成员函数

### 构造函数

```c++
class CLASSNAME{
    CLASSNAME(){
        
    }
    CLASSNAME([ARGUMENTS]){		//带参数的构造函数
        
    }
};
```

- 构造函数的名字 = 类的名字

- 没有返回值类型：不返回任何数据

- 创建对象时自动调用，初始化成员变量

- 每个类需要至少一个构造函数，若不写，则默认为：

  ```c++
  CLASSNAME(){
      
  }
  ```

类初始化时构造函数的调用顺序：

- 初始化对象的储存空间或为零或为null
- 按顺序分别调用类成员变量和对象成员的初始化
- 调用构造函数

```c++
#include <iostream>
using namespace std;
class Dollar{
    public:
    	int n;
    	Dollar(){
            	n = 100;
            	cout << n << " dollars come to my home" <<endl;
            }
};

class Money{
    public:
    	Dollar d;
    	Money(){
            	cout << "All money come to my home" << endl;
            }
};

int main(){
    Money m;
    
    return 0;
}
```

运行结果为：

```bash
$ .\test.exe
100 dollars come to my home
All money come to my home
```

#### 默认构造函数

调用时可以不需要实参的构造函数

- 参数列表为空
- 全部参数都有默认值（略）

每个类只能有一个默认的构造函数，否则将编译错误

一个构造函数的例子：

```c++
class Baby {
public:
    char name[20];
    char gender;
    int age;
    double weight;
    int numPoops;
    Baby(char myname[], char g, double w) 	//创建对象时确定
                                       : age(0), numPoops(0) {	//初始化成员列表，在定义类的时候确定
        strcpy(name, myname);
        gender = g; 
        weight = w;
    }   
};
```

#### 调用带参数的构造函数

```c++
//创建对象
Baby dawa = Baby("大力士", 'M', 20);

//或者
Baby dawa("大力士", 'M', 20);
```

### 成员函数

形如

```c++
class Baby{
    public:
    	...
            void poop(){
            	numPoops ++;
            	cout << "Dear mother," << "I have pooped." << endl;
            }
            void sayHi(){
    		cout << "Hi, my name is " << name << endl;
	    }

	    void eat(double foodWeight){
    		weight +=foodWeight;
	    } 
}
```

综上，完整的Baby类为

```c++
class Baby{
    public:
    	char name[20];
    	char gender;
    	int age;
    	double weight;
    	int numPoops;

    	Baby(char myname[], char g, double w) 	//创建对象时确定
                                       : age(0), numPoops(0) {	//初始化成员列表，在定义类的时候确定
        	strcpy(name, myname);
        	gender = g; 
        	weight = w;
    	}

    	void poop(){
            	numPoops ++;
            	cout << "Dear mother," << "I have pooped." << endl;
            }
            void sayHi(){
    		cout << "Hi, my name is " << name << endl;
	    }

	    void eat(double foodWeight){
    		weight +=foodWeight;
	    } 
}
```

### 函数声明与实现分离

对于普通函数，函数原型与函数的实现可以分离，即把函数原型放在文件开头，而把函数的实现放在后面

类似的，类的成员函数也可以这样做，函数的实现需要用`::`来表示它是哪一个类的成员函数

```c++
//baby.h 头文件
class Baby{
    public:
    	char name[20];
    	char gender;
    	int age;
    	double weight;
    	int numPoops;
    	Baby(char*, char, double);
    	void poop();
    	void sayHi();
    	void eat(double foodweight);
}

//baby.cpp 源文件
#include "baby.h"
Baby::Baby(char myname[], char g, double w){
	...    
}
void Baby::poop(){
    ...
}
void Baby::eat(double foodweight){
    weight +=foodweight;
}
```

### 类的使用

```c++
//类定义
class Baby{...}

//类实例
Baby dawa = Baby("大力士", 'M', 20);
Baby erwa = Baby("千里眼", 'M', 16);
Baby sanwa = Baby("钢筋铁骨", 'M', 18);
Baby siwa = Baby("火神", 'M', 16);
```

#### 访问成员变量

`Object.FIELD_NAME`

```c++
cout << dawa.name << endl;
cout << erwa.weight << endl;
cout << sanwa.numPoops << endl;
```

#### 调用成员参数

`Object.METHOD_NAME([参数])`

```c++
dawa.sayHi();	//把sayHi作用于dawa
erwa.eat(1);	//把eat(1)作用于erwa
sanwa.poop();	//把poop作用于sanwa
```

## 进一步内容

### 对象的赋值

```c++
class Point {
	public:
    	int x, y;
    	Point(){ x = 0; y = 0;}
};

int main() 
{
    Point start, end;
    start.x = 3;
    start.y = 4;
    end = start;  //对象的赋值
    cout << end.x << ' ' << end.y << endl;
    return 0;
}
```

### 对象作为函数参数

```c++
#include <iostream>
using namespace std;

class DaGongRen{
    public:
    	int salary, nCars, nHouses;
};

void GetRich(DaGongRen p, int s, int c, int h){
    p.salary += s;
    p.nCars += c;
    p.nHouses += h;
}

int main() 
{
    DaGongRen poor;
    poor.salary = 6000;
    poor.nCars = 0;
    poor.nHouses = 0;
    GetRich(poor, 12000, 2, 1);
    cout << poor.salary << ' ' << poor.nCars << ' ' << poor.nHouses << endl;
    return 0;
}

然而GetRich并没有改变初始化的DaGongRen p对象里的值，还是需要引用或指针
```

使用另一对象来初始化

```c++
class Singer { 
	public:
    	string name;
    	int birth;
    	Singer() {}  //默认构造函数
    	Singer(Singer &o){ //复制构造函数，形参为引用
        	name = o.name; birth = o.birth;  
    	}
};
int main() {
{
    Singer MJ; //使用默认构造函数
    MJ.name = "Michael Jackson"; 
    MJ.birth = 1958;
    Singer wang(MJ); //使用复制构造函数
}
```

### 指向对象的指针

```c++
Baby dawa("大力士", 'M', 20);
Baby *p = new Baby("大力士", 'M', 20);		//new 相当于 malloc
cout << p->name << ' ' << p->gender		//->用法类似于结构体
```

![c&c++_3](..\img\c&c++_3.png)

对象dawa和指针p都是在栈空间中，new出来的新对象在堆空间中

### 静态类型(不作要求)

static类型表示“静态”或“全局”的意思，适用于成员变量和方法

#### 静态成员变量

- 是类一级的定义，独立于该类的任何对象（实例），为所有对象所共享
- 必须在类外初始化，用`::`指明类
- 可以在任何类对象创建之前访问（静态成员函数也是如此）

```c++
class Baby {
    public:
        static int numBabiesMade; 
    };
    int Baby::numBabiesMade = 0;
    void main() {
        Baby::numBabiesMade = 100; 
        Baby b1, b2;
        b1.numBabiesMade = 1;
        b2.numBabiesMade = 2;
        cout << Baby::numBabiesMade<<' '
           << b1.numBabiesMade << ' '
           << b2.numBabiesMade << endl;
}
```

### 常类型（不作要求）



## 常用的C++类

### String类

字符串：

- 用双引号括起来的若干个字符
- 如"你好"、"no zuo no dai"
- C语言：存储在字符数组，以0结尾
- C++：string类
  - 表示字符串
  - 实际上是对字符数组操作的封装

### string类的构造函数

```c++
string();	//默认构造函数，建立长度为0的串
string s1;

string(const char *s);	//用s字符串来初始化
string s2 = "abc";	//或
string s2("abc");

string(const string& s) 	//复制构造函数
string s3 = s2;
```

### 字符串存储

字符串中的字符下标从0开始：

`String name = 'Ultimate';`

![c&c++_2](..\img\c&c++_1.png)

- 首字符下标为0，尾字符下标为N-1
- 每个元素的类型为char

### string类的一些成员函数

| 函数名称                                                     | 功能描述                                        |
| ------------------------------------------------------------ | ----------------------------------------------- |
| +、=                                                         | 字符串的拼接和赋值                              |
| ==、!=、<、<=、>、>=                                         | 各种关系运算                                    |
| int length()                                                 | 返回字符串的长度                                |
| s[i]                                                         | 访问字符串s中下标为i的字符                      |
| string substr(int pos, int n)                                | 返回pos开始的n个字符组成的子串                  |
| int find(const char *s, int pos)<br />int find(char c, int pos) | 从pos开始查找字符串s或字符c在当前字符串中的位置 |
| string&insert(int p0,char *s)                                | 在当前字符串的p0位置插入字符串s                 |
| string&erase(int pos, int n)                                 | 删除pos开始的n个字符，返回新串                  |

### string类举例

```c++
string s1 = "too ";
string s2 = s1 + "how";
cout << s2 << endl;		//“too how”

//index		012345678901
string s3 = "Stuart Reges";

cout << s3.length() << endl;	//12
cout << s3.find("e", 0) << endl;	//8
cout << s3.substr(7, 3) << endl;	//"Reg"
```

## 访问控制

### 抽象与封装

- 将类的本质行为和它的具体实现分开
- 外部观点：类的对外接口
- 内部观点：类的具体实现

| 关键字         | 含义                                    |
| -------------- | --------------------------------------- |
| public         | 任何地方都可以访问（的成员变量和方法）  |
| protected      | 类及其子类的函数可访问，但对象.成员不行 |
| private        | 只能在类内部访问                        |
| 未指定（默认） | 等价于private                           |

### 直接访问成员变量

```c++
#include <iostream>
#include <string>
using namespace std;

class BankAccount{
    public:
    	string number;
    	double balance;
    	string password;  
};

int main(){
    BankAccount account;
    account.balance = 1000000;
    cout << "password is:" << account.password << endl;
    return 0;
}
```

### 不能直接访问成员变量

```c++
#include <iostream>
#include <string>
using namespace std;

class BankAccount{
    private:
    	string number;
    	double balance;
    	string password;  
    public:
    	void deposit(double money){
            balance += money;
        }
    	void withdraw(double money){
            balance -= money;
        }
    	void resetPassword(string pwd){
            password = pwd;
        }
};

int main(){
    BankAccount account;
	account.deposit(1000000);
    string pwd ="abc123";
    account.resetPassword(pwd);
    return 0;
}
```

接口函数命名：`getXXX()` 查询类；`setXXX()` 修改类

## 函数重载

其他类型转字符串

C语言的实现办法

```c
char *itoa(int value,char *string,int radix);

char *fcvt(double value, int ndigit,int *decpt, int *sign);

char *ultoa(unsigned long value, char *string, int radix);
```

C++11的实现方法

```c++
std::to_string(int i)
std::to_string(unsigned u)
std::to_string(long l)
std::to_string(unsigned long l)
std::to_string(float f)
std::to_string(double d)
```

例子：小狗阿黄

```c++
#include <iostream>
using namespace std;

class Dog{
    public:
    	void bark(){
            cout << "汪汪汪！" << endl;
        }
    	void bark(bool injured){
            if(injured){
                cout << "呜咽..." <<endl;
            }
        }
    	void bark(int mood){
            if(mood == 0){
                cout << "汪汪汪！" << endl;
            }else if(mood == 1){
                cout << "汪！汪汪！" << endl;
            }else if(mood == 2){
                cout << "呜-呜" << endl;
            }
        }
};

void main(){
    Dog ahuang;
    ahuang.bark();
    ahuang.bark(true);
    ahuang.bark(1);
}
```

重载的条件

成员方法重名，但是

- 参数个数不同
- 参数的类型不同
- 顺序不同

```c++
class Calculation{
    public:
    	void sum(int a,int b){
            cout << (a+b);
        }
    	void sum(int a,int b,int c){
            cout << (a+b+c);
        }
}

class Calculation2{
    public:
    	void sum(int a,int b){
            cout << (a+b);
        }
    	void sum(double a,double b){
			cout << (a+b);           
        }
}

class Calculation3{
    public:
    	void sum(int a,double b){
            cout << (a+b);
        }
    	void sum(double b,int a){
            cout << (a+b);
        }
}

void main(String args[]){
    Calculation obj;
    obj.sum(10,10,10);
    obj.sum(20,20);
    
    Calculation2 obj2;
    obj2.sum(10.5,10.5);
    obj2.sum(20,20);
    
    Calculation3 obj3;
    obj3.sum(10,10.5);
    obj3.sum(20.5,20);
}
```

参数个数相同，各个参数的数据类型和顺序也相同，但变量名不同。不能够构成函数重载。

```c++
int move(string snake);
int move(string turtle);
```

## 存储管理（不作要求）

寄存器、栈（stack）、堆（heap）、静态数据区

### 内存分布

![c&c++_2](..\img\c&c++_2.png)

## 类的继承

类与类之间的关系

- 泛化（Generalization）：“is a”关系，继承关系，一般类/特殊类，父类/子类，基类/派生类
- 聚合（Aggregation）：“part of”关系，整体/部分
- 关联（Association）：类与类之间存在某种语义关联

继承是类与类之间的关系，而非对象之间的关系

- 便于软件重用
- 不同子类之间有许多共同的属性
- 但是子类之间又有区别

![c&c++_4](..\img\c&c++_4.png)

- Y继承了X的属性和操作
- Y只需要定义新增的属性和操作
- Y的对象同时也是X的对象

类的层次结构

- 不同程度的抽象可得到不同层次的类

![c&c++_5](..\img\c&c++_5.png)

```c++
class Dude {
    public:
        string name;
        int hp;	// 血量
        int mp;	// 魔力值
        Dude() { hp = 100; mp = 0; }
        void sayName() {
            cout << name << endl;
        }
        void punchFace(Dude &target) {
            target.hp -= 10;
        }    
};

class Wizard  :  public Dude{ 		// Wizard 是 Dude 的一个子类
    string spells[20];
    public:
        void cast(string spell){
            // cool stuff here
            ...
            mp -= 10;
        }  
};

class GrandWizard : public Wizard {
    public:
        void sayName(){
            cout << "Grand wizard " << name;
        } 
};

grandWizard1.name = "Flash";
grandWizard1.sayName();
```

C++看到如下语句时会如何做？`grandWizard1.punchFace(dude1);`

- 在GrandWizard类中寻找punchFace()；
- 没有找到！grandWizard有父类吗？
- 在Wizard类中查找punchFace()；
- 没有找到！Wizard有父类吗？
- 在Dude类中查找punchFace()；
- 找到了！调用punchFace()；
- 减少dude1的hp值

### 不同的继承方式

 三类继承方式：公有继承（public）、私有继承（private）、保护继承（protected）

|          | 访问属性的继承                                               | 子类成员函数                                               | 子类对象                       |
| -------- | ------------------------------------------------------------ | ---------------------------------------------------------- | ------------------------------ |
| 公有继承 | 父类的public和protected成员的访问属性在子类中不变，private不能访问 | 可以访问父类中的public和protected成员，不能访问private成员 | 只能访问从父类继承的public成员 |
| 私有继承 | 父类的public和protected成员以private出现在子类，private不能访问 | 同上                                                       | 不能访问从父类继承的任何成员   |
| 保护继承 | 父类的public和protected成员以protected出现在子类，private不能访问 | 同上                                                       | 不能访问从父类继承的任何成员   |

### 子类对象的存储

在创建一个子类对象后

- 一方面，该子类对象本身是一个独立、完整的对象
- 另一方面，在该对象内部，又包含了一个父类子对象(subobject)
- 该子对象与正常创建的父类对象相同

具体实现

![c&c++_6](..\img\c&c++_6.png)



## 多态(不作要求)



