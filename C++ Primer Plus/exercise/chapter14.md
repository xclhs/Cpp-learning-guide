# chapter14

#### 问答题

1. 对于以下每一组类，指出public还是private基础对B列更合适：

|               A                |          B           | result  |
| :----------------------------: | :------------------: | :-----: |
|           class Bear           |   class PolarBear    | public  |
|         class Kitchen          |      class Home      | private |
|          class Person          |   class Programmer   | public  |
|          class Person          | class HorseAndJockey | private |
| class Person, class Automobile |     class Driver     | private |



2.假设你有以下定义

```c++
class Frabjous { 
    private:
    	char fab[20]; 
    public:
		Frabjous(const char * s = "C++") : fab(s) { } 
        virtual void tell() { cout << fab; }
};

class Gloam { 
    private:
		int glip;
		Frabjous fb;
	public:
		Gloam(int g = 0, const char * s = "C++"); 
        Gloam(int g, const Frabjous & f);
    	void tell();
};
```

鉴于Gloam版本的tell()应该显示glip和fb的值，请提供这三个Gloam方法的定义。

```c++
void Gloam::tell() {
    cout<<"Glip::"<<glip<<'\n';
    fb.tell();
}
Gloam::Gloam(int g, const Frabjous &f) {
  glip=g;
  fb=f;
}

Gloam::Gloam(int g, const char *s) {
    glip=g;
    fb=Frabjous(s);
}
```



3.假设你有以下定义：

```c++
class Frabjous { 
    private:
		char fab[20]; 
    public:
		Frabjous(const char * s = "C++") : fab(s) { } 
        virtual void tell() { cout << fab; }
};

class Gloam : private Frabjous{ 
    private:
		int glip; 
    public:
		Gloam(int g = 0, const char * s = "C++"); 
    	Gloam(int g, const Frabjous & f);
		void tell();
};

```

鉴于Gloam版本的tell()应该显示glip和fab的值，请提供这三个Gloam方法的定义。

```c++
void Gloam::tell() {
    cout<<"Glip::"<<glip<<'\n';
    Frabjous::tell();
}
Gloam::Gloam(int g, const Frabjous &f): Frabjous(f) {
  glip=g;
}

Gloam::Gloam(int g, const char *s): Frabjous(s) {
    glip=g;
    
}
```



4.假设你有以下定义，基于Stack模板和Worker类：

```c++
Stack<Worker *> sw;
```

写出将被生成的类声明。只要写出类的声明，不要写出非内联的类方法。

```c++
class Stack{
private:
    enum {MAX = 10};	// constant specific to class
    Worker* items[MAX];	// holds stack items
    int top;	// index for top stack item
public:
    Stack();
    bool isempty();
    bool isfull();
    bool push(const Worker* & item); // add item to stack
    bool pop(Worker* & item);	// pop top into item
};
```



5.使用本章中的模板定义来定义以下内容：

- 一个字符串对象的数组

- 一个double数组的栈

- 一个指向Worker对象的栈 

产生了多少个模板类定义？

1.产生了一个字符串的数组定义

2.产生了一个double数组定义，一个double数组的栈定义

3.产生了一个Worker对象的栈



6.描述一下虚拟基类和非虚拟基类之间的区别。

虚拟基类：

- 派生类共享虚拟基类一个对象
- C++会禁止通过中间类向基类自动传递信息

非虚拟基类：

- 派生类单独拥有基类对象的副本
- 可以通过中间类向基类传递信息



#### 编程题

1. 葡萄酒类有一个string class对象成员（见第4章），它持有葡萄酒的名称和一个Valarray\<int>对象的Pair对象(如本章所讨论的)。每个Pair对象的第一个成员持有年份，第二个成员持有相应的特定年份所拥有的瓶子数量。

   例如，Pair对象的第一个valarray对象可能持有1988年、1992年和1996年，而第二个valarray对象可能持有24、48和144瓶的数量。对于Wine来说，拥有一个存储年份的int成员可能是很方便的。另外，一些类型化的定义对于简化编码可能是有用的：

```c++
typedef std::valarray<int> ArrayInt; 
typedef Pair<ArrayInt, ArrayInt> PairArray;
```



通过使用**包含**法实现Wine类

该类应该有一个默认的构造函数和至少以下的构造函数：

```c++
// initialize label to l, number of years to y,
// vintage years to yr[], bottles to bot[]
Wine(const char * l, int y, const int yr[], const int bot[]);
// initialize label to l, number of years to y,
// create array objects of length y 
Wine(const char * l, int y);
```



2.





3.



4.





5.





