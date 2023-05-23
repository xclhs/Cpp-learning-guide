# 第七章

#### 问答题

1. 使用一个函数的三个步骤是什么？

   函数定义、函数原型、函数调用

   

2. 构建符合下列描述的函数原型。

   a. igor()不需要参数，也没有返回值。

   `void igor();`

   b. tofu()接收一个int参数并返回一个float。

   `float tofu(int);`

   c. mpg()接收两个double参数并返回一个double

   `double mpg(double,double);`

   d. summation()接收一个long 数组的名称和一个数组大小作为值，并返回一个long。

   `long summation(long*,int)`;

   e. doctor()接收一个字符串参数（该字符串不会被修改）并返回一个double。

   `double docotr(const string);`

   f. ofcourse()接收一个boss结构作为参数，不返回任何东西。

   `void ofcourse(boss);`

   g. plot()以一个指向map结构的指针为参数，返回一个字符串。

   `string plot(map*);`

3. 编写一个函数，它需要三个参数：一个int数组的名称，数组的大小，和一个int值。让该函数将数组的每个元素设置为int值。

```c++
void funciton(int *arr,int size,int value){
    while(size-->0){
        arr[size]=value;
    }
}
```

4.	写一个函数，该函数需要三个参数：一个指向数组中某一范围的第一个元素的指针，一个指向数组中某一范围结束后的元素的指针，以及一个int值。让该函数将数组中的每个元素设置为int值。

```c++
void funciton(int *begin,int * end,int value){
    while(begin!=end){
        *begin++=value;
    }
}
```

5. 编写一个以双数组名称和数组大小为参数的函数，并返回该数组中的最大值。注意，这个函数不应该改变数组的内容。

```c++
double funciton(const double *arr,int size){
  double max=*arr;
  while(size-->0){
      if(arr[size]>max)
          max=arr[size];
  }
  return max;
}
```



6. 为什么不对属于基本类型之一的函数参数使用const限定词？

因为，基本类型使用值传递，不会影响原始的内容



7. 在C++程序中，C风格的字符串可以有哪三种形式？

- 字符串常量
- char数组
- 指向str的指针



8.写一个有此原型的函数。

`int replace(char * str, char c1, char c2)`
让该函数用c2替换字符串str中出现的每一个c1，并让该函数返回它所替换的数量。

```c++
int replace(char * str, char c1, char c2){
    int count=0;
    while(*str){
        if(*str==c1){
            *str=c2;
            count++;
        }
    }
    return count;
}
```



9.表达式`*"pizza "`是什么意思？那` "taco"[2]`呢？

"pizza"是字符串常量名的地址,即为第一个元素的地址，则`*"pizaa"=='p'`;

同理，"taco"[2]为字符'c';



10.C++使你能够按值传递结构，它也让你传递结构的地址。如果glitz是一个结构变量，你将如何按值传递它？你将如何传递它的地址？这两种方法的取舍是什么？

struct可以值传递，参数类型写glitz即可值传递，参数类型写glitz*则地址传递，两种方法的取舍是时间与空间的取舍。

11.函数judge()有一个int类型的返回值。 作为参数，它接受一个函数的地址。传递地址的函数又接受一个指向 const char 的指针作为参数并返回一个 int。编写函数原型。

```c++
int judge(int (*pf)(const char*));
```



12.假设我们有以下的结构声明。

```c++
struct applicant { 
    char name[30];
    int credit_ratings[3];
};

```



a.	编写一个以applicant 结构为参数的函数，并显示其内容。

```c++
void show_content(applicant app){
    using  std::cout;
    cout<<"\tName:"<<app.name<<'\n'
    <<"\tCredit Rating:"<<app.credit_ratings<<'\n';
}
```

b.	写一个函数，以申请人结构的地址为参数，显示指向的结构的内容

```c++
void show_content(applicant* app){
    using  std::cout;
    cout<<"\tName:"<<app->name<<'\n'
        <<"\tCredit Rating:"<<app->credit_ratings<<'\n';
}
```

13.	假设函数f1()和f2()的原型如下。

```c++
void f1(applicant * a);
const char * f2(const applicant * a1, const applicant * a2);
```

声明p1是一个指向f1的指针，p2是指向f2的指针。

```c++
void (*p1)(applicant * a)=f1;
const char *(*p2)(const applicant * a1, const applicant * a2)=f2;
```

将ap声明为与p1相同类型的五个指针数组，将pa声明为指向与p2相同类型的十个指针数组的指针。使用typedef作为辅助。

```c++
typedef     void (*p1)(applicant * a);
typedef const char * (*p2)(const applicant * a1, const applicant * a2);
    void (*p3[5])(applicant * a);
    const char * (*p2[10])(const applicant * a1, const applicant * a2);
    p1* ap[5];
    p2* pa[10];
```



#### 编程题

1. 编写一个程序，反复要求用户输入成对的数字，直到这对数字中至少有一个是0。对于每一对数字，程序应使用一个函数来计算数字的调和平均数。该函数应将答案返回给main()，main()应报告结果。数字的调和平均数是反数的平均值，可按以下方法计算。

   调和平均值=2.0 × x × y / (x + y)

   ```c++
   double harmonicMean(int ,int );
   
   int main() {
       using std::cin,std::cout;
       int num1,num2;
       cout<<"Please enter pairs of numbers:";
    while(cin>>num1>>num2){
        if(num2==0||num1==0){
            break;
        }else{
            cout<<harmonicMean(num1,num2)<<'\n';
        }
    }
   }
   double harmonicMean(int x,int y){
       return 2*x*y/(x+y);
   }
   ```



2.编写一个程序，要求用户输入最多10个高尔夫球的分数，这些分数将被储存在一个数组中。你应该提供一种方法，让用户在输入10个分数之前终止输入。用三个独立的数组处理函数来处理输入、显示和平均计算。

```c++
int max=10;
void display(const float* arr,int size);
float average(const float* arr,int size);
int  fill(float *begin,int limit);

int main() {
    using std::cin,std::cout;
    float golf_scores[max];
    int size=fill(golf_scores,max);
    float av=average(golf_scores,size);
    display(golf_scores,size);
    cout<<"\n The average of the score is "<<av;
 return 0;
}
void display(const float* arr,int size) {
    using std::cout;
    cout<<"There are all the scores:\n";
    while(size-->0){
        cout<<arr[size]<<" ";
    }

}

float average(const float* arr,int size){
    if(size==0)
        exit(EXIT_FAILURE);
    float sum=0;
    int index=0;
    while(index<size){
        sum+=arr[index++];
    }
    return sum/index;
}
int  fill(float *begin,int limit){
    int index=0;
    using std::cin,std::cout;
    float score;
    while(index<limit){
        cout<<"Please enter the score of golf(press -1 to exit):";
        while(!(cin>>score)){
            cin.clear();
            while(cin.get()!='\n');
            cout<<"Please enter the score of golf(press -1 to exit):";
        }
        if(score==-1){
            break;
        }
        begin[index++]=score;
    }
    return index;
}
```

3、下面是一个结构声明。

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

a.	写一个函数，按值传递一个box structure ，并显示每个成员的值。

```c++
void display(box b){
    using std::cout;
    cout<<"Maker:"<<b.maker<<'\n';
    cout<<"Height:"<<b.height<<'\n';
    cout<<"Width:"<<b.width<<'\n';
    cout<<"Length:"<<b.length<<'\n';
    cout<<"Volume:"<<b.volume<<'\n';
    
}
```

b.	写一个函数，传递一个box structure的地址，并将其体积成员设置为体积成员为其他三个维度的乘积。

```c++
void display(box* b){
    using std::cout;
    b->volume=b->width*b->length*b->height;
}
```

c.	写一个使用这两个函数的简单程序。

```c++
struct box
{
    char maker[40];
    float height;
    float width;
    float length;
    float volume;
};
void set(box*);
void display(box b);
int main() {
    using std::cin,std::cout;
    box b={"maker",20,15,10};
    set(&b);
    display(b);
    return 0;
}
void set(box* b){
    using std::cout;
    b->volume=b->width*b->length*b->height;
}
void display(box b){
    using std::cout;
    cout<<"Maker:"<<b.maker<<'\n';
    cout<<"Height:"<<b.height<<'\n';
    cout<<"Width:"<<b.width<<'\n';
    cout<<"Length:"<<b.length<<'\n';
    cout<<"Volume:"<<b.volume<<'\n';

}
```



4.许多州的彩票使用listing 7.4所描述的简单彩票的变体。在这些变化中，你从一组数字中选择几个数字，并称它们为字段数字。例如，你可以从1-47的领域中选择5个号码。你也可以从第二个范围（如1-27）中选择一个单一的号码（称为巨型号码或强力球等）。例如：在这里描述的例子中，获胜的概率是在47个数字中正确选取5个的概率乘以在27个数字中正确选取1个的概率的乘积。修改listing 7.4来计算这种彩票的中奖概率。

```c++
long double probability(unsigned numbers, unsigned picks);
int main() {
    using std::cin,std::cout;

    using namespace std;
    double field, choices,set;
    cout << "Enter the total number of one set and the number of picks allowed "
            "and the number of field numbers:\n";
    while ((cin >> set >> choices>>field) && choices<=set&&field>=1)
    {
        cout << "The probability of winning this kind of lottery is";
        cout << probability(set, choices)* probability(field,1); // compute the odds
        cout << "\nNext three numbers (q to quit): ";
    }
    cout << "bye\n";
 return 0;
}

long double probability(unsigned numbers, unsigned picks){
    long double result = 1.0; // here come some local variables
    long double n;
    unsigned p;
    for (n = numbers, p = picks; p > 0; n--, p--)
        result = result * p / n ;
    return result;
}
```





5.	定义一个递归函数，接收一个整数参数并返回该参数的阶乘。记得3阶乘，写成3！，等于3  2！，以此类推，0！定义为1。一般来说，如果n大于0，n！=n*（n-1）！。在一个程序中测试你的函数，该程序使用一个循环，允许用户输入各种值，程序报告阶乘。

```c++
long factorials(long order);
int main() {
    using std::cin,std::cout;
    cout<<"Please enter a natural number(press -1 to quit)";
    long num;
    while(cin>>num&&num!=-1){
        cout<<"The  the factorial of "<<num<<" is "<<factorials(num)<<'\n';
        cout<<"Please enter a natural number(press -1 to quit)";
    }
 return 0;
}

long factorials(long order){
    if(order==0){
        return 1;
    }else{
        return order*(factorials(order-1));
    }
}
```



6.编写一个使用下列函数的程序。
Fill_array()的参数是一个双值数组的名称和一个数组的大小。它提示用户输入要输入数组中的双倍值。当数组已满或用户输入非数字时，它将停止接受输入，并返回实际的条目数。
Show_array()以一个双值数组的名称和数组大小作为参数，显示数组的内容。
Reverse_array()以一个双值数组的名称和数组大小为参数，颠倒存储在数组中的值的顺序。
程序应该使用这些函数来填充一个数组，显示数组，反转数组，显示数组，反转数组中除第一个和最后一个元素外的所有元素，然后显示数组。

```c++
int Fill_array(double* arr,int size);
void Show_array(const double* arr,int size);
void Reverse_array(double *arr,int size);

int main() {
    using std::cin,std::cout;
    double test[10];
    Fill_array(test,10);
    Show_array(test,10);
    Reverse_array(test,10);
    Show_array(test,10);
    return 0;
}

int Fill_array(double* arr,int size){
    using std::cin,std::cout;
    int index=0;
    double value;
    cout<<"Please enter the a double value:(press non-numeric input to exit)";
    while(index<size&&cin>>value){
        arr[index++]=value;
        cout<<"Please enter the a double value:(press non-numeric input to exit)";
    }
    return index;
}

void Show_array(const double* arr,int size){
    using std::cout;
     int index=0;
     cout<<"\nShow the content of double array.\n";
     while(index<size)
     {
         cout<<"#"<<(index+1)<<":"<<arr[index++]<<'\n';
     }
}

void Reverse_array(double *arr,int size){
    int begin=0;
    int end=size-1;
    double temp;
    while(begin<end){
        temp=arr[begin];
        arr[begin++]=arr[end];
        arr[end--]=temp;
    }
}
```



7.重做listing 7.7，修改三个数组处理函数，使其各自使用两个指针参数来表示一个范围。fill_array()函数，不应返回实际读取的项目数，而应返回一个指向最后一个填充位置之后的指针；其他函数可以使用这个指针作为第二个参数来确定数据的结束。

```c++
const int Max=16;
double* fill_array(double* begin, double* end );
void show_array(const double* begin, const double* end);
void revalue(double r, double* begin, double * end);

int main() {
    using std::cin,std::cout;
    double properties[Max];
    double* end= fill_array(properties, properties+Max);
    show_array(properties, end);
    if (end!=properties)
    {
        cout << "Enter revaluation factor: ";
        double factor;
        while (!(cin >> factor)) // bad input
        {
            cin.clear();
            while (cin.get() != '\n')
                continue;
            cout << "Bad input; Please enter a number: ";
        }
        revalue(factor, properties, end);
        show_array(properties, end);
    }
    cout << "Done.\n";
    return 0;
}

double* fill_array(double* begin, double* end )
{
    using namespace std;
    double temp;
    int i;
    while (begin!=end)
    {
        cout << "Enter value #" << (i + 1) << ": ";
        cin >> temp;
        if (!cin) // bad input
        {
            cin.clear();
            while (cin.get() != '\n')
                continue;
            cout << "Bad input; input process terminated.\n";
            break;
        }
        else if (temp < 0) // signal to terminate
            break;
        *begin++ = temp;
    }
    return begin;
}
// the following function can use, but not alter,
// the array whose address is ar
void show_array(const double* begin, const double* end)
{
    using namespace std;
    while (begin<end)
    {
        cout << *begin << endl;
    }
}
// multiplies each element of ar[] by r
void revalue(double r, double* begin, double * end)
{
    while(begin!=end){
        *begin*=r;
        begin++;
    }

}
```



8.重做listing 7.15，不使用数组类。

做两个版本。
a.	使用一个普通的 const char *数组来表示 season的名字，并使用一个普通的 double 数组来表示费用。

```c++
void fill(double *arr);
// function that uses array object without modifying it
void show(const double* arr);

const int Seasons = 4;
const char* Snames[Seasons] =
        {"Spring", "Summer", "Fall", "Winter"};



int main() {
    using std::cin,std::cout;
    double expenses[Seasons];
    fill(expenses);
    show(expenses);
    clock_t start=clock();
    clock_t delay=6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}



void fill(double*  begin)
{
    using namespace std;
    for (int i = 0; i < Seasons; i++)
    {
        cout << "Enter " << Snames[i] << " expenses: ";
        cin >> begin[i];
    }
}
void show(const double*arr)
{
    using namespace std;
    double total = 0.0;
    cout << "\nEXPENSES\n";
    for (int i = 0; i < Seasons; i++)
    {
        cout << Snames[i] << ": $" << arr[i] << endl;
        total += arr[i];
    }
    cout << "Total Expenses: $" << total << endl;
}
```

b.	使用一个普通的const char *数组来表示海员姓名，并使用一个结构，其唯一成员是一个普通的double来表示费用。(这种设计类似于数组类的基本设计）。

```c++
struct exp{
    double expenses;
};
const int Seasons = 4;
const char* Snames[Seasons] =
        {"Spring", "Summer", "Fall", "Winter"};

// function to modify array object
void fill(struct exp *arr);
// function that uses array object without modifying it
void show(const struct exp* arr);



int main() {
    using std::cin,std::cout;
    struct exp expenses[Seasons];
    fill(expenses);
    show(expenses);
    clock_t start=clock();
    clock_t delay=6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}



void fill(struct exp *arr)
{
    using namespace std;
    for (int i = 0; i < Seasons; i++)
    {
        cout << "Enter " << Snames[i] << " expenses: ";
        cin >> arr[i].expenses;
    }
}
void show(const struct exp *arr)
{
    using namespace std;
    double total = 0.0;
    cout << "\nEXPENSES\n";
    for (int i = 0; i < Seasons; i++)
    {
        cout << Snames[i] << ": $" << arr[i].expenses << endl;
        total += arr[i].expenses;
    }
    cout << "Total Expenses: $" << total << endl;
}
```



9.这个练习提供了编写处理数组和结构的函数的练习。下面是一个程序的骨架。通过提供所描述的函数来完成它。

```c++
#include <iostream> using namespace std;
 
const int SLEN = 30; 
struct student {
    char fullname[SLEN]; 
    char hobby[SLEN]; 
    int ooplevel;
};
// getinfo() has two arguments: a pointer to the first element of
// an array of student structures and an int representing the
// number of elements of the array. The function solicits and
// stores data about students. It terminates input upon filling
// the array or upon encountering a blank line for the student
// name. The function returns the actual number of array elements
// filled.
int getinfo(student pa[], int n);

// display1() takes a student structure as an argument
// and displays its contents 
void display1(student st);
// display2() takes the address of student structure as an
// argument and displays the structure’s contents 
void display2(const student * ps);

// display3() takes the address of the first element of an array
// of student structures and the number of array elements as
// arguments and displays the contents of the structures 
void display3(const student pa[], int n);

int main()
{
cout << “Enter class size: “; int class_size;
cin >> class_size;
while (cin.get() != '\n’) continue;

student * ptr_stu = new student[class_size]; int entered = getinfo(ptr_stu, class_size); for (int i = 0; i < entered; i++)
{
display1(ptr_stu[i]); display2(&ptr_stu[i]);
}
display3(ptr_stu, entered); delete [] ptr_stu;
cout << “Done\n”; return 0;
}

```



```c++
const int SLEN = 30;
struct student {
    char fullname[SLEN];
    char hobby[SLEN];
    int ooplevel;
};
void display1(student st);
void display2(const student * ps);
void display3(const student pa[], int n);
int getinfo(student pa[], int n);
int main() {
    using std::cin,std::cout;
    cout << "Enter class size: ";
    int class_size;
    cin >> class_size;
    while (cin.get() != '\n')
        continue;
    student * ptr_stu = new student[class_size];
    int entered = getinfo(ptr_stu, class_size);

    for (int i = 0; i < entered; i++)
    {
        display1(ptr_stu[i]);
        display2(&ptr_stu[i]);
    }
    display3(ptr_stu, entered);
    delete [] ptr_stu;
    cout << "Done\n";
    return 0;
}

void display1(student st){
    using std::cout;
    cout<<"fullname:"<<st.fullname<<'\n'
    <<"hobby:"<<st.hobby<<'\n'
    <<"ooplevel:"<<st.ooplevel<<'\n';
}

void display2(const student * ps){
    using std::cout;
    cout<<"fullname:"<<ps->fullname<<'\n'
        <<"hobby:"<<ps->hobby<<'\n'
        <<"ooplevel:"<<ps->ooplevel<<'\n';
}

void display3(const student pa[], int n){
    int index=0;
    while(index<n){
        display1(pa[index++]);
    }
}

int getinfo(student pa[], int n){
    using std::cout,std::cin;
    int index=0;
    cout<<"Press enter at the fullname to exit\n";
    while(index<n){
        cout<<"Please enter the fullname:";
        cin.getline(pa[index].fullname,SLEN);
        if(pa[index].fullname==std::string("\n"))
            break;
        cout<<"Please enter the hobby:";
        cin.getline(pa[index].hobby,SLEN);
        cout<<"Please enter the ooplevel:";
        cin>>pa[index].ooplevel;
        cin.get();
        index++;
    }
    return index;
}
```



10.设计一个函数 calculate() 接受两个双精度值和一个指向接受两个双精度参数并返回双精度的函数的指针。计算() 函数也应该是双精度类型，并且它应该返回指向的值 函数计算，使用 calculate() 的双参数。 例如，假设您对 add() 函数有以下定义：

```c++
double add(double x, double y)
{
return x + y;
}
```

然后，下面的函数调用会导致 calculate() 将值 2.5 和 10.4 传递给 add() 函数，然后返回 add() 的返回值 (12.9)：
```c++
double q = calculate(2.5, 10.4, add);
```

在程序的 add() 模型中使用这些函数和至少一个附加函数。程序应该使用允许用户输入一对数字的循环。 对于每一对，使用 calculate() 来调用 add() 和至少一个其他函数。 如果您喜欢冒险，请尝试创建一个指向 add() 样式函数的指针数组，并使用循环通过使用这些指针将 calculate() 连续应用于一系列函数。 提示：下面是声明这样一个三指针数组的方法：

```c++
double (*pf[3])(double, double);
```

您可以使用通常的数组初始化语法和函数名称作为地址来初始化这样的数组。

```c++
double add(double x, double y);
double calculate(double v1,double v2,double (*pf)(double v1,double v2));
double sub(double x,double y);
double mul(double x,double y);
double div(double x,double y);
int main() {
    using std::cin,std::cout;
    double (*ap[4])(double,double)={add,sub,mul,div};
    int n=4;
    double num1,num2;
    while(n-->0){
        cout<<"Please enter two double:";
        cin>>num1>>num2;
        cout<<"The calculate result is:"<<ap[n](num1,num2)<<'\n';
    }
    clock_t start=clock();
    clock_t delay=6*CLOCKS_PER_SEC;
    while((clock()-start)<delay);
    return 0;
}
double calculate(double v1,double v2,double (*pf)(double v1,double v2)){
    return pf(v1,v2);
}



double add(double x, double y)
{
    return x + y;
}
double sub(double x,double y){
    return x-y;
}
double mul(double x,double y){
    return x*y;
}
double div(double x,double y){
    return x/y;
}
```

















