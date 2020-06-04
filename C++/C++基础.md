# C++ 基础

### 命名空间

```C++
namespace space_name{
   //  变量类型 变量;
   // 函数返回类型 函数原型;
    void A(){};
    int b;
}
// 使用
space_name::A();
space_name::b;
// 或
using namespace space_name;
// 或
using std::cout;
using std::cin;
using std::endl;    // 推荐用这种引用方式

// 直接写在主文件中的变量，都属于匿名空间
// 使用匿名空间
int num =1; // 全局变量， 同时也属于全局匿名空间
cout<< ::num << endl; // 只通过两个点，来表示匿名空间
```

命名空间可以分开写，但是最终还是一个命名空间

```c++
namespace A{
    int a = 100;
}
namespace B{        
    void display_b(){
        cout << A::a << endl;    // 由于此处使用到了A::a，所以A中a变量必须定义在使用之前。定义在之后会出错
    }
}
namespace A{    // 分开定义namespace A
    void display_a(){
        cout << "display:A" << endl;
    }
}
```



### 常量

```c++
// 在C语言中的常量，通过 define 来定义
#define MAX 5
// C++ 中通过 const 关键字来定义
const int b = 15; // 定义常量b ，并分配初始值。 const常量必须在定义时，分配值。

// const 相比于 #define 的优势在于：
// #define 是预编译，只是进行简单的代码替换
// const 不是，const是在编译时进行分配赋值。而且会进行类型的检查
//
//--------------------------------------
int a = 5;
int b = 10;
const int * p = &a;    // 通常叫做 ==> 常量指针
p = &b;  // 该句可以成功执行
*p = 1000;  // 该句不能被正常执行。   指针const，是它所指的变量不能被修改，而不是指针引用不能被修改。
//--------------------------------------
int * const p1 = &a;    // =====>指针常量。
*p1 = 1000;  // 可以执行
p1 = &b;     // 不能成功被执行   
// 综上，const 跟谁近，谁就是const。就不能被修改

// 也可以
const int * const p = &a;   // 双重锁定，都不能改变
```



### 创建堆空间

```c++
// C 语言方式
int * mm = (int *) malloc(sizeof(int));
*mm = 5;
free(mm);

// C++ 方式
int * mm1 = new int(2);
delete mm1; 
int * mm2 = new int[20];
delete [] mm2;  // 删除一个数组
```

区别：

1. new / delete 是表达式，malloc / free 是库函数
2. malloc只负责开辟空间，不会进行初始化 （需要memset清零）/ new 会进行初始化，通过参数。



### 引用类型

引用类型，相当于该统一个变量起多个名字，类似曾用名。

```c++
int a = 10;
int & b = a;   
b = 5;
cout << a; // 输出5。 对b的任何造作，就等于对a的操作。
```

引用区别与指针：

1. 指针是一个变量，它存储着指向数据的空间地址
2. 引用只是一个别名。

3. 当作函数参数。由于引用是一个变量的别名，因而它必须``初始化``，因而不能单独出现。必须有关联的变量。

而且一旦被初始化，就再也不能指向一个新的变量（成为一个新的变量的别名）。正是由于引用的这个特点，即一旦初始化，就永远是某个变量的别名，即等同于const。``因而它可以当作函数的参数来对待。``

这样他就能克服形式参数赋值的缺点。在参数赋值时，他还是指向原来所指的那个变量。 

4. 引用还可以用做函数的返回值

```c++
int arr[5] = {0, 1, 2, 3, 4};
int & tt(int index){
    return arr[index];  // 此处返回的就是 arr[2]
}
int tt1(int index){
    return arr[index];  // 此处返回的是一个新的int变量，只是值相同。
    // 当一个函数要返回一个对象时，这种情况表现尤其突出。因为不加引用，则会额外的生成一个对象。
}
```



### 强制类型转换

``static_cast<double>``，用该关键字来实现转换。例如

```c++
int a = 5;
double b = static_cast<double> a;
```

``dynamic_cast``

``const_cast``

``reinterpret_cast``



### 重载

重载overload，是指函数名相同，函数参数不同。在函数调用时差别调用的一种形式。

函数重载使用的技术为``名字改编 name mangling``。他在C中是不支持的（C是具有名字唯一性的语言），所以用gcc编译则会报错。

相反用g++编译，即c++的方式则不会。名子改编的原理是，**在编译的时候将名字与变量进行拼接，从而形成一个独特的名字，这个名字在系统中唯一**。该方式会根据函数参数的，``类型、个数、顺序``的不同来做出不同的可以唯一确定的函数。

可以通过``g++ -c test.cpp``文件生成编译后未连接的文件``test.o``，然后在通过``nm test.o``来观察。

```c++
// test.cpp 
int add(int a, int b, int c){     // 会发现编译后 名字被改编为。 __Z3addiii
    return a + b + c;
}
int add(int a, int b){   // 名字被改编为 __Z3addii
    return a + b;
}
double add(double a, double b, double c){   // __Z3addddd
    return a + b + c;
}
```



**不使用名字改编**：有需c++默认会对所有函数进行名字改编，即使不使用到重载，它也会对名字进行改编。

若要兼容C，切不希望c++的编译对名字进行改编自需要用``extern "C"``

```c++
//extern c
extern "c"{
    int add(int a, int b){   // 名字被改编为 __Z3addii
    return a + b;
	}
}

// 通过如下方式对 extern C代码进行 预编译包括。可以实现 C/C++混合编程，实现兼容编程
#ifdef __cplusplus
extern "c"{
#endif
        int add(int a, int b){   // 名字被改编为 __Z3addii
    		return a + b;
		}
#ifdef __cplusplus
}
#endif
```



### inline 函数

inline 函数类似于``带参数的宏``。使用他的时候，它不再是函数，因而不用创建函数栈，执行指针跳转等，因而会提高执行效率

```c++
// C 中的带参数宏，只进行替换代码
#define Max(a, b)  (a) > (b) ? (a) : (b)

// C++ inline函数
inline int Max(int a, int b){   // 此处不再是函数，效果等效上面的宏
    return a > b? a : b;
}
```



### 默认参数

```c++
int add(int a, int b = 5){  // 带有默认参数的函数应该放在列表后面 
    return a + b;
}

add(10);
// 同时 如果一个函数发生了重载，那么也有可能产生二意性问题
```



### string 类型

string 是一个模版类型，定义在std命名空间中

```c++
#include <string>
using std::string;
using std::cout;

void main(void){
    string s1 = "hello";
    string s2 = "world";
    string s3 = s1 + " " + s3;  // string 类型是一个对象，它可以调用自己的方法
    cout << s3;
    cout << s3.size() << endl; // string 类型的方法
    // 还有 s.find , s.substr 等方法 
}
```

string 的迭代器

```c++
 for(string::iterator it = s1.begin(); it != s1.end(); it++){
        cout << *it <<endl;;
 }
```

string 在 c++11 中也可以被当作``容器``。若要编译为c++11需要用一下命令``g++ test.cpp -std=c++11``

```c++
for(auto & elem : s1){  // auto 关键字会自动判断s1中的每个字符是什么类型
    cout << elem << endl;
}
```



### 类

类的定义

```c++
class A{                      // 类与struct 的一个区别是，struct中的成员默认为public，而类默认private
    private: 
    	int a;    // 私有
    	void func1(){};  // 私有
    public: 
    	int b;  // 公有
    	void func2(){};
    protected: int c;
};   // 注意类的结尾需要一个分号

```

类外定义的函数

```c++
class B{
    private:
    	int func3(){ return 0; };   // 类内部可以定义 成员函数
    	int func4(); // 声明函数，然后由类外定义。此函数为private属性
}

int B::func4(){    // 类外定义成员函数。需要先在类里面声明
    return 5;
}
```

构造函数

1. 名字与类名相同

2. 不能有返回值，不能设置返回类型

3. 系统会默认提供一个无参数的构造函数。但是一旦用户自定义构造函数，那么系统就不再提供默认无参数的构造函数。用户就只能使用自己的构造函数

4. ```c++
    class C{
        private:
        	int _ix;  // 两个成员变量
        	int _iy; 
        public:
        	 // 为了健壮性，补全默认构造函数
        	C(): _ix(0), _iy(0)  // 初始化语句
            {};       // 构造函数逻辑
        
        	// 重载构造函数
        	C(int ix, int iy):_ix(ix), _iy(iy)
            {}; // 构造函数其他逻辑
        
        	// 补全系统自带的 复制构造函数，即用一个已经存在的对象，去初始化一个新的对象。
        	// 用法即，C c1;  C c2(c1); C c3 = c1;  用对象c1来初始化对象c2和c3
        	C(const C & rhs)  // 引用符号，不能去掉，否则就会重新创建一个对象，而且会出现无限递归的情况。
    }
    ```

5. 构造函数比较复杂。他会在很多情况下被调用🌟🌟🌟🌟🌟🌟
    - 创建时调用
    - 函数传递对象型参数，而且参数不是引用时候，构造函数会在子函数中的形参中构造
    - return语句返回值时，若返回值不为引用。则在返回时，也会进行一次形参和实参的传递（调用构造函数）

析构函数

1. 系统会默认提供一个析构函数