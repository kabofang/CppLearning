## C++程序设计语言读书笔记

[TOC]

### 第一章	致读者

~~~ markdown
欲速则不达。
							————屋大维，恺撒 ● 奥古斯都
~~~

#### 1.1 本书结构

##### 1.1.1 本书包含四个部分

- 第一部分：第 1 章，介绍C++的背景知识；第 2~5 章，对C++语言及其标准库进行简要介绍。
- 第二部分：第 6~15 章， 介绍C++的内置类型和基本特性以及如何用它们构造程序。
- 第三部分：第 16~29 章， 介绍C++的抽象机制及如何用这些机制编写面向对象和泛型程序。
- 第四部分：第 30~44 章， 概述标准库并讨论一些兼容问题。

##### 1.1.2 C++程序设计语言及其标准库的主要概念和特性

- 第2章：基础知识，介绍C++的内存模型、计算模型和错误处理模型；
- 第3章：抽象机制，介绍用来支持数据抽象、面向对象编程以及泛型编程的语言特性；
- 第4章：容器与算法，介绍标准库提供的字符串、简单I/O、容器和算法等特性；
- 第5章：并发与实用功能，概述与资源管理、并发、数学计算、正则表达式以及其他一些方面相关的标准库工具。

##### 1.1.3 C++类型、对象、作用域和存储的基本概念，计算的基础以及支撑模板化的特性

- 第6章：类型与声明；基础类型、命名、作用域、初始化、简单类型推断、对象生命周期和类型别名
- 第7章：指针、数组与引用；
- 第8章：结构、联合与枚举；
- 第9章：声明语句、选择语句（if和switch）、迭代语句（for、while和do）、goto语句和注释语句
- 第10章：表达式；运行符、常量表达式和隐式类型转换
- 第11章：逻辑运算符、条件表达式、递增和递减、内存空间（new和delete）、｛｝列表、lambda表达式和显式类型转换（static_cast和const_cast）
- 第12章：函数；函数声明和定义、inline函数、constexpr函数、实参传递、重载函数、前置和后置条件、函数的指针和宏
- 第13章：异常处理；错误处理风格、异常保证、资源管理、强制不变量、throw和catch、一个vector的实现
- 第14章：命名空间；namespace、模块化和接口、使用命名空间组织代码
- 第15章：源文件与程序；分离编译、链接、使用头文件及程序启动和结束

##### 1.1.4 C++抽象机制

- 第16章：类；用户自定义类型，也就是类的概念，是所有C++抽象机制的基础
- 第17章：构造、析构、拷贝和移动；
- 第18章：运算符重载；
- 第19章：特殊运算符；用于下标的[],用于函数对象的()和用于智能指针的->
- 第20章：派生类；介绍构建类层次的基本语言特性及其基本使用方法
- 第21章：类层次；
- 第22章：运行时类型信息；
- 第23章：模板；
- 第24章：泛型程序设计
- 第25章： 特例化；介绍特例化技术，如何利用给定的一组模板参数，从模板生成类和函数
- 第26章：实例化；介绍名字绑定规则
- 第27章：模板和类层次；介绍模板层次和类层次如何结合使用
- 第28章：元编程；介绍如何用模板生成程序。模板提供了一种生成代码的图灵完备机制
- 第29章：一个矩阵设计

#### 1.2 C++程序设计风格

##### 1.2.1 C++四种程序设计风格：

- 过程式程序设计：专注于处理和设计恰当的数据结构；该风格是C语言的设计目标
- 数据抽象： 专注于接口的设计以及一般实现细节的隐藏和特殊的表示方式
- 面向对象程序设计： 专注于类层次的设计、实现和使用；类层次提供了运行时多态和封装机制
- 泛型程序设计： 专注于通用算法的设计、实现和使用



### 第二章	C++基础知识

~~~ markdown
首要任务，干掉所有语言专家
							————《亨利六世》（第二部分）
~~~

#### 2.1 基本概念

##### 2.1.1 C++是一种编译型语言；g++编译器拿到C++源文件到生成可执行程序，中间一共经历四个步骤：

~~~sh
# 第一步：g++ -E hello.cpp -o hello.i  （通过预处理cpp得到hello.i文件）
# 第二步：g++ -S hello.i -o hello.s    （通过编译器g++得到hello.s汇编文件）
# 第三步：g++ -c hello.s -o hello.o    （通过汇编器as得到hello.o二进制文件）
# 第四步：g++ hello.o print.o -o hello （通过链接器ld得到hello可执行文件）
~~~

##### 2.1.2 C++是一种静态类型语言；编译器在处理任何实体（对象、值、表达式）时，都必须清楚它的类型。对象的类型决定了能在该对象上执行哪些操作。

##### 2.1.3 类型、变量；

- 每个名字和每个表达式都有一个类型，类型决定所能执行的操作
- 声明（declaration）是一条语句，负责为程序引入一个新的名字，并指定该命名实体的类型：

~~~markdown
类型(type)：定义一组可能的值以及一组（对象上的）操作
对象（object）：是存放某类型值的内存空间
值（value）：是一组二进制位，具体的含义由其类型决定
变量（variable）：是一个命名的对象
~~~

##### 2.1.4 常量；

~~~c++
// const: "承诺不改变这个值"，主要用于说明接口，这样把变量传入函数时就不必担心变量会在函数内被改变了。编译器负责确认并执行const的承诺。
// constexpr: "在编译是求值"，主要用于说明常量，作用是允许将数据置于只读内存中以及提升性能。

int n = 10;			 					// n为变量
const int c_n = 10;  					// c_n为常量
constexpr double m1 = 1.4 * square(n) 	// 错误：n不是常量表达式
constexpr double m2 = 1.4 * square(c_n) // 正确
// 总结：在编译期就可以求值的，可以使用constexpr, 在运行期才求值的用const
~~~

#### 2.2 用户自定义类型

- 通过基本类型、const修饰符和声明运算符构造出来的类型称为内置类型（built-in type）;内置类型的优点是能够直接有效地展现传统计算机硬件的特性，但是并不能向程序员提供便于书写高级应用程序的上层特性。
- C++语言扩充内置类型和操作，提供了一套成熟的抽象机制，程序员可以使用这套机制实现其所需的上层功能；
- C++抽象机制的目的：主要是让程序员能够设计并实现他们自己的数据类型，这些数据类型具有恰当的表现形式和操作，程序员可以简单优雅地使用它们。
- 为了与内置类型区别，C++ 的抽象机制构建的新类型称为用户自定义类型，如结构、类和枚举等等；

##### 2.2.1 结构类型

​	会分割数据及其操作

~~~c++
// 结构定义
struct Vector {
    int size;
    int *elem;
};
// 结构初始化
Vector *init(int n) {
    Vector *v = new Vector();
    v->elem = new int[n];
    v->size = n;
    return v;
}
// 销毁结构
void clear(Vector *v) {
    if (v == nullptr) return ;
    delete[] v->elem;
    delete v;
    return ;
} 
~~~

##### 2.2.2 类

​	表示形式和操作之间建立紧密的联系，易于使用和修改，数据的使用具有一致性，且表示形式对用户时不可见的；把类型的接口（所有代码都可使用的部分）与其实现（对其他不可访问的数据具有访问权限）分离开；类含有一系列成员（member）,可能是数据、函数或者类型。类的public成员定义该类的接口，private成员则只能通过接口访问。

~~~c++
class Vector {
public:
    Vector(int n = 0) : cnt(n) {
        elem = new int[n];
    }
    ~Vector() {
        if (elem == nullptr) return ;
        delete[] elem;
        elem = nullptr;
    }
    int size() { return cnt; }
    int &operator[](int i) { return elem[i]; }
private:    
    int *elem;
    int cnt;
};
~~~

##### 2.2.3 枚举

​	常用于描述规模较小的整数集合，通过使用有指代意义的枚举值名字可提高代码的可读性，降低出错的风险。

~~~c++
// enum 后面的class指明了枚举是强类型的，且它的枚举值位于指定的作用域中。
enum class Color {red, blue, green};
enum class Light {green, yellow, red};
Color x = red; 					// 错误：哪个red?
Color y = Light::red; 			// 错误：这个red不是Color的对象
Color z = Color::red; 			// OK
// 不可以隐式转换  
int i = Color::red; 			// 错误
Color c = 2; 					// 错误
// 去掉class， 可以隐式转换 
~~~

#### 2.3 模块化

- 一个C++程序可能包含许多独立开发的部分（如函数、用户自定义类型、类层次和模板）
- 构建C++程序的关键就是清晰地定义组成部分之间的交互关系；第一步是将某个部分的接口和实现分离开。

##### 2.3.1 分离编译

- 用户代码只能看见所用类型和函数的声明，它们的定义放置在分离的源文件中，并被分别编译；
- 分离编译机制有助于将一个程序组织成一组半独立的代码片段；优点是编译时间减到最少，并且强制要求程序中逻辑独立的部分分离开来（降低出错率）；

~~~c++
// vector22.h 头文件
#ifndef _VECTOR2_H
#define _VECTOR2_H
class Vector {
public:
    Vector(const int&);
    ~Vector();
    int size();
    int &operator[](int);
private:
    int cnt;
    int *elem;
};
#endif
~~~

~~~c++
// vector2.cpp 文件
#include "vector2.h"

Vector::Vector(const int& n = 0) : cnt(n) {
    elem = new int[10];
}
Vector::~Vector() {
    if (elem == nullptr) return ;
    delete[] elem;
    elem = nullptr;
}
int Vector::size() { return cnt; }
int& Vector::operator[](int i) { 
    return elem[i]; 
}
~~~

~~~c++
// main.cpp 文件
#include <iostream>
#include "vector2.h"
using namespace std;
int main() {
    const int n = 5;
    Vector v(n);
    for (int i = n - 1; i >= 0; --i) {
        cin >> v[i];
    }
    for (int i = 0; i < n; ++i) {
        i && cout << ", ";
        cout << v[i];
    }
    cout << endl;
    return 0;
}
~~~

##### 2.3.2 名字空间

- 名字空间（namespace）的机制，一方面表达某些声明是属于一个整体的，另一方面表面它们的名字不会与其他名字空间中的名字冲突。

~~~c++
#define BEGIN(x) namespace x {
#define END(x) }

BEGIN(test)
int main(int n) {
    return n * n;
}     
END(test)
int main() {
    cout << test::main(4) << endl;
    return 0;
}
~~~

##### 2.3.3 错误处理

- 异常

~~~c++
int &Vector::operator[](int i) {
    if (i < 0 || i >= size) throw out_of_range{"Vector::operator"};
    return elem[i];
}
void func(Vector &v) {
	// ... 
    try { 					// 此处的异常将被后面定义的处理模块处理
        v[v.size()] = 7; 	// 试图越界访问
    } catch (out_of_range) {// 发现越界错误
        // 在此处理越界错误
    }
    // ...
}
~~~

- 不变式： 构造函数和析构函数支撑的资源管理概念的基础

~~~c++
Vector::Vector(int n) {
    if (n < 0) throw length_error{};
    // ...
}
void test() {
    try { 					// 此处的异常将被后面定义的处理模块处理
        Vector v(-27); 	// 试图越界访问
    } catch (std::length_error) { // 发现错误
        // 在此处理负值错误
    } catch (std::bad_alloc) {
        // 处理内存耗尽错误
    }
}
~~~

- 静态断言：程序异常负责报告运行时发生的错误

~~~c++
// 如果条件为true的话， 输出断言信息
static_assert(4 <= sizeof(int), "intergers are too small");
~~~

### 第三章	C++抽象机制

~~~markdown
不必惊慌失措！
                                     ————道格拉斯 ● 亚当斯
~~~

#### 3.1 类

- C++最核心的语言特性就是类；类是一种用户自定义的数据类型，用于程序代码中表示某种概念。
- 本质来说，基础类型、运算符和语句之外的所有语言特性存在的目的是帮助程序员定义更好的类以及更方便地使用它们。
- 3种重要的类的基本支持：具体类、抽象类、类层次中的类。 

##### 3.1.1 具体类型（concrete type）

- 具体类型的典型特征：它的表现形式是其定义的一部分；表现形式只不过是一个或几个指向保持在别处的数据的指针，这种表现形式出现在具体类的每一个对象中；这使得实现可以在时空上达到最优；

~~~markdown
1. 把具体类型的对象放置在栈、静态分配的内存或其他对象中
2. 直接引用对象（而非仅仅通过指针或引用）
3. 创建对象后立即进行完整的初始化（如使用构造函数）
4. 拷贝对象
~~~

###### 3.1.1.1 一种算术类型：复数类（complex）

~~~c++
class complex {
private:
    double m_re, m_im;
public:
    complex(double re = 0.0, double im = 0.0) : m_re(re), m_im(im) {}

    double get_real() const { return m_re; }
    void set_real(double d) { m_re = d; }
    double get_imag() const { return m_im; }
    void set_real(double d) { m_im = d;}

    complex& operator+=(const complex& z) {
        this->m_re += z.m_re;
        this->m_im += z.m_im;
        return *this;
    }
    complex& operator-=(cons complex& z) {
        this->m_re -= z.m_re;
        this->m_im -= z.m_im;
        return *this;
    }
    // ...
};
~~~

###### 3.1.1.2 容器

- 容器（container）是指一个包含若干元素的对象，因为Vector（前面自定义的类型）的对象都是容器，故Vector是一种容器对象。

- 资源获取即初始化（RAII）：在构造函数中请求资源，然后再析构函数中释放它们的技术；该技术可以避免在普通代码中分配内存，而是把分配操作隐藏在行为良好的抽象的实现内部；避免“裸new”和“裸delete”可以远离各种潜在风险，避免资源泄露。

###### 3.1.1.3 初始化容器

- 初始化器列表构造函数（Initializer-list constructor）: 使用元素的列表进行初始化；

~~~c++
// 用一个列表初始化
Vector::Vector(std::initializer_list<int> lst) 
    : cnt{lst.size()}, elem{new int[lst.size()]} 
{
	copy(lst.begin(), lst.end(), elem); 	// 把lst的内容复制到elem中；        
}

int main() {
    Vector v = {1, 2, 3, 4, 5};
    return 0;
}
~~~

##### 3.1.2 抽象类型（abstract type）

- 具体类型（concrete type）: 复数类（complex）和 Vector（容器）等类型，它们的表现形式属于定义的一部分， 与内置类型很相似；
- 抽象类型（abstract type）:将使用者与类的实现细节完全隔离开来；分离接口与表现形式并且放弃纯局部变量；我们对抽象类型的表现形式一无所知，故必须从自由存储为对象分配空间，然后通过引用或指针访问对象。
- 含有纯虚函数的类称为抽象类（abstract class）;

~~~c++
// 为Container类设计接口
class Container {
public:
    virtual int size(); 				// 虚函数
    virtual int& operator[](int) = 0;   // 纯虚函数
    virtual ~Container() {}             // 虚析构函数
};
~~~

- 如果一个类负责为其他一些类提供接口，那么把前者称为多态类型（polymorphic type）;

##### 3.1.3 虚函数（virtual function）

- 编译器将虚函数的名字转换成函数指针表中对应的索引值，这张表就是所谓的虚函数表（vtbl）;
- 每个含有虚函数的类都有它自己的虚函数表(vtbl)用于辨识虚函数；

~~~c++
// Container类设计为接口
class Container {
public:
    //...
    virtual int& operator[](int) = 0;
    //...
};
// Vector类继承Container类
class Vector : public Container {
public:
    //...
    int &operator[](int i) { return v[i]; } // 重写
    //...
};
// List类继承Container类
class List : public Container {
public:
    //...
    int &operator[](int i) { 	// 重写
        for (auto &x : lst) {
            return x;
        }
    }
    //...
}
// 普通函数 use
void use(Container& c) {
    for (int i = 0; i < c.size(); ++i) {
        cout << c[i] << endl;
    }
}
int main() {
    Vector vec{1, 2, 3, 4, 5};
    use(vec);                 		// Container &c = Vector vec;
    List lst{5, 4, 3, 2, 1};
    use(lst);						// Container &c = List lst;
    return 0;
}
~~~

##### 3.1.4 类层次（class hierarchy）

- 类层次（class hierarchy）：是指通过继承（派生）创建的一组类，在框架中有序排列；
- 接口继承（Interface inheritance）：派生类对象可以用在任何需要基类对象的地方。
- 实现继承（Implementation inheritance）: 基类负责提供可以简化派生类实现的函数或数据

#### 3.2 拷贝和移动

- 默认的拷贝构造函数和拷贝赋值运算符都是浅拷贝， 需要自定义拷贝构造函数（copy constructor） 与 拷贝赋值运算符（copy assignment）达到深拷贝；

- 拷贝

~~~c++
void test(Vector& v) {
    Vector v1{v};  		// 拷贝初始化
    Vector v2;
    v3 = v2; 			// 拷贝赋值
}
~~~

##### 3.2.1 类对象的拷贝操作可以通过两个成员来定义：拷贝构造函数（copy constructor） 与 拷贝赋值运算符（copy assignment）

~~~c++
// 拷贝构造函数
Vector::Vector(const Vector& v) : cnt(v.cnt), elem(new int[cnt]) {
    for (int i = 0; i < cnt; ++i) {
        elem[i] = v.elem[i];
    }
}
// 拷贝赋值运算符
Vector& Vector::operator=(const Vector& v) {
    int *p = new int[v.cnt];
    for (int i = 0; i < v.cnt; ++i) {
        p[i] = v.elem[i];
    }
    delete[] this->elem;
    this->elem = p;
    this->cnt = v.cnt;
    return *this;
}
~~~

##### 3.2.2 移动构造（move constructor）

~~~c++
Vector::Vector(Vector&& v) : cnt(v.cnt), elem(v.elem) {
    v.cnt = 0;
    v.elem = nullptr;
}

std::move() 		// 可以将值强转成右值
std::forward<T>() 	// 按照T的类型继承
~~~

##### 3.2.3 资源管理

- 通过构造函数、拷贝操作、、移动操作和析构函数，程序员就能对受控资源的全生命周期进行管理。
- 移动构造函数允许对象从一个作用域简单便捷地移动到另一个作用域。
- 把指针转化为资源句柄，可以实现强资源安全，可以消除资源泄露。

##### 3.2.4 抑制操作

- 尽量避免使用默认的拷贝和移动操作，最好的做法删除掉默认的拷贝和移动操作。

~~~c++
class Vector {
public:
    Vector(const Vector& ) = delete;
    Vector& operator=(const Vector&) = delete;
        
    Vector(const Vector&& ) = delete;
    Vector(const Vector&& ) = delete;
}
~~~

- 如果在类中显式地声明了析构函数，则移动操作将不会隐式地生成；故即使编译器可以隐式地提供析构函数，也最好自己定义一析构函数。

#### 3.3 模板

- 模板是一种编译时的机制，与“手工编写的代码”相比，并不会产生任何额外的运行时开销

##### 3.3.1 参数化类型

~~~c++
// template<typename T> 指明T是该声明的形参
template<typename T>
class Vector {
private:
    T* elem;
    int cnt;
public:
    //...
    T& operator[](int);
    //...
}
// 成员函数的定义
template<typename T>
T& Vector<T>::operator[](int i) {
    //...
}
// 使用方式
Vector<int> vi(10);
Vector<double> vd(20);
~~~

~~~c++
// 定义begin()和end（）函数，使Vector支持范围for循环
template<typename T>
T* begin(Vector<T>& v) {
    return &v[0];
}

template<typename T>
T* end(Vector<T>& v) {
    return v.begin() + v.size();
}

for (auto &x : Vec) {
    //...
}

~~~

##### 3.3.2 函数模板

~~~c++
template<typename T, typename U>
U func(const T& Vec, U sum) {
    for (auto v : Vec) {
        sum += v;
    }
    return sum;
}
~~~

##### 3.3.3 函数对象（functor）

- 重载括号（），可以实现函数对象

- lambda表达式，可以生产一个函数对象

~~~c++
[](){}
// [] 是一个捕获列表， [&x]表示所用的局部名字x将通过引用访问， [=x] 表示给函数对象传递一个x的拷贝
// [] 表示什么也不捕获，[&]表示捕获所有通过引用的局部名字，[=] 表示所有以值访问的局部名字
~~~

##### 3.3.4 可变参数模板（variadic template）

- 实现可变参数模板的关键：当你传给它多个参数时，谨记把第一个参数和其他参数区分对待。

~~~c++
template<typename T, typename... ARGS>
void func(T head, ARGS... tail) {
    print(head); 	// 对head做某些操作
    func(ARGS..); 	// 再次处理tail
}

~~~

##### 3.3.5 别名

~~~c++
// 将unsigned int 重命名为 size_t
using size_t = unsigned int;
~~~

### 第四章	容器与算法

~~~markdown
当无知只是瞬间， 又何必浪费时间学习呢？
													————霍布斯
~~~

#### 4.1 字符串 < string >

- string对象是可变的，重载了各种运算符和操作符。

#### 4.2 I/O流 < iostream >

##### 4.2.1 输出 （ostream类）

- cout对象： 标准输出流， cerr对象：错误输出流

##### 4.2.1 输入 （istream类）

- cin对象：标准输入流
- getline()读取一整行（包括换行符）

##### 4.2.3 自定义类型的I/O

~~~c++
// 重载输出运算符<< 
ostream& operator<<(ostream& out, const Vector& v) {
    return out << "(" << v << ")";
}
// 重载输入运算符>>
istream& operator>>(istream& in, const Vetor& v) {
    //...
    return in;
}

~~~

#### 4.3 容器

~~~c++
vector<T>;   				// 可变大小向量
list<T>; 					// 双向链表
forward_lis<T>;				// 单向链表
deque<T>; 					// 双端队列
set<T>; 					// 集合
multiset<T>; 				// 允许重复值的set
map<K, V>; 					// 关联数组 
muiltmap<K, V>; 			// 允许重复值的map
unordered_map<K, V>; 		// 采用哈希搜索的map
unordered_multimap<K, V>; 	// 采用哈希搜索的muiltmap
unordered_set<T>; 			// 采用哈希搜索的set
unordered_muiltset<T>; 		// 采用哈希搜索的muiltset
~~~

#### 4.4 算法

##### 4.4.1 迭代器

- 迭代器的一个重要的作用是分离算法和容器。算法通过迭代器来处理数据，但它对存储的容器一无所知。容器对算法也是一无所知，它所做的全部事情就是按需求提供迭代器（如 begin()和end()）。这种数据存储和算法分离的模型催生出非常通用和灵活的软件。
- 任何一种特定的迭代器都是某种类型的对象。

### 第五章	并发与实用功能

~~~markdown
要想让别人听得明白，言辞必须简洁。
										————西塞罗		
~~~

#### 5.1 资源管理

##### 5.1.1 unique_ptr 与 shared_ptr