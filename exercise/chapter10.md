# 第十章

## 问答题

1、什么是class?

类是C++在面向对象编程的一种体现，类是用户定义的一种类型，类声明指明了数据将如何存储，同提供了访问和操作这些数据的方法。

具有已下特性：

- 抽象性
- 封装和隐藏
- 多态性
- 继承性
- 代码的可复用性



2、类是如何完成抽象、封装和数据隐藏的？

抽象：只提供方法的公有接口，隐藏实现的细节

封装：将实现的细节和抽象部分 分离开即所谓的分装

数据隐藏：封装的一个实例，类的数据成员默认私有，只能通过公有接口进行间接访问私有数据。



3、对象和类之间的关系是什么？

类定义了给定类型的所有实体结构和对该结构的操作行为，对象是该给定类型的一个特定实例，其关系等同于标准类型和其变量之间的关系。



4、除了是函数之外，类的函数成员与类的数据成员有什么不同？

创建一个类的对象实例时都会有各自的空间对其数据进行存储，而类的函数存储在单独的空间并在同一类的对象之间共享。类的数据成员的范围局限在它的实例之中，而类的成员函数则可以访问所有实例中的数据。



5、定义一个类来表示一个银行账户。数据成员应该包括储户的姓名、账号（使用字符串）和余额。成员的功能应该允许以下内容：

- 创建一个对象并初始化它
- 显示存款人的姓名、账号和余额。
- 存入一个由参数给出的金额。
- 提取一个参数所给的金额

只显示类的声明，不显示方法的实现。(编程练习1为你提供了一个编写实现的机会)。

```c++
class bankAccount{
    enum{num=13};
    std::string m_name;
    double m_balance;
    unsigned long m_accountNumber;
public:

    bankAccount(std::string name="undefined",double balance=0,unsigned long an=0);
    ~bankAccount();
    void show()const;
    void deposite(double amount);
    bool withdraw(double amount);
};

```

6、什么时候调用类的构造函数？什么时候调用类的析构器？

当创建类的实例时，会自动调动类的构造函数，当类失效时会自动调用类的析构函数。



7、为复习题5中的银行账户类提供构造函数的代码。

```c++
bankAccount::bankAccount(std::string name, double balance, unsigned long an) {
    m_name=name;
    m_balance=balance;
    m_accountNumber=an;
}
```



8、什么是默认构造函数？有一个构造函数的好处是什么？

默认构造函数指的是不带任何参数的构造函数，如果你没有提供任何构造函数，则编译器会为你自动生成一个默认构造函数，构造函数的好处在于运行你再声明类的变量的时候不提供任何参数的情况下实例创建，这样有助于创建类的数组。



9、修改Stock类的定义，使其有成员函数来返回各个数据成员的值。注意：一个返回公司名称的成员不应该提供改变数组的武器。也就是说，它不能简单地返回一个字符串引用。它可以返回一个常量引用。

e.g. stock.hpp

```c++
class Stock
{
private:
    std::string company;
    int shares;
    double share_val;
    double total_val;
    void set_tot() { total_val = shares * share_val; }
public:
    Stock(); // default constructor
    Stock(const std::string & co, long n = 0, double pr = 0.0);
    ~Stock(); // do-nothing destructor
    void buy(long num, double price);
    void sell(long num, double price);
    void update(double price);
    void show()const;
    const Stock & topval(const Stock & s) const;
    const std::string getCompanyName() const;
    int getShares()const;
    double getShareValue()const;
};
```



e.g. stock.cpp

```c++
Stock::Stock() // default constructor
{
    company = "no name";
    shares = 0;
    share_val = 0.0;
    total_val = 0.0;
}
Stock::Stock(const std::string & co, long n, double pr)
{
    company = co;
    if (n < 0)
    {
        std::cout << "Number of shares can’t be negative; "
                  << company << " shares set to 0.\n";
        shares = 0;
    }
    else
        shares = n;
    share_val = pr;
    set_tot();
}
// class destructor
Stock::~Stock() // quiet class destructor
{
}
// other methods
void Stock::buy(long num, double price)
{
    if (num < 0)
    {
        std::cout << "Number of shares purchased can’t be negative. "
                  << "Transaction is aborted.\n";
    }
    else
    {
        shares += num;
        share_val = price;
        set_tot();
    }
}
void Stock::sell(long num, double price)
{
    using std::cout;
    if (num < 0)
    {
        cout << "Number of shares sold can’t be negative. "
             << "Transaction is aborted.\n";
    }
    else if (num > shares)
    {
        cout << "You can’t sell more than you have! "
             << "Transaction is aborted.\n";
    }
    else
    {
        shares -= num;
        share_val = price;
        set_tot();
    }
}
void Stock::update(double price)
{
    share_val = price;
    set_tot();
}
void Stock::show() const
{
    using std::cout;
    using std::ios_base;
// set format to #.###
    ios_base::fmtflags orig =
            cout.setf(ios_base::fixed, ios_base::floatfield);
    std::streamsize prec = cout.precision(3);
    cout << "Company: " << company
         << " Shares: " << shares << '\n';
    cout << " Share Price: $" << share_val;
// set format to #.##
    cout.precision(2);
    cout << " Total Worth: $" << total_val << '\n';
// restore original format
    cout.setf(orig, ios_base::floatfield);
    cout.precision(prec);
}
const Stock & Stock::topval(const Stock & s) const
{
    if (s.total_val > total_val)
    return s;
    else
    return *this;
}

const std::string Stock::getCompanyName() const {
    return company;
}

double Stock::getShareValue() const {
    return share_val;
}

int Stock::getShares() const {
    return shares;
}
```



10、什么是this和*this？

this是调研该成员函数的实例的地址，*this则是调用该成员函数的实例本身。



#### 编程题

1. 为问答题5中描述的类提供方法定义，并编写一个简短的程序来说明所有的功能。

main.cpp

```c++
int main() {
    //,5,10,6,9
    using namespace std;
    bankAccount ba;
    ba.withdraw(200);
    ba.deposit(200);
    ba.show();
    _mywait();
    return 0;
}
```



bank.cpp

```c++
void bankAccount::show() const {
    using std::cout;
    using std::ios_base;
    ios_base::fmtflags orig=cout.setf(ios_base::fixed,ios_base::floatfield);
    std::streamsize prec =cout.precision(3);
    cout<<"Name:"<<m_name<<'\n';
    cout<< "Account number:"<<std::setfill('0')<<std::setw(13)<<m_accountNumber<<'\n';
    cout<< "Balance:"<<m_balance<<'\n';
    cout.setf(orig);
    cout.precision(prec);
}

void bankAccount::deposit(double amount) {
    if(amount<0){
        std::cout<<"The amount of the deposit cannot be negative"<<'\n';
    }
    m_balance+=amount;
}

bool bankAccount::withdraw(double amount) {
    if(amount<0){
        std::cout<<"The amount of withdraw cannot be negative.\n";
        return false;
    }
    if(amount>m_balance){
        std::cout<<"The amount of withdraw cannot more than your balance.\n";
        return false;
    }
    m_balance-=amount;
    return true;
}

bankAccount::~bankAccount() {

}

bankAccount::bankAccount(std::string name, double balance, unsigned long an) {
    m_name=name;
    m_balance=balance;
    m_accountNumber=an;
}
```



2.	下面是一个相当简单的类定义：

```c++
class Person { 
    private:
    static const LIMIT = 25;
    string lname;	// Person’s last name char fname[LIMIT]; // Person’s first name
public:
    Person() {lname = ""; fname[0] = ‘\0’; } // #1
    Person(const string & ln, const char * fn = "Heyyou");	// #2
    // the following methods display lname and fname
    void Show() const;	// firstname lastname format 
    void FormalShow() const; // lastname, firstname format
};

```

它同时使用了一个字符串对象和一个字符数组，这样你就可以比较这两种形式的使用方法)。编写一个程序，通过提供未定义方法的代码来完成实现。你使用该类的程序也应该使用三种可能的构造函数调用（无参数、一个参数和两个参数）和两个显示方法。下面是一个使用构造函数和方法的例子：

```c++
Person one;	// use default constructor
Person two("Smythecraft");	// use #2 with one default argument Person three("Dimwiddy", "Sam"); // use #2, no defaults
one.Show(); cout << endl;
one.FormalShow();
// etc. for two and three
```



```c++
void Person::FormalShow() const {
    std::cout<<fname<<" "<<lname<<'\n';
}

void Person::Show() const {
    std::cout<<lname<<" "<<fname<<'\n';
}



Person::Person(const std::string &ln, const char *fn) {
    lname=ln;
    strcpy(fname,fn);
}
```



3.做第9章的编程练习1，但用适当的高尔夫类声明替换其中的代码。将setgolf(golf &, const char* ,int），用一个带有适当参数的构造函数来提供初始值。保留setgolf()的交互式版本，但通过使用con-structor来实现它。(例如，对于setgolf()的代码，获取数据，将数据传递给构造函数以创建一个临时对象，并将临时对象分配给调用对象，也就是*this。）

golf.hpp

```c++
class golf
{
    enum{Len=40};
    char m_fullname[Len];
    int m_handicap;
public:
    golf( const char * name, int hc);
    int setgolf(golf & g);
    void handicap(golf & g, int hc);
    void showgolf(const golf & g) const;
};
```



golf.cpp

```c++
golf::golf(const char * name, int hc){
    int index=0;
    while(*name!='\0'){
        m_fullname[index++]=*name++;
    }
    m_fullname[index]=='\0';
    m_handicap=hc;
}

int golf::setgolf(golf & g){
    std::cout<<"please enter the full name of the golf:";
    //If no characters were extracted, calls setstate(failbit).
    char name[Len];
    std::cin.get(name,Len);
    while(getchar()!='\n')
        continue;
    if(name[0]=='\0')
        std::cin.clear();
    std::cout<<"please enter the handicap of the golf:";
    int handicap;
    std::cin>>(handicap);
    *this=golf(*this,name,handicap);
    while(getchar()!='\n')
        continue;
    if(name[0]=='\0')
        return 0;

    return 1;
}

void golf::handicap(golf & g, int hc){
    g.m_handicap=hc;
}


void golf::showgolf(const golf & g) const{
    std::cout<<"Name:"<<g.m_fullname<<'\n';
    std::cout<<"Handicap:"<<g.m_handicap<<'\n';
}
```

4.做第九章的编程练习4，但要把Sales结构及其相关函数转换为一个类及其方法。用一个构造函数代替setSales(Sales &, double [], int)函数。通过使用构造函数实现交互式的setSales(Sales &)方法。将该类保持在SALES命名空间内。

e.g. 头文件

```c++
namespace SALES{
    Sales::Sales(const double ar[], int n) {
        n=n<=4?n:4;
        double total=0;
        for(int i=0;i<n;i++){
            sales[i]=ar[i];
            if(i==0){
                max=min=ar[i];
            }
            if(ar[i]>max){
                max=ar[i];
            }
            if(ar[i]<min){
                min=ar[i];
            }
            total+=ar[i];
        }
        average=total/n;
    }

    void Sales::setSales(Sales & s){
        double num;
        double total=0;
        double arr[4];
        for(int i=0;i<4;i++) {
            std::cout << "Please enter the value of #" << (i + 1) << ':';
            if (std::cin >> num) {
                arr[i] = num;
            }
        }
        s=Sales(arr,4);
    }

    void Sales::showSales(const Sales & s)const{
        for(int i=0;i<4;i++){
            std::cout<<"The value of sales #"<<(i+1)<<":"<<s.sales[i]<<'\n';
        }
        std::cout<<"The average of sales:"<<s.average<<'\n';
        std::cout<<"The max of sales:"<<s.max<<'\n';
        std::cout<<"The min of sales:"<<s.min<<'\n';
    }

}
```

e.g. 源文件

```c++
namespace SALES{
    Sales::Sales(const double ar[], int n) {
        n=n<=4?n:4;
        double total=0;
        for(int i=0;i<n;i++){
            sales[i]=ar[i];
            if(i==0){
                max=min=ar[i];
            }
            if(ar[i]>max){
                max=ar[i];
            }
            if(ar[i]<min){
                min=ar[i];
            }
            total+=ar[i];
        }
        average=total/n;
    }

    void Sales::setSales(Sales & s){
        double num;
        double total=0;
        double arr[4];
        for(int i=0;i<4;i++) {
            std::cout << "Please enter the value of #" << (i + 1) << ':';
            if (std::cin >> num) {
                arr[i] = num;
            }
        }
        s=Sales(arr,4);
    }

    void Sales::showSales(const Sales & s)const{
        for(int i=0;i<4;i++){
            std::cout<<"The value of sales #"<<(i+1)<<":"<<s.sales[i]<<'\n';
        }
        std::cout<<"The average of sales:"<<s.average<<'\n';
        std::cout<<"The max of sales:"<<s.max<<'\n';
        std::cout<<"The min of sales:"<<s.min<<'\n';
    }

}
```





5.请考虑以下结构声明：

```c++
struct customer { 
    char fullname[35]; 
    double payment；
};
```

编写一个程序，从一个堆栈中添加和删除客户结构，该堆栈由一个Stack类声明代表。每当一个客户被删除，他或她的付款应该被添加到一个运行总数中，并且运行总数应该被报告。注意：你应该可以不加修改地使用Stack类；只要改变typedef声明，使Item是customer类型而不是unsigned long。

main.cpp

```c++
int main() {
// etc. for two and three
    double total=0;
    Stack mystack;
    int ch;
    while(ch=getchar()){
        switch (ch) {
            case 'q':
            case 'Q':
                std::cout<<"Bye!\n";
                break;
            case 'a':
            case 'A':
            {
                customer temp;
                std::cout<<"Please enter the name of customer:";
                std::cin.getline(temp.fullname,35);
                while(getchar()!='\n');
                std::cout<<"Please enter the number of payment:";
                std::cin>>temp.payment;
                while(getchar()!='\n');
                mystack.push(temp);
            }
                break;
            case 'd':
            case 'D':
            {
                customer temp;
                mystack.pop(temp);
                total+=temp.payment;
                std::cout<<"Now the total of payment is:"<<total<<'\n';
            }
        }
    }
    return 0;
}
```

stack.hpp

```c++
using std::string;

struct customer {
    char fullname[35];
    double payment;
};

typedef customer Item;
class Stack
{
private:
    enum{MAX=10};
    Item items[MAX];
    int top;
public:
    Stack();
    bool isempty() const;
    bool isfull() const;
    bool push(const Item &item);
    bool pop(Item &item);
};
```

stack.cpp

```c++
Stack::Stack() {
    top=0;
}

bool Stack::pop(Item &item) {
    if(isempty()){
        return false;
    }
    item=items[--top];
    return true;
}

bool Stack::push(const Item &item) {
    if(isfull()){
        return false;
    }
    items[top++]=item;
    return true;
}

bool Stack::isempty() const {
    return top==0;
}

bool Stack::isfull() const {
    return top==MAX;
}
```



6.下面是一个类的声明：

```c++
class Move
{
private:
double x; double y;
public:
Move(double a = 0, double b = 0);	// sets x, y to a, b 
    showmove() const;	// shows current x, y values 
    Move add(const Move & m) const;
// this function adds x of m to x of invoking object to get new x,
// adds y of m to y of invoking object to get new y, creates a new
// move object initialized to new x, y values and returns it 
    reset(double a = 0, double b = 0); // resets x,y to a, b
};

```

创建成员函数定义和练习该类的程序。

main.cpp

```c++
int main() {
// etc. for two and three
    Move one(2,3);
    one.showmove();
    Move two=Move(4,4);
    two.showmove();
    Move three=one.add(two);
    three.showmove();
    return 0;
}
```

Move.cpp

```c++
Move Move::add(const Move &m) const {
    Move res;
    res.x= x+m.x;
    res.y=y+m.y;
    return res;
}

void Move::reset(double a, double b) {
    x=a;
    y=b;
}

void Move::showmove() const {
    std::cout<<"x:"<<x<<" y:"<<y<<'\n';
}

Move::Move(double a, double b) {
    x=a;
    y=b;
}
```



7.一个Betelgeusean plorg有这些属性：
**数据**

- 一个plorg有一个不超过19个字母的名字。
- 一个plorg有一个contentment index（CI），是一个整数

**操作**

- 一个新的plorg开始时有一个名字和一个50的CI。一个plorg的CI可以改变。
- 一个plorg可以报告其名称和CI。
- 默认plorg的名字是 "Plorga"。

写一个代表plorg的Plorg类声明（包括数据成员和成员函数类型），写出成员函数的定义。

plorg.hpp

```c++
class plorg{
    char m_name[20];
    int m_CI;
public:
    plorg(int CI=50,char* name="Plorga");
    void updateCI(int CI);
    void showInfo()const;
};
```



plorg.cpp

```c++
void plorg::showInfo() const {
    using std::cout;
    cout<<"The plorg's name:"<<m_name<<'\n';
    cout<<"The plorg's CI:"<<m_CI<<'\n';
}

void plorg::updateCI(int CI) {
    m_CI=CI;
}

plorg::plorg(int CI, char *name) {
    m_CI=CI;
    strcpy(m_name, name);
}
```



main.cpp

```c++
int main() {
// etc. for two and three
    plorg one;
    one.showInfo();
    one.updateCI(25);
    one.showInfo();
    return 0;
}
```



8.你可以这样描述一个简单列表：
简单列表可以容纳一些特定类型的零个或多个项目。
你可以创建一个空列表。
你可以向列表中添加项目。
你可以确定该列表是否为空。
你可以确定列表是否已满。
你可以访问列表中的每个项目，并对其执行一些操作。
正如你所看到的，这个列表真的很简单；例如，它不允许插入或删除。你应该提供一个包含类声明的list.h头文件和一个包含类方法实现的list.cpp文件。你还应该创建一个利用你的设计的简短程序。保持列表规范简单的主要原因是为了简化这个编程练习。你可以把列表实现为一个数组，或者，如果你对数据类型很熟悉，可以实现为一个链接列表。但是公共接口不应该依赖于你的选择。也就是说，公共接口不应该有数组索引、指向节点的指针，等等。它应该用创建一个列表、向列表添加一个项目等一般概念来表达。通常处理访问每个项目并形成一个动作的方法是使用一个以函数指针为参数的函数：

```c++
void visit(void (*pf)(Item &))；
```

这里pf指向一个函数（不是成员函数），该函数需要一个对Item参数的引用，其中Item是列表中项目的类型。visit()函数将这个函数应用于列表中的每个项目。

```c++
int main() {
// etc. for two and three
    myList list;
    item n=2;
    list.add(n);
    auto pow=[](item& i){i=i*i;};
    list.visit(pow);
    return 0;
}
```

list.hpp

```c++
typedef  unsigned long item;
struct Node{
    item it;
    Node* next;
    Node(item &i){
        it=i;
        next= nullptr;
    }
};

class myList{
    Node* head;
    int len;
    int num;
public:
    myList();
    void add(item i);
    bool isEmpty();
    bool isFull();
    void visit(void(*f)(item&));
};
```

list.cpp

```c++
void myList::add(item i) {
    Node* node=new Node(i);
    node->next=head;
    head=node;
}

bool myList::isEmpty() {
    return num==0;
}

bool myList::isFull() {
    return num==len;
}

myList::myList() {
    head= nullptr;
    num=0;
    len=20;
}

void myList::visit(void (*f)(item &)) {
    Node* temp=head;
    while(temp){
        f(temp->it);
        temp=temp->next;
    }
}
```
