# 第八章

#### 问答题

1.哪些类型的函数是内联状态的良好候选者

代码执行时间短



2.假设Song()函数有这样的原型。

```c++
void song(const char * name, int times);
```

a.你将如何修改这个原型，使次数的默认值为 是1？

```c++
void song(const char * name, int times=1);
```

b.你将在函数定义中做哪些修改？

不需要改变

c.你能为名字提供一个默认值 "O, My Papa "吗？

```c++
void song(int times=1,const char * name="O, My Papa");
```



3 . 写出iquote()的重载版本，这个函数将其参数用双引号括起来显示,写出三个版本：一个用于int参数，一个用于double参数，一个用于字符串参数。

```c++
template <typename T>
void iquote(T t){
    using std::cout;
    cout<<"\""<<t<<"\"";
}
```



4.下面是一个structure模板

```c++
struct box
{
    char maker[40]; 
    float height; 
    float width; 
    float length; 
    float volume;
};
```

a.	编写一个函数，将对box structure的引用作为其形式参数，并显示每个成员的值。

```c++
void show(const box & b);
void show(box & b){
    using std::cout;
    cout<<"Maker:"<<b.maker<<'\n';
    cout<<"Height:"<<b.height<<'\n';
    cout<<"Width:"<<b.width<<'\n';
    cout<<"Length:"<<b.length<<'\n';
    cout<<"Volume:"<<b.volume<<'\n';
}
```

b.编写一个函数，将对box structure的引用作为其形式参数，并将体积成员设置为其他三个尺寸的乘积。

```c++
void set(box &b);
void set(box &b){
    b.volume=b.width*b.height*b.length;
}
```



5.需要对listing 7.15做哪些修改，以便使函数fill()和show()使用引用参数？

```c++
// function to modify array object
void fill(std::array<double, Seasons> & pa);
// function that uses array object without modifying it
void show(const std::array<double, Seasons> &da);


void fill(std::array<double, Seasons> & pa)
{
    using namespace std;
    for (int i = 0; i < Seasons; i++)
    {
        cout << "Enter " << Snames[i] << " expenses: ";
        cin >> pa[i];
    }
}

void show(const std::array<double, Seasons>& da)
{
    using namespace std;
    double total = 0.0;
    cout << "\nEXPENSES\n";
    for (int i = 0; i < Seasons; i++)
    {
        cout << Snames[i] << ": $" << da[i] << endl;
        total += da[i];
    }
    cout << "Total Expenses: $" << total << endl;
}

```



6.以下是一些期望的效果。指出是否可以用默认参数、函数重载、两者都有或两者都没有，实现并提供适当的原型。

a. mass(density, volume) 返回具有density密度和volume体积的物体的质量，而mass(density) 返回具有density密度和1.0立方米的体积的质量，所有的量都是double。

```c++
//默认参数
double mass(double density, double volume=1);

double mass(double density, double volume){
    return density*volume;
}

//函数重载
double mass(double density, double volume);

double mass(double density, double volume){
    return density*volume;
}

double mass(double density);

double mass(double density){
    return density;
}
```



b.repeat(10, "I'm OK") 显示指定的字符串10次， repeat("But you're kind of stupid") 显示指定字符串5次。

```c++
//函数重载
void repeat(int num,char* str);
void repeat(char* str);


void repeat(int num,char* str){
    using std::cout;
    while(num--){
        cout<<str<<'\n';
    }
}
void repeat(char* str){
    int num=5;
    using std::cout;
    while(num--){
        cout<<str<<'\n';
    }
}
```



c. average(3,6) 返回两个int参数的int平均数，和 average(3.0, 6.0) 返回两个double的double平均数。

```c++
int  average(int ,int);
double average(double,double);

int  average(int x ,int y){
    return (x+y)/2;
}

double average(double x,double y){
    return (x+y)/2;
}
```



d. mangle("I'm glad to meet you") 返回字符I或指向字符串 "I'm mad to gleet you "的指针，这取决于你是将返回值分配给char变量还是char*变量。

两者都没有



7.写一个函数模板，返回其两个参数中较大的一个

```c++
template <typename T>
T larger(T x,T y){
    return x>y?x:y;
}
```



8.给出第7题的模板和第4题的box structure，提供一个模板的特殊化，该模板接受两个box的参数并返回体积较大的那个。

```c++
template <typename T>
T larger(T x,T y);

template int larger(int x,int y);//显示实例化
int main(){
   larger<int>(x,y);//显示实例化 
}



template <>
box larger(box x,box y){
    return x.volume>y.volume?x:y;//显示特殊化
}

template <>
box larger<box>(box x,box y){
    return x.volume>y.volume?x:y;//显示特殊化
}
//-----------
//结果
struct box
{
    char maker[40];
    float height;
    float width;
    float length;
    float volume;
};
template <typename T>
T larger(T x,T y);

template <> box larger(box x,box y);

template <typename T>
T larger(T x,T y){
    return x>y?x:y;
}

template<> box larger<box>(box x,box y){
    return x.volume>y.volume?x:y;
}

```



9. 在下面的代码中，v1、v2、v3、v4和v5被分配了什么类型（假设该代码是一个完整程序的一部分）?

```c++


int g(int x);

...

float m = 5.5f; 

float & rm = m; 

decltype(m) v1 = m; //float

decltype(rm) v2 = m; //& float

decltype((m)) v3 = m; // &float

decltype (g(100)) v4;  //int

decltype (2.0 * m) v5; //float
```



#### 编程题

1.编写一个函数，通常需要一个参数，即一个字符串的地址，并打印该字符串一次。但是，如果提供了第二个int类型的参数，并且非零，该函数应该打印该字符串的次数等于该函数在该点被调用的次数。(注意，字符串被打印的次数不等于第二个参数的值；它等于该函数被调用的次数）。是的，这是一个愚蠢的函数，但它让你使用本章所讨论的一些技术。在一个简单的程序中使用该函数，演示该函数如何工作。

```c++
void method(std::string* str,int num){
    if(num<0){
        return;
    }
    std::cout<<*str<<'\n';
    if(--num){
        method(str,num);
    }
}
```



2.

```c++
void Set(CandyBar& cb,char* brand,double weight,int num){
    cb.brand=brand;
    cb.weight=weight;
    cb.number=num;
}

void display(const CandyBar&);
void Set(CandyBar&,char* ="Millennium Munch",double =2.85,int = 350);


void display(const CandyBar& cb){
    using std::cout;
    cout<<"Brand Name:"<<cb.brand<<'\n';
    cout<<"Weight:"<<cb.weight<<'\n';
    cout<<"Number of Calories:"<<cb.number<<'\n';
}
void Set(CandyBar& cb,char* brand,double weight,int num){
    cb.brand=brand;
    cb.weight=weight;
    cb.number=num;
}
```



3.编写一个函数，以一个字符串对象的引用为参数，将字符串的内容转换为大写字母。使用第6章表6.4中描述的toupper()函数。编写一个使用循环的程序，允许你用不同的输入测试该函数。一个运行样本可能是这样的。

> Enter a string (q to quit): go away
> GO AWAY
> Next string (q to quit): good grief!
> GOOD GRIEF!
> Next string (q to quit): q 
>
> Bye.

```c++
void Uppercase(std::string& str);


int main() {
    using std::cin,std::cout,std::ios;
    std::string str;
    cout<<"Enter a string (q to quit):";
    while(std::getline(cin,str)&&str!="q"){
        Uppercase(str);
        cout<<str<<'\n';
        cout<<"Next string (q to quit):";
    }
    cout<<"Bye.\n";
    clock_t start = clock();
    clock_t delay = 6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}

void Uppercase(std::string& str){
    int index=str.size();
    while (index--){
        if(std::islower(str[index])){
            str[index]=str[index]-'a'+'A';
        }
    }
}
```



4.下面是一个程序的骨架。

```c++
#include <iostream> 
using namespace std;
#include <cstring>	// for strlen(), strcpy()
struct stringy {
char * str;	// points to a string
int ct;	// length of string (not counting '\0')
};

// prototypes for set(), show(), and show() go here 
int main()
{
stringy beany;
char testing[] = "Reality isn't what it used to be.";

set(beany, testing);	// first argument is a reference,
// allocates space to hold copy of testing,
// sets str member of beany to point to the
// new block, copies testing to new block,
// and sets ct member of beany
 
    show(beany);	// prints member string once 
    show(beany, 2);	// prints member string twice 
    testing[0] = 'D';
    testing[1] = 'u';
    show(testing);	// prints testing string once 
    show(testing, 3); // prints testing string thrice 
    show("Done!");
return 0;
}


```

通过提供描述的函数和原型来完成这个骨架。注意，应该有两个show()函数，每个都使用默认参数。在适当的时候使用常量参数。注意set()应该使用new来分配足够的空间来容纳指定的字符串。这里使用的技术与设计和实现类时使用的技术相似。(你可能需要改变头文件的名称，并删除using指令，这取决于你的编译器）。

```c++
struct stringy {
    char * str;	// points to a string
    int ct;	// length of string (not counting '\0')
};
void set(stringy&,const char*);
void show(const stringy&,int=1);
void show(const char*,int =1);

int main() {
    using std::cin,std::cout,std::ios;

    stringy beany;
    char testing[] = "Reality isn't what it used to be.";

    set(beany, testing);	// first argument is a reference,
// allocates space to hold copy of testing,
// sets str member of beany to point to the
// new block, copies testing to new block,
// and sets ct member of beany

    show(beany);	// prints member string once
    show(beany, 2);	// prints member string twice
    testing[0] = 'D';
    testing[1] = 'u';
    show(testing);	// prints testing string once
    show(testing, 3); // prints testing string thrice
    show("Done!");
    clock_t start = clock();
    clock_t delay = 6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}
void set(stringy& sy,const char* str){
    int len=strlen(str);
    sy.str=new char[len+1];
    for(int i=0;i<len;i++){
        sy.str[i]=str[i];
    }
    sy.ct=len;
}

void show(const char* str,int num){
    while(num--){
        std::cout<<str<<'\n';
    }
}

void show(const stringy& str,int num){
    while(num--){
        std::cout<<str.str<<'\n';
    }
}
```



5.编写一个模板函数max5()，它的参数是一个包含5个T型项的数组，并返回数组中最大的项。(由于大小是固定的，它可以被硬编码到循环中，而不是作为一个参数传递）。在一个使用该函数的程序中测试它，该函数使用了一个有五个int值的数组和一个有五个double值的数组。

```c++
template <class T>
T max5(std::array<T,5> arr);

int main() {
    using std::cin,std::cout,std::ios;
    std::array<int,5> num={1,23,4,324,23};
    std::array<double,5> farr={22.3,23.1,376.3,45.2,675.2};
    cout<<max5(num)<<'\n';
    cout<<max5(farr)<<'\n';
    clock_t start = clock();
    clock_t delay = 6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}
template <class T>
T max5(std::array<T,5> arr){
    T max=arr[0];
    for(int i=0;i<5;i++){
        if(arr[i]>max){
            max=arr[i];
        }
    }
    return max;
}
```



6.编写一个模板函数maxn()，它的参数是一个T类型的项目数组和一个代表数组中元素数量的整数，并返回数组中最大的项目。在一个使用该函数模板的程序中测试它，该程序有六个int值的数组和四个double值的数组。如果多个字符串并列拥有最长的长度，该函数应该返回并列最长的第一个字符串的地址。

```c++
template <class T>
T maxn(const T*,int);
template <>
char* maxn(char* const *,int);
int main() {
    using std::cin,std::cout,std::ios;
    int num[6]={1,23,34,26,78,5};
    double di[4]={3.23,65.3,7.23,9.5,};
    char* str[4]={"on my god","hello","why","aredf king"};
    cout<<maxn(num,6)<<'\n';
    cout<<maxn(di,4)<<'\n';
    cout<<maxn(str,4)<<'\n';

    clock_t start = clock();
    clock_t delay = 6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}
template <class T>
T maxn(const T* arr,int num){
    if(num<0)
        exit(EXIT_FAILURE);
    T max=arr[0];
    for(int i=0;i<num;i++){
        if(max<arr[i])
            max=arr[i];
    }
    return max;
}


template <>
char* maxn(char*const *arr,int num){
    char *max=arr[0];
    int len=strlen(max);
    if(num<0)
        exit(EXIT_FAILURE);
    for(int i=0;i<num;i++){
        if(len<strlen(arr[i])){
            len=strlen(arr[i]);
            max=arr[i];
        }
    }
    return max;
}
```



7.修改listing 8.14，使其使用两个名为SumArray(的模板函数来返回数组内容的总和，而不是显示内容。

```c++
struct debts{
    char name[50];
    double amount;
};


template <typename T>
T SumArray(const T arr[],int n);


template <typename T>//<class T> is backward compatible
T SumArray( T* const arr[],int n);


int main() {
    using std::cin,std::cout,std::ios;
    int things[6] = {13, 31, 103, 301, 310, 130};
    debts mr_E[3]={
            {"Ima Wolfe", 2400.0},
            {"Ura Foxe", 1300.0},
            {"Iby Stout", 1800.0}

    };

    double* pd[3];
    for (int i = 0; i < 3; i++)
        pd[i] = &mr_E[i].amount;


    cout << "Listing Mr. E's total number of things:\n";
    cout<<SumArray(things,6)<<'\n';
    cout << "Listing Mr. E's the sum of all the debts:\n";
    cout<<SumArray(pd,3)<<'\n';

    clock_t start = clock();
    clock_t delay = 6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}

template <typename T>
T SumArray(const T arr[],int n){
    T sum=arr[0];
    while(n-->1){
        sum+=arr[n];
    }
    return sum;
}
template <typename T>//<class T> is backward compatible
T SumArray(T* const arr[],int n){
    T sum=*arr[0];
    while(n-->1){
        sum+=*arr[n];
    }
    return sum;
}
```











