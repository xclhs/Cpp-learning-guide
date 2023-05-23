# 第十二章

#### 问答题

1.假设一个String类有以下私有成员：

```c++
class String
{
private:
    char * str;	// points to string allocated by new 
    int len;	// holds length of string
//...
};

```

a.	这个默认构造函数有什么问题？

```c++
String::String() {}
```

没有对相关成员对象进行初始化



b.这个构造函数有什么问题？

```c++
String::String(const char * s)
{
str = s;
len = strlen(s);
}
```

只是浅拷贝，会导致运行错误



c.	这个构造函数有什么问题？

```c++
String::String(const char * s)
{
    strcpy(str, s); 
    len = strlen(s);
}
```

没有分配内存就进行拷贝



2.如果你定义了一个使用new来初始化指针内存的类，请说出可能出现的三个问题。请指出如何解决这些问题。

- 没有delete分配的内存导致内存泄露，析构函数对分配的内存进行delete
- 拷贝和赋值操作只进行了浅拷贝，导致运行错误，单独实现该赋值或复制操作，进行深拷贝
- delete和new操作不一致，统一进行规定



3.如果你没有明确地提供哪些类方法，编译器会自动生成这些方法？说明这些隐式生成的函数是如何表现的。

析构函数、构造函数：不进行任何操作的空函数，当使用者没有提供时，编译器自动生成

拷贝构造函数、赋值运算函，地址运算：当程序使用到时，编译器自动生成，拷贝和赋值都进行浅复制

拷贝构造函数：将一个对象拷贝到一个新创建的对象中，值传递，值返回，有时（<span style="color: #5F9EA0">编译器生成临时对象时</span>）

赋值运算：当你把一个对象分配给另一个存在的对象时

取值运算：进行取址时



4.识别并纠正以下类声明中的错误：

```c++
class nifty
{
// data
    char personality[]; int talents;
// methods
    nifty(); 
    nifty(char * s);
    ostream & operator<<(ostream & os, nifty & n);
}

nifty:nifty()
{
personality = NULL; talents = 0;
}

nifty:nifty(char * s)
{
personality = new char [strlen(s)]; personality = s;
talents = 0;
}
 
ostream & nifty:operator<<(ostream & os, nifty & n)
{
os << n;
}

```

矫正后

头文件.hpp

```c++
class nifty
{
// data
    char* personality;
    int talents;
// methods
public:
    nifty();
    nifty(char * s);
    nifty(const nifty& n);
    nifty& operator=(const nifty& n);
    nifty& operator=(const char*s);
    ~nifty(){
        delete[] personality;
    }
    friend std::ostream& operator<<(std::ostream & os,const nifty & n) ;
};


```



源代码文件.hpp

```c++
nifty::nifty()
{
    personality = NULL;
    talents = 0;
}

nifty::nifty(char * s)
{
    talents = strlen(s);
    personality = new char[talents+1];
    strcpy(personality,s);

}

std::ostream & operator<<(std::ostream & os, const nifty & n)
{
    os<<n.personality<<'\n';
    os<<n.talents<<'\n';
    return os;
}

nifty::nifty(const nifty &n) {
    talents = n.talents;
    personality = new char[talents+1];
    strcpy(personality,n.personality);
}

nifty &nifty::operator=(const char *s) {
    if(s==personality){
        return *this;
    }else{
        if(personality)
            delete[] personality;
        talents=strlen(s);
        personality=new char(talents+1);
        strcpy(personality,s);
    }
    return *this;
}

nifty &nifty::operator=(const nifty &n) {
    if(&n== this){
        return *this;
    }else{
        if(personality)
            delete[] personality;
        talents = n.talents;
        personality = new char[talents+1];
        strcpy(personality,n.personality);
    }
    return *this;
}
```



5.考虑一下下面的类声明：

```c++
class Golfer
{
private:
    char * fullname;	// points to string containing golfer's name 
    int games;	// holds number of golf games played
    int * scores;	// points to first element of array of golf scores 
public:
    Golfer();
    Golfer(const char * name, int g= 0);
    // creates empty dynamic array of g elements if g > 0 
    Golfer(const Golfer & g);
    ~Golfer();
};
```

a.	以下每条语句会调用哪些类方法？

Golfer nancy;	// #1

调用 Golfer();



Golfer lulu(“Little Lulu”);	// #2

调用Golfer(const char * name, int g= 0);



Golfer roy(“Roy Hobbs”, 12);	// #3

调用Golfer(const char * name, int g= 0);



Golfer * par = new Golfer;	// #4

调用Golfer();



Golfer next = lulu;	// #5 

调用  Golfer(const Golfer & g);



Golfer hazzard = “Weed Thwacker”;	// #6

调用  Golfer(const Golfer & g)和Golfer(const Golfer & g);



*par = nancy;	// #7

调用赋值函数



nancy = “Nancy Putter”;	// #8

调用了Golfer(const char * name, int g= 0)和赋值函数



b.很明显，这个类需要更多的方法来使它有用。它需要什么额外的方法来防止数据损坏？

赋值运算操作



#### 编程题

1.考虑一下下面的类声明：

```c++
class Cow {
    char name[20];
    char * hobby;
    double weight;
public:
    Cow();
    Cow(const char * nm, const char * ho, double wt);
    Cow(const Cow &c);
    ~Cow();
    Cow & operator=(const Cow & c);
    void ShowCow() const; // display all cow data
};

```

提供该类的实现，并编写一个使用所有成员函数的简短程序。

源代码文件

```c++
void Cow::ShowCow() const {
    using std::cout;
    if(hobby)
    cout<<name<<":"<<hobby<<"\nweight:"<<weight<<'\n';
    else
        cout<<"none\n";
}

Cow::Cow() {
    strcpy(name,"none");
    hobby= nullptr;
    weight=0;
}

Cow::Cow(const Cow &c) {
    strcpy(name,c.name);
    hobby=new char[strlen(c.hobby)+1];
    strcpy(hobby,c.hobby);
    weight=c.weight;
}

Cow::Cow(const char *nm, const char *ho, double wt) {
    strcpy(name,nm);
    hobby=new char[strlen(ho)+1];
    strcpy(hobby,ho);
    weight=wt;
}

Cow::~Cow() {
    delete[] hobby;
}

Cow &Cow::operator=(const Cow &c) {
    if(&c== this){
        return *this;
    }else
        if(hobby)
            delete[] hobby;
    strcpy(name,c.name);
    hobby=new char[strlen(c.hobby)+1];
    strcpy(hobby, c.hobby);
    weight=c.weight;
    return *this;
}
```



main.cpp

```c++
int main(){
    Cow c("cow","writing",3);
    c.ShowCow();
    Cow b=c;
    b.ShowCow();
    Cow n;
    n.ShowCow();
    return 0;
}
```



2.通过以下方式增强String类的声明（即把string1.h升级为string2.h）：

a.重载+运算符，允许你将两个字符串连接成一个。

b.提供一个stringlow()成员函数，将一个字符串中的所有字母字符转换为小写字母。(不要忘记cctype系列的字符函数）。

c.提供一个stringup()成员函数，将字符串中的所有字母字符转换为大写字母。

d.提供一个成员函数，接收一个char参数并返回该字符在字符串中出现的次数。

在下面的程序中测试你的工作：

```c++
int main(){
    using std::cout;
    using std::cin;
    String s1(" and I am a C++ student.");
    String s2 = "Please enter your name: ";
    String s3;
    cout << s2;	// overloaded << operator
    cin >> s3;	// overloaded >> operator
    s2 = "My name is "+s3;	// overloaded =, + operators
    cout << s2 << ".\n";
    s2 = s2 + s1;
    s2.stringup();	// converts string to uppercase
    cout << "The string\n" << s2 << "\ncontains " << s2.has('A')
    << " 'A' characters in it.\n";
    s1 = "red";	// String(const char *),
// then String & operator=(const String&)
    String rgb[3] = { String(s1), String("green"), String("blue")};
    cout << "Enter the name of a primary color for mixing light: ";
    String ans;
    bool success = false;
    while (cin >> ans)

    {
        ans.stringlow();	// converts string to lowercase
        for (int i = 0; i < 3; i++)
        {
            if (ans == rgb[i]) // overloaded == operator
            {
                cout << "That's right!\n";
                success = true;
                break;
            }
        }
        if (success) break;
        else
            cout << "Try again!\n";
    }
    cout << "Bye\n";
    return 0;
}

```

你的输出应该看起来像这样的示例运行：

> Please enter your name: **Fretta Farbo**
>
> My name is Fretta Farbo. The string
>
> MY NAME IS FRETTA FARBO AND I AM A C++ STUDENT.
>
> contains 6 'A' characters in it.
>
> Enter the name of a primary color for mixing light: yellow Try again!
>
> **BLUE**
>
> That's right!
>
> Bye

源代码文件

```c++
using std::cout;
int String::num_strings=0;

String::String() {
    len=1;
    str=new char[1];
    std::strcpy(str,"");
    num_strings++;
}

String::String(const char *s) {
    len = std::strlen(s);
    str=new char[len+1];
    std::strcpy(str,s);
    num_strings++;
}

String::~String(){
    num_strings--;
    delete []str;
}

std::ostream & operator<<(std::ostream & os,
                          const String & st){
    os<<st.str;
    return os;
}

String::String(const String &s) {
    len=s.len;
    str=new char[len+1];
    str[0]='\0';
    num_strings++;
}

String &String::operator=(const char* s) {
    if(str==s){
        return *this;
    }
    len=strlen(s);
    delete[] str;
    str=new char[len+1];
    std::strcpy(str,s);
    return  *this;
}

int String::HowMany() {
    return num_strings;
}

char &String::operator[](int i) {
    return str[i];
}

const char &String::operator[](int i) const {
    return str[i];
}

bool operator<(const String &st, const String &st2){
    return std::strcmp(st.str,st2.str)<0;
}
bool operator>(const String &st1, const String &st2){
    return std::strcmp(st1.str,st2.str)>0;
}
bool operator==(const String &st, const String &st2){
    return std::strcmp(st.str,st2.str)==0;
}
std::istream& operator>>(std::istream & is, String & st){
    char temp[String::CINLIM];
    is.get(temp,String::CINLIM);
    if(is){
        st=temp;
    }
    while(is&&is.get()!='\n'){
        continue;
    }
    return is;
}


String& String::operator=(const String &s) {
    if(this==&s){
        return *this;
    }
    len=s.len;
    delete[] str;
    str=new char[len+1];
    strcpy(str,s.str);
    return *this;
}

int String::has(char ch) {
    int res=0;
    for(int i=0;i<len;i++){
        if(str[i]==ch)
            res++;
    }
    return res;
}

String &String::operator+(const String &s) {
    len = len+s.len;
    char* res=new char[len+1];
    strcpy(res,str);
    if(s.len>0){
        delete[] str;
        strcat(res,s.str);
        str=res;
    }
    return *this;
}

String &String::stringlow() {
    for(int i=0;i<len;i++){
        if(std::isupper(str[i])){
            str[i]=str[i]-'A'+'a';
        }
    }
    return *this;
}

String &String::stringup() {
    for(int i=0;i<len;i++){
        if(std::islower(str[i])){
            str[i]=str[i]-'a'+'A';
        }
    }
    return *this;
}

String operator+(const String& s1,const String&s2){
    int len;
    len = s1.len+s2.len;
    char* res=new char[len+1];
    strcpy(res,s1.str);
    if(s2.len>0){
        strcat(res,s2.str);
    }
    String s(res);
    delete[] res;
    return s;
}
```



头文件

```c++
class String
{
private:
    char * str;    // pointer to string
    int len;   // length of string
    static int num_strings;    // number of objects
    static const int CINLIM=80;//cin input limit
public:
    String(const char * s); // constructor
    String();  // default constructor
    ~String(); // destructor
    String(const String& s);
    String& operator=(const String&);
    String& operator=(const char* s);
// friend function
    friend std::ostream & operator<<(std::ostream & os,
                                     const String & st);
    int length () const { return len; }
    friend bool operator<(const String &st, const String &st2);
    friend bool operator>(const String &st1, const String &st2);
    friend bool operator==(const String &st, const String &st2);
    String& operator+(const String& s);
    friend String operator+(const String& s1,const String&s);
    String& stringlow();
    String& stringup();
    int has(char ch);
    friend std::ostream & operator<<(std::ostream & os, const String & st);
    friend std::istream& operator>>(std::istream & is, String & st);
    char & operator[](int i);
    const char & operator[](int i) const;
    static int HowMany();
};
```



3.重写stock类，使其直接使用动态分配的内存而不是使用字符串类对象来保存股票名称。同时用一个重载的operator<<()定义替换show()成员函数。测试新定义程序。

头文件

```c++
class Stock
{
private:
    char* company;
    int shares;
    double share_val;
    double total_val;
    void set_tot() { total_val = shares * share_val; }
public:
    Stock(); // default constructor
    Stock(const Stock& s);
    Stock(const char*  co, long n = 0, double pr = 0.0);
    ~Stock(); // do-nothing destructor
    void buy(long num, double price);
    void sell(long num, double price);
    void update(double price);
    const Stock & topval(const Stock & s) const;
    const char* getCompanyName() const;
    int getShares()const;
    double getShareValue()const;
    Stock& operator=(const Stock& s);
    friend std::ostream & operator<<(std::ostream& os,const Stock& s);

};
```



源代码文件

```c++
Stock::Stock() // default constructor
{
    company=new char[8];
    strcpy(company,"no name");
    shares = 0;
    share_val = 0.0;
    total_val = 0.0;
}
Stock::Stock(const char* co, long n, double pr)
{
    company = new char[strlen(co)+1];
    strcpy(company,co);
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
    delete[] company;
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



const Stock & Stock::topval(const Stock & s) const
{
    if (s.total_val > total_val)
        return s;
    else
        return *this;
}

const char* Stock::getCompanyName() const {
    return company;
}

double Stock::getShareValue() const {
    return share_val;
}

int Stock::getShares() const {
    return shares;
}

std::ostream & operator<<(std::ostream& os,const Stock& s){
    using std::cout;
    using std::ios_base;
    ios_base::fmtflags orig =
            cout.setf(ios_base::fixed, ios_base::floatfield);
    std::streamsize prec = cout.precision(3);
    cout << "Company: " << s.company
         << " Shares: " << s.shares << '\n';
    cout << " Share Price: $" << s.share_val;
// set format to #.##
    cout.precision(2);
    cout << " Total Worth: $" << s.total_val << '\n';
// restore original format
    cout.setf(orig, ios_base::floatfield);
    cout.precision(prec);
    return os;
}


Stock::Stock(const Stock &s) {
    company=new char[strlen(s.company)+1];
    strcpy(company,s.company);
    shares=s.shares;
    share_val=s.share_val;
    set_tot();
}

Stock &Stock::operator=(const Stock &s) {
    if(&s==this){
        return *this;
    }
    if(company)
        delete[] company;
    company = new char[strlen(s.company)+1];
    strcpy(company,s.company);
    shares=s.shares;
    share_val=s.share_val;
    set_tot();
    return *this;
}
```



main函数

```
int main(){
    Stock s;
    s.update(3);
    std::cout<<s<<'\n';
    Stock m("hello",3,4);
    std::cout<<m<<'\n';
    Stock f=m;
    std::cout<<f<<'\n';
    return 0;
}
```



4.考虑Stack类的以下变化：

// stack.h -- 堆栈ADT的类声明 

```c++
typedef unsigned long Item;

class Stack
{
 
private:
    enum {MAX = 10};	// constant specific to class 
    Item * pitems;	// holds stack items
	int size;	// number of elements in stack
	int top;	// index for top stack item 
public:
	Stack(int n = MAX);	// creates stack with n elements 
    Stack(const Stack & st);	
    ~Stack();
    bool isempty() const; 
    bool isfull() const;
// push() returns false if stack already is full, true otherwise 
    bool push(const Item & item); // add item to stack
// pop() returns false if stack already is empty, true otherwise 
    bool pop(Item & item); // pop top into item
    Stack & operator=(const Stack & st);
};
```

正如私有成员所暗示的，这个类使用一个动态分配的数组来保存堆栈项目,并写一个程序来演示所有的方法，包括复制构造函数和赋值运算符。

源代码文件

```c++
bool Stack::pop(Item &item) {
    if(!isempty()){
        top--;
        size--;
        item=pitems[top];
        return true;
    }else{
        return false;
    }
}

bool Stack::push(const Item &item) {
    if(!isfull()){
        pitems[top]=item;
        top++;
        size++;
        return true;
    }else{
        return false;
    }
}

bool Stack::isempty() const {
    if(size==0){
        return true;
    }else
        return false;
}

bool Stack::isfull() const {
    if(size==MAX)
        return true;
    else return false;
}

Stack::Stack(int n) {
    if(n>MAX){
        n=MAX;
    }
    pitems= new Item[MAX];
    size=0;
    top=0;
}

Stack::Stack(const Stack &st) {
    size=st.size;
    top=st.top;
    pitems=new Item[MAX];
    for(int i=0;i<size;i++){
        pitems[i]=st.pitems[i];
    }
}

Stack &Stack::operator=(const Stack &st) {
    if(this==&st){
        return *this;
    }else{
        size=st.size;
        top=st.top;
        for(int i=0;i<size;i++){
            pitems[i]=st.pitems[i];
        }
        return *this;
    }
}

Stack::~Stack() {
    delete[] pitems;
}
```



main.cpp

```c++
int main(){
    Stack h;
    h.push(3);
    h.push(4);
    Stack f=h;
    f.push(6);
    Item  i;
    while(f.pop(i))
        std::cout<<i<<" ";
    std::cout<<'\n';
    Stack m=f;
    m=h;
    while(m.pop(i))
        std::cout<<i<<" ";
    return 0;
}
```



3.希瑟银行进行了一项研究，表明自动取款机的顾客不会排队超过一分钟。使用模拟法找到一个每小时顾客人数的数值，使顾客的平均等待时间为1分钟（至少使用100小时的试验期）。

修改main函数如下，进行多次随机实验

```c++
int main(){
    using std::cin;
    using std::cout;
    using std::ios_base;
    std::srand(time(0));
    while(1){
        cout<<"Case Study:Bank of Heather Automatic Teller\n";
        cout<<"Enter maximum size of queue:";
        int qs=rand()%100+1;
        cout<<qs<<'\n';
        Queue line(qs);
        cout<<"Enter the number of simulation hours:";
        int hours;
        hours=rand()%10+100;
        cout<<hours<<'\n';
        long cyclelimit = MIN_PER_HR*hours;
        cout<<"Enter the average number of customers per hour:";
        double perhour;
        perhour=rand()%40+1;
        cout<<perhour<<'\n';
        double min_per_cust;
        min_per_cust=MIN_PER_HR/perhour;
        Item temp;
        long turnaways = 0;
        long customers = 0;
        long served = 0;
        long sum_line = 0;
        int wait_time=0;
        long line_wait=0;
        for(int cycle=0;cycle<cyclelimit;cycle++){
            if(newcustomer(min_per_cust)){
                if(line.isfull()){
                    turnaways++;
                }else{
                    customers++;
                    temp.set(cycle);
                    line.enqueue(temp);
                }
            }
            if(wait_time<=0&&!line.isempty()){
                line.dequeue(temp);
                wait_time=temp.ptime();
                line_wait+=cycle-temp.when();
                served++;
            }
            if(wait_time>0)
                wait_time--;
            sum_line+=line.queuecount();
        }

        if(customers>0){
            cout << "customers accepted: " << customers << '\n';
            cout << " customers served: " << served << '\n';
            cout << "  turnaways: " << turnaways << '\n';
            cout << "average queue size: ";
            cout.precision(2);
            cout.setf(ios_base::fixed, ios_base::floatfield);
            cout<<(double)sum_line/cyclelimit<<'\n';
            cout << " average wait time: "<< (double) line_wait / served << " minutes\n";
        }else
            cout << "No customers!\n";
        if(round((double)sum_line/cyclelimit)==1){
            break;
        }

    }
    cout << "Done!\n";
    return 0;
}
```

> Case Study : Bank of Heather Automatic Teller
>
> Enter maximum size of queue: 29
> Enter the number of simulation hours: 100
> Enter the average number of customers per hour: 18.00
>
> customers accepted: 1783
> customers served: 1783
>
> turnaways: 0
> average queue size: 0.28
> average wait time: 0.95 minutes
>
> Done!



4.希瑟银行想知道如果增加第二台自动取款机会发生什么。修改本章中的模拟，使其有两个队列。假设如果第一条队列中的人数少于第二条队列，顾客就会加入第一条队列，否则就会加入第二条队列。同样，找到一个每小时顾客数量的值，使平均等待时间为一分钟。（注意：这是一个非线性问题，因为自动取款机的数量增加一倍并不能使每小时能处理的顾客数量增加一倍，而且最多能等待一分钟。）

```c++
int main(){
    using std::cin;
    using std::cout;
    using std::ios_base;
    std::srand(time(0));
    while(1){
        cout<<"Case Study:Bank of Heather Automatic Teller\n";
        cout<<"Enter maximum size of queue:";
        int qs=rand()%100+1;
        cout<<qs<<'\n';
        Queue line1(qs);
        Queue line2(qs);
        cout<<"Enter the number of simulation hours:";
        int hours;
        hours=rand()%200+1;
        cout<<hours<<'\n';
        long cyclelimit = MIN_PER_HR*hours;
        cout<<"Enter the average number of customers per hour:";
        double perhour;
        perhour=rand()%200;
        cout<<perhour<<'\n';
        double min_per_cust;
        min_per_cust=MIN_PER_HR/perhour;
        Item temp;
        long turnaways = 0;
        long customers = 0;
        long served = 0;
        long sum_line = 0;
        int wait_time1=0;
        int wait_time2=0;
        long line_wait=0;
        for(int cycle=0;cycle<cyclelimit;cycle++){
            if(newcustomer(min_per_cust)){
                if(line1.isfull()&&line2.isfull()){
                    turnaways++;
                }else{
                    customers++;
                    temp.set(cycle);
                    if(line1.isfull()){
                        line2.enqueue(temp);
                    }else if(line2.isfull()){
                        line1.enqueue(temp);
                    }else{
                        if(line1.queuecount()<line2.queuecount()){
                            line1.enqueue(temp);
                        }else{
                            line2.enqueue(temp);
                        }
                    }
                }
            }
            if(wait_time1<=0&&!line1.isempty()){
                line1.dequeue(temp);
                wait_time1=temp.ptime();
                line_wait+=cycle-temp.when();
                served++;
            }
            if(wait_time2<=0&&!line2.isempty()){
                line2.dequeue(temp);
                wait_time2=temp.ptime();
                line_wait+=cycle-temp.when();
                served++;
            }
            if(wait_time1>0)
                wait_time1--;
            if(wait_time2>0){
                wait_time2--;
            }
            sum_line+=line1.queuecount();
            sum_line+=line2.queuecount();
        }

        if(customers>0){
            cout << "customers accepted: " << customers << '\n';
            cout << " customers served: " << served << '\n';
            cout << "  turnaways: " << turnaways << '\n';
            cout << "average queue size: ";
            cout.precision(2);
            cout.setf(ios_base::fixed, ios_base::floatfield);
            cout<<(double)sum_line/cyclelimit<<'\n';
            cout << " average wait time: "<< (double) line_wait / served << " minutes\n";
        }else
            cout << "No customers!\n";
        if(round((double)line_wait/ served)==1){
            break;
        }

    }
    cout << "Done!\n";
    return 0;
}

bool newcustomer(double x){
    return (std::rand()*x/RAND_MAX<1);
}
```

> Case Study : Bank of Heather Automatic Teller
>
> Enter maximum size of queue: 41
> Enter the number of simulation hours:108
> Enter the average number of customers per hour:52.00
>
> customers accepted: 5648
> customers served: 5648
>
> turnaways: 0
> average queue size: 0.95
> average wait time: 1.09 minutes
>
> Done!
