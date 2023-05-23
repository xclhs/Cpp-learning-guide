# 练习题

## 问答题

1.使用一个成员函数来重载Stonewt class的乘法运算符，让运算符将数据成员乘以一个类型的double值。
注意，这将需要结转石磅的表示。也就是说，10石8磅的两倍是21石2磅。

```c++
Stonewt operator *(double ) const;
Stonewt Stonewt::operator*(double d) const {
    return Stonewt(d*pounds);
}
```

2.友元函数和成员函数之间有什么区别？

友元函数非成员函数，可以被非成员对象调用，也属于该类的公共接口，可访问私有数据和调用私有成员函数。



3.非成员函数是否必须是友元才能访问一个类的成员？

必须是友元才能访问私有成员，即使不是友元也可以访问公有成员。



4.使用一个友元函数来重载Stonewt类的乘法运算符，让该运算符将double乘以Stone值。

```c++
friend Stonewt operator*(double ,Stonewt);
Stonewt operator*(double d,Stonewt s){
    return Stonewt(d*s.pounds);
}
```



5.哪些运算符不能被重载？

|      运算符      |        描述        |
| :--------------: | :----------------: |
|      sizeof      |   操作符 sizeof    |
|        .         |     成员操作符     |
|        .*        | 指针到成员的操作符 |
|        ::        |  范围分辨率运算符  |
|        ?:        |     条件运算符     |
|      typeid      |   一个RTTI操作符   |
|    const_cast    | 一个类型转换操作符 |
|   dynamic_cast   | 一个类型转换操作符 |
| reinterpret_cast | 一个类型转换操作符 |
|   static_cast    | 一个类型转换操作符 |



6.下列运算符的重载有什么限制？ =, (), [], and ->

只能以成员函数的方式进行重载



7.为Vector类定义一个转换函数，将Vector对象转换为代表该向量大小的double数值。

```c++
 operator double() const;
 Vector::operator double() const {
        return mag;
    }
```





## 编程题

1.修改代码，使其将随机行走者的连续位置写入一个文件。用步骤编号标记每个位置。同时，让程序将初始条件（目标距离和步长）和汇总结果写入文件。

文件内容可能如下：

> Target Distance: 100, Step Size: 20
> 0: (x,y) = (0, 0)
> 1: (x,y) = (-11.4715, 16.383)
> 2: (x,y) = (-8.68807, -3.42232)
> ...
> 26: (x,y) = (42.2919, -78.2594)
> 27: (x,y) = (58.6749, -89.7309)
> After 27 steps, the subject has the following location: (x,y) = (58.6749, -89.7309)
> or
> (m,a) = (107.212, -56.8194)
> Average outward distance per step = 3.97081

```c++
int main() {
    using std::cout;
    using std::endl;
    using std::cin;
    using VECTOR::Vector;
    srand(time(0));
    double direction;
    Vector step;
    Vector result(0,0);
    unsigned long steps=0;
    double target;
    double dstep;
    std::fstream fout;
    fout.open("try.txt");
    if(!fout.is_open()){
        exit(EXIT_FAILURE);
    }
    cout << "Enter target distance (q to quit): ";
    int num=0;
    while (cin >> target)
    {
        cout << "Enter step length: ";
        if (!(cin >> dstep))
            break;
        fout<<"Target Distance:"<<target<<",Step Size:"<<dstep<<'\n';
        while (result.magval() < target)
        {
            direction = rand() % 360;
            step.reset(dstep, direction, Vector::POL);
            result = result + step;
            steps++;
            fout<<num<<":(x,y)=("<<result.xval()<<","<<result.yval()<<")\n";
            num++;
        }
        fout << "After " << steps << " steps, the subject " "has the following location:\n";
        fout << result << endl;
        result.polar_mode();
        fout << " or\n" << result << endl;
        fout << "Average outward distance per step = "
             << result.magval()/steps << endl;
        steps = 0;
        result.reset(0.0, 0.0);
        cout << "Enter target distance (q to quit): ";
    }
    cout << "Bye!\n";
    cin.clear();
    while (cin.get() != '\n')
        continue;
    return 0;
}
```



2.修改Vector类的头文件和实现文件，使幅度和角度不再作为数据组件存储。你应该保持公共接口不变（相同的公共方法和相同的参数），但改变私有部分，包括一些私有方法和方法的实现。测试修改后的版本，因为Vector类的公共接口没有改变，所以应该保持不变。

vector.cpp

```c++

namespace VECTOR {
    using std::cout;
    const double PI_2_Deg = 180 / 3.14159267;
    void Vector::reset(double n1, double n2, VECTOR::Vector::Mode form) {
        mode = form;
        if (form == RECT) {
            y = n1;
            x = n2;
        } else if (form == POL) {
//            mag = n1;
//            ang = n2 / PI_2_Deg;
            set_y(n1,n2/PI_2_Deg);
            set_x(n1,n2/PI_2_Deg);
        } else {
            cout << "Incorrect 3rd argument to Vector() -- ";
            cout << "vector set to 0\n";
            x = y = 0.0;
            mode = RECT;
        }
    }


    void Vector::polar_mode() {
        mode = POL;
    }

    void Vector::rect_mode() {
        mode = RECT;
    }

/*    void Vector::set_ang() {
        if (x == 0 && y == 0) {
            ang = 0;
        } else {
            ang = atan2(y, x);
        }
    }

    void Vector::set_mag() {
        mag = sqrt(x * x + y * y);
    }*/

    void Vector::set_x(double mag,double ang) {
        x = mag * cos(ang);
    }

    void Vector::set_y(double mag,double ang) {
        y = mag * sin(ang);
    }

    Vector::Vector() {
        x = y = 0;
        mode = RECT;
    }

    Vector::Vector(double n1, double n2, VECTOR::Vector::Mode form) {
        reset(n1, n2, form);
    }

    Vector::~Vector() {

    }

    Vector Vector::operator-() const {
        return Vector(-x, -y);
    }

    Vector Vector::operator-(const VECTOR::Vector &b) const {
        return Vector(x - b.x, y - b.y);
    }

    Vector Vector::operator*(double n) const {
        return Vector(x * n, y * n);
    }

    Vector Vector::operator+(const VECTOR::Vector &b) const {
        return Vector(x + b.x, y + b.y);
    }

    Vector operator*(double n, const Vector &a) {
        return a * n;
    }

    std::ostream &operator<<(std::ostream &os, const Vector &v) {
        if (v.mode == Vector::RECT)
            os << "(x,y) = (" << v.x << ", " << v.y << ")";
        else if (v.mode == Vector::POL) {
            os << "(m,a) = (" << v.magval() << ", "
               << v.angval()* PI_2_Deg << ")";
        } else
            os << "Vector object mode is invalid";
        return os;
    }

    Vector::operator double() const {
        return magval();
    }

    double Vector::magval() const {
        return sqrt(x * x + y * y);
    }

    double Vector::angval() const {
        return atan2(y,x);
    }

}

```



3.使其不再报告某一特定目标/步骤组合的单一试验结果，而是报告N次试验的最高、最低和平均步骤数，其中N是用户输入的整数。

```c++
int main() {
    using std::cout;
    using std::endl;
    using std::cin;
    using VECTOR::Vector;
    srand(time(0));
    double direction;
    Vector step;
    Vector result(0,0);
    unsigned long steps=0;
    unsigned long total=0,high=0,low=LONG_MAX;
    double target;
    double dstep;
    cout << "Enter target distance (q to quit): ";
    int num=0,trial;
    while (cin >> target)
    {
        cout << "Enter step length: ";
        if (!(cin >> dstep))
            break;
        cout<<"Enter the number of trials:";
        if(!(cin>>trial))
            break;

        for(int i=0;i<trial;i++){
            fout<<"Target Distance:"<<target<<",Step Size:"<<dstep<<'\n';
            while (result.magval() < target)
            {
                direction = rand() % 360;
                step.reset(dstep, direction, Vector::POL);
                result = result + step;
                steps++;
                num++;
            }
            if(low>steps){
                low=steps;
            }
            if(high<steps){
                high=steps;
            }
            total+=steps;
            steps = 0;
            result.reset(0.0, 0.0);
        }
        cout<<"The highest number of steps for N trials is "<<high<<'\n';
        cout<<"The average number of steps for N trials is "<<int(total/trial)<<'\n';
        cout<<"The lowest number of steps for N trials is "<<low<<'\n';
        result.reset(0.0, 0.0);
        total=0,high=0,low=ULONG_MAX;
        cout << "Enter target distance (q to quit): ";
    }
    cout << "Bye!\n";
    cin.clear();
    while (cin.get() != '\n')
        continue;
    return 0;
}
```

4.重写最后的Time类例子，使所有的重载操作符都用友方函数实现。

time.hpp

```c++
class Time{
    int hours;
    int minutes;
public:
    Time();
    Time(int h,int m=0);
    void AddMin(int m);
    void AddHr(int h);
    void Reset(int h=0,int m=0);
/*    Time operator+(const Time&t) const;
    Time operator*(double mult) const;
    Time operator-(const Time&t) const;*/
    friend Time operator+(const Time &t1,const Time &t2);
    friend Time operator-(const Time &t1,const Time &t2);
    friend Time operator*(const Time &t1,double mult);

    friend std::ostream& operator<<(std::ostream& os,const Time& t);
    friend  Time operator*(double mult,const Time &t){
        return t*mult;
    }

};
```



time.cpp

```c++
void Time::AddHr(int h) {
    hours+=h;
}

void Time::AddMin(int m) {
    minutes+=m;
    hours+=minutes/60;
    minutes%=60;
}

void Time::Reset(int h, int m) {
    hours=h;
    minutes=m;
}

/*Time Time::operator+(const Time &t) const{
    Time sum;
    sum.minutes=t.minutes+minutes;
    sum.hours=t.hours+hours+sum.minutes/60;
    sum.minutes%=60;
    return sum;
}*/

Time::Time() {
    hours=minutes=0;
}

Time::Time(int h, int m) {
    hours=h;
    minutes=m;
}

/*
Time Time::operator*(double mult) const{
    Time result;
    long totalminutes=hours*mult*60+minutes*mult;
    result.minutes=totalminutes%60;
    result.hours=totalminutes/60;
    return result;
}

Time Time::operator-(const Time &t) const {
    Time diff;
    int tot1,tot2;
    tot1=hours*60+minutes;
    tot2=t.hours*60+t.minutes;
    diff.minutes=(tot1-tot2)%60;
    diff.hours=(tot1-tot2)/60;
    return diff;
}
*/


std::ostream& operator<<(std::ostream& os,const Time& t){
    os<<t.hours << " hours, " << t.minutes << " minutes";
    return os;
}

Time operator+(const Time &t1,const Time &t2){
    Time sum;
    sum.minutes=t1.minutes+t2.minutes;
    sum.hours=t1.hours+t2.hours+sum.minutes/60;
    sum.minutes%=60;
    return sum;
}

Time operator-(const Time &t1,const Time &t2){
    Time sub;
    sub.minutes=t1.minutes+t1.hours*60-t2.minutes-t2.hours*60;
    sub.hours=sub.minutes/60;
    sub.minutes%=60;
    return sub;
}

Time operator*(const Time &t1,double mult){
    Time mul;
    mul.minutes=t1.minutes*mult+t1.hours*60*mult;
    mul.hours=mul.minutes/60;
    mul.minutes%=60;
    return mul;
}
```



5.重写Stonewt类，使其有一个状态成员，控制对象是以石块形式、整数磅形式还是浮点数磅形式解释。重载<<操作符来代替show_stn()和show_lbs()方法。重载加法、减法和乘法运算符，这样就可以对Stonewt值进行加、减和乘法运算。用一个简短的程序来测试你的类，该程序使用了所有的类方法和好友。

main.cpp

```c++
int main() {
    using std::cout;
    using std::endl;
    using std::cin;
    Stonewt st(100);
    cout<<st<<'\n';
    st=3;
    st = st+st;
    cout<<st<<'\n';
    cout<<(st+st)<<'\n';
    cout<<st*3<<'\n';
    return 0;
}
```

Stonewt.hpp

```c++
class Stonewt{
private:
    enum{Lbs_per_stn = 14};
    enum State{STONE,IPOUND,FPOUND};
    int stone;
    double pds_left;
    double pounds;//entire weight in pounds;
    State state;


public:
    Stonewt(double lbs,State s=STONE) ;
    Stonewt(int stn,double lbs,State s=STONE);
    Stonewt();
    ~Stonewt();
    Stonewt operator+(const Stonewt &st) const;
    Stonewt operator-(const Stonewt &st) const;
    Stonewt operator*(double value) const;
    explicit operator double() const;
    explicit operator int() const;
    friend std::ostream& operator<<(std::ostream &os, const Stonewt&st);
    void setState(State s);
};
```

Stonewt.cpp

```c++
Stonewt::Stonewt() {
    pds_left=pounds=pds_left=0;
    state=STONE;
}

Stonewt::Stonewt(double lbs,State s) {
    pounds=lbs;
    stone=lbs/Lbs_per_stn;
    pds_left=lbs-Lbs_per_stn*stone;
    state=s;
}


Stonewt::Stonewt(int stn, double lbs,State s) {
    stone=stn;
    pds_left=lbs;
    pounds=lbs+Lbs_per_stn*stn;
    state=s;
}

Stonewt::~Stonewt() {

}

Stonewt::operator double() const{
    return pounds;
}

Stonewt::operator int() const{
    return int(pounds+0.5);
}

std::ostream& operator<<(std::ostream &os,const Stonewt&st){
    switch (st.state) {
        case Stonewt::STONE:
            cout<<"Stone:"<<st.stone<<'\n';
            break;
        case Stonewt::IPOUND:
            cout<<"Interger pounds:"<<int(st)<<'\n';
            break;
        case Stonewt::FPOUND:
            cout<<"Floating-point Pounds:"<<double(st)<<'\n';
            break;
    }
    return os;
}

Stonewt Stonewt::operator+(const Stonewt &st) const {
    return Stonewt{pounds+st.pounds};
}

Stonewt Stonewt::operator-(const Stonewt &st) const {
    return Stonewt{pounds-st.pounds};
}

Stonewt Stonewt::operator*(const double value) const {
    return Stonewt{pounds*value};
}

void Stonewt::setState(State s){
    state=s;
}
```



6.重写Stonewt类(Listings 11.16和11.17)，使其重载所有六个关系运算符。这些运算符应该比较pounds成员并返回一个bool类型的值。编写一个程序，声明一个包含六个Stonewt对象的数组，并在数组声明中初始化前三个对象。然后，它应该使用循环来读取用于设置其余三个数组元素的值。接下来，它应该报告最小的元素、最大的元素以及有多少元素大于或等于11英石。(最简单的方法是创建一个初始化为11英石的Stonewt对象，并将其他对象与该对象进行比较。)

main.cpp

```c++
int main() {
    using std::cout;
    using std::endl;
    using std::cin;
    std::array<Stonewt,6> arr{3,400,Stonewt(3,4)};
    double pounds;
    Stonewt basic(11,0);
    for(int i=0;i<3;i++){
        cout<<"Please enter the number of pounds of #"<<i+4<<":";
        cin>>pounds;
        arr[i+3]=pounds;
    }
    double smallest=INT_MAX,largest=INT_MIN;
    int num=0;
    for(int i=0;i<6;i++){
        if(arr[i]>largest){
            largest=double(arr[i]);
        }

        if(arr[i]<smallest){
            smallest=double(arr[i]);
        }

        if(arr[i]>basic){
            num++;
        }
    }
    cout<<"The largest element is "<<largest<<".\nThe smallest element is "<<smallest
    <<".\nThere are "<<num<<" elements greater or equal to 11 stone.";
    return 0;
}
```



Stonewt.hpp

```c++
class Stonewt{
private:
    enum{Lbs_per_stn = 14};
    enum State{STONE,IPOUND,FPOUND};
    int stone;
    double pds_left;
    double pounds;//entire weight in pounds;
    State state;


public:
    Stonewt(double lbs,State s=STONE) ;
    Stonewt(int stn,double lbs,State s=STONE);
    Stonewt();
    ~Stonewt();
    Stonewt operator+(const Stonewt &st) const;
    Stonewt operator-(const Stonewt &st) const;
    Stonewt operator*(double value) const;
    bool operator<(const Stonewt &st)const;
    bool operator>(const Stonewt &st)const;
    bool operator>=(const Stonewt &st)const;
    bool operator<=(const Stonewt &st)const;
    bool operator==(const Stonewt &st)const;
    bool operator!=(const Stonewt &st)const;
    explicit operator double() const;
    explicit operator int() const;
    friend std::ostream& operator<<(std::ostream &os, const Stonewt&st);
    void setState(State s);
};
```



Stonewt.cpp

```c++
Stonewt::Stonewt() {
    pds_left=pounds=pds_left=0;
    state=STONE;
}

Stonewt::Stonewt(double lbs,State s) {
    pounds=lbs;
    stone=lbs/Lbs_per_stn;
    pds_left=lbs-Lbs_per_stn*stone;
    state=s;
}


Stonewt::Stonewt(int stn, double lbs,State s) {
    stone=stn;
    pds_left=lbs;
    pounds=lbs+Lbs_per_stn*stn;
    state=s;
}

Stonewt::~Stonewt() {

}

Stonewt::operator double() const{
    return pounds;
}

Stonewt::operator int() const{
    return int(pounds+0.5);
}

std::ostream& operator<<(std::ostream &os,const Stonewt&st){
    switch (st.state) {
        case Stonewt::STONE:
            cout<<"Stone:"<<st.stone<<'\n';
            break;
        case Stonewt::IPOUND:
            cout<<"Interger pounds:"<<int(st)<<'\n';
            break;
        case Stonewt::FPOUND:
            cout<<"Floating-point Pounds:"<<double(st)<<'\n';
            break;
    }
    return os;
}

Stonewt Stonewt::operator+(const Stonewt &st) const {
    return Stonewt{pounds+st.pounds};
}

Stonewt Stonewt::operator-(const Stonewt &st) const {
    return Stonewt{pounds-st.pounds};
}

Stonewt Stonewt::operator*(const double value) const {
    return Stonewt{pounds*value};
}

void Stonewt::setState(State s){
    state=s;
}

bool Stonewt::operator!=(const Stonewt &st) const {
    return pounds!=st.pounds;
}

bool Stonewt::operator<=(const Stonewt &st) const {
    return pounds<=st.pounds;
}

bool Stonewt::operator==(const Stonewt &st) const {
    return pounds==st.pounds;
}

bool Stonewt::operator<(const Stonewt &st) const {
    return pounds<st.pounds;
}

bool Stonewt::operator>(const Stonewt &st) const {
    return  pounds>st.pounds;
}

bool Stonewt::operator>=(const Stonewt &st) const {
    return pounds>=st.pounds;
}
```



7.一个复数有两个部分：实部和虚部。一个虚数的写法是这样的：（3.0，4.0）。这里3.0是实部，4.0是虚部。

假设a = (A,Bi)，c = (C,Di)。

下面是一些复数运算：

- 加法：a + c = (A + C, (B + D)i)
- 减法：a - c = (A - C, (B - D)i)
- 乘法：a × c = (A × C - B×D, (A×D + B×C)i)
- 乘法：（x为实数）：x ×c = (x×C,x×Di)
- 共轭： ~a = (A, - Bi)

定义一个复数类，使下面的程序可以使用它并得到正确的结果：

```c++
#include <iostream> 
using namespace std;
#include "complex0.h" // to avoid confusion with complex.h 
int main()
{
complex a(3.0, 4.0);	// initialize to (3,4i)
 
complex c;
cout << "Enter a complex number (q to quit):\n"; 
    while (cin >> c)
    {
    	cout << "c is " << c << '\n';
    	cout << "complex conjugate is " << ~c << '\n'; 
        cout << "a is " << a << '\n";
        cout << "a + c is " << a + c << '\n'; 
        cout << "a - c is " << a - c << '\n'; 
        cout << "a * c is " << a * c << '\n'; 
        cout << "2 * c is " << 2 * c << '\n';
        cout << "Enter a complex number (q to quit):\n";
    }
    cout << "Done!\n"; 
    return 0;
}

```

注意，你必须重载<<和>>操作符。标准C++已经有对复数的支持--比本例中的更广泛--在一个复数头文件中，所以使用complex0.h来避免冲突。只要有必要就使用const。

下面是该程序的一个运行示例：

> Enter a complex number (q to quit):
>
> real: **10**
>
> imaginary: **12**
>
> c is (10,12i)
>
> complex conjugate is (10,-12i) a is (3,4i)
>
> a + c is (13,16i) a - c is (-7,-8i) a * c is (-18,76i)
>
> 2 * c is (20,24i)
>
> Enter a complex number (q to quit):
>
> real: **q**
>
> Done!

注意，cin >> c，通过重载，提示为实部和虚部。

complex.hpp

```c++
using std::string;
class complex{
private:
    double _m_real;
    double _m_imaginary;
public:
    complex(int real=0,int imaginary=0);
    ~complex();
    friend std::ostream&  operator<<(std::ostream& os,const complex& c);
    complex operator~() const;
    complex operator+(const complex& c) const;
    complex operator-(const complex& c) const;
    complex operator*(const complex& c) const;
    friend complex operator*(double d,const complex& c);
    friend std::istream& operator>>(std::istream& is,complex &c);
};
#endif //LEARNING_SUPPORT_H
```



complex.cpp

```c++
using std::cout;
using std::cout;

complex::complex(int real, int imaginary) {
    _m_real=real;
    _m_imaginary=imaginary;
}

complex::~complex() {

}

complex complex::operator*(const complex &c) const {
    complex res;
    res._m_real=_m_real*c._m_real-_m_imaginary*c._m_imaginary;
    res._m_imaginary=_m_imaginary*c._m_real+_m_real*c._m_imaginary;
    return res;
}

complex complex::operator-(const complex &c) const {
    return complex(_m_real-c._m_real,_m_imaginary-c._m_imaginary);
}

complex complex::operator+(const complex &c) const {
    return complex(_m_real+c._m_real,_m_imaginary+c._m_imaginary);
}

complex complex::operator~() const {
    return complex(_m_real,-_m_imaginary);
}

complex operator*(double d,const complex& c){
    return complex(d*c._m_real,d*c._m_imaginary);
}

std::ostream&  operator<<(std::ostream& os,const complex& c){
    cout<<"("<<c._m_real<<","<<c._m_imaginary<<"i)";
}

std::istream& operator>>(std::istream& is,complex &c){
    cout<<"real:";
    double real,imaginary;
    is>>real;
    if(is.fail()||(char)real=='q'){
        is.setstate(std::ios::failbit);
        return is;
    }
    while(getchar()!='\n');
    cout<<"imaginary:";
    is>>imaginary;
    while(getchar()!='\n');
    c._m_real=real;
    c._m_imaginary=imaginary;
    return is;
}
```

