# 第九章答案

#### 问答题

1.对于以下情况，你会使用什么存储方案？

a. homer是一个函数的正式参数（参数）

自动存储

b.	secret变量要由两个文件共享

静态存储，外部链接

c.	topsecret变量由一个文件中的函数共享，但对其他文件隐藏

静态存储，内部链接

d. beencalled跟踪包含它的函数被调用了多少次

静态存储，无链接



2.描述using声明和using指令之间的区别。

using声明使指定的标识符可用，using指令使整个命名空间可用，using声明在限定名词前加using，using声明就好像在声明上的位置上对进行对该名字进行声明，如果该范围具有相同的名字，将会产生冲突，就不能使用using声明。using指令类似于批量使用范围解析符，using指令没有本地范围，所以会被本地同名隐藏。



3.重写下面的内容，使其不使用using声明或using指令：

```c++
#include <iostream> using namespace std; 
int main()
{
double x;
cout << "Enter value: "; 
    while (! (cin >> x) )
    {
    cout << "Bad input. Please enter a number: "; 
        cin.clear();
    while (cin.get() != '\n') 
        continue;

    }
    cout << "Value = " << x << endl; 
    return 0;
    }

```



```c++
int main()
{
    double x;
    std::cout << "Enter value: ";
    while (! (std::cin >> x) )
    {
    std::cout << "Bad input. Please enter a number: ";
    std::cin.clear();
    while (std::cin.get() != '\n')
        continue;

}
    std::cout << "Value = " << x << std::endl;
    clock_t start = clock();
    clock_t delay = 6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}
```

4.重写下面的内容，使其使用using声明而不是using指令

```c++
#include <iostream> 
using namespace std; 
int main()
{
    double x;
    cout << "Enter value: "; while (! (cin >> x) )
    {
    cout << "Bad input. Please enter a number: "; 
        cin.clear();
    while (cin.get() != '\n') 
        continue;
    }
    cout << "Value = " << x << endl; 
    return 0;
}


```



```c++
int main()
{
    using std::cout,std::cin,std::endl;
    double x;
    cout << "Enter value: "; 
    while (! (cin >> x) )
    {
        cout << "Bad input. Please enter a number: ";
        cin.clear();
        while (cin.get() != '\n')
            continue;
    }
    cout << "Value = " << x << endl;
    return 0;
}
```



5.假设你希望average(3,6)函数在一个文件中被调用时返回两个int参数的平均值，而在同一程序中的第二个文件中被调用时，你希望它返回两个int参数的双倍平均值。你可以如何设置？

在头文件中定义如下

```c++
namespace self{
    int average(int,int);
}

namespace other{
    double average(int,int);
}
```



在一个文件中声明如下：

```c++
using self::averge;
```



在第二个文件中声明如下：

```c++
using other::average;
```



6.以下两个文件的程序将显示什么？

```c++
// file1.cpp
using namespace std;
void other();
void another();
int x = 10;
int y;

int main()
{
    cout << x << endl;
    {
        int x = 4;
        cout << x << endl; 
        cout << y << endl;

    }
    other(); 
    another();
    clock_t  start = clock();
    clock_t dalay=6*CLOCKS_PER_SEC;
    while((clock()-start)<dalay);
    return 0;
}

void other()
{
    int y = 1;
    cout << "Other: " << x << ", " << y << endl;
}
```





```c++
#include <iostream>
using namespace std;
extern int x;
namespace
{
int y = -4;
}

void another()
{
    cout << "another(): " << x << ", " << y << endl;
}
```

> 10
>
> 4
>
> 0
>
> Other: 10,1
>
> another():10,-4



7.	下面的程序会显示什么？

```c++
using namespace std; 
void other(); 
namespace n1
{
    int x = 1;
}

namespace n2
{
    int x = 2;
}


int main()
{
    using namespace n1; 
    cout << x << endl;
    {

        int x = 4;
        cout << x << ", " << n1::x << ", " << n2::x << endl;
    }
    using n2::x;
    cout << x << endl; 
    other();
    return 0;
}

void other()
{
    using namespace n2; 
    cout << x << endl;
{
    int x = 4;
    cout << x << ", " << n1::x << ", " << n2::x << endl;
}
    using n2::x;
cout << x << endl;
}

```

> 1
>
> 4,1,2
>
> 2
>
> 2
>
> 4,1,2
>
> 2



#### 编程题

1、

头文件

```c++
// golf.h -- for pe9-1.cpp

const int Len = 40; 
struct golf
{
	char fullname[Len]; 
    int handicap;
};

// non-interactive version:
// function sets golf structure to provided name, handicap
// using values passed as arguments to the function 
void setgolf(golf & g, const char * name, int hc);

// interactive version:
// function solicits name and handicap from user
// and sets the members of g to the values entered
// returns 1 if name is entered, 0 if name is empty string 
int setgolf(golf & g);
 
// function resets handicap to new value 
void handicap(golf & g, int hc);

// function displays contents of golf structure 
void showgolf(const golf & g);

```



请注意，setgolf()是重载的。使用setgolf()的第一个版本会是这样的,这个函数调用提供了存储在ann结构中的信息。

```c++
golf ann；
setgolf(ann, "Ann Birdfree", 24)；
```



使用第二个版本的setgolf()会是这样的：

```c++
golf andy; 
setgolf(andy);
```

该函数将提示用户输入姓名和差点，并将它们存储在andy结构中。这个函数可以（但不需要）在内部使用第一个版本。



在这个头文件的基础上，编写一个多文件程序。一个名为golf.cpp的文件应该提供合适的函数定义以匹配头文件中的原型。第二个文件应该包含main()，并演示原型函数的所有特性。

 例如，一个循环应该征求对高尔夫结构数组的输入，并在数组已满或用户输入空字符串作为球手的名字时终止。

main.cpp

```c++
const int num=10;

int main()
{
    golf arr[num];
    int i=0;
    while(i<num){
        if(!setgolf(arr[i++]))
            break;
    }
    for(int j=0;j<i;)
        showgolf(arr[j++]);
    return 0;
}



```



golf.cpp

```c++
void setgolf(golf & g, const char * name, int hc){
    int index=0;
    while(*name!='\0'){
        g.fullname[index++]=*name++;
    }
    g.fullname[index]=='\0';
    g.handicap=hc;
}

int setgolf(golf & g){
    std::cout<<"please enter the full name of the golf:";
    //If no characters were extracted, calls setstate(failbit).
    std::cin.get(g.fullname,Len);
    while(getchar()!='\n')
        continue;
    if(g.fullname[0]=='\0')
        std::cin.clear();
    std::cout<<"please enter the handicap of the golf:";
    std::cin>>(g.handicap);
    while(getchar()!='\n')
        continue;
    if(g.fullname[0]=='\0')
        return 0;

    return 1;
}

void handicap(golf & g, int hc){
    g.handicap=hc;
}


void showgolf(const golf & g){
    std::cout<<"Name:"<<g.fullname<<'\n';
    std::cout<<"Handicap:"<<g.handicap<<'\n';
}
```



2.重做代码，用一个字符串对象代替字符数组。程序应该不再需要检查输入的字符串是否合适，它可以将输入的字符串与""比较，以检查是否有空行。





3.从下面的结构声明开始：

```c++
struct chaff
{
	char dross[20]; 
    int slag；
};
```

编写一个程序，使用place new将两个这样的结构组成的数组放入缓冲区。然后给结构成员赋值（记得对char数组使用strcpy()），并使用一个循环来显示内容。选项1是使用一个静态数组，像清单9.10中的那样，作为缓冲区。选项2是使用常规的new来分配缓冲区。





4.在以下命名空间的基础上编写一个三文件程序：

```c++
namespace SALES
{
    const int QUARTERS = 4;
    struct Sales
    {
        double sales[QUARTERS];
        double average;
        double max;
        double min;
    };
// copies the lesser of 4 or n items from the array ar
// to the sales member of s and computes and stores the
// average, maximum, and minimum values of the entered items;
// remaining elements of sales, if any, set to 0
    void setSales(Sales & s, const double ar[], int n);
// gathers sales for 4 quarters interactively, stores them
// in the sales member of s and computes and stores the
// average, maximum, and minimum values
    void setSales(Sales & s);
// display all information in structure s
    void showSales(const Sales & s);
}

```

第一个文件应该是一个包含命名空间的头文件。第二个文件应该是一个扩展命名空间的源代码文件，以提供三个原型函数的定义。第三个文件应该声明两个销售对象。它应该使用交互式版本的setSales()来为一个结构提供数值，并使用非交互式版本的setSales()来为第二个结构提供数值。它应该通过使用showSales()来显示两个结构的内容。



头文件

```c++
//
// Created by student on 2023/3/27.
//

#ifndef LEARNING_SUPPORT_H
#define LEARNING_SUPPORT_H

#endif //LEARNING_SUPPORT_H
namespace SALES
{
    const int QUARTERS = 4;
    struct Sales
    {
        double sales[QUARTERS];
        double average;
        double max;
        double min;
    };
// copies the lesser of 4 or n items from the array ar
// to the sales member of s and computes and stores the
// average, maximum, and minimum values of the entered items;
// remaining elements of sales, if any, set to 0
    void setSales(Sales & s, const double ar[], int n);
// gathers sales for 4 quarters interactively, stores them
// in the sales member of s and computes and stores the
// average, maximum, and minimum values
    void setSales(Sales & s);
// display all information in structure s
    void showSales(const Sales & s);
}
```



源代码文件

```c++
namespace SALES{
    void setSales(Sales &s, const double ar[], int n) {
        n=n<=4?n:4;
        double total=0;
        for(int i=0;i<n;i++){
            s.sales[i]=ar[i];
            if(i==0){
                s.max=s.min=ar[i];
            }
            if(ar[i]>s.max){
                s.max=ar[i];
            }
            if(ar[i]<s.min){
                s.min=ar[i];
            }
            total+=ar[i];
        }
        s.average=total/n;
    }

    void setSales(Sales & s){
        double num;
        double total=0;
        for(int i=0;i<4;i++){
            std::cout<<"Please enter the value of #"<<(i+1)<<':';
            if(std::cin>>num){
                if(i==0){
                    s.max=s.min=num;
                }
                s.sales[i]=num;
                if(s.max<num){
                    s.max=num;
                }
                if(s.min>num){
                    s.min=num;
                }
                total+=num;
            }

        }
        s.average=total/4;
    }

    void showSales(const Sales & s){
        for(int i=0;i<4;i++){
            std::cout<<"The value of sales #"<<(i+1)<<":"<<s.sales[i]<<'\n';
        }
        std::cout<<"The average of sales:"<<s.average<<'\n';
        std::cout<<"The max of sales:"<<s.max<<'\n';
        std::cout<<"The min of sales:"<<s.min<<'\n';
    }

}
```



main.cpp

```c++
int main()
{
    using SALES::Sales;
    Sales s1,s2;
    SALES::setSales(s1);
    double nums[4]{23,10.32,432.3,120.0};
    SALES::setSales(s2,nums,4);
    SALES::showSales(s1);
    SALES::showSales(s2);
    _mywait();
    return 0;
}
```