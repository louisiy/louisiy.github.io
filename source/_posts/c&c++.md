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

## 存储管理

寄存器、栈（stack）、堆（heap）、静态数据区

### 内存分布

![image-20230531174310352](C:\Users\Louisiy\AppData\Roaming\Typora\typora-user-images\image-20230531174310352.png)
