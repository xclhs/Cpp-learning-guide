# 第十三章

#### 问答题

1.派生类从基类中继承了什么？

public和protected的成员函数和成员变量（除构造函数、析构函数、赋值函数）



2.派生类不从基类继承什么？

不继承构造函数、析构函数、赋值操作，友元函数



3.假设baseDMA::operator=()函数的返回类型被定义为void而不是baseDMA &.如果有的话，会有什么影响？如果返回类型是baseDMA而不是baseDMA &呢？

影响：

void:

a.不能进行链式运算

baseDMA:

a.当派生类作为参数时，只有基类部分被赋值，丢失派生类的部分

b.返回时会触发拷贝复制操作，创建临时变量，运行会比较低效



4.当一个派生类对象被创建和删除时，类的构造函数和类的析构函数是按照什么顺序调用的？

create:调用基类构造函数，调用派生类构造函数

delete:调用派生类析构函数，调用基类析构函数



5.如果一个派生类不向基类添加任何数据成员，派生类是否需要构造函数？

派生类无法继承基类的构造函数，故需要构造函数



6.假设一个基类和一个派生类都定义了一个同名的方法，一个派生类对象调用了该方法，那么调用的是什么方法？

调用的是派生类定义的方法



7.派生类何时应该定义赋值运算符？

派生类的成员变量涉及到动态存储，需要定义赋值运算符



8.你能把派生类的对象的地址分配给基类的指针吗？你能把基类的一个对象的地址分配给派生类的指针吗？

第一种可以，第二种不可以（一般情况下）



9.你能把派生类的一个对象分配给基类的一个对象吗？你能把基类的一个对象赋给派生类的一个对象吗？

第一种可以，第二种不可以（一般情况下）



10.假设你定义了一个以基类对象的引用为参数的函数。为什么这个函数也可以使用派生类对象作为参数？

将派生类引用作为参考时，该函数会只处理该派生类的基类部分。（向上转换）



11.假设你定义了一个以基类对象为参数的函数（也就是说，该函数通过值传递一个基类对象），为什么这个函数也可以使用派生类对象作为参数？

对象切分机制：当派生类作为参数传递给基类对象的形参时，编译器会创造一个派生类的基类部分的临时拷贝。



12.为什么通过引用传递对象通常比通过值传递更好？

a.更高效

b.避免对象切分

c.可变性（对参数的变化会影响原对象）

d.避免无意义的拷贝

e.多态性



13.假设Corporation是一个基类，PublicCorporation是一个派生类。还假设每个类都定义了一个head()成员函数，ph是一个指向Corporation类型的指针，并且ph被分配了一个PublicCorporation对象的地址。如果基类将head()定义为a，那么ph->head()如何解释？

a.	普通的非虚拟方法

调用的Corporation::head()

b.	虚拟方法

调用的PublicCorporation::head()



14.如果有的话，下面的代码有什么问题吗？

```c++
class Kitchen
{
private:
    double kit_sq_ft; 
public:
    Kitchen() { kit_sq_ft = 0.0; }
    virtual double area() const { 
        return kit_sq_ft * kit_sq_ft; 
    }
};

class House : public Kitchen
{
private:
    double all_sq_ft; 
public:
    House() {
        all_sq_ft += kit_sq_ft;
    }
    double area(const char *s) const { 
        cout << s; 
        return all_sq_ft; 
    }
};

```

House()函数：基类的私有数据无法直接获取,同时all_sq_ft没有进行初始化

由于基类的area是virtual则派生类需要重载virtual函数，保持参数和函数名一致

```c++
class Kitchen {
private:
    double kit_sq_ft;
public:
    Kitchen() {
        kit_sq_ft = 0.0;
    }
    virtual double area() const {
        return kit_sq_ft * kit_sq_ft;
    }
};

class House : public Kitchen {
private:
    double all_sq_ft;
public:
    House() : all_sq_ft(0.0) { // Initialize all_sq_ft to 0.0
        all_sq_ft += sqrt(area()); // Increment all_sq_ft with the area of Kitchen
    }
    virtual double area() const override { // Override Kitchen's area() function
        return all_sq_ft;
    }
};
```





#### 编程题

1.从下面的类声明开始：

```c++
// base class
class Cd { // represents a CD disk private:
    char performers[50]; 
    char label[20];
    int selections;	// number of selections 
    double playtime; // playing time in minutes
public:
	Cd(char * s1, char * s2, int n, double x); 
    Cd(const Cd & d);
    Cd();
    ~Cd();
	void Report() const; // reports all CD data 
    Cd & operator=(const Cd & d);
};

```

派生出一个Classic类，增加一个char成员数组，该数组将容纳一个字符串，用于识别CD上的主要工作。如果基类要求任何函数是虚拟的，请修改基类的声明，使其成为虚拟的。如果声明的方法不需要，就从定义中删除它。用下面的程序测试你的产品：

```c++
#include <iostream> 
using namespace std;
#include "classic.h"	// which will contain #include cd.h
 
void Bravo(const Cd & disk); 
int main()
{
	Cd c1("Beatles", "Capitol", 14, 35.5);
	Classic c2 = Classic("Piano Sonata in B flat, Fantasia in C", "Alfred Brendel", "Philips", 2, 57.17);
    Cd *pcd = &c1;

    cout << "Using object directly:\n"; 
    c1.Report();	// use Cd method 
    c2.Report();	// use Classic method

    cout << "Using type cd * pointer to objects:\n"; 
    pcd->Report(); // use Cd method for cd object 
    pcd = &c2;
    pcd->Report(); // use Classic method for classic object

    cout << "Calling a function with a Cd reference argument:\n"; 
    Bravo(c1);
    Bravo(c2);

    cout << "Testing assignment: "; 
    Classic copy;
    copy = c2; 
    copy.Report()

    return 0;
}

void Bravo(const Cd & disk)
{
    disk.Report();
}

```

> Label:Philips                                                                                                          
>
>  Selections:2                                                                                                           
>
> PlayTime:57.17 minutes                                                                                                  
>
> Primary work:Piano Sonata in B flat, Fantasia in C                                                                      
>
> Using type cd * pointer to objects:                                                                                    
>
>  Perfermers:Beatles                                                                                                      
>
> Label:Capitol                                                                                                           
>
> Selections:14                                                                                                           
>
> PlayTime:35.5 minutes                                                                                                   
>
> Perfermers:Alfred Brendel                                                                                               
>
> Label:Philips                                                                                                          
>
>  Selections:2                                                                                                           
>
>  PlayTime:57.17 minutes                                                                                                  
>
> Primary work:Piano Sonata in B flat, Fantasia in C                                                                      
>
> Calling a function with a Cd reference argument:                                                                        
>
> Perfermers:Beatles                                                                                                      
>
> Label:Capitol                                                                                                           
>
> Selections:14                                                                                                           
>
> PlayTime:35.5 minutes                                                                                                   
>
> Perfermers:Alfred Brendel                                                                                               
>
> Label:Philips                                                                                                           
>
> Selections:2                                                                                                            
>
> PlayTime:57.17 minutes                                                                                                  
>
> Primary work:Piano Sonata in B flat, Fantasia in C                                                                     
>
>  Testing assignment: Perfermers:Alfred Brendel                                                                           
>
> Label:Philips                                                                                                           
>
> Selections:2                                                                                                            
>
> PlayTime:57.17 minutes                                                                                                  
>
> Primary work:Piano Sonata in B flat, Fantasia in C  

头文件

```c++
class Cd { // represents a CD disk private:
    char performers[50];
    char label[20];
    int selections;    // number of selections
    double playtime; // playing time in minutes
public:
    Cd(char * s1="null", char * s2="null", int n=-1, double x=0);
    Cd(const Cd & d);
    virtual ~Cd();
    virtual void Report() const; // reports all CD data
};


class Classic:public Cd{
private:
    enum {LEN=100};
    char primary[LEN];
public:
    Classic(char* p="null",char* s1="null",char* s2="null",int n=-1,double x=0);
    Classic(char* p,const Cd& cd);
    Classic(const Classic& c);
    virtual ~Classic();
    virtual void Report() const override;
};
```



源代码文件

```c++
using std::cout;
void Cd::Report() const {
    cout<<"Perfermers:"<<performers<<'\n';
    cout<<"Label:"<<label<<'\n';
    cout<<"Selections:"<<selections<<'\n';
    cout<<"PlayTime:"<<playtime<<" minutes\n";
}

Cd::Cd(char *s1, char *s2, int n, double x) {
    strncpy(performers,s1,49);
    performers[49]='\0';
    strncpy(label,s2,19);
    label[19]='\0';
    selections=n;
    playtime=x;
}

Cd::Cd(const Cd &d) {
    strncpy(performers,d.performers,49);
    performers[49]='\0';
    strncpy(label,d.label,19);
    label[19]='\0';
    selections=d.selections;
    playtime=d.playtime;
}

Cd::~Cd() noexcept {

}

void Classic::Report() const {
    Cd::Report();
    cout<<"Primary work:"<<primary<<'\n';
}

Classic::Classic(const Classic &c):Cd(c) {
    strncpy(primary,c.primary,LEN-1);
    primary[LEN-1]='\0';
}

Classic::Classic(char *p, const Cd &cd):Cd(cd) {
    strncpy(primary,p,LEN-1);
    primary[LEN-1]='\0';
}

Classic::Classic(char *p, char *s1, char *s2, int n, double x):Cd(s1,s2,n,x) {
    strncpy(primary,p,LEN-1);
    primary[LEN-1]='\0';
}

Classic::~Classic() noexcept {
}
```



2.做编程练习1，但对两个类所追踪的各种字符串使用动态内存分配而不是固定大小的数组。

头文件

```c++
class Cd { // represents a CD disk private:
    char* performers;
    char* label;
    int selections;    // number of selections
    double playtime; // playing time in minutes
public:
    Cd(char * s1="null", char * s2="null", int n=-1, double x=0);
    Cd(const Cd & d);
    virtual Cd& operator=(const Cd& cd);
    virtual ~Cd();
    virtual void Report() const; // reports all CD data
};


class Classic:public Cd{
private:
    char* primary;
public:
    Classic(char* p="null",char* s1="null",char* s2="null",int n=-1,double x=0);
    Classic(char* p,const Cd& cd);
    Classic(const Classic& c);
    virtual ~Classic();
    virtual void Report() const override;
    virtual Classic& operator=(const Classic& c);
};
```



源代码文件

```c++
Cd &Cd::operator=(const Cd &cd) {
    if(&cd==this){
        return *this;
    }else{
        delete[] performers;
        delete[] label;
        performers=new char[sizeof(cd.performers)+1];
        label=new char[sizeof(cd.label)+1];
        strcpy(performers,cd.performers);
        strcpy(label,cd.label);
        selections=cd.selections;
        playtime=cd.playtime;
    }

}

void Cd::Report() const {
    cout<<"Perfermers:"<<performers<<'\n';
    cout<<"Label:"<<label<<'\n';
    cout<<"Selections:"<<selections<<'\n';
    cout<<"PlayTime:"<<playtime<<" minutes\n";
}

Cd::Cd(char *s1, char *s2, int n, double x) {
    performers=new char[strlen(s1)+1];
    strcpy(performers,s1);
    label = new char[strlen(s2)+1];
    strcpy(label,s2);
    selections=n;
    playtime=x;
}

Cd::Cd(const Cd &d) {
    performers=new char[sizeof(d.performers)+1];
    strcpy(performers,d.performers);
    label = new char[sizeof(d.label)+1];
    strcpy(label,d.label);
    selections=d.selections;
    playtime=d.playtime;
}

Cd::~Cd() noexcept {
    delete[] performers;
    delete[] label;
}

void Classic::Report() const {
    Cd::Report();
    cout<<"Primary work:"<<primary<<'\n';
}

Classic::Classic(const Classic &c):Cd(c) {
    primary=new char[sizeof(c.primary)+1];
    strcpy(primary,c.primary);
}

Classic::Classic(char *p, const Cd &cd):Cd(cd) {
    primary=new char[strlen(p)+1];
    strcpy(primary,p);
}

Classic::Classic(char *p, char *s1, char *s2, int n, double x):Cd(s1,s2,n,x) {
    primary=new char[strlen(p)+1];
    strcpy(primary,p);
}

Classic &Classic::operator=(const Classic &c) {
    if(&c==this){
        return *this;
    }else{
        Cd::operator=(c);
        delete[] primary;
        primary=new char[sizeof(c.primary)+1];
        strcpy(primary,c.primary);
        return *this;
    }
}

Classic::~Classic() noexcept {
    delete[] primary;
}
```



3.修改baseDMA-lacksDMA-hasDMA类的层次结构，使所有三个类都是从ABC派生的。

头文件.hpp

```c++
class base{
private:
    char* label;
    int rating;
public:
    virtual void View()=0;
    base(const char* l="null",int r=0);
    base(const base&);
    base& operator=(const base&);
    friend std::ostream &operator<<(std::ostream& os,const base&);
    virtual ~ base();
};


class baseDMA:public base{
public:
    baseDMA(const char* l="null",int r=0);
    baseDMA(const baseDMA& rs);
    baseDMA(const base& b);
    virtual ~baseDMA();
    baseDMA & operator=(const baseDMA& rs);
    friend std::ostream & operator<<(std::ostream &os,const baseDMA& rs);
    virtual void View() override;
};

class lacksDMA:public base{
private:
    enum {COL_LEN=40};
    char color[COL_LEN];
public:
    lacksDMA(const char* c="blank",const char* l="null",int r=0);
    lacksDMA(const char* c,const base& rs);
    friend std::ostream &operator<<(std::ostream& os,const lacksDMA& rs);
    virtual void View() override;
};

class hasDMA:public base{
private:
    char* style;
public:
    hasDMA(const char* s="none",const char* l="null",int r=0);
    hasDMA(const char* s,const base& rs);
    hasDMA(const hasDMA& rs);//拷贝复制函数
    ~hasDMA();
    hasDMA& operator=(const hasDMA& rs);
    friend std::ostream & operator<<(std::ostream& os,const hasDMA& rs);
    virtual void View() override;
};
```



源代码文件.cpp

```c++
void base::View() {
    cout<<"Label:"<<label<<'\n';
    cout<<"Rating:"<<rating<<'\n';
}

base::base(const base &b) {
    label=new char[sizeof(b.label)+1];
    strcpy(label,b.label);
    rating=b.rating;
}

base::base(const char *l, int r) {
    label=new char[strlen(l)+1];
    strcpy(label,l);
    rating=r;
}

base& base::operator=(const base &b) {
    if(this==&b){
        return *this;
    }else{
        delete[] label;
        label=new char[sizeof(b.label)+1];
        strcpy(label,b.label);
        rating=b.rating;
        return *this;
    }
}

std::ostream& operator<<(std::ostream &os, const base &b) {
    os<<"Label:"<<b.label<<'\n';
    os<<"Rating:"<<b.rating<<'\n';
    return os;
}

base::~base() {
    delete[] label;
}

lacksDMA::lacksDMA(const char *c, const base &rs): base(rs) {
    strncpy(color,c,COL_LEN-1);
    color[COL_LEN-1]='\0';
}

lacksDMA::lacksDMA(const char *c, const char *l, int r): base(l,r){
    strncpy(color,c,COL_LEN-1);
    color[COL_LEN-1]='\0';
}

std::ostream &operator<<(std::ostream& os,const lacksDMA& rs){
    os<<(base&)rs;
    os<<"Color:"<<rs.color<<'\n';
}


void lacksDMA::View() {
    base::View();
    cout<<"Color:"<<color<<'\n';
}

void hasDMA::View() {
    base::View();
    cout<<"Style:"<<style<<'\n';
}

hasDMA::hasDMA(const hasDMA &rs): base(rs) {
    style=new char[sizeof(rs.style)+1];
    strcpy(style,rs.style);
}

hasDMA::hasDMA(const char *s, const base &rs): base(rs) {
    style=new char[sizeof(s)+1];
    strcpy(style,s);
}

hasDMA::hasDMA(const char *s, const char *l, int r): base(l,r) {
    style=new char[sizeof(s)+1];
    strcpy(style,s);
}

hasDMA::~hasDMA() noexcept {
    delete[] style;
}

hasDMA &hasDMA::operator=(const hasDMA &rs) {
    if(this==&rs){
        return *this;
    }
    base::operator=(rs);
    if(style){
        delete[] style;
    }
    style=new char[sizeof(rs.style)+1];
    strcpy(style,rs.style);
    return *this;
}

std::ostream & operator<<(std::ostream& os,const hasDMA& rs){
    os<<(base&) rs;
    os<<"Style:"<<rs.style<<'\n';
    return os;
}

baseDMA &baseDMA::operator=(const baseDMA &rs) {
}

baseDMA::baseDMA(const baseDMA &rs):base(rs) {
}

baseDMA::baseDMA(const char *l, int r):base(l,r) {

}

baseDMA::~baseDMA() noexcept {}

std::ostream & operator<<(std::ostream &os,const baseDMA& rs){
    os<<(base&)rs;
    return os;
}

void baseDMA::View() {
    base::View();
}


baseDMA::baseDMA(const base &b): base(b) {}
```



main.cpp

```c++
int main() {
    using std::cout;
    using std::endl;
    using std::cin;
    base* pArray[POINTER];
    int type;
    string label;
    int rating;
    string style;
    string color;
    for(int i=0;i<POINTER;i++) {
        cout << "Enter number of object to be created:(1-baseDMA/2-lacksDMA/3-hasDMA):";
        if(cin >> type) {
            while (getchar() != '\n');
            switch (type) {
                case 1: {
                    cout << "Please enter the label:";
                    getline(cin, label);
                    cout << "Please enter the rating:";
                    cin >> rating;
                    while (getchar() != '\n');
                    pArray[i] = new baseDMA(label.c_str(), rating);
                }
                    break;
                case 2: {
                    cout << "Please enter the label:";
                    getline(cin, label);
                    cout << "Please enter the rating:";
                    cin >> rating;
                    while (getchar() != '\n');
                    cout << "Please enter the color:";
                    getline(cin, color);
                    pArray[i] = new lacksDMA(color.c_str(), label.c_str(), rating);
                }
                    break;
                case 3: {
                    cout << "Please enter the label:";
                    getline(cin, label);
                    cout << "Please enter the rating:";
                    cin >> rating;
                    while (getchar() != '\n');
                    cout << "Please enter the style:";
                    getline(cin, style);
                    pArray[i] = new hasDMA(style.c_str(), label.c_str(), rating);
                }
                    break;
                default:
                    break;
            }
        }
    }
    cout<<'\n';
    for(int i=0;i<POINTER;i++){
        pArray[i]->View();
        cout<<'\n';
    }

    for(int i=0;i<POINTER;i++){
        delete pArray[i];
    }
    cout<<"Done.\n";
    return 0;
}
```



4.The Benevolent Order of Programmers维护着一个瓶装的Port.为了描述它，BOP Portmaster设计了一个Port类，声明如下： 

```c++
#include <iostream> 
using namespace std; 
class Port
{
private:
    char * brand;
    char style[20]; // i.e., tawny, ruby, vintage 
    int bottles;
public:
    Port(const char * br = "none", const char * st = "none", int b = 0);
    Port(const Port & p);	// copy constructor 
    virtual ~Port() { delete [] brand; }
    Port & operator=(const Port & p);
    Port & operator+=(int b);	// adds b to bottles
    Port & operator-=(int b);	// subtracts b from bottles, if available
    int BottleCount() const { return bottles; } 
    virtual void Show() const;
	friend ostream & operator<<(ostream & os, const Port & p);
};

```



Show() 方法以下列格式显示信息：

> Brand: Gallo 
>
> Kind: tawny 
>
> Bottles: 20



operator<<()函数以如下格式显示信息（末尾没有换行符）：

> Gallo, tawny, 20



在被解除职务之前，波特大师完成了Port类的方法定义，并衍生了VintagePort类，如下所示:(这次解职是因为他把一瓶‘45年的科克伯恩酒错误地送给了一个正在准备实验性烧烤酱的人)

```c++
class VintagePort : public Port // style necessarily = "vintage"
{
private:
	char * nickname;	// i.e., "The Noble" or "Old Velvet", etc. 
    int year;	// vintage year
public:
    VintagePort();
    VintagePort(const char * br, int b, const char * nn, int y); 
    VintagePort(const VintagePort & vp);
	~VintagePort() { delete [] nickname; } 
    VintagePort & operator=(const VintagePort & vp);
    void Show() const;
    friend ostream & operator<<(ostream & os, const VintagePort & vp);
};


```



你得到的工作是完成VintagePort的工作。

a.你的第一个任务是重新创建Port方法的定义，因为前任管理员在被解职后焚烧了他的方法。

```c++
void Port::Show() const {
    cout<<"Brand:"<<brand<<'\n';
    cout<<"Kind:"<<style<<'\n';
    cout<<"Bottles:"<<bottles<<'\n';
}



Port::Port(const Port &p) {
    brand=new char[sizeof(p.brand)+1];
    strcpy(brand,p.brand);
    strncpy(style,p.style,19);
    style[19]='\0';
    bottles=p.bottles;
}

Port::Port(const char *br, const char *st, int b) {
    brand=new char[strlen(br)+1];
    strcpy(brand,br);
    strncpy(style,st,19);
    style[19]='\0';
    bottles=b;
}


Port &Port::operator=(const Port &p) {
    if(this==&p){
        return *this;
    }else{
        delete[] brand;
        brand=new char[sizeof(p.brand)+1];
        strcpy(brand,p.brand);
        strncpy(style,p.style,19);
        style[19]='\0';
        bottles=p.bottles;
        return *this;
    }
}

Port &Port::operator+=(int b) {
   bottles+=b;
   return *this;
}

Port &Port::operator-=(int b) {
    if(b<bottles){
        bottles-=b;
        return *this;
    }else{
        perror("There are not enough of them\n");
        exit(EXIT_FAILURE);
    }
}

ostream & operator<<(ostream & os, const Port & p){
    os<<p.brand<<','<<p.style<<','<<p.bottles<<'\n';
    return os;
}
```



b.你的第二个任务是解释为什么某些方法被重新定义，而其他方法没有。

因为构造函数不能被继承，析构函数不能继承，赋值函数不能被继承，且非成员函数不能被继承，这些方法需要重新定义。



c.你的第三个任务是解释为什么operator=()和operator<<()不是虚拟的。

operator=()不能被基础，所以不是虚拟的

operator<<()是非成员函数，不能被基础，所以不是虚拟的



d.你的第四项任务是为VintagePort方法提供定义。

```c++
void VintagePort::Show() const {
    Port::Show();
    cout<<"Nickname:"<<nickname<<'\n';
    cout<<"Year:"<<year<<'\n';
}

VintagePort &VintagePort::operator=(const VintagePort &vp) {
    if(this==&vp){
        return *this;
    }else{
        Port::operator=(vp);
        delete nickname;
        nickname = new char[sizeof(vp.nickname)];
        strcpy(nickname,vp.nickname);
        year=vp.year;
        return *this;
    }
}

VintagePort::VintagePort(): Port("none","vintage",0) {
    nickname= new char[10];
    strcpy(nickname,"The Noble");
    year=0;
}

VintagePort::VintagePort(const VintagePort &vp): Port(vp) {
    nickname=new char[sizeof(vp.nickname)];
    strcpy(nickname,vp.nickname);
    year=vp.year;
}

VintagePort::VintagePort(const char *br, int b, const char *nn, int y): Port(br,"vintage",b) {
    nickname=new char[sizeof(nn)];
    strcpy(nickname,nn);
    year=y;
}

ostream & operator<<(ostream & os, const VintagePort & vp){
    os<<"Nickname:"<<vp.nickname<<'\n';
    os<<"Year:"<<vp.year<<'\n';
    return os;
}
```
