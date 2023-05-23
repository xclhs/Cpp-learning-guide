# 第五章练习 

#### 问答题

1.入门条件循环和出口条件循环之间有什么区别? C ++循环中是哪种?

入门：先测试，测试为真再执行[for\while]

出口：先执行，测试为假则不执行[do while]



2.如果以下代码片段是有效程序的一部分，它将打印什么？

```c++
int i;
for（i = 0; i <5; i ++）cout << i;
cout << endl;
```

打印:

01234[换行]

3.如果以下代码片段是有效程序的一部分，它将打印什么？

```c++
int j;
for (j = 0; j < 11; j += 3) cout << j;
cout << endl << j << endl;
```

打印：

0369

12



4.如果以下代码片段是有效程序的一部分，它将打印什么？

```c++
int j = 5;
while ( ++j < 9)
cout << j++ << endl;
```

打印：

6

8



5.如果以下代码片段是有效程序的一部分，它将打印什么？

```c++
int k = 8; 
do
cout <<" k = " << k << endl; 
while (k++ < 5);
```

 k= 8



6.编写一个for循环，通过在每个循环中把一个计数变量的值增加2倍来打印出1 2 4 8 16 32 64的值。

```c++
int res=1;
do{
    cout<<res<<" ";
}
while((res*=2)<=64);
```



7.如何使循环主体包含多个语句？

通过大括号包含多行语句



8.以下语句有效吗？ 如果不是，为什么不呢？ 如果是，它做什么？

```c++
int x = (1,024);
```

有效，给x赋值为20



```c++
int y;
y = 1,024;
```

有效，因为逗号运算符优先级最低，故先给y赋值为1，然后逗号运算符给该表达式返回值20



9. 如何看待cin>>ch与ch=cin.get()和cin.get(ch)输入方面有什么不同？

- cin>>ch：跳过空格等空白符
- cin.get(ch)：接收下一个输入为字符并赋值给ch,返回istream类对象，有相关成员函数对将该对象转换为bool类型
- ch=cin.get()：接收下一个输入字符返回为int字符码，遇见EOF返回EOF在iostream预定义的字符码，一般为-1



#### 编程题

1.编写一个要求用户输入两个整数的程序。然后，程序应计算并报告两个整数之间和包括两个整数之间的所有整数的总和。 在这一点上，假设首先输入较小的整数。 例如，如果用户输入2和9，则该程序应报告2到9的所有整数的总和为44。

```c++
int main() {
    using namespace std;
    int begin,end;
    cout<<"Please enter two integers:";
    cin>>begin>>end;
    int sum=0;
    while(begin<=end){
        sum+=begin;
        begin++;
    }
    cout<<"the sum of all the integers from "<<begin<<" through "<<end<<" is "<<sum;
    return 0;
}
```



2.2.	重做listing 5.4，使用数组对象而不是内置数组，使用long double而不是long long类型。找到100的值!

```c++
cout.setf(ios_base::fixed,ios_base::floatfield);
cout.precision(0);
long double factorials[ArSize];
factorials[1] = factorials[0] = 1;
for(int i = 2;i<ArSize;i++){
    factorials[i]=factorials[i-1]*i;
}
for( int i=0;i<ArSize;i++){
    std::cout << i << "! = " << factorials[i] << std::endl;
}
```

100!=93326215443944152638794559865674755492198008588591912695446327057475410739667358279337083640500903674397991236655991540282944112877360209308025154088724332544



3.编写一个程序，要求用户输入数字。每次输入后，程序应报告到目前为止输入的累计总和。当用户输入0时，程序应终止。

```c++
int main() {
    using namespace std;
    int num;
    cout<<"Please enter in numbers,and enters 0 when you want to end.\n";
    long long sum=0;
    do{
        cin>>num;
        sum+=num;
        cout<<"The cumulative sum of entries is:"<<sum<<'\n';
    }
    while(num);

    return 0;
}
```



4.4.	达芙妮以10%的单利投资了100美元。也就是说，每年都能赚到原始投资的10%，即每年10美元。

interest = 0.10 ×original balance

同时，Cleo以5%的复利投资了100美元。也就是说，利息是当前余额的5%，包括以前增加的利息。

interest = 0.05 ×current balance

Cleo在第一年赚取了100美元的5%，使她获得了105美元。

编写一个程序，找出Cleo的投资价值需要多少年才能超过Daphne的投资价值，然后显示当时这两项投资的价值。

```c++
int main() {
    using namespace std;
    double Daphne = 100;
    double Cleo=100;
    double InterestofD=100*0.10;
    int year=0;
    while((Daphne+=InterestofD)>=(year++,Cleo*=1.05));
    cout<<"It takes "<<year<<" years for value of Cleo's investment to exceed the value of Daphne's investment.\n";
    cout<<"The value of Daphne's investment is "<<Daphne<<'\n';
    cout<<"The value of Cleo's investment is "<<Cleo<<'\n';
    return 0;
}
```



5.你卖 C++ for Fools 这本书。写一个程序，让你输入一年的月销售额（根据书的数量，而不是金钱）。程序应该使用一个循环来按月提示你，使用数组 char *（或字符串对象数组，如果您愿意）初始化为月份字符串并将输入数据存储在一个 int 数组中。然后，程序应找到数组内容的总和并报告那一年的总销售额 。

```c++
int main() {
    using namespace std;
    string months[12]={"January","February","March","April","May","June",
                       "July","August","September","October","November",
                       "December"};
    int sales[month];
    int sum=0;
    for(int i=0;i<month;i++){
        cout<<"Please enter the number of book C++ for Fools sold in "<<months[i]<<" : ";
        cin>>sales[i];
        sum+=sales[i];
    }
    cout<<"The total sales of the book C++ For Fool for the year is "<<sum<<'\n';
    return 0;
}
```



6. 做编程练习 5，但使用二维数组存储 3 年月销售额的输入。 报告每个单独年份和合并年份的总销售额。

```c++
const int month=12;
const int years=3;
int main() {
    using namespace std;
    string months[12]={"January","February","March","April","May","June",
                       "July","August","September","October","November",
                       "December"};
    int sales[years][month];
    int sum[years]={0};
    for(int year=0;year<years;year++){
        switch (year) {
           case 0:
               cout<<"The first year:\n";
                break;
            case 1:
                cout<<"The second year:\n";
                break;
            case 2:
                cout<<"The third year:\n";
                break;
            default:
                break;
        }
        for(int i=0;i<month;i++){
            cout<<"Please enter the number of book C++ for Fools sold in "<<months[i]<<" : ";
            cin>>sales[year][i];
            sum[year]+=sales[year][i];
        }
    }

    cout<<"The total sales of the book C++ For Fool for the first year is "<<sum[0]<<'\n';
    cout<<"The total sales of the book C++ For Fool for the second year is "<<sum[1]<<'\n';
    cout<<"The total sales of the book C++ For Fool for the third year is "<<sum[2]<<'\n';
    cout<<"The total sales of the book C++ For Fool of all years "<<sum[1]+sum[2]+sum[0]<<'\n';

    return 0;
}
```



7.设计一个名为 car 的结构，它包含以下关于汽车的信息：它的制造商，作为字符数组或字符串对象中的字符串，以及它的制造年份，作为一个整数。编写一个程序，询问 用户有多少汽车要编目。然后程序应该使用 new 来创建一个包含那么多汽车结构的动态数组。 接下来，它应该提示用户输入每个结构的品牌（可能包含多个单词）和年份信息。 请注意，这需要小心，因为它交替读取字符串和数字数据（参见第 4 章）。 最后，它应该显示每个结构的内容。 示例运行应如下所示：

How many cars do you wish to catalog? **2**

Car #1:

Please enter the make: **Hudson Hornet** 

Please enter the year made: **1952** 

Car #2:

Please enter the make: **Kaiser** 

Please enter the year made: **1951** 

Here is your collection:

1952 Hudson Hornet

1951 Kaiser

```c++
struct car{
    char make[40];
    int built_year;



};

int main() {
    using namespace std;
    cout<<"How many cars do you wish to catalog?";
    int num=0;
    cin>>num;
    getchar();
    char* test;
     car* cars=new car[num];
     for(int i=0;i<num;i++){
         cout<<"Car #"<<i+1<<'\n';
         cout<<"Please enter the make:";
         cin.get((char*)cars[i].make,40);
         cout<<"Please enter the year made:";
         cin>>cars[i].built_year;
         getchar();
     }
     cout<<"Here is your collection:\n";

     for(int i=0;i<num;i++){
         cout<<cars[i].built_year<<" "<<cars[i].make<<'\n';
     }

    return 0;
}
```



8.编写一个程序，使用 char 数组和循环一次读取一个单词，直到输入单词 done。然后程序应报告输入的单词数（不计算完成）。 示例运行可能如下所示：

Enter words (to stop, type the word done):

**anteater birthday category dumpster **

**envy finagle geometry done for sure** 

You entered a total of 7 words.

您应该包含 cstring 头文件并使用 strcmp() 函数进行比较测试。

```c++
#include <iostream>
#include <ctime>
#include <cstring>
#include <array>

const int maxChar=1024;

int main() {
    using namespace std;
    char line[maxChar];
    int count = 0;
    bool end= false;
    cout<<"Enter words (to stop, type the word done):\n";
    while (!end&&cin.getline(line, maxChar)) {
        int size = strlen(line) + 1;
        int begin = 0;
        int space_index = 0;
        while (space_index < size) {
            while (line[space_index] != '\0'&&line[space_index] != ' ')
                space_index++;
            count++;
            line[space_index] = '\0';
            if (!strcmp(&line[begin], "done")){
                end= true;
                count-=1;
                break;
            }
            begin = ++space_index;
        }
    }

    cout<<"You entered a total of "<<count<<" words.";
    return 0;
}
```



9.编写一个与编程练习 8 中的程序描述相匹配的程序，但使用字符串类对象而不是数组。 包含字符串头文件并使用关系运算符进行比较测试。

```c++
#include <iostream>
#include <ctime>
#include <cstring>
#include <array>
#include <string>


int main() {
    using namespace std;
    string word;
    int count = 0;
    cout<<"Enter words (to stop, type the word done):\n";
    while (word!="done") {
        cin>>word;
        count++;
    }
    count--;
    cout<<"You entered a total of "<<count<<" words.";
    return 0;
}
```



10.编写一个使用嵌套循环的程序，要求用户输入一个要显示的行数的值。然后显示该行的星号，第一行有一个星号，第二行有两个，以此类推。对于每一行，星号前面都有一定数量的句号，以使所有行显示的字符总数与行数相等。示例运行如下所示：

> Enter number of rows: **5**
>
> **....***
>
> ...**
>
> ..***
>
> .****
>
> *****

```c++
int main() {
    using namespace std;
    string word;
    int num;
    cout<<"Enter number of rows:";
    cin>>num;
    for(int row=1;row<=num;row++){
        for(int col=1;col<=num;col++){
            if(num-col<row){
                cout<<"*";
            }else{
                cout<<".";
            }
        }
        cout<<'\n';
    }
    return 0;
}
```