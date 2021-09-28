# C++
## 前言
需要一定的C语言的基础知识，比如判断语句（if），循环语句（for,while）,数据类型，等。  

IDE推荐：CLion  
编译器：GCC或G++  
调试器：GDB  
## C++基础及进阶

### 1. C++与C的对比

1. C++有三种编程方式：过程性，面向对象，泛型编程。
2. C++函数符号由 函数名+参数类型 组成，C只有函数名。所以，C没有函数重载的概念。
3. C++ 在C的基础上增加了封装、继承、多态的概念
4. C++增加了泛型编程
5. C++增加了异常处理，C没有异常处理
6. C++增加了bool型
7. C++允许无名的函数形参（如果这个形参没有被用到的话）
8. C允许main函数调用自己
9. C++支持默认参数，C不支持
10. C语言中，局部变量必须在函数开头定义，不允许类似`for(int a = 0; ;;)`这种定义方法。
11. C++增加了引用
12. C允许变长数组，C++不允许
13. C中函数原型可选，C++中在调用之前必须声明函数原型
14. C++增加了STL标准模板库来支持数据结构和算法

### 2. 基础进阶

#### 1. const关键字  
##### 1.1 const含义  

常类型是指使用类型修饰符`const`说明的类型，常类型的变量或对象的值是不能被赋值改变的。  

##### 1.2 const作用  

###### 1.2.1 const于数据类型，指针：    

```c++
const int a = 10;
//常量：必须初始化

char * const p = &a;
//指向类型对象的const指针，或者说常指针、const指针：指向的值可以改变，地址不可改变，必须初始化

const char * const p = &a;
//指向const对象的const指针：指向的值不可改变，地址不可改变，必须初始化

const char *p;
char const *p;
//指向const对象的指针或者说指向常量的指针：指向的值不可改变，地址可以改变，可以不用初始化
```

###### 1.2.2 const于函数  

```c++
const int func1(); // 本身无意义
const int* func2(); // 指针指向的内容不能改变，返回的内容不可改变
int* const func2(); // 指针本身不能改变，返回内容的地址不可改变

void func(const int var); // 传递过来的参数var的值不能改变
void func(int* const var); // 指针本身不可变，var的地址不可改变，值可以变
```

###### 1.2.3 const于类  

```c++
class Apple
{
    private:
        int people[100];
    public:
        Apple(int i):apple_number(i){}
        const int apple_number;
};
```
const对象只能访问const成员函数，而非const对象可以访问任意的成员函数，包括const成员函数。
```c++
class Apple
{
    private:
        int a;
        const int b;
    public:
        Apple(const int a,const int b):a(a),b(b){}
        /*
            常函数不可改变成员变量
            类确定，即this所指向的地址确定，不可改变，但所指内容可以改变，加const即确定所指内容，使其不可改变，如同常量
        */
        const int inita(const int a) const
        {
            this->a = a;//不可改变
            return this->a;
        }
        const int initb(const int b)
        {
            this->b = b;//不可改变
            return this->b;
        }
};
```

##### 1.3 const的意义    

- 作用相当于`#define`，不同的是`#define`作用于全局。
- 提高程序的健壮性  

#### 2. static关键字  
##### 2.1 stztic含义  

当与不同类型一起使用时，Static关键字具有不同的含义。我们可以使用static关键字： 

##### 2.2 静态变量： 函数中的变量，类中的变量  

###### 2.2.1 函数中的静态变量    

当变量声明为static时，空间将在程序的生命周期内分配。即当变量声明为static，内存确定，即使多次调用，系统也不会为其分配新内存，其中的内容不会消失重置。  

```c++
#include <iostream> 
#include <string> 
using namespace std; 
void demo() 
{ 
    static int count = 0; 
    cout << count << " " << endl;
    count++; 
} 
int main() 
{
    for (int i=0; i<5; i++)
        demo();
    return 0;
}
//输出 0 1 2 3 4
//每次调用函数时，都不会对变量计数进行初始化。
```

###### 2.2.2 类中的静态变量  

由于声明为static的变量只被初始化一次，因为它们在单独的静态存储中分配了空间，不属于任何一个类，因此类中的静态变量由对象共享。对于不同的对象，不能有相同静态变量的多个副本。也是因为这个原因，静态变量不能使用构造函数初始化。  

```c++
#include<iostream> 
using namespace std; 

class Apple 
{ 
        public: 
            static int i; 
            Apple() { }; 
}; 
int Apple::i = 0;//必须类外初始化
int main() 
{ 
    Apple obj1; 
    Apple obj2; 
    obj1.i = 5; 
    cout << obj1.i << " " << obj2.i << endl;
    obj2.i = 3; 
    cout << obj1.i << " " << obj2.i << endl;
}
//输出 
//5 5
//3 3
//调用同一片内存
```

##### 2.3 静态类的成员： 类对象和类中的函数  

###### 2.3.1 类对象为静态  

静态类对象始终贯穿程序，在程序结束时，释放内存。

###### 2.3.2 类中的静态函数  

1. 静态成员函数只能访问静态类成员或其他静态成员函数，它们无法访问类中的非静态成员或成员函数。  
2. 成员函数和成员对象分开存储。  

#### 3. this指针  
##### 3.1 this指针的用处  

1. 一个对象的this指针不占用类的内存。  
2. this指针指向本类  
3. this作用域是在类内部，作用于非静态成员变量，使用时无需定义。

##### 3.2 this指针的使用  

1. 在类的非静态成员函数中返回类对象本身的时候，直接使用 `return *this`。  
2. 当参数与成员变量名相同时，如`this->n = n` （不能写成 `n = n`)。  

##### 3.3 this指针程序举例：  

```c++
class A
{
    public:
        static int func01(int a1)//静态成员函数没有this指针
        {
            a1 = a1;       
        }
        int func02(int a2)
        {
            this->a2 = a2;//this指向A
        }

        static int a1;
        int a2;
};
class B
{
    public:
        B(int b)
        {
            this->b = b;
        }
        B* a_add(B &b)
        {
            this->b += b.b;
            return this;//this本身为指向本类的指针，为本类的地址
        }
        B& a_add(B *b)//如果返回值，将返回备份对象，必须返回引用
        {
            this->b += b->b;
            return *this;//this本身为指向本类的指针，为本类的地址
        }
        int b;
};
int A::a1 = 0;
void test01()
{
    A a;//初始化类
    a.func02(10);
    cout << "A:" << a.a2 << endl;//输出10
}
void test02()
{
    B b1(20);//构造函数调用
    cout << "B:" << b1.b << endl;

    b1.a_add(b1)->a_add(b1)->a_add(b1);//链式调用
    cout << "B:" << b1.b << endl;

    b1.a_add(&b1).a_add(&b1).a_add(&b1);
    cout << "B:" << b1.b << endl;
}
int main()
{
    test01();
    test02();

    system("pause");
    return 0;
}
```

#### 4. inline关键字  
##### 4.1 inline的用处  

- 内联能提高函数效率，但并不是所有的函数都定义成内联函数！内联是以代码膨胀(复制)为代价，仅仅省去了函数调用的开销，从而提高函数的执行效率。   
- 如果执行函数体内代码的时间相比于函数调用的开销较大，那么效率的收获会更少！  
- 每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。

##### 4.2 以下情况不宜用内联：  

1. 如果函数体内的代码比较长，使得内联将导致内存消耗代价比较高。  

2. 如果函数体内出现循环，那么执行函数体内代码的时间要比函数调用的开销大。

##### 4.3 虚函数（virtual）可以是内联函数（inline）：  

- 虚函数可以是内联函数，内联是可以修饰虚函数的，但是当虚函数表现多态性的时候不能内联。

- 内联是在编译器建议编译器内联，而虚函数的多态性在运行期，编译器无法知道运行期调用哪个代码，因此虚函数表现为多态性时（运行期）不可以内联。

- `inline virtual` 唯一可以内联的时候是：编译器知道所调用的对象是哪个类（如 `Base::who()`），这只有在编译器具有实际对象而不是对象的指针或引用时才会发生。

- 程序实现：

  ```c++
  class Base
  {
      public:
          inline virtual void who()
          {
              cout << "I am Base\n";
          }
          virtual ~Base() {}
  };
  class Derived : public Base
  {
  	public:
          inline void who()  // 不写inline时隐式内联
          {
              cout << "I am Derived\n";
          }
  };
  
  int main()
  {
      // 此处的虚函数 who()，是通过类（Base）的具体对象（b）来调用的，编译期间就能确定了，所以它可以是内联的，但最终是否内联取决于编译器。 
      Base b;
      b.who();
  
      // 此处的虚函数是通过指针调用的，呈现多态性，需要在运行时期间才能确定，所以不能为内联。  
      Base *ptr = new Derived();
      ptr->who();
  
      // 因为Base有虚析构函数（virtual ~Base() {}），所以 delete 时，会先调用派生类（Derived）析构函数，再调用基类（Base）析构函数，防止内存泄漏。
      delete ptr;
      ptr = nullptr;
  
      system("pause");
      return 0;
  } 
  ```

#### 5. 指针和引用  
##### 5.1 指针  

###### 5.1.1 指针 

1. 直接指向地址
2. p ==> 保存a的地址；*p ==> 保存a的内容
3. 指针可以不初始化
4. 指针可以为空

###### 5.1.2 指针变量的语法  

- 变量：
    ```c++
    int a = 10;
    int *p = &a;//可以不用初始化
    ```
    或
    ```c++
    int a = 10;
    int *p = NULL;//可以不用初始化
    p = &a;
    ```
- 数组：  
    ```c++
    int a[10];
    int *p = a;//a为a[10]的首地址
    ```
    或
    ```c++
    int a[10];
    int *p = NULL;//可以不用初始化
    p = a;
    ```
- 函数： 
    ```c++
    int* func(int *p)
    {
        return p;//返回*p的地址
    }
    int main()
    {
        int a;
    	int b[10];
    	int *c = &a;
        
        int *p = func(&a);//返回a的地址
        func(b);//直接传入首地址
        func(c);
        
        return 0;
    }
    ```

##### 5.2 引用  

###### 5.2.1 引用

1. 实际上就是为变量起一个别名
2. 一个变量只能作为另一个变量的别名，不能重复引用
3. 引用不能更换目标
4. 引用必须初始化
5. 引用不能为空

###### 5.2.2 引用的语法

- 变量

  ```c++
  int a = 10;
  int &b = a;//必须初始化
  //a,b的地址相同
  ```

- 函数

  ```c++
  int func(int &p)
  {
      return p;//返回*p的地址
  }
  int main()
  {
      int a;
  	int *c = &a;
      int *p = func(a);返回a的地址
      func(b);
      func(*c);
      
      return 0;
  }
  ```

##### 5.3 总结

|     引用     |     指针     |
| :----------: | :----------: |
|  必须初始化  | 可以不初始化 |
|   不能为空   |   可以为空   |
| 不能更换目标 | 可以更换目标 |

C++中引入了引用操作，在对引用的使用加了更多限制条件的情况下，保证了引用使用的安全性和便捷性，还可以保持代码的优雅性。在适合的情况使用适合的操作，引用的使用可以一定程度避免“指针满天飞”的情况，对于提升程序稳定性也有一定的积极意义。最后，指针与引用底层实现都是一样的，不用担心两者的性能差距。

#### 6. new与delete关键字

##### 6.1 栈区和堆区区别： 

- 堆和栈中的存储内容：栈存局部变量、函数参数等。堆存储使用`new`、`malloc`申请的变量等；   
- 申请方式：栈内存由系统分配，堆内存由自己申请；
- 申请后系统的响应： 
  栈————只要栈的剩余空间大于所申请空间，系统将自动为程序提供内存，否则将报异常提示栈溢出。 
  堆————首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表 中删除，并将该结点的空间分配给程序；  
- 申请大小的限制：Windows下栈的大小一般是2M，堆的容量较大；
- 申请效率的比较：
  栈由系统自动分配，速度较快。 
  堆使用`new`、`malloc`等分配，较慢；   

##### 6.2 new  

###### 1. 开辟单变量地址空间  

使用`new`运算符时必须已知数据类型，`new`运算符会向系统堆区申请足够的存储空间，如果申请成功，就返回该内存块的首地址，如果申请不成功，则返回零值。 
`new`运算符返回的是一个指向所分配类型变量（对象）的指针。对所创建的变量或对象，都是通过该指针来间接操作的，而动态创建的对象本身没有标识符名。 
一般使用格式：  

```c++
格式1：指针变量名=new 类型标识符；  
格式2：指针变量名=new 类型标识符（初始值）；  
格式3：指针变量名=new 类型标识符 [内存单元个数]； 
```
说明：格式1和格式2都是申请分配某一数据类型所占字节数的内存空间；但是格式2在内存分配成功后，同时将一初值存放到该内存单元中；而格式3可同时分配若干个内存单元，相当于形成一个动态数组。例如： 

1. `new int`;  //开辟一个存放整数的存储空间,返回一个指向该存储空间的地址。`int *a = new int` 即为将一个int类型的地址赋值给整型指针a  
2. `int *a = new int(5)` 作用同上,但是同时将整数空间赋值为5    

###### 2. 开辟数组空间  

对于数组进行动态分配的格式为：

```c++
指针变量名=new 类型名[下标表达式];
delete [ ] 指向该数组的指针变量名;
```
两式中的方括号是非常重要的，两者必须配对使用，如果`delete`语句中少了方括号，因编译器认为该指针是指向数组第一个元素的指针，会产生回收不彻底的问题（只回收了第一个元素所占空间），加了方括号后就转化为指向数组的指针，回收整个数组。 
 `delete []`的方括号中不需要填数组元素数，系统自知。即使写了，编译器也忽略。 
 请注意“下标表达式”不必是常量表达式，即它的值不必在编译时确定，可以在运行时确定。

```c++
 一维: int *a = new int[100];    //开辟一个大小为100的整型数组空间
 二维: int **a = new int[5][6]
 三维及其以上:依此类推.
```
 一般用法: new 类型 （初值）

##### 6.3 delete语法  

- 1.删除单变量地址空间  
    ```c++
    int *a = new int;
    delete a;   //释放单个int的空间
    ```
- 2 删除数组空间
    ```c++
    int *a = new int[5];
    delete []a;    //释放int数组空间
    ```

##### 6.4 使用注意事项

1. new 和delete都是内建的操作符，语言本身所固定了，无法重新定制，想要定制new和delete的行为，徒劳无功的行为。
2. 动态分配失败，则返回一个空指针（NULL），表示发生了异常，堆资源不足，分配失败。
3. 指针删除与堆空间释放。删除一个指针p（delete p;）实际意思是删除了p所指的目标（变量或对象等），释放了它所占的堆空间，而不是删除p本身（指针p本身并没有撤销，它自己仍然存在，该指针所占内存空间并未释放），释放堆空间后，ｐ成了空指针。
4. 内存泄漏（memory leak）和重复释放。new与delete 是配对使用的， delete只能释放堆空间。如果new返回的指针值丢失，则所分配的堆空间无法回收，称内存泄漏，同一空间重复释放也是危险的，因为该空间可能已另分配，所以必须妥善保存new返回的指针，以保证不发生内存泄漏，也必须保证不会重复释放堆内存空间。
5. 动态分配的变量或对象的生命期。我们也称堆空间为自由空间（free store），但必须记住释放该对象所占堆空间，并只能释放一次，在函数内建立，而在函数外释放，往往会出错。
6. 要访问new所开辟的结构体空间,无法直接通过变量名进行,只能通过赋值的指针进行访问。    
7. 与malloc的区别 

|        特征        |             new/delete             |             malloc/free              |
| :----------------: | :--------------------------------: | :----------------------------------: |
|   分配内存的位置   |             自由存储区             |                  堆                  |
|    内存分配失败    |              抛出异常              |               返回NULL               |
|   分配内存的大小   |       编译器根据类型计算得出       |            显式指定字节数            |
|      处理数组      |         有处理数组的new[]          | 需要用户计算数组的大小后进行内存分配 |
|  已分配内存的扩张  |               不支持               |           使用realloc完成            |
|   分配时内存不足   | 可以指定处理函数或者重新制定分配器 |       无法通过用户代码进行处理       |
|    是否可以重载    |                可以                |                不可以                |
| 构造函数与析构函数 |                调用                |                不调用                |

#### 7. sizeof()函数

##### 7.1 sizeof()的作用

返回所占的内存空间大小

##### 7.2 类的sizeof()

- 空类的大小为1字节
- 一个类中，虚函数本身、成员函数（包括静态与非静态）和静态数据成员都是不占用类对象的存储空间。
- 对于包含虚函数的类，不管有多少个虚函数，只有一个虚指针,vptr的大小。
- 普通继承，派生类继承了所有基类的函数与成员，要按照字节对齐来计算大小
- 虚函数继承，不管是单继承还是多继承，都是继承了基类的vptr。(32位操作系统4字节，64位操作系统 8字节)！
- 虚继承,继承基类的vptr。

##### 7.3 举例说明

```c++
class A{};
int main()
{
    cout << sizeof(A) << endl;
    return 0;
}
//输出“1”，起占位作用
```
```c++
class A
{
    public:
        char b;
        virtual void fun() {};//虚函数返回虚指针
        static int c;
        static int d;
        static int f;
};
int main()
{
    cout << sizeof(A) << endl; 
    return 0;
}
//输出“8”，或者“16”
//char占1bit，内存对齐，系统为char分配4bit或者8bit
//32位，指针占4bit；64位，指针占8bit；
//静态变量不属于任何一个类，不占类的内存空间，系统会为其另外分配内存空间
```
```c++
class A
{
    virtual void fun();
    virtual void fun1();
    virtual void fun2();
    virtual void fun3();
};
int main()
{
    cout << sizeof(A) << endl;
    return 0;
}
//输出“4”或者“8”
//对于包含虚函数的类，不管有多少个虚函数，只有一个虚指针,vptr的大小
```
```c++
class A
{
    public:
        char a;
        int b;
};
class B:A
{
    public:
        short a;
        long b;
};
class C
{
    A a;
    char c;
};
class A1
{
    virtual void fun(){}
};
class C1:public A
{
};

int main()
{
    cout<<sizeof(A)<<endl; // 8 内存对齐
    cout<<sizeof(B)<<endl; // 24 内存对齐 A:8 B:16
    cout<<sizeof(C)<<endl; // 12 a:8 c:4
    cout<<sizeof(C1)<<endl; // 8 继承+内存对齐
    return 0;
}
```

#### 8. class类
##### 8.1 class基本组成结构  

###### 8.1.1 三种权限  

1. 公共权限`public` 
   - 类内（可以）访问，修改  
   - 类外（可以）访问，（可以）修改  
   - 子类（可以）继承   

2. 保护权限`protected`  
   - 类内（可以）访问，修改  
   - 类外（不可以）访问，（不可以）修改  
   - 子类（可以）继承  

3. 私有权限`private` （class默认为private） 
   - 类内（可以）访问，修改  
   - 类外（不可以）访问，（不可以）修改  
   - 子类（不可以）继承  

###### 8.1.2 三种默认函数  

1. 构造函数  
   - 如果程序员不写，系统会自动写上，此时构造函数为空  
   - 构造函数会自动调用，而且只调用一次  
   - 不用写返回值，可以重载，可以有参数  
   - 构造函数名称与类名相同  
   - 分类：
     - 按照参数分类  
     - 按照类型分类  
   - 调用：  
     - 括号法 
     - 显示法  
     - 隐式转换法  
   - 注意事项：  
     - 在调用默认构造函数时，不要加()   
     - 不要利用拷贝构造函数来初始化匿名对象，编译器会将其等价为对象声明  

2. 析构函数  
   - 如果程序员不写，系统会自动写上，此时构造函数为空    
   - 构造函数会自动调用，而且只调用一次   
   - 不用写返回值，不可以重载，不可以有参数   
   - 构造函数名称与类名相同 + ”~“   
   - 一般在最后执行 

3. 拷贝函数  

   - 拷贝会将被拷贝的类所有成员及函数，连内容带地址全部拷贝到新函数中

   - 浅拷贝 ==> 连同地址拷贝，可能会释放两边相同的内存    

     ```c++
     class A
     {
         public:
         	A()
             {
                 age = 10;
             }
             A(const A &p)//浅拷贝构造函数，将p的所有属性上传
             {
                 age = p.age;
                 cout << age << "   " << "拷贝构造函数" << endl;
             }
         	~A()
             {}
         private:
             int age;
     };
     int main()
     {
         A a1;
         A a2 = a1;
     }
     ```

   - 深拷贝 ==> 在堆区开辟新内存    

     ```c++
     class A
     {
         public:
             int *a;
             int *b;
             A()
             {
                 this->a = new int(102);
                 this->b = new int(202);
                 cout << "A的构造函数！" << endl;
             }
         //在堆区新建一块内存来保存拷贝数据
             A(const A &p)//如果使用浅拷贝会导致在拷贝前，被拷贝的类成员被释放，无法拷贝而保存
             {
                 a = new int(*p.a);
                 b = new int(*p.b);
             }
             ~A()
             {
                 delete a;
                 a = NULL;
                 delete b;
                 b = NULL;
                 cout << "A的析构函数！" << endl;
             }
     };
     int main(){
         A a1;
         A a2 = a1;
         cout << *a2.a << endl;
     
         return 0;
     }
     ```

4. 程序举例  

   ```c++
   class A
   {
       public:
           A()//无参构造（默认构造）
           {
               cout << "无参构造函数" << endl;
           }
           A(int a)//有参构造
           {
               age = a;  
               cout << age << "   " << "有参构造函数" << endl;
           }
           A(const A &p)//浅拷贝构造函数，将p的所有属性上传
           {
               age = p.age;
               cout << age << "   " << "拷贝构造函数" << endl;
           }
           ~A()//析构函数
           {
               cout << "析构函数" << endl;
           }
       private:
           int age;
   };
   void test01()//函数内容建立在栈内，执行结束自动释放内存
   {
       //括号法
       A a1;//无参（默认）
       A a2(10);//有参
       A a3(a1);//无参拷贝
       A a4(a2);//有参拷贝
   
       //显示法
       A a1;//无参（默认）
       A a2 = A(20);//有参
       A a3 = A(a1);//无参拷贝
       A a4 = A(a2);//有参拷贝
       A(20);//匿名对象  特点：当前行执行结束后，系统会立即回收匿名对象
   
       //隐式转换法
       A a1;//无参
       A a2 = 30;//有参  相当于A a2 = A(30)
       A a3 = a1;//有参拷贝  相当于A a3 = A(a1)
       A a4 = a2;//无参拷贝  相当于A a4 = A(a2)
   }
   int main()
   {
       //A a;   //main函数没有执行完
       test01();            
       return 0;
   }
   ```

##### 8.2 class的拓展组成结构  

###### 8.2.1 friend友元

1. 概述 

  - 友元提供了一种 普通函数或者类成员函数 访问另一个类中的私有或保护成员 的机制。也就是说有两种形式的友元：  

  - 友元函数：普通函数对一个访问某个类中的私有或保护成员。  

  - 友元类：类A中的成员函数访问类B中的私有或保护成员。  

  - 优点：提高了程序的运行效率。  

  - 缺点：破坏了类的封装性和数据的透明性。  

  - 总结：  

    - 能访问私有成员  
    - 破坏封装性   
    - 友元关系不可传递   
    - 关系的单向性   

            - 友元声明的形式及数量不受限制  

2. 友元函数 
  在类声明的任何区域中声明，而定义则在类的外部。 
  注意：友元函数只是一个普通函数，并不是该类的类成员函数，它可以在任何地方调用，友元函数中通过对象名来访问该类的私有或保护成员。  

    ```c++
    class A
    {
        friend int get(A &c); // 友元函数，相当于加了friend的函数声明
        public:
            A(int a):a(a){};
        private:
            int a;
    };
    int get(A &c) 
    {
        return c.a;
    }
    int main()
    {
        A a(3);    
        cout << get(a) << endl;
        return 0;
    }
    ```

3. 友元类 
  友元类的声明在该类的声明中，而实现在该类外。 
  类B是类A的友元，那么类B可以直接访问A的私有成员。

    ```c++
    class B;//声明
    class A
    {
        friend class B;//放在本类之前
        public:
            A(int _a):a(_a){};   
        private:
            int a;
    };
    class B
    {
        public:
            int getb(A ca) 
            {
                return  ca.a; 
            }
    };
    int main() 
    {
        A a(3);
        B b;
        cout << b.getb(a) << endl;
        return 0;
    }
    ```

4. 注意  
    - 友元关系没有继承性 假如类B是类A的友元，类C继承于类A，那么友元类B是没办法直接访问类C的私有或保护成员。  
    - 友元关系没有传递性 假如类B是类A的友元，类C是类B的友元，那么友元类C是没办法直接访问类A的私有或保护成员，也就是不存在“友元的友元”这种关系。  

###### 8.2.2 this指针  

见[this指针](#3-this指针)  

###### 8.2.3 static静态成员变量及函数  

见[static静态成员及函数](#2-static关键字)  

###### 8.2.4 const常成员变量及成员函数  

见[const](#1-const关键字)  

###### 8.2.5 mutable关键字  

1. `mutalbe`的中文意思是“可变的，易变的”，跟`const`是反义词。 

2. 在C++中，`mutable`也是为了突破`const`的限制而设置的。被`mutable`修饰的变量，将永远处于可变的状态，即使在一个`const`函数中。 

3. 被`const`关键字修饰的函数的一个重要作用就是为了能够保护类中的成员变量。即：该函数可以使用类中的所有成员变量，但是不能修改他们的值。然而，在某些特殊情况下，我们还是需要在`const`函数中修改类的某些成员变量，因为要修改的成员变量与类本身并无多少关系，即使修改了也不会对类造成多少影响。只修改某个成员变量，其余成员变量仍然希望被`const`保护。即：为了解除类内常成员函数对成员变量的保护作用，但不想全部解除，所以在需要改变的成员变量前加`mutable`。

4. 程序举例

   ```c++
   class A
   {
       private: 
       	mutable int b;//可以被常函数改变
       public:
           A(int b)
           {
               this->b = b;
           }
           void set(int b) const//可以且仅可以改变mutable修饰的变量
           {
               this->b = b;
           }
           void show()
           {
               cout << b << endl;
           }
   };
   int main()
   {
       A a(30);
       a.set(20);
       a.show();
       return 0;
   }
   ```

##### 8.3 class的运用  

###### 8.3.1 运算符重载   

直接对类操作，进行各种运算  

- 双目运算符重载 
  对“+，-，*，/，%”进行重载 

  - 类内定义：  

    ```c++
    class B
    { 
        public:
            B(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            int a;
            int b;
    };
    class A
    {
        public:
            A operator+(B &b)
            {
                A temp;
                temp.a = this->a + b.a;
                temp.b = this->b + b.a;
                return temp;
            }
            A operator+(B &b);
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            int a;
            int b;
    };
    A A::operator+(B &b)
    {
        A *temp;
        temp->a = this->a + b.a;
        temp->b = this->b + b.b;
        return *temp;
    }    
    int main()
    {
        A a;
        a.init(40,30);
        B b;
        b.init(20,10);
        a = a + b;
        cout << a.a << endl;
        cout << a.b << endl;
    }
    ```

  - 类外定义：

    ```c++
    class B
    {   
        public:
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            int a;
            int b;
    };
    class A
    {
        public:
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            int a;
            int b;
    };          
    A operator+(B &b,A &a)
    {
        A temp;
        temp.a = a.a + b.a;
        temp.b = a.b + b.b;
        return temp;
    }
    int main()
    {
        A a;
        B b;
        b.init(40,30);
        a.init(20,10);
        a = b + a;
        cout << a.a << endl;
        cout << a.b << endl;
        return 0;
    }
    ```

- 位运算符重载 
  “<<，>>”，只能类外重载

  - 输出类外重载：

    ```c++
    class A
    {
        public:
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            int a;
            int b;
    };          
    ostream& operator<<(ostream &cout,A &a)//加“&”可以连续输出
    {
        cout << a.a << endl;
        cout << a.b << endl;
    }
    int main()
    {
        A a;
        a.init(20,10);
        cout << a << endl;
        return 0;
    }
    ```

  - 输入类外重载：

    ```c++
    #include <iostream>
    using namespace std; 
    
    class A
    {
        public:
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            int a;
            int b;
    };          
    istream& operator>>(istream &cin,A &a)//加“&”可以连续输入
    {
        cin >> a.a >> a.b;
    }
    ostream& operator<<(ostream &cout,A &a)//加“&”可以连续输出
    {
        cout << a.a << "  " << a.b << endl;
    }
    int main()
    {
        A a;
        a.init(20,10);
        cin >> a;
        cout << a << endl;
        return 0;
    }    
    ```

- 自增自减运算符重载 
  “--，++”： 
  a++(--)先输出再自加（自减） 
  ++(--)a先自加（自减）再输出

  ```c++
  class A
  {
      public:
          void init(int a,int b)
          {
              this->a = a;
              this->b = b;
          }
          A& operator++()//类内++a
          {
              ++a;
              ++b;
              return *this;
          }
          int a;
          int b;
  };
  //必须返回引用，负责会建立副本，相当于拷贝
  A& operator++(A &a,int)//类外a++
  {
      A *c = new A;
      c->a = a.a;
      c->b = a.b;
      a.a ++;
      a.b ++;
      return *c;
  }
  ostream& operator<<(ostream &cout,A &a)//加“&”可以连续输出
  {
      cout << a.a << "  " << a.b << endl;
  }
  int main()
  {
      A a,b;
      a.init(20,10);
      b.init(40,30);
  
      cout << a << endl;
      cout << ++a << endl;
      cout << a << endl;
  
      cout << b << endl;
      cout << b++ << endl;
      cout << b << endl;
  
      int c= 10,d=20;
      cout << c << endl;
      cout << c++ << endl;
      cout << c << endl;
  
      cout << d << endl;
      cout << ++d << endl;
      cout << d << endl;
      return 0;
  }      
  ```

- 关系运算符重载 
  “<，>，<=，>=，!=，==”

  ```c++
  class B
  {
      public:
          int a,b;
          void init(int a,int b)
          {
              this->a = a;
              this->b = b;
          }
  };
  class A
  {
      public:
          int a,b;
          void init(int a,int b)
          {
              this->a = a;
              this->b = b;
          }
          bool operator==(B &b)
          {
              if(this->a == b.a && this->b == b.b)
              {
                  return true;
              }
              return false;
          }
          bool operator<=(B &b)
          {
              if(this->a <= b.a && this->b <= b.b)
              {
                  return true;
              }
              return false;
          }
  };
  bool operator!=(A &a,B &b)
  {
      if(a.a != b.a && a.b != b.b){
          return true;
      }
      return false;
  }
  int main()
  {
      A a;
      B b;
      a.init(10,20);
      b.init(10,20);
      cout << (a <= b) << endl;
      cout << (a == b) << endl;
      cout << (a != b) << endl;
      return 0;
  }
  ```

- 赋值运算符 
  与拷贝函数类似，拷贝函数就是特殊的"="重载

  - 在栈内创建：

    ```c++
    class A
    {
        public:
            int a;
            int b;
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
    };
    class B
    {
        public:
            int a;
            int b;
            void init(int a,int b)
            {
                this->a = a;
                this->b = b;
            }
            B& operator=(A &a)
            {
                this->a = a.a;
                this->b = a.b;
                return *this;
            }
    };
    int main()
    {
        A a;
        a.init(10,20);
        b = a;
        cout << b.a << endl;
        cout << b.b << endl;
    }
    ```

  - 在堆内创建：

    ```c++
    class A
    {
        public:
            int *a,*b;
            void init(int a,int b)
            {
                this->a = new int(a);
                this->b = new int(b);
            }
            A& operator=(A &a)
            {
                if(this->a != NULL || this->b != NULL)
                {
                    delete this->a;
                    this->a = NULL;
                    delete this->b;
                    this->b = NULL;
                }
                this->a = new int(*a.a);
                this->b = new int(*a.b);
                return *this;
            }
            ~A()
            {
                if(a != NULL || b != NULL)
                {
                    cout << "A" << endl;
                    delete a;
                    a = NULL;
                    delete b;
                    b = NULL;
                }
            }
    };
    ostream& operator<<(ostream &cout,A &a)
    {
        cout << *a.a << endl;
        cout << *a.b << endl;
    }
    int main(){
        A a;
        A a1;
        a1.init(0,0);
        a.init(10,20);
        a1 = a;
        cout << a1 << endl;
        return 0;
    }
    ```

###### 8.3.2 继承  

1. 语法： 
    class 子类（派生类）A:（继承方式）public 父类（基类）B
2. 继承方式：
  - 公共继承： 
        父类是公共，继承到子类是公共，类外可以访问 
        父类是保护，继承到子类是保护，类外不可访问 
        父类是私有，无法继承到子类  
  - 保护继承： 
        父类是公共，继承到子类是保护，类外不可访问 
        父类是保护，继承到子类是保护，类外不可访问 
        父类是私有，无法继承到子类  
  - 私有继承： 
        父类是公共，继承到子类是私有，类外不可访问 
        父类是保护，继承到子类是私有，类外不可访问 
        父类是私有，无法继承到子类  
3. 对象模型： 
        将所有父类成员继承到子类中，只是被编译器隐藏，没有显示，权限限制
4. 注意：  
  - 继承后的权限向上兼容，高级权限不可降低，低级权限可以升高  
    父类权限|继承方式|子类权限
    :-:|:-:|:-:
    公共|公共|公共
    保护|公共|保护
    公共|保护|保护
    保护|保护|保护
    公共|私有|私有
    保护|私有|私有
    
  - 父类的私有成员不可被继承  

  - 程序举例：

    ```c++
    class base//公共页面
    {
        public://向下兼容访问，继承
            int a;
        protected://向下兼容访问，继承
            int b;
        private://无论如何都无法访问，继承
            int c;
    };
    class Java:public base//公共继承
    {
        public:
            void init()
            {
                a = 10;//父类是公共成员，继承到子类是公共成员
                b = 20;//父类是保护成员，继承到子类是保护成员
                // c = 20;//父类是私有成员，无法继承到子类
            }
        protected:
            int d;
        private:
    };
    class Python:protected base
    {
        public:
            void init()
            {
                a = 10;//父类是公共成员，继承到子类是保护成员
                b = 20;//父类是保护成员，继承到子类是保护成员
                // c = 20;//父类是私有成员，无法继承到子类
            }
        protected:
            int d;
        private:
    };
    class CPP:private base
    {
        public:
            void init()
            {
                a = 10;//父类是公共成员，继承到子类是私有成员
                b = 20;//父类是保护成员，继承到子类是私有成员
                // c = 20;//父类是私有成员，无法继承到子类
            }
        protected:
            int d;
        private:
    };
    ```

  - 子类的构造函数可以覆盖父类的构造函数  

    ```c++
    class base
    {
        public:
            void init()
            {
                cout << "base!" << endl;
            }
    };
    class A:public base
    {
        public:
        	void init()
            {
                cout << "A!" << endl;
            }
    };
    int main()
    {
        A a;
        a.init();
        a.base::init();
        return 0;
    }
    /*
    A!
    base!
    */
    ```

  - 子类的成员可以覆盖父类中的所有同名的成员，如果需要访问父类同名成员，需加作用域，如果子类中没有与父类中同名的成员变量，即子类继承父类的成员变量以及值  

    ```c++
    class base
    {
        public:
        	int a;
        	int b;
        	int c;
        	int d;
        	base()
            {
                a = 101;
                b = 201;
                c = 301;
                d = 401;
            }
    };
    class A:public base
    {
        public:
        	int a;
        	int b;
        	A()
            {
                a = 102;
                b = 202;
            }
    };
    int main()
    {
        A a;
        cout << a.a << endl;//102
        cout << a.b << endl;//202
        cout << a.c << endl;//301
        cout << a.d << endl;//401
        
        return 0;
    }
    ```

  - 如果子类有同名成员，则调用子类，不会调用父类成员  

  - 先有父类，在有子类  

  - 栈：先进后出  

    ```c++
    class base
    {
        public:
        	base()
            {
                cout << "base的构造函数！"
            }
        	~base()
            {
                cout << "base的析构函数！"
            }
    };
    class A:public base
    {
        public:
        	A()
            {
                cout << "A的构造函数！"
            }
        	~A()
            {
                cout << "A的析构函数！"
            }
    };
    int main()
    {
        A a;
        
        return 0;
    }
    /*
    base的构造函数！
    A的构造函数！
    A的析构函数！
    base的析构函数！
    */
    ```

  - 静态共用一块内存

    ```c++
    class base
    {
        public:
            static int a;
            static int b;
    };
    class A:public base
    {
        public:
        	static int a;
    };
    int base::a = 101;
    int base::b = 102;
    int A::a = 201;
    int main()
    {
        A a;
        cout << a.a << endl;//201
        cout << a.base::a << endl;//101
        cout << a.b << endl;//102
        cout << a.base::b << endl;//102
        return 0;
    }
    ```

  - 程序举例：
    ```c++
    class base
    {
        public:
            int a;
            int b;
            static int c;
            base()
            {
                this->a = 101;
                this->b = 201;
                cout << "base的构造函数！" << endl;
            }
            ~base()
            {
                cout << "base的析构函数！" << endl;
            }
    };
    class A:public base
    {
        public:
            int a;
            int b;
            static int c;
            A()
            {
                this->a = 102;
                this->b = 202;
                cout << "A的构造函数！" << endl;
            }
            ~A()
            {
                cout << "A的析构函数！" << endl;
            }
    };
    int base::c = 301;
    int A::c = 302;
    int main()
    {
        A a;
        cout << a.base::a << endl;//作用域
        cout << a.a << endl;
        cout << a.base::c << endl;
        cout << a.c << endl;
        cout << base::c << endl;
    
        a.base::c+=10;
        cout << a.base::c << endl;
        cout << a.c << endl;
    
        return 0;
    }
    /*输出
    base的构造函数！ //先有父类
    A的构造函数！ //再有子类
    101
    102
    301
    302
    301
    311
    302
    A的析构函数！
    base的析构函数！
    */
    ```

  - 如果子类同时继承多个类并且存在多个同名的成员变量及函数，而且子类没有定义时，则在调用时必须指定父类作用域，否则会错误（二义性）

###### 8.3.3 多态  

- 静态多态： 
    
    - 函数重载
    - 运算符重载 
    - 函数地址早绑定，编译阶段确定函数地址  
- 动态多态： 

    - 派生类 

    - 虚函数 

    - 函数地址晚绑定，运行阶段确定函数地址  
      1. 条件： 

         - 有继承关系 
         - 子类必须要有完全“重写”父类的虚函数（一模一样），子类的重写函数“virtual”可写可不写 

      2. 使用： 
             父类的指针或者引用，指向子类的对象 

         ```c++
         base *a = new A;//在堆区创建base实例化
         base &baseA = a
         ```

         ```c++
         class base
         {
             public:
                 void show1()//父类函数
                 {
                     cout << "base1" << endl;
                 }
                 virtual void show2()//父类虚函数
                 {
                     cout << "base2" << endl;
                 }
         };
         class A:public base//继承
         {
             public:
             	void show1()//子类重写
                 {
                     cout << "A1" << endl;
                 }
                 void show2()//子类重写
                 {
                     cout << "A2" << endl;
                 }
         };
         class B:public base
         {
             public:
             	void show1()//子类重写
                 {
                     cout << "B1" << endl;
                 }
                 void show2()//子类重写
                 {
                     cout << "B2" << endl;
                 }
         };
         int main()
         {
             base *a = new A;//在堆区创建base实例化
             base *b = new B;//在堆区创建base实例化
             a->show1();//调用父类函数
             b->show1();//调用父类函数
             a->show2();//调用子类重写函数
             b->show2();//调用子类重写函数
             
             A a;
             B b;
             base &baseA = a;//创建引用
             base &baseB = b;//创建引用
             baseA.show1();//调用父类函数
             baseB.show1();//调用父类函数
             baseA.show2();//调用子类重写函数
             baseB.show2();//调用子类重写函数
             
             return 0;
         }
         /*
         base1
         base1
         A2
         B2
         */
         ```

      3. 原理： 

         - 当父类在定义虚函数时，编译器会保存一个虚函数指针，指向一个虚函数表，表中记录着虚函数的入口地址。
         - 而子类在继承时会将这个虚函数复制下来，包括虚函数指针和虚函数表，虚函数表中依然保存父类函数的地址。
         - 当子类没有重写虚函数时，子类的虚函数表保存着继承而来的父类函数的地址；
         - 当子类重写虚函数时，子类的虚函数表的父类函数地址会被子类的重写函数的函数地址替代。
         - 父类的虚函数，虚函数指针，虚函数表没有发生变化。
         - 当发生引用或指针指向时，发生动态多态，转而执行子类的函数，而子类函数地址只有在运行时才会知道。	

    - 程序演示：

    ```c++
    #include <iostream>
    using namespace std;
    
    class base
    {
        public:
            int a;
            int b;
            base()
            {
                a = 101;
                b = 102;
                cout << "base的构造函数！" << endl;
            }
            ~base()
            {
                cout << "base的析构函数！" << endl;
            }
            void show1()
            {
                cout << "base1" << endl;
            }
            virtual void show2()
            {
                cout << "base2" << endl;
            }
    };
    class A:public base
    {
        public:
            int a;
            int b;
            A()
            {
                a = 201;
                b = 202;
                cout << "A的构造函数！" << endl;
            }
            ~A()
            {
                cout << "A的析构函数！" << endl;
            }
        	void show1()
            {
                cout << "A1" << endl;
            }
            void show2()
            {
                cout << "A2" << endl;
            }
    };
    class B:public base
    {
        public:
            int a;
            int b;
            B()
            {
                a = 301;
                b = 302;
                cout << "B的构造函数！" << endl;
            }
            ~B()
            {
                cout << "B的析构函数！" << endl;
            }
        	void show1()
            {
                cout << "B1" << endl;
            }
            void show2()
            {
                cout << "B2" << endl;
            }
    };
    int main()
    {
        A a;
        B b;
        base &baseA = a;
        baseA.show1();
    	baseA.show2();
    	cout << baseA.a << endl;
    	cout << baseA.b << endl;
    	
        base &baseB = b;
        baseB.show1();
        baseB.show2();
        cout << baseB.a << endl;
        cout << baseB.b << endl;
        
        return 0;
    }
    /*
    base的构造函数！===> A的base构造
    A的构造函数！
    base的构造函数！===> B的base构造
    B的构造函数！
    base1
    A2
    101
    102
    base1
    B2
    101
    102
    B的析构函数！
    base的析构函数！===> A的base析构
    A的析构函数！
    base的析构函数！===> B的base析构
    */
    ```

###### 8.3.4 作用域  

1. 全局作用域符（::name）：用于类型名称（类、类成员、成员函数、变量等）前，表示作用域为全局命名空间
2. 类作用域符（class::name）：用于表示指定类型的作用域范围是具体某个类的
3. 命名空间作用域符（namespace::name）:用于表示指定类型的作用域范围是具体某个命名空间的

###### 8.3.5 纯虚函数与抽象类

1. 纯虚函数与抽象类

   C++中的纯虚函数(或抽象函数)是我们没有实现的虚函数！我们只需声明它！通过声明中赋值0来声明纯虚函数！

   ```c++
   class base 
   {    
   	public:
       	virtual void show() = 0; //虚函数定义
   }; 
   ```

   - 纯虚函数：没有函数体的虚函数
   - 抽象类：包含纯虚函数的类

   抽象类只能作为基类来派生新类使用，不能创建抽象类的对象（实例化），抽象类的指针和引用“->”由抽象类派生出来的类的对象！

   ```c++
   base b; // error 抽象类，不能创建对象
   base *b1; // ok 可以定义抽象类的指针
   base *b2 = new base(); // error, A是抽象类，不能创建对象
   ```

2. 实现抽象类

   抽象类中：在成员函数内可以调用纯虚函数，在构造函数/析构函数内部不能使用纯虚函数。

   如果一个类从抽象类派生而来，它必须实现了基类中的所有纯虚函数，才能成为非抽象类。

   ```c++
   class A 
   {
   	public:
           virtual void f() = 0;  // 纯虚函数
           void g()
           { 
               this->f(); //指针指向
           }
           A(){}
   };
   class B:public A
   {
       public:
           void f()
           { 
               cout<<"B:f()"<<endl;
           }
   };
   int main()
   {
       B b;
       b.g();//B:f()
       return 0;
   }
   ```

3. 重要点

   - 纯虚函数使一个类变成抽象类

     ```c++
     class Test 
     { 
         int x; 
         public: 
             virtual void show() = 0; //纯虚函数
             int getX() 
             { 
                 return x; 
             } 
     }; 
     int main(void) 
     { 
         Test t;  //error! 不能创建抽象类的对象
         return 0; 
     } 
     ```

   - 抽象类类型的指针和引用

     ```c++
     class Base
     { 
         int x; 
         public: 
             virtual void show() = 0; //纯虚函数
             int getX() 
             { 
                 return x; 
             } 
     }; 
     class Derived: public Base 
     { 
         public: 
             void show() //子类重写
             { 
                 cout << "In Derived \n" << endl; 
             } 
             Derived(){}
     }; 
     int main(void) 
     { 
         //Base b;  //error! 不能创建抽象类的对象
         //Base *b = new Base(); error!
         Base *bp = new Derived(); // 抽象类的指针和引用 -> 由抽象类派生出来的类的对象
         bp->show();
         return 0; 
     } 
     ```

   - 如果我们不在派生类中覆盖纯虚函数，那么派生类也会变成抽象类。

     ```c++
     class Base
     { 
         int x; 
         public: 
             virtual void show() = 0; 
             int getX() 
             { 
                 return x; 
             } 
     }; 
     class Derived: public Base 
     { 
         public: 
         //    void show() { } 
     }; 
     int main(void) 
     { 
         Derived d;  //error! 派生类没有实现纯虚函数，那么派生类也会变为抽象类，不能创建抽象类的对象
         return 0; 
     } 
     ```

   - 抽象类可以有构造函数

     ```c++
     class Base 
     { 
         protected: 
             int x; 
         public: 
             virtual void fun() = 0; 
             Base(int i) 
             { 
                 x = i; 
             } 
     }; 
     class Derived: public Base 
     { 
         int y; 
         public: 
         	Derived(int i, int j):Base(i)//初始化
             { 
                 y = j; 
             } 
             void fun() 
             { 
                 cout << "x = " << x << ", y = " << y; 
             } 
     }; 
     int main(void) 
     { 
         Derived d(4, 5); 
         d.fun(); //x = 4,y = 5
         return 0; 
     } 
     ```

   - 构造函数不能是虚函数，而析构函数可以是虚析构函

     ```c++
     class Base  
     {
         public:
             Base()
             { 
                 cout << "Base的构造函数" << endl; 
             }
             virtual ~Base()//可以使子类的析构函数执行
             { 
                 cout << "Base的析构函数" << endl; 
             }
     };
     class Derived: public Base 
     {
         public:
             Derived()   
             { 
                 cout << "Derived的构造函数" << endl; 
             }
             ~Derived()   
             { 
                 cout << "Derived的析构函数" << endl; 
             }
     };
     int main()  
     {
         Base *Var = new Derived();
         delete Var;
         return 0;
     }
     /*
     Base的构造函数
     Derived的构造函数
     Derived的析构函数
     Base的析构函数
     */
     ```

   - 当基类指针指向派生类对象并删除对象时，我们可能希望调用适当的析构函数。如果析构函数不是虚拟的，则只能调用基类析构函数。

###### 8.3.6 virtual

1. 虚函数的调用取决于指向或者引用的对象的类型，而不是指针或者引用自身的类型。

2. 默认参数是静态绑定的，虚函数是动态绑定的。 默认参数的使用需要看指针或者引用本身的类型，而不是对象的类型。实际上，默认参数不参与多态。

   ```c++
   class Base 
   { 
       public: 
           virtual void fun ( int x = 10 ) 
           { 
               cout << "Base::fun(), x = " << x << endl; 
           } 
   }; 
   class Derived : public Base 
   { 
       public: 
           virtual void fun ( int x=20 ) 
           { 
               cout << "Derived::fun(), x = " << x << endl; 
           } 
   }; 
   int main() 
   { 
       Derived d1; 
       Base *bp = &d1; 
       bp->fun();  // 10
       return 0; 
   } 
   //Derived::fun(), x = 10
   ```

3. 静态函数不可以声明为虚函数，同时也不能被const 和 volatile关键字修饰

   - static成员函数不属于任何类对象或类实例，所以即使给此函数加上virutal也是没有任何意义

   - 虚函数依靠vptr和vtable来处理。vptr是一个指针，在类的构造函数中创建生成，并且只能用this指针来访问它，静态成员函数没有this指针，所以无法访问vptr。

4. 构造函数不可以声明为虚函数。同时除了inline|explicit之外，构造函数不允许使用其它任何关键字。

   ```c++
   class Base 
   { 
       public: 
           Base() {} 
           virtual  ~Base() {} 
           virtual void ChangeAttributes() = 0; 
           static Base *Create(int id); 
           virtual Base *Clone() = 0; 
   }; 
   class Derived1 : public Base 
   { 
       public: 
           Derived1() 
           { 
               cout << "Derived1的构造函数！" << endl; 
           } 
           Derived1(const Derived1& rhs) 
           { 
               cout << "Derived1的拷贝函数！" << endl; 
           } 
           ~Derived1() 
           { 
               cout << "~Derived1的析构函数！" << endl; 
           } 
           void ChangeAttributes() 
           { 
               cout << "Derived1" << endl; 
           } 
           Base *Clone() 
           { 
               return new Derived1(*this); 
           } 
   }; 
   class Derived2 : public Base 
   { 
       public: 
           Derived2() 
           { 
               cout << "Derived2的构造函数！" << endl; 
           } 
           Derived2(const Derived2& rhs) 
           { 
               cout << "Derived2的拷贝函数！" << endl; 
           } 
           ~Derived2() 
           { 
               cout << "~Derived2的析构函数！" << endl; 
           } 
           void ChangeAttributes() 
           { 
               cout << "Derived2" << endl; 
           } 
           Base *Clone() 
           { 
               return new Derived2(*this); 
           } 
   }; 
   class Derived3 : public Base 
   { 
       public: 
           Derived3() 
           { 
               cout << "Derived3的构造函数！" << endl; 
           } 
           Derived3(const Derived3& rhs) 
           { 
               cout << "Derived3的拷贝函数！" << endl; 
           } 
           ~Derived3() 
           { 
               cout << "~Derived3的析构函数！" << endl; 
           } 
           void ChangeAttributes() 
           { 
               cout << "Derived3" << endl; 
           } 
           Base *Clone() 
           { 
               return new Derived3(*this); 
           } 
   }; 
   Base *Base::Create(int id) 
   {
       if( id == 1 ) 
       { 
           return new Derived1; 
       } 
       else if( id == 2 ) 
       { 
           return new Derived2; 
       } 
       else
       { 
           return new Derived3; 
       } 
   } 
   class User 
   { 
       public: 
           User() : pBase(0) 
       	{ 
               int input; 
           	cout << "输入(1, 2 or 3): "; 
           	cin >> input; 
               while( (input != 1) && (input != 2) && (input != 3) ) 
               { 
                   cout << "只能输入(1, 2 or 3):  "; 
                   cin >> input; 
               } 
          	 	pBase = Base::Create(input); 
      	 	} 
           ~User() 
           { 
               if( pBase ) 
               { 
                   delete pBase; 
                   pBase = 0; 
               } 
           } 
           void Action() 
           { 
               Base *pNewBase = pBase->Clone(); 
               pNewBase->ChangeAttributes(); 
               delete pNewBase; 
           } 
       private: 
           Base *pBase; 
   }; 
   int main() 
   { 
       User *user = new User(); //对User实例化
       user->Action(); 
       delete user; 
   } 
   ```

5. 析构函数可以声明为虚函数。如果我们需要删除一个指向派生类的基类指针时，应该把析构函数声明为虚函数。 事实上，只要一个类有可能会被其它类所继承， 就应该声明虚析构函数(哪怕该析构函数不执行任何操作)。

   ```c++
   class base 
   { 
       public: 
           base()      
           { 
               cout<<"Constructing base \n"; 
           } 
           virtual ~base() 
           { 
               cout<<"Destructing base \n"; 
           }      
   }; 
   class derived: public base 
   { 
       public: 
           derived()      
           { 
               cout<<"Constructing derived \n"; 
           } 
           ~derived() 
           { 
               cout<<"Destructing derived \n"; 
           } 
   }; 
   int main(void) 
   { 
       derived *d = new derived();   
       base *b = d; 
       delete b; 
       return 0; 
   } 
   ```

6. 通常类成员函数都会被编译器考虑是否进行内联。 但通过基类指针或者引用调用的虚函数必定不能被内联。 当然，实体对象调用虚函数或者静态调用时可以被内联，虚析构函数的静态调用也一定会被内联展开。

   ```c++
   class Base 
   { 
       public: 
           virtual void who() 
           { 
               cout << "I am Base\n"; 
           } 
   }; 
   class Derived: public Base 
   { 
       public: 
           void who() 
           {  
               cout << "I am Derived\n"; 
           } 
   }; 
   int main() 
   { 
       Base b; 
       b.who(); 
       Base *ptr = new Derived(); 
       ptr->who(); 
   
       return 0; 
   } 
   /*
   I am Base
   I am Derived
   */
   ```

   - 虚函数可以是内联函数，内联是可以修饰虚函数的，但是当虚函数表现多态性的时候不能内联。
   - 内联是在编译器建议编译器内联，而虚函数的多态性在运行期，编译器无法知道运行期调用哪个代码，因此虚函数表现为多态性时（运行期）不可以内联。
   - `inline virtual` 唯一可以内联的时候是：编译器知道所调用的对象是哪个类（如 `Base::who()`），这只有在编译器具有实际对象而不是对象的指针或引用时才会发生。

#### 9. stuct结构体

##### 9.1 C中struct

- 在C中struct只单纯的用作数据的复合类型，也就是说，在结构体声明中只能将数据成员放在里面，而不能将函数放在里面。

- 在C结构体声明中不能使用C++访问修饰符，如：public、protected、private 而在C++中可以使用。

- 在C中定义结构体变量，如果使用了下面定义必须加struct。

- C的结构体不能继承（没有这一概念）。

- 若结构体的名字与函数名相同，可以正常运行且正常的调用！例如：可以定义与 struct Base 不冲突的 void Base() {}。

  ```c++
  struct Base {            // public
      int v1; 
  	//public:      //error
          int v2; 
      //private:
          int v3; 
      //void print()// c中不能在结构体中嵌入函数
      //{       
      //    printf("%s\n","hello world");//error!
      //};    
  };
  void Base()
  {
      printf("%s\n","I am Base func");
  }
  //struct Base base1;  //ok
  //Base base2; //error
  int main() {
      struct Base base;
      base.v1=1;
      //base.print();
      printf("%d\n",base.v1);
      Base();
      return 0;
  }
  ```

##### 9.2 C++中struct

- C++结构体中不仅可以定义数据，还可以定义函数。

- C++结构体中可以使用访问修饰符，如：public、protected、private 。

- C++结构体中成员默认为public，而类中默认为private，基本只有这两个区别

- C++结构体使用可以直接使用不带struct。

- C++继承。

- 若结构体的名字与函数名相同，可以正常运行且正常的调用！但是定义结构体变量时候只用用带struct的！

- 未添加同名函数前：

  ```c++
  struct Student {};
  Student(){}
  Struct Student s; //ok
  Student s;  //ok
  ```

- 添加同名函数后：

  ```c++
  struct Student {};
  Student(){}
  Struct Student s; //ok
  Student s;  //error
  ```

##### 9.3 总结

| C                                                      | C++                                                          |
| :----------------------------------------------------- | ------------------------------------------------------------ |
| 不能将函数放在结构体声明                               | 能将函数放在结构体声明                                       |
| 在C结构体声明中不能使用C++访问修饰符。                 | public、protected、private 在C++中可以使用。                 |
| 在C中定义结构体变量，如果使用了下面定义必须加struct。  | 可以不加struct                                               |
| 结构体不能继承（没有这一概念）。                       | 可以继承                                                     |
| 若结构体的名字与函数名相同，可以正常运行且正常的调用！ | 若结构体的名字与函数名相同，使用结构体，只能使用带struct定义！ |

#### 10. 文本文件操作

##### 10.1 文本文件写操作

1. 包含头文件：

   ```c++
   #include <fstream>
   ```

2. 创建流对象：

   ```c++
   ofstream ofs;
   ```

3. 打开文件：

   ```c++
   ofs.open("文件路径",打开方式);
   ```

   打开方式：

   |    代码     |            意义            |
   | :---------: | :------------------------: |
   |   ios::in   |       为读文件而打开       |
   |  ios::out   |       为写文件而打开       |
   |  ios::ate   |      初始位置：文件尾      |
   |  ios::app   |       追加方式写文件       |
   | ios::trunc  | 如果文件存在先删除，在创建 |
   | ios::binary |         二进制方式         |

   可以使用 “|” 配合使用，如：

   ```c++
   ofs.open("文件路径",ios::app | ios::trunc);
   ```

4. 写数据：

   ```c++
   ofs << "数据" ;
   ```

5.  关闭文件：

   ```c++
   ofs.close();
   ```

6. 完整程序

   ```c++
   #include <iostream>
   #include <fstream>
   #include <string>
   using namespace std;
   int main()
   {
       ofstream ofs;//类实例化
       ofs.open("./fstream_1.txt",ios::out);
       ofs << "姓名：张三" << endl;
       ofs << "年龄：20" << endl;
       ofs << "年级：大二" << endl;
       ofs << "学历：本科在读" << endl;
       ofs.close();
       
       return 0;
   }
   ```

##### 10.2 文本文件读操作

1. 包含头文件：

   ```c++
   #include <fstream>
   ```

2. 创建流对象：

   ```c++
   ifstream ifs;
   ```

3. 打开文件并判断文件是否打开成功：

   ```c++
   ifs.open("文件路径",打开方式);
   ```

4. 读数据

   四种读取方式

   ```c++
   void read1(ifstream &ifs)
   {
       ifs.open("./fstream_1.txt",ios::in);
       if(!ifs.is_open())
       {
           cout << "文件打开失败！" << endl;
           return;
       }
       char buf[1024] = {0};
       while(ifs >> buf)
           cout << buf << endl;
   }
   ```

   ```c++
   void read2(ifstream &ifs)
   {
       ifs.open("./fstream_1.txt",ios::in);
       if(!ifs.is_open())
       {
           cout << "文件打开失败！" << endl;
           return;
       }
       char buf[1024] = {0};
       while(ifs.getline(buf,sizeof(buf)))
           cout << buf << endl;
   }
   ```

   ```c++
   void read3(ifstream &ifs)
   {
       ifs.open("./fstream_1.txt",ios::in);
       if(!ifs.is_open())
       {
           cout << "文件打开失败！" << endl;
           return;
       }
       string buf;
       while(getline(ifs,buf))
           cout << buf << endl;
   }
   ```

   ```c++
   void read4(ifstream &ifs)
   {
       ifs.open("./fstream_1.txt",ios::in);
       if(!ifs.is_open())
       {
           cout << "文件打开失败！" << endl;
           return;
       }
       char c;
       while((c=ifs.get()) != EOF)
           cout << c;
   }
   ```

5. 关闭文件：

   ```c++
   ofs.close();
   ```

6. 完整程序：

   ```c++
   #include <iostream>
   #include <fstream>
   #include <string>
   using namespace std;
   int main()
   {
       ifstream ifs;
       read1(ifs);
       read2(ifs);
       read3(ifs);
       read4(ifs);
       ifs.close();
       
       return 0;
   }
   ```

#### 11. 模板

## 初识STL库









