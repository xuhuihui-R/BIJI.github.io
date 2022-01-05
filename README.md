# 文件操作

对文件操作必须包含头文件< fstream >

操作文件的类：

- ofstream: 写操作
- ifstream: 读操作
- fstream: 读写操作

打开文件：对象名.open("文件路径"，打开方式)；

|  打开方式   | 解释                        |
| :---------: | :-------------------------- |
|   ios::in   | 为读文件而打开文件          |
|  ios::out   | 为写文件而打开文件          |
|  ios::ate   | 初始位置：文件尾            |
|  ios::app   | 追加方式写文件              |
| ios::trunc  | 如果文件存在 先删除，再创建 |
| ios::binary | 二进制方式                  |

注意：文件打开方式可以配合使用，利用 ”|“ 操作符

- 文件打开方式：

```
ifstream ifs;
第一种：
char buf[1024] = {0};
while(ifs >> buf)
{
	cout<< buf <<endl;
}
第二种：
char buf[1024] = {0};
while(ifs.getline(buf,sizeof(buf)))
{
	cout<< buf <<endl;
}
第三种：
string buf;
while(getline(ifs,buf))
{
	cout<< buf <<endl;
}
第四种：
char c;
while((c = ifs.get())!=EOF)
{
	cout<< c <<endl;
}
```

```
对象名.is_open() :判断文件是否打开成功 
EOF：文件的尾部
```

## 二进制文件

打开方式指定为： ios::binary

- 二进制方式写文件主要利用流对象调用成员函数write
- 函数原型： ostream & write (const char * buffer ,int len);
- 参数解释： 字符指针buffer指向内存一段存储空间，len是读写的字节数。

读文件：

- 函数原型：istream& read (char * buffer ，int len);
- 参数解释：字符指针buffer指向内存中一段存储空间，len是读写的字符数

# 模板

```
特点：不可直接使用，只是一个框架；模板的通用不是万能的。
```

## 1.1 函数模板

作用：建立一个通用函数，其返回值类型可以不具体指定，用一个虚拟的类型来表示

- 语法：

```
函数声明或定义
template<typename T>
```

template: 声明创建模板。

typename: 表示其后面的符号是一种数据类型，可以用class代替。

T： 通用的数据类型，名称可以替换，通用为大写字母

```
注意事项：
- 自动类型推导：必须推导出一致的数据类型T
- 模板必须确定出T的数据类型，才可以使用。
```

## 1.2 普通函数与函数模板区别

- 普通函数调用时可以发生自动类型转换（隐式类型转换）
- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换；如果利用显示指定类型的方式，可以发生隐式类型转换

## 1.3 普通函数与函数模板的调用规则

- 如果函数模板和普通函数都可以实现，优先调用普通函数
- 可以通过空模板参数列表来强制调用模板函数
- 函数模板也可以发生重载
- 如果函数模板可以产生更好的匹配，优先调用函数模板（普通函数中要发生隐式类型转换时，函数模板是更好的匹配。）

## 1.4 类模板

```
语法：
template <class T>
后跟类
```

## 1.5 类模板与函数模板的区别

- 类模板没有自动类型推导的使用方式
- 类模板在模板列表中可以有默认参数

```
template<class T = int(或其他类型)>
```

## 1.6 类模板中成员函数创建时机

- 普通类中的成员函数一开始可以创建
- **类模板中的成员函数调用时才创建**

## 类模板对象作函数参数

- 类模板实例化的对象，向函数传递的方式

三种传入方式：

指定传入的类型：直接显示对象的数据类型（使用比较广泛）

参数模板化：将对象中的参数变为模板进行传递

整个类模板化：将这个对象类型模板化进行传递

## 1.7 类模板与继承

- 子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型。
- 如果不指定，编译器无法给子类分配内存空间。
- 如果想灵活指定父类中T的类型，子类也需要变为类模板。

```
如果父类是类模板，子类需要指定出父类中T的数据类型
```

## 1.8 类模板成员函数类外实现

```
类模板中成员函数类外实现时
```

## 1.9 类模板分文件编写

- 问题：

类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

- 解决：

1、直接包含.cpp源文件

2、将声明和实现写到同一个文件中，并更改后缀名为.hpp，

# STL

## 1.1 基本概念

STL（Standard	Template	Library）标准模板库；

STL从广义上分为：容器（containter），算法（algorithm），迭代器（iterator）；

容器和算法之间通过迭代器进行链接；

STL几乎所有的代码都采用了类模板和函数模板；

## 1.2 STL六大组件

分别为:容器、算法、迭代器、仿函数、适配器、空间配置器。

1、容器：各种数据结构、如vector，list，deque，set，map等用来存放数据

2、算法：各种常用的算法，如sort，find，copy，for_each等

3、迭代器：扮演了容器 与算法之间的胶合剂

4、仿函数：行为类似函数，可作为算法的某种策略

5、适配器：一种用来修饰容器或者仿函数或迭代器接口的东西

6、空间配置器：负责空间的配置与管理

## 1.3 STL中容器、算法、迭代器

- 容器

序列式容器：强调值得排序，序列式容器中得每个元素均有固定的位置

关联式容器：二叉树结构，各元素之间没有严格的物理上的顺序

- 算法

质变算法：是指运算过程中会更改区间内的元素内容，例如拷贝，替换，删除等

非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找，计算，遍历，寻找极值等

- 迭代器

| 种类           | 功能                                                     | 支持算法                                         |
| -------------- | -------------------------------------------------------- | ------------------------------------------------ |
| 输入迭代器     | 对数据的只读访问                                         | 只读、支持++，==，！=                            |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持++                                     |
| 前向迭代器     | 读写操作，并能向前推进迭代器                             | 读写，支持++，==，！=                            |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持++，- -                                |
| 随机访问迭代器 | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持++，- - ，[ n ], - n ，< , <= , > , >= |

- 常用的容器中迭代器种类为双向迭代器，和随机访问迭代器。

## 1.4 string容器

### 1.4.1 string基本概念

- 本质：string是C++风格的字符串，而string本质是一个类。

string和char*区别：

- char*是一个指针
- string是一个类，类内部封装了char*，管理这个字符串，是一个 char *型的容器。

特点：

- string类内部封装了很多方法；例如：查找find、拷贝copy、删除delete、替换replace、插入insert。
- string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责。

### 1.4.2 string构造函数

构造函数原型：

```
string()；创建一个空的字符串，例如：string str；
string(const char* s); 使用字符串 s 初始化；
string（const string &str）;使用一个string对象初始化另一个string对象；                           
string（int n,char c）;使用n个字符c初始化；
```

### 1.4.3 string的赋值操作

assign()是string类的成员函数。

赋值的函数原型：

```
重载 ‘=’：
operator = (const char* s); char*类型字符串赋值给当前的字符串。
operator = (const string &s); 把字符串 s赋值给当前的字符串。
operator = (char c); 字符赋值给当前的字符串。

assign(const char *s); 把字符串 s 赋值给当前的字符串。
assign(const char *s ,int n); 把字符串 s 的前 n 个字符赋值给当前的字符串。
assign(const string &s); 把字符串 s 赋值给当前字符串。
assign(int n,char c); 用n个字符 c 赋值给当前字符串。
```

### 1.4.4 string字符串的拼接

- 实现在字符串末尾拼接字符串

```
重载 ‘+=’：
operator += (const char* str); 重载+=操作符。后追加一个char字符串。
operator += (const char c); 重载+=操作符。 后追加一个字符。
operator += (const string& str); 重载+=操作符。 后追加一个string字符串

append(const char*s); 把字符串s连接到当前字符串尾。
append(const char* s ,int n); 把字符串s的前n个字符连接到当前字符结尾。
append(const string &s); 同operator+=(const string &str).
append(const string &s ,int pos ,int n); 字符串s中从pos开始n个字符连接到字符串结尾。
```

### 1.4.5 string 查找和替换

函数find（）和rfind（）：在主字符串中查找模板字符串；

区别：

- find（）函数是从左往右查找；
- rfind（）函数是从右往左查找；
- find找到字符串后返回查找的第一个字符位置，找不到返回-1。

替换函数replace（）；

```
replace(int pos ,int n ,const string& str); 替换从pos开始n个字符尾字符串str
replace(int pos ,int n ,const char& s); 替换从pos开始的n个字符为字符串s。
```

- 是将从pos到n的字符替换成指定的字符串 s 。

### 1.4.6 string字符串比较

函数：compare（）

### 1.4.7 string字符串的存取

string中单个字符存取方式：

- operator[] (int n);    	通过[]方式取字符
- at(int n);	 通过at方式取字符

### 1.4.8 string字符串的插入和删除

- 插入函数：insert（int n , str）; 在n位置插入字符str。
- 删除函数：erase（int pos ， int n）； 删除从pos开始的n个字符。

### 1.4.9 string子串

- 从字符串获取想要的子串。

函数：string  substr(int pos , int n); 返回由pos开始的n个字符组成的字符串。返回的是一个字符串。

## 1.5 vector容器

### 1.5.1 基本概念

- vector数据结构和数组非常类似，也称为**单端数组**。

vector与普通数组的区别：

- 不同之处在于数组是静态空间，而vector可以动态扩展。

动态扩展：

- 并不是在原空间后续新空间，而是找更大的内存空间，然后将原有数据拷贝新空间，释放原空间。

- vector容器的迭代器是支持随机访问的迭代器。

### 1.5.2 构造函数

- 创建vector容器。

函数原型：

```
vector<T> v; 采用模板实现类实现，默认构造函数。
vector(v.begin(),v.end()); 将v[begin(),end())区间中的元素拷贝给本身
vector(n,elem); 构造函数将n个elem拷贝给本身
vector(const vector & vec); 拷贝构造函数
```

### 1.5.3 vector赋值操作

```
重载‘=’
operator= (const vector & vec); 重载等号操作符
assign(beg,end); 将[beg,end)区间中的数据拷贝赋值给本身
assign(n,elem); 将n个elem拷贝赋值给本身
```

### 1.5.4 vector容量大小

```
empty(); 判断容器是否为空
capacity(); 容器的容量
size(); 返回容器中元素的个数
resize(int num);  重新指定容器的长度为num，若容器变长，则以默认值填充新位置；如果容器变短，则末尾超出容器长度的元素被删除
resize(int num,elem);重新指定容器的长度为num，若容器变长，则以elem值填充新位置；如果容器变短，则末尾超出容器长度的元素被删除
```

### 1.4.5 vector插入删除

```
push_back(ele); 尾部插入元素ele
pop_back(); 删除最后一个元素

insert(const_iterator pos ,ele); 迭代器指向位置pos插入ele
insert(const_iterator pos,int count ,ele); 迭代器指向位置pos插入count个元素ele

erase(const_iterator pos); 删除迭代器指向的位置
erase(const_iterator star ,const_iterator end);删除迭代器从start到end之间的元素
clear(); 清空容器
```

### 1.5.6 vector数据存取

```
at(int idx); 返回索引idx所指的数据
operator[]; 返回索引idx所指的数据
front(); 返回容器中的第一个数据元素
back(); 返回容器中的最后一个元素
```

### 1.5.7 vector容器交换

```
swap(vec); 将vec与本身的元素互换
```

用途：收缩内存。

```
vector<int>v;
vector<itn>(v).swap(v); vector<int>(v)为匿名对象
一个vector容器的容量很大，而容器实际存储大小很小，导致浪费空间。使用swap()函数可以将容器容量的大小收缩至存储大小。
```

### 1.5.8 vector预留空间

功能：减少vector在动态扩展容量时的扩展次数

```
reserve(int len); 容器预留len个元素长度，预留位置不初始化，元素不可访问
```

- 如果数据量较大，可以开始时reserve预留出空间

## 1.6 deque容器

### 1.6.1 deque容器的基本概念

功能：**双端数组**，可以对头端进行插入删除。

deque与vector的区别：

- vector对于头部的插入删除效率低，数据量越大，效率越低。
- deque相对而言，对头部的插入删除速度会比vector快。
- vector访问元素时的速度比deque快。

deque内部工作原理：

- deque内部有一个中控器，维护每段缓冲区的内容，缓冲区中存放真实数据，中控器维护的是每个缓冲区的地址，使得deque像一片连续的内存空间。
- deque容器的迭代器支持随机访问

<img src="C:/Users/12258/Desktop/%E5%8C%85/%E9%9B%86%E5%90%88/QQ%E6%88%AA%E5%9B%BE20220101100201.jpg" 
alt="QQ截图20220101100201" style="zoom: 80%;" />

### 1.6.2 deque的构造函数

函数原型：

```
deque<T>; 默认构造形式
deque(beg,end); 构造函数将[beg,end)区间中的元素拷贝给自身
deque(n,elem); 构造函数将n个elem拷贝给自身
deque(const deque &dep); 拷贝构造函数
```

- 如果限定容器为只读状态，则迭代器更改为const_iterator

```
deque<int>::const_iterator
```

### 1.6.3 deque大小操作

```
deque.empty();
deque.size();
deque.resize();
```

- deque没有容量capacity概念

### 1.6.4 deque插入删除

两端插入删除操作：

```
push_back(elem); 在容器尾部添加一个数据
push_front(elem); 在容器的头部插入一个数据
pop_back(); 删除容器的最后一个数据
pop_front(); 删除容器的第一数据
```

指定位置操作：

```
pos为提供的一个迭代器
insert(pos,elem); 在pos位置插入一个elem元素的拷贝，返回新数据的位置
insert(pos,n,elem); 在pos位置插入n个elem数据，无返回值
insert(pos,beg,end); 在pos位置插入[beg,end)区间的数据，无返回值
clear(); 清空容器中所有数据
erase(beg，end); 删除[beg,end)区间的数据，返回下一个数据的位置
erase(pos); 删除pos位置的数据，返回下一个数据的位置
```

### 1.6.5 deuqe数据存取

```
at(int idx); 返回索引idx所指的数据
operator[]; 返回索引idx所指的数据
front(); 返回容器中第一个数据元素
back();返回容器中最后一个数据元素
```

### 1.6.6 deque排序

```
sort(iterator beg,iterator end); 对beg和end区间内元素进行排序
使用时包含头文件"algorithm"
```

- 对于支持随机访问的迭代器的容器(vector,deuqe...)，都可以**利用sort()算法直接对其进行排序**

## 1.7 stack容器

### 1.7.1 satck基本概念

- stack是一种先进后出（First In Last Out）的数据结构，它只有一个出口。
- 栈中只有顶端的元素才可以被外界使用，栈不允许有遍历的行为。

### 1.7.2 stack常用接口

- 构造函数：

```
stack<T> stk; stack采用模板类实现，stack对象的默认构造形式
stack(const stack &stack); 拷贝构造函数
```

- 赋值操作:

```
stack& operator=(const stack &stack); 重载等号操作符
```

- 数据存取：

```
push(elem); 向栈顶元素添加元素
pop(); 从栈顶移除第一个元素
top(); 返回栈顶元素
```

- 大小操作：

```
empty(); 判断d
size(); 返回栈的大小
```

## 1.8 queue容器

### 1.8.1 queue基本概念

- queue是一种先进先出(First In First Out) 的数据结构。

队列容器允许从一端新增元素，从另一端移除元素。

队列中只有对头和队尾才可以被外界使用，因此队列不允许遍历行为。

### 1.8.2 queue常用接口

- 构造函数：

```
queue<T> que; queue采用模板类实现，queue对象的默认构造形式
queue(const queue &que); 拷贝构造函数
```

- 赋值操作：

```
queue& operator=(const queue &que); 重载等号操作符 
```

- 数据存取：

```
push(elem); 往队尾添加元素
pop(); 从队头移除第一个元素
back(); 返回最后一个元素
front(); 返回第一个元素
```

- 大小操作：

```
empty();
size(); 
```

## 1.9 list容器

### 1.9.1 list基本概念

- 将数据进行链式存储。

链表（list）是一种物理存储单元上不连续的存储结构，数据元素的逻辑顺序是通过链表的指针链接实现的。

链表的组成：链表有一系列结点组成。

结点的组成：一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域。

**STL中的链表是一个双向循环链表。**

- 由于链表的存储方式并不是连续的内存空间，因此链表中的迭代器只支持前移和后移，属于双向迭代器

list的优点：

- 采用动态存储分配，不会造成内存浪费和溢出
- 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点：

- 链表灵活，但是空间（指针域）和时间（遍历）额外消耗较大

list的一个重要的性质：插入和删除操作都不会造成原有list迭代器失效，vector不成立（当插入的数据超过原有容量时会进行扩展，迭代器的位置就会改变）。

### 1.9.2 list构造函数

```
list<T>lst; list采用模板实现，对象的默认构造形式
list(beg,end); 构造函数将[beg,end)区间中的元素拷贝给自身
list(n,elem); 构造函数将n个elem拷贝给自身
list(const list& lst); 拷贝构造函数
```

### 1.9.3 list赋值和交换

```
assign(beg,end); 将[beg,end)区间中的数据拷贝赋值给自身
assign(n,elem); 将n个elem拷贝赋值给自身
list& operator=(const list& lst); 重载等号操作符
swap(lst); 将lst与本身的元素互换
```

### 1.9.4 list大小操作

```
size(); 
empty();
resize(num);
resize(num,elem);
```

### 1.9.5 list插入和删除

```
push_back(elem);
pop_back();
push_front(elem);
pop_front();

insert(pos,elem);  //pos为迭代器对象
insert(pos,n,elem);
insert(pos,beg,end);

clear();
erase(beg,end);
erase(pos);
remove(elem); 删除容器中所有与elem值匹配的元素
```

### 1.9.6 list数据存取

```
front(); 返回第一个元素
back(); 返回最后一个元素
```

### 1.9.7 list反转和排序

```
reverse(); 反转链表
sort(); 是一个成员函数 ，需要容器对象调用此函数。
```

- 所有不支持随机访问的迭代器的容器，不可以用标准算法（标准算法是全局函数）。

## 2.0 set/multiset容器

### 2.0.1 set基本概念

- 所有元素都会在插入时自动排序

- set/multiset属于关联式容器，底层结构是用二叉树实现

set和multiset区别：

- set不允许容器中有重复元素、
- multiset允许容器中有重复元素

### 2.0.2 set构造和赋值

```
set<T> st;
set(const set& st);

set& operator=(const set &st);
```

### 2.0.3 set大小和交换

```
size();
empty();
swap(st);
```

###  2.0.4 set插入删除

```
insert(elem);
clear();
erase(pos);
erase(beg,end);
erase(e)
```

### 2.0.4 set查找和统计

```
find(key); 查找key是否存在，如存在返回该键值的迭代器，如不存在返回set.end()
count(); 统计key的元素个数
```

### 2.0.5 set和miltiset的区别

区别：

- set不可以插入重复的数据，multiset则可以。
- set插入数据的同时返回插入结果，表示插入是否成功。
- multiset不会检测数据，因此可以插入重复数据

### 2.0.6 pair对组创建

- 成对出现的数据，利用对组返回两个数据

````
pair<type,type> p(value1,value2);
pair<type,type> p = make_pair(value1,value2);
````

###  2.0.7 set容器排序

- set容器默认排序为从小到大
- 利用仿函数，可以改变排序规则

## 2.1 map和multimap容器

### 2.1.1 map基本概念

- map中所有元素都是pair
- pair中第一个元素为key(键值)，起到索引作用，第二个元素为value(实值)
- 所有元素都会根据元素的键值自动排序

本质：

- map/multimap属于关联式容器，底层结构是用二叉树实现

优点：

- 可以根据key值快速找到value值

map和multimap区别：

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素

### 2.1.2 map构造和赋值

```
map<T1,T2>mp;
map(const map &mp);

map &operator=(const map &mp);
```

### 2.1.3 map大小和交换

```
size();
empty();
swap(st);
```

### 2.1.4 map插入删除

```
insert(elem); 以对组的形式插入
clear();
erase(pos);
erase(beg,end);
erase(key);
```

### 2.1.5 map查找和统计

```
find(key); 若找到返回该键值的迭代器，否则返回set.end()
count(key);
```
