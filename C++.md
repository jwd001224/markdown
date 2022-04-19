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

##### 11.1.模板的概念

模板就是建立**通用的模具**，大大**提高复用性**

模板的特点：

* 模板不可以直接使用，它只是一个框架
* 模板的通用并不是万能的

##### 11.2 函数模板

* C++另一种编程思想称为 ==泛型编程== ，主要利用的技术就是模板


* C++提供两种模板机制:**函数模板**和**类模板** 

###### 11.2.1 函数模板语法

​	11.3.1. **语法**

```C++
template<typename T>
函数声明或定义
```

​	11.3.2. **解释**

​		template  ---  声明创建模板

​		typename  --- 表面其后面的符号是一种数据类型，可以用class代替

​		T    ---   通用的数据类型，名称可以替换，通常为大写字母

​	11.3.3. **示例**

```C++
//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a, T& b)
{
	T temp = a;
	a = b;
	b = temp;
}
void test01()
{
	int a = 10;
	int b = 20;
	
	//1、自动类型推导
	mySwap(a, b);//利用模板实现交换
	//2、显示指定类型
	mySwap<int>(a, b);//利用模板实现交换

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

​	11.3.4. 总结


* 函数模板利用关键字 template
* 使用函数模板有两种方式：自动类型推导、显示指定类型
* 模板的目的是为了提高复用性，将类型参数化

###### 11.2.2 函数模板案例

​	案例描述：

* 利用函数模板封装一个排序的函数，可以对**不同数据类型数组**进行排序
* 排序规则从大到小，排序算法为**选择排序**
* 分别利用**char数组**和**int数组**进行测试

​	示例：

```C++
//交换的函数模板
template<typename T>
void mySwap(T &a, T&b)
{
	T temp = a;
	a = b;
	b = temp;
}
template<class T> // 也可以替换成typename
//利用选择排序，进行对数组从大到小的排序
void mySort(T arr[], int len)
{
	for (int i = 0; i < len; i++)
	{
		int max = i; //最大数的下标
		for (int j = i + 1; j < len; j++)
		{
			if (arr[max] < arr[j])
			{
				max = j;
			}
		}
		if (max != i) //如果最大数的下标不是i，交换两者
		{
			mySwap(arr[max], arr[i]);
		}
	}
}
template<typename T>
void printArray(T arr[], int len) 
{
	for (int i = 0; i < len; i++) 
    {
		cout << arr[i] << " ";
	}
	cout << endl;
}
void test01()//测试char数组
{
	char charArr[] = "bdcfeagh";
	int num = sizeof(charArr) / sizeof(char);
	mySort(charArr, num);
	printArray(charArr, num);
}
void test02()//测试int数组
{
	int intArr[] = { 7, 5, 8, 1, 3, 9, 2, 4, 6 };
	int num = sizeof(intArr) / sizeof(int);
	mySort(intArr, num);
	printArray(intArr, num);
}
int main() 
{
	test01();
	test02();
    
	system("pause");
	return 0;
}
```

###### 11.2.3 普通函数与函数模板的调用规则

​	调用规则如下：

1. 如果函数模板和普通函数都可以实现，优先调用普通函数

2. 可以通过空模板参数列表来强制调用函数模板

3. 函数模板也可以发生**重载**

4. 如果函数模板可以产生更好的匹配,优先调用函数模板

   **示例：**

   ```C++
   //普通函数与函数模板调用规则
   void myPrint(int a, int b)
   {
   	cout << "调用的普通函数" << endl;
   }
   template<typename T>
   void myPrint(T a, T b) 
   { 
   	cout << "调用的模板函数" << endl;
   }
   template<typename T>
   void myPrint(T a, T b, T c) 
   { 
   	cout << "调用重载的模板" << endl; 
   }
   void test01()
   {
   	//1、如果函数模板和普通函数都可以实现，优先调用普通函数
   	// 注意 如果告诉编译器  普通函数是有的，但只是声明没有实现，或者不在当前文件内实现，就会报错找不到
   	int a = 10;
   	int b = 20;
   	myPrint(a, b); //调用普通函数
   
   	//2、可以通过空模板参数列表来强制调用函数模板
   	myPrint<>(a, b); //调用函数模板
   
   	//3、函数模板也可以发生重载
   	int c = 30;
   	myPrint(a, b, c); //调用重载的函数模板
   
   	//4、 如果函数模板可以产生更好的匹配,优先调用函数模板
   	char c1 = 'a';
   	char c2 = 'b';
   	myPrint(c1, c2); //调用函数模板
   }
   int main() 
   {
   	test01();
   
   	system("pause");
   	return 0;
   }
   ```

总结：既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性

###### 11.2.4 模板的局限性

**局限性：**

* 模板的通用性并不是万能的

**例如：**

```C++
template<class T>
void f(T a, T b)
{ 
    a = b;
}
```

在上述代码中提供的赋值操作，如果传入的a和b是一个数组，就无法实现了

再例如：

```C++
template<class T>
void f(T a, T b)
{ 
    if(a > b) { ... }
}
```

在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行

因此C++为了解决这种问题，提供模板的重载，可以为这些**特定的类型**提供**具体化的模板**

**示例：**

```C++
#include<iostream>
using namespace std;
#include <string>

class Person
{
	public:
        Person(string name, int age)
        {
            this->m_Name = name;
            this->m_Age = age;
        }
        string m_Name;
        int m_Age;
};
template<class T>//普通函数模板
bool myCompare(T& a, T& b)
{
	if (a == b)
	{
		return true;
	}
	return false;
}
//具体化，显示具体化的原型和定意思以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1, Person &p2)
{
	if ( p1.m_Name  == p2.m_Name && p1.m_Age == p2.m_Age)
	{
		return true;
	}
	return false;
}
void test01()
{
	int a = 10;
	int b = 20;
	//内置数据类型可以直接使用通用的函数模板
	bool ret = myCompare(a, b);
	if (ret)
	{
		cout << "a == b " << endl;
	}
	else
	{
		cout << "a != b " << endl;
	}
}

void test02()
{
	Person p1("Tom", 10);
	Person p2("Tom", 10);
	//自定义数据类型，不会调用普通的函数模板
	//可以创建具体化的Person数据类型的模板，用于特殊处理这个类型
	bool ret = myCompare(p1, p2);
	if (ret)
	{
		cout << "p1 == p2 " << endl;
	}
	else
	{
		cout << "p1 != p2 " << endl;
	}
}
int main() 
{
	test01();
	test02();

	system("pause");
	return 0;
}
```

##### 11.3 类模板

###### 11.3.1 类模板语法

类模板作用：

* 建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：** 

```c++
template<class T>
类
```

**解释：**

​	template  ---  声明创建模板

​	typename  --- 表面其后面的符号是一种数据类型，可以用class代替

​	T    ---   通用的数据类型，名称可以替换，通常为大写字母

**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType> 
class Person
{
    public:
        Person(NameType name, AgeType age)
        {
            this->mName = name;
            this->mAge = age;
        }
        void showPerson()
        {
            cout << "name: " << this->mName << " age: " << this->mAge << endl;
        }
    public:
        NameType mName;
        AgeType mAge;
};
void test01()
{
	// 指定NameType 为string类型，AgeType 为 int类型
	Person<string, int>P1("孙悟空", 999);
	P1.showPerson();
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

###### 11.3.2 类模板与函数模板区别

类模板与函数模板区别主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数


**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
    public:
        Person(NameType name, AgeType age)
        {
            this->mName = name;
            this->mAge = age;
        }
        void showPerson()
        {
            cout << "name: " << this->mName << " age: " << this->mAge << endl;
        }
    public:
        NameType mName;
        AgeType mAge;
};
//1、类模板没有自动类型推导的使用方式
void test01()
{
	// Person p("孙悟空", 1000); // 错误 类模板使用时候，不可以用自动类型推导
	Person <string ,int>p("孙悟空", 1000); //必须使用显示指定类型的方式，使用类模板
	p.showPerson();
}
//2、类模板在模板参数列表中可以有默认参数
void test02()
{
	Person <string> p("猪八戒", 999); //类模板中的模板参数列表 可以指定默认参数
	p.showPerson();
}
int main() 
{
	test01();
	test02();
    
	system("pause");
	return 0;
}
```

总结：

* 类模板使用只能用显示指定类型方式
* 类模板中的模板参数列表可以有默认参数

###### 11.3.3 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

* 普通类中的成员函数一开始就可以创建
* 类模板中的成员函数在调用时才创建

**示例：**

```C++
class Person1
{
    public:
        void showPerson1()
        {
            cout << "Person1 show" << endl;
        }
};
class Person2
{
    public:
        void showPerson2()
        {
            cout << "Person2 show" << endl;
        }
};
template<class T>
class MyClass
{
public:
	T obj;

	//类模板中的成员函数，并不是一开始就创建的，而是在模板调用时再生成
	void fun1() { obj.showPerson1(); }
	void fun2() { obj.showPerson2(); }
};
void test01()
{
	MyClass<Person1> m;
	m.fun1();
	//m.fun2();//编译会出错，说明函数调用才会去创建成员函数
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：类模板中的成员函数并不是一开始就创建的，在调用时才去创建

###### 11.3.4 类模板对象做函数参数

学习目标：

* 类模板实例化出的对象，向函数传参的方式

一共有三种传入方式：

1. 指定传入的类型   --- 直接显示对象的数据类型
2. 参数模板化           --- 将对象中的参数变为模板进行传递
3. 整个类模板化       --- 将这个对象类型 模板化进行传递

**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
    public:
        Person(NameType name, AgeType age)
        {
            this->mName = name;
            this->mAge = age;
        }
        void showPerson()
        {
            cout << "name: " << this->mName << " age: " << this->mAge << endl;
        }
    public:
        NameType mName;
        AgeType mAge;
};

//1、指定传入的类型
void printPerson1(Person<string, int> &p) 
{
	p.showPerson();
}
void test01()
{
	Person <string, int >p("孙悟空", 100);
	printPerson1(p);
}

//2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p)
{
	p.showPerson();
	cout << "T1的类型为： " << typeid(T1).name() << endl;
	cout << "T2的类型为： " << typeid(T2).name() << endl;
}
void test02()
{
	Person <string, int >p("猪八戒", 90);
	printPerson2(p);
}

//3、整个类模板化
template<class T>
void printPerson3(T & p)
{
	cout << "T的类型为： " << typeid(T).name() << endl;
	p.showPerson();

}
void test03()
{
	Person <string, int >p("唐僧", 30);
	printPerson3(p);
}

int main() 
{
	test01();
	test02();
	test03();

	system("pause");
	return 0;
}
```

总结：

* 通过类模板创建的对象，可以有三种方式向函数中进行传参
* 使用比较广泛是第一种：指定传入的类型

###### 11.3.5 类模板与继承

当类模板碰到继承时，需要注意一下几点：

* 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
* 如果不指定，编译器无法给子类分配内存
* 如果想灵活指定出父类中T的类型，子类也需变为类模板


**示例：**

```C++
template<class T>
class Base
{
	T m;
};
//class Son:public Base  //错误，c++编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son :public Base<int> //必须指定一个类型
{
};
void test01()
{
	Son c;
}
//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
    public:
        Son2()
        {
            cout << typeid(T1).name() << endl;
            cout << typeid(T2).name() << endl;
        }
};
void test02()
{
	Son2<int, char> child1;
}
int main() 
{
	test01();
	test02();

	system("pause");
	return 0;
}
```

总结：如果父类是类模板，子类需要指定出父类中T的数据类型

###### 11.3.6 类模板成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现

**示例：**

```C++
#include <string>

//类模板中成员函数类外实现
template<class T1, class T2>
class Person {
    public:
        //成员函数类内声明
        Person(T1 name, T2 age);
        void showPerson();
    private:
        T1 m_Name;
        T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) 
{
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() 
{
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}

void test01()
{
	Person<string, int> p("Tom", 20);
	p.showPerson();
}

int main()
{
	test01();

	system("pause");
	return 0;
}
```

总结：类模板中成员函数类外实现时，需要加上模板参数列表

###### 11.3.7 类模板分文件编写

学习目标：

* 掌握类模板成员函数分文件编写产生的问题以及解决方式

问题：

* 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到


解决：

* 解决方式1：直接包含.cpp源文件
* 解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制


**示例：**

person.hpp中代码：

```C++
#pragma once
#include <iostream>
using namespace std;
#include <string>

template<class T1, class T2>
class Person 
{
    public:
        Person(T1 name, T2 age);
        void showPerson();
    private:
        T1 m_Name;
        T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) 
{
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() 
{
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
```

类模板分文件编写.cpp中代码

```C++
#include<iostream>
using namespace std;

//解决方式2，将声明和实现写到一起，文件后缀名改为.hpp
#include "person.hpp"
void test01()
{
	Person<string, int> p("Tom", 10);
	p.showPerson();
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp

###### 11.3.8 类模板与友元

学习目标：

* 掌握类模板配合友元函数的类内和类外实现

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在

**示例：**

```C++
#include <string>

//2、全局函数配合友元  类外实现 - 先做函数模板声明，下方在做函数模板定义，在做友元
template<class T1, class T2> class Person;

//如果声明了函数模板，可以将实现写到后面，否则需要将实现体写到类的前面让编译器提前看到
//template<class T1, class T2> void printPerson2(Person<T1, T2> & p); 

template<class T1, class T2>
void printPerson2(Person<T1, T2> & p)
{
	cout << "类外实现 ---- 姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
}
template<class T1, class T2>
class Person
{
        //1、全局函数配合友元   类内实现
        friend void printPerson(Person<T1, T2> & p)
        {
            cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
        }
        //2、全局函数配合友元   类外实现
        friend void printPerson2<>(Person<T1, T2> & p);
    public:
        Person(T1 name, T2 age)
        {
            this->m_Name = name;
            this->m_Age = age;
        }
    private:
        T1 m_Name;
        T2 m_Age;

};

//1、全局函数在类内实现
void test01()
{
	Person <string, int >p("Tom", 20);
	printPerson(p);
}

//2、全局函数在类外实现
void test02()
{
	Person <string, int >p("Jerry", 30);
	printPerson2(p);
}
int main()
{
	//test01();
	test02();

	system("pause");
	return 0;
}
```

总结：建议全局函数做类内实现，用法简单，而且编译器可以直接识别

###### 11.3.9 类模板案例

案例描述:  实现一个通用的数组类，要求如下：

* 可以对内置数据类型以及自定义数据类型的数据进行存储
* 将数组中的数据存储到堆区
* 构造函数中可以传入数组的容量
* 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
* 提供尾插法和尾删法对数组中的数据进行增加和删除
* 可以通过下标的方式访问数组中的元素
* 可以获取数组中当前元素个数和数组的容量

**示例：**

myArray.hpp中代码

```C++
#pragma once
#include <iostream>
using namespace std;

template<class T>
class MyArray
{
    public:
        MyArray(int capacity)//构造函数
        {
            this->m_Capacity = capacity;
            this->m_Size = 0;
            pAddress = new T[this->m_Capacity];
        }
        MyArray(const MyArray & arr)//拷贝构造
        {
            this->m_Capacity = arr.m_Capacity;
            this->m_Size = arr.m_Size;
            this->pAddress = new T[this->m_Capacity];
            for (int i = 0; i < this->m_Size; i++)
            {
                //如果T为对象，而且还包含指针，必须需要重载 = 操作符，因为这个等号不是 构造 而是赋值，
                // 普通类型可以直接= 但是指针类型需要深拷贝
                this->pAddress[i] = arr.pAddress[i];
            }
        }
        MyArray& operator=(const MyArray& myarray) //重载= 操作符  防止浅拷贝问题
        {
            if (this->pAddress != NULL) 
            {
                delete[] this->pAddress;
                this->m_Capacity = 0;
                this->m_Size = 0;
            }

            this->m_Capacity = myarray.m_Capacity;
            this->m_Size = myarray.m_Size;
            this->pAddress = new T[this->m_Capacity];
            for (int i = 0; i < this->m_Size; i++) 
            {
                this->pAddress[i] = myarray[i];
            }
            return *this;
        }
        T& operator[](int index)//重载[] 操作符  arr[0]
        {
            return this->pAddress[index]; //不考虑越界，用户自己去处理
        }
        void Push_back(const T & val)//尾插法
        {
            if (this->m_Capacity == this->m_Size)
            {
                return;
            }
            this->pAddress[this->m_Size] = val;
            this->m_Size++;
        }
        void Pop_back()//尾删法
        {
            if (this->m_Size == 0)
            {
                return;
            }
            this->m_Size--;
        }
        int getCapacity()//获取数组容量
        {
            return this->m_Capacity;
        }
        int	getSize()//获取数组大小
        {
            return this->m_Size;
        }
        ~MyArray()//析构
        {
            if (this->pAddress != NULL)
            {
                delete[] this->pAddress;
                this->pAddress = NULL;
                this->m_Capacity = 0;
                this->m_Size = 0;
            }
        }
    private:
        T * pAddress;  //指向一个堆空间，这个空间存储真正的数据
        int m_Capacity; //容量
        int m_Size;   // 大小
};
```

类模板案例—数组类封装.cpp中

```C++
#include "myArray.hpp"
#include <string>

void printIntArray(MyArray<int>& arr) 
{
	for (int i = 0; i < arr.getSize(); i++) 
    {
		cout << arr[i] << " ";
	}
	cout << endl;
}

//测试内置数据类型
void test01()
{
	MyArray<int> array1(10);
	for (int i = 0; i < 10; i++)
	{
		array1.Push_back(i);
	}
	cout << "array1打印输出：" << endl;
	printIntArray(array1);
	cout << "array1的大小：" << array1.getSize() << endl;
	cout << "array1的容量：" << array1.getCapacity() << endl;

	cout << "--------------------------" << endl;

	MyArray<int> array2(array1);
	array2.Pop_back();
	cout << "array2打印输出：" << endl;
	printIntArray(array2);
	cout << "array2的大小：" << array2.getSize() << endl;
	cout << "array2的容量：" << array2.getCapacity() << endl;
}

//测试自定义数据类型
class Person 
{
    public:
        Person() {} 
        Person(string name, int age) 
        {
            this->m_Name = name;
            this->m_Age = age;
       	}    
    public:
        string m_Name;
        int m_Age;
};

void printPersonArray(MyArray<Person>& personArr)
{
	for (int i = 0; i < personArr.getSize(); i++) 
    {
		cout << "姓名：" << personArr[i].m_Name << " 年龄： " << personArr[i].m_Age << endl;
	}
}

void test02()
{
	//创建数组
	MyArray<Person> pArray(10);
	Person p1("孙悟空", 30);
	Person p2("韩信", 20);
	Person p3("妲己", 18);
	Person p4("王昭君", 15);
	Person p5("赵云", 24);

	//插入数据
	pArray.Push_back(p1);
	pArray.Push_back(p2);
	pArray.Push_back(p3);
	pArray.Push_back(p4);
	pArray.Push_back(p5);

	printPersonArray(pArray);

	cout << "pArray的大小：" << pArray.getSize() << endl;
	cout << "pArray的容量：" << pArray.getCapacity() << endl;

}

int main()
{
	//test01();
	test02();

	system("pause");
	return 0;
}
```

总结：

能够利用所学知识点实现通用的数组

## 初识STL库

### 1 STL初识

#### 1.1 STL的诞生

* 长久以来，软件界一直希望建立一种可重复利用的东西

* C++的**面向对象**和**泛型编程**思想，目的就是**复用性的提升**

* 大多情况下，数据结构和算法都未能有一套标准,导致被迫从事大量重复工作

* 为了建立数据结构和算法的一套标准,诞生了**STL**



#### 1.2 STL基本概念

* STL(Standard Template Library,**标准模板库**)
* STL 从广义上分为: **容器(container) 算法(algorithm) 迭代器(iterator)**
* **容器**和**算法**之间通过**迭代器**进行无缝连接。
* STL 几乎所有的代码都采用了模板类或者模板函数

#### 1.3 STL六大组件

STL大体分为六大组件，分别是:**容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器**

1. 容器：各种数据结构，如vector、list、deque、set、map等,用来存放数据。
2. 算法：各种常用的算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的胶合剂。
4. 仿函数：行为类似函数，可作为算法的某种策略。
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西。
6. 空间配置器：负责空间的配置与管理。

#### 1.4  STL中容器、算法、迭代器

**容器：**置物之所也

STL**容器**就是将运用**最广泛的一些数据结构**实现出来

常用的数据结构：数组, 链表,树, 栈, 队列, 集合, 映射表 等

这些容器分为**序列式容器**和**关联式容器**两种:

​	**序列式容器**:强调值的排序，序列式容器中的每个元素均有固定的位置。
​	**关联式容器**:二叉树结构，各元素之间没有严格的物理上的顺序关系



**算法：**问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们叫做算法(Algorithms)

算法分为:**质变算法**和**非质变算法**。

**质变算法**：是指运算过程中会更改区间内的元素的内容。例如拷贝，替换，删除等等

**非质变算法**：是指运算过程中不会更改区间内的元素内容，例如查找、计数、遍历、寻找极值等等



**迭代器：**容器和算法之间粘合剂

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式。

每个容器都有自己专属的迭代器

迭代器使用非常类似于指针，初学阶段我们可以先理解迭代器为指针



迭代器种类：

| 种类           | 功能                                                     | 支持运算                                |
| -------------- | -------------------------------------------------------- | --------------------------------------- |
| 输入迭代器     | 对数据的只读访问                                         | 只读，支持++、==、！=                   |
| 输出迭代器     | 对数据的只写访问                                         | 只写，支持++                            |
| 前向迭代器     | 读写操作，并能向前推进迭代器                             | 读写，支持++、==、！=                   |
| 双向迭代器     | 读写操作，并能向前和向后操作                             | 读写，支持++、--，                      |
| 随机访问迭代器 | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持++、--、[n]、-n、<、<=、>、>= |

常用的容器中迭代器种类为双向迭代器，和随机访问迭代器

#### 1.5 容器算法迭代器初识

了解STL中容器、算法、迭代器概念之后，我们利用代码感受STL的魅力

STL中最常用的容器为Vector，可以理解为数组，下面我们将学习如何向这个容器中插入数据、并遍历这个容器

##### 1.5.1 vector存放内置数据类型

容器：     `vector`

算法：     `for_each`

迭代器： `vector<int>::iterator`

**示例：**

```C++
#include <vector>
#include <algorithm>

void MyPrint(int val)
{
	cout << val << endl;
}
void test01() 
{
	//创建vector容器对象，并且通过模板参数指定容器中存放的数据的类型
	vector<int> v;
	//向容器中放数据
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);

	//每一个容器都有自己的迭代器，迭代器是用来遍历容器中的元素
	//v.begin()返回迭代器，这个迭代器指向容器中第一个数据
	//v.end()返回迭代器，这个迭代器指向容器元素的最后一个元素的下一个位置
	//vector<int>::iterator 拿到vector<int>这种容器的迭代器类型

	vector<int>::iterator pBegin = v.begin();
	vector<int>::iterator pEnd = v.end();

	//第一种遍历方式：
	while (pBegin != pEnd) 
    {
		cout << *pBegin << endl;
		pBegin++;
	}

	//第二种遍历方式：
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << *it << endl;
	}
	cout << endl;

	//第三种遍历方式：
	//使用STL提供标准遍历算法  头文件 algorithm
	for_each(v.begin(), v.end(), MyPrint);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

##### 1.5.2 Vector存放自定义数据类型

学习目标：vector中存放自定义数据类型，并打印输出

**示例：**

```c++
#include <vector>
#include <string>

//自定义数据类型
class Person 
{
    public:
        Person(string name, int age) 
        {
            mName = name;
            mAge = age;
        }
    public:
        string mName;
        int mAge;
};

//存放对象
void test01() 
{
	vector<Person> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);

	for (vector<Person>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << "Name:" << (*it).mName << " Age:" << (*it).mAge << endl;
	}
}

//放对象指针
void test02() 
{
	vector<Person*> v;

	//创建数据
	Person p1("aaa", 10);
	Person p2("bbb", 20);
	Person p3("ccc", 30);
	Person p4("ddd", 40);
	Person p5("eee", 50);

	v.push_back(&p1);
	v.push_back(&p2);
	v.push_back(&p3);
	v.push_back(&p4);
	v.push_back(&p5);

	for (vector<Person*>::iterator it = v.begin(); it != v.end(); it++) 
    {
		Person * p = (*it);
		cout << "Name:" << p->mName << " Age:" << (*it)->mAge << endl;
	}
}
int main() 
{
	test01();
	test02();

	system("pause");
	return 0;
}
```

##### 1.5.3 Vector容器嵌套容器

学习目标：容器中嵌套容器，我们将所有数据进行遍历输出

**示例：**

```C++
#include <vector>

//容器嵌套容器
void test01() 
{
	vector< vector<int> >  v;

	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<int> v4;

	for (int i = 0; i < 4; i++) 
    {
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
		v4.push_back(i + 4);
	}

	//将容器元素插入到vector v中
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	v.push_back(v4);

	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) 
    {
		for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++) 
        {
			cout << *vit << " ";
		}
		cout << endl;
	}
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

### 2 STL- 常用容器

#### 2.1 string容器

##### 2.1.1 string基本概念

**本质：**

* string是C++风格的字符串，而string本质上是一个类

**string和char * 区别：**

* char * 是一个指针
* string是一个类，类内部封装了char\*，管理这个字符串，是一个char*型的容器。

**特点：**

string 类内部封装了很多成员方法

例如：查找find，拷贝copy，删除delete 替换replace，插入insert

string管理char*所分配的内存，不用担心复制越界和取值越界等，由类内部进行负责

##### 2.1.2 string构造函数

构造函数原型：

* `string();`          				//创建一个空的字符串 例如: string str;
* `string(const char* s);`	        //使用字符串s初始化
* `string(const string& str);`    //使用一个string对象初始化另一个string对象
* `string(int n, char c);`           //使用n个字符c初始化 

**示例：**

```C++
#include <string>
//string构造
void test01()
{
	string s1; //创建空字符串，调用无参构造函数
	cout << "str1 = " << s1 << endl;

	const char* str = "hello world";
	string s2(str); //把c_string转换成了string

	cout << "str2 = " << s2 << endl;

	string s3(s2); //调用拷贝构造函数
	cout << "str3 = " << s3 << endl;

	string s4(10, 'a');
	cout << "str3 = " << s3 << endl;
}

int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：string的多种构造方式没有可比性，灵活使用即可

##### 2.1.3 string赋值操作

功能描述：

* 给string字符串进行赋值

赋值的函数原型：

* `string& operator=(const char* s);`             //char*类型字符串 赋值给当前的字符串
* `string& operator=(const string &s);`         //把字符串s赋给当前的字符串
* `string& operator=(char c);`                          //字符赋值给当前的字符串
* `string& assign(const char *s);`                  //把字符串s赋给当前的字符串
* `string& assign(const char *s, int n);`     //把字符串s的前n个字符赋给当前的字符串
* `string& assign(const string &s);`              //把字符串s赋给当前字符串
* `string& assign(int n, char c);`                  //用n个字符c赋给当前字符串


**示例：**

```C++
//赋值
void test01()
{
	string str1;
	str1 = "hello world";
	cout << "str1 = " << str1 << endl;

	string str2;
	str2 = str1;
	cout << "str2 = " << str2 << endl;

	string str3;
	str3 = 'a';
	cout << "str3 = " << str3 << endl;

	string str4;
	str4.assign("hello c++");
	cout << "str4 = " << str4 << endl;

	string str5;
	str5.assign("hello c++",5);
	cout << "str5 = " << str5 << endl;


	string str6;
	str6.assign(str5);
	cout << "str6 = " << str6 << endl;

	string str7;
	str7.assign(5, 'x');
	cout << "str7 = " << str7 << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

​	string的赋值方式很多，`operator=`  这种方式是比较实用的

##### 2.1.4 string字符串拼接

**功能描述：**

* 实现在字符串末尾拼接字符串

**函数原型：**

* `string& operator+=(const char* str);`                   //重载+=操作符
* `string& operator+=(const char c);`                         //重载+=操作符
* `string& operator+=(const string& str);`                //重载+=操作符
* `string& append(const char *s); `                               //把字符串s连接到当前字符串结尾
* `string& append(const char *s, int n);`                 //把字符串s的前n个字符连接到当前字符串结尾
* `string& append(const string &s);`                           //同operator+=(const string& str)
* `string& append(const string &s, int pos, int n);`//字符串s中从pos开始的n个字符连接到字符串结尾


**示例：**


```C++
//字符串拼接
void test01()
{
	string str1 = "我";

	str1 += "爱玩游戏";

	cout << "str1 = " << str1 << endl;
	
	str1 += ':';

	cout << "str1 = " << str1 << endl;

	string str2 = "LOL DNF";

	str1 += str2;

	cout << "str1 = " << str1 << endl;

	string str3 = "I";
	str3.append(" love ");
	str3.append("game abcde", 4);
	//str3.append(str2);
	str3.append(str2, 4, 3); // 从下标4位置开始 ，截取3个字符，拼接到字符串末尾
	cout << "str3 = " << str3 << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：字符串拼接的重载版本很多，初学阶段记住几种即可

##### 2.1.5 string查找和替换

**功能描述：**

* 查找：查找指定字符串是否存在
* 替换：在指定的位置替换字符串

**函数原型：**

* `int find(const string& str, int pos = 0) const;`              //查找str第一次出现位置,从pos开始查找
* `int find(const char* s, int pos = 0) const; `                     //查找s第一次出现位置,从pos开始查找
* `int find(const char* s, int pos, int n) const; `               //从pos位置查找s的前n个字符第一次位置
* `int find(const char c, int pos = 0) const; `                       //查找字符c第一次出现位置
* `int rfind(const string& str, int pos = npos) const;`      //查找str最后一次位置,从pos开始查找
* `int rfind(const char* s, int pos = npos) const;`              //查找s最后一次出现位置,从pos开始查找
* `int rfind(const char* s, int pos, int n) const;`              //从pos查找s的前n个字符最后一次位置
* `int rfind(const char c, int pos = 0) const;  `                      //查找字符c最后一次出现位置
* `string& replace(int pos, int n, const string& str); `       //替换从pos开始n个字符为字符串str
* `string& replace(int pos, int n,const char* s); `                 //替换从pos开始的n个字符为字符串s


**示例：**

```C++
//查找和替换
void test01()
{
	//查找
	string str1 = "abcdefgde";

	int pos = str1.find("de");

	if (pos == -1)
	{
		cout << "未找到" << endl;
	}
	else
	{
		cout << "pos = " << pos << endl;
	}
	

	pos = str1.rfind("de");

	cout << "pos = " << pos << endl;

}
void test02()
{
	//替换
	string str1 = "abcdefgde";
	str1.replace(1, 3, "1111");

	cout << "str1 = " << str1 << endl;
}

int main() 
{
	//test01();
	//test02();

	system("pause");
	return 0;
}
```

总结：

* find查找是从左往后，rfind从右往左
* find找到字符串后返回查找的第一个字符位置，找不到返回-1
* replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串


#####    2.1.6 string字符串比较

**功能描述：**

* 字符串之间的比较

**比较方式：**

* 字符串比较是按字符的ASCII码进行对比

= 返回   0

\> 返回   1 

< 返回  -1

**函数原型：**

* `int compare(const string &s) const; `  //与字符串s比较
* `int compare(const char *s) const;`      //与字符串s比较

**示例：**

```C++
//字符串比较
void test01()
{

	string s1 = "hello";
	string s2 = "aello";

	int ret = s1.compare(s2);

	if (ret == 0) 
    {
		cout << "s1 等于 s2" << endl;
	}
	else if (ret > 0)
	{
		cout << "s1 大于 s2" << endl;
	}
	else
	{
		cout << "s1 小于 s2" << endl;
	}

}

int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：字符串对比主要是用于比较两个字符串是否相等，判断谁大谁小的意义并不是很大

##### 2.1.7 string字符存取

string中单个字符存取方式有两种

* `char& operator[](int n); `     //通过[]方式取字符
* `char& at(int n);   `                    //通过at方法获取字符

**示例：**

```C++
void test01()
{
	string str = "hello world";

	for (int i = 0; i < str.size(); i++)
	{
		cout << str[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < str.size(); i++)
	{
		cout << str.at(i) << " ";
	}
	cout << endl;

	//字符修改
	str[0] = 'x';
	str.at(1) = 'x';
	cout << str << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：string字符串中单个字符存取有两种方式，利用 [ ] 或 at

##### 2.1.8 string插入和删除

**功能描述：**

* 对string字符串进行插入和删除字符操作

**函数原型：**

* `string& insert(int pos, const char* s);  `                //插入字符串
* `string& insert(int pos, const string& str); `        //插入字符串
* `string& insert(int pos, int n, char c);`                //在指定位置插入n个字符c
* `string& erase(int pos, int n = npos);`                    //删除从Pos开始的n个字符 

**示例：**

```C++
//字符串插入和删除
void test01()
{
	string str = "hello";
	str.insert(1, "111");
	cout << str << endl;

	str.erase(1, 3);  //从1号位置开始3个字符
	cout << str << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

**总结：**插入和删除的起始下标都是从0开始

##### 2.1.9 string子串

**功能描述：**

* 从字符串中获取想要的子串

**函数原型：**

* `string substr(int pos = 0, int n = npos) const;`   //返回由pos开始的n个字符组成的字符串


**示例：**

```C++
//子串
void test01()
{

	string str = "abcdefg";
	string subStr = str.substr(1, 3);
	cout << "subStr = " << subStr << endl;

	string email = "hello@sina.com";
	int pos = email.find("@");
	string username = email.substr(0, pos);
	cout << "username: " << username << endl;

}

int main() 
{
	test01();

	system("pause");
	return 0;
}
```

**总结：**灵活的运用求子串功能，可以在实际开发中获取有效的信息

#### 2.2 vector容器

##### 2.2.1 vector基本概念

**功能：**

* vector数据结构和**数组非常相似**，也称为**单端数组**

**vector与普通数组区别：**

* 不同之处在于数组是静态空间，而vector可以**动态扩展**

**动态扩展：**

* 并不是在原空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间

![说明: 2015-11-10_151152](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002.jpg)



* vector容器的迭代器是支持随机访问的迭代器

##### 2.2.2 vector构造函数

**功能描述：**

* 创建vector容器

**函数原型：**

* `vector<T> v; `               		     //采用模板实现类实现，默认构造函数
* `vector(v.begin(), v.end());   `       //将v[begin(), end())区间中的元素拷贝给本身。
* `vector(n, elem);`                            //构造函数将n个elem拷贝给本身。
* `vector(const vector &vec);`         //拷贝构造函数。


**示例：**


```C++
#include <vector>

void printVector(vector<int>& v) 
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
void test01()
{
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int> v2(v1.begin(), v1.end());
	printVector(v2);

	vector<int> v3(10, 100);
	printVector(v3);
	
	vector<int> v4(v3);
	printVector(v4);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

**总结：**vector的多种构造方式没有可比性，灵活使用即可

##### 2.2.3 vector赋值操作

**功能描述：**

* 给vector容器进行赋值

**函数原型：**

* `vector& operator=(const vector &vec);`//重载等号操作符


* `assign(beg, end);`       //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`        //将n个elem拷贝赋值给本身。

**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) 
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}

//赋值操作
void test01()
{
	vector<int> v1; //无参构造
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	v2 = v1;
	printVector(v2);

	vector<int>v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);

	vector<int>v4;
	v4.assign(10, 100);
	printVector(v4);
}

int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结： vector赋值方式比较简单，使用operator=，或者assign都可以

##### 2.2.4  vector容量和大小

**功能描述：**

* 对vector容器的容量和大小操作

**函数原型：**

* `empty(); `                            //判断容器是否为空

* `capacity();`                      //容器的容量

* `size();`                              //返回容器中元素的个数

* `resize(int num);`             //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  ​					      //如果容器变短，则末尾超出容器长度的元素被删除。

* `resize(int num, elem);`  //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

  ​				              //如果容器变短，则末尾超出容器长度的元素被删除


**示例：**


```C++
#include <vector>

void printVector(vector<int>& v) 
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
void test01()
{
	vector<int> v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);
	if (v1.empty())
	{
		cout << "v1为空" << endl;
	}
	else
	{
		cout << "v1不为空" << endl;
		cout << "v1的容量 = " << v1.capacity() << endl;
		cout << "v1的大小 = " << v1.size() << endl;
	}

	//resize 重新指定大小 ，若指定的更大，默认用0填充新位置，可以利用重载版本替换默认填充
	v1.resize(15,10);
	printVector(v1);

	//resize 重新指定大小 ，若指定的更小，超出部分元素被删除
	v1.resize(5);
	printVector(v1);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 判断是否为空  --- empty
* 返回元素个数  --- size
* 返回容器容量  --- capacity
* 重新指定大小  ---  resize

##### 2.2.5 vector插入和删除

**功能描述：**

* 对vector容器进行插入、删除操作

**函数原型：**

* `push_back(ele);`                                         //尾部插入元素ele
* `pop_back();`                                                //删除最后一个元素
* `insert(const_iterator pos, ele);`        //迭代器指向位置pos插入元素ele
* `insert(const_iterator pos, int count,ele);`//迭代器指向位置pos插入count个元素ele
* `erase(const_iterator pos);`                     //删除迭代器指向的元素
* `erase(const_iterator start, const_iterator end);`//删除迭代器从start到end之间的元素
* `clear();`                                                        //删除容器中所有元素

**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) 
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}

//插入和删除
void test01()
{
	vector<int> v1;
	//尾插
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v1.push_back(40);
	v1.push_back(50);
	printVector(v1);
	//尾删
	v1.pop_back();
	printVector(v1);
	//插入
	v1.insert(v1.begin(), 100);
	printVector(v1);

	v1.insert(v1.begin(), 2, 1000);
	printVector(v1);

	//删除
	v1.erase(v1.begin());
	printVector(v1);

	//清空
	v1.erase(v1.begin(), v1.end());
	v1.clear();
	printVector(v1);
}

int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 尾插  --- push_back
* 尾删  --- pop_back
* 插入  --- insert    (位置迭代器)
* 删除  --- erase  （位置迭代器）
* 清空  ---  clear  

##### 2.2.6 vector数据存取

**功能描述：**

* 对vector中的数据的存取操作

**函数原型：**

* `at(int idx); `     //返回索引idx所指的数据
* `operator[]; `       //返回索引idx所指的数据
* `front(); `            //返回容器中第一个数据元素
* `back();`              //返回容器中最后一个数据元素

**示例：**

```C++
#include <vector>

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < v1.size(); i++)
	{
		cout << v1.at(i) << " ";
	}
	cout << endl;

	cout << "v1的第一个元素为： " << v1.front() << endl;
	cout << "v1的最后一个元素为： " << v1.back() << endl;
}

int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 除了用迭代器获取vector容器中元素，[ ]和at也可以
* front返回容器第一个元素
* back返回容器最后一个元素

##### 2.2.7 vector互换容器

**功能描述：**

* 实现两个容器内元素进行互换

**函数原型：**

* `swap(vec);`  // 将vec与本身的元素互换

**示例：**

```C++
#include <vector>

void printVector(vector<int>& v) 
{
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}

void test01()
{
	vector<int>v1;
	for (int i = 0; i < 10; i++)
	{
		v1.push_back(i);
	}
	printVector(v1);

	vector<int>v2;
	for (int i = 10; i > 0; i--)
	{
		v2.push_back(i);
	}
	printVector(v2);

	//互换容器
	cout << "互换后" << endl;
	v1.swap(v2);
	printVector(v1);
	printVector(v2);
}

void test02()
{
	vector<int> v;
	for (int i = 0; i < 100000; i++) 
    {
		v.push_back(i);
	}

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	//收缩内存
	vector<int>(v).swap(v); //匿名对象

	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;
}

int main() 
{
	test01();
	test02();

	system("pause");
	return 0;
}
```

总结：swap可以使两个容器互换，可以达到实用的收缩内存效果

##### 2.2.8 vector预留空间

**功能描述：**

* 减少vector在动态扩展容量时的扩展次数

**函数原型：**

* `reserve(int len);`//容器预留len个元素长度，预留位置不初始化，元素不可访问。

**示例：**

```C++
#include <vector>

void test01()
{
	vector<int> v;

	//预留空间
	v.reserve(100000);

	int num = 0;
	int* p = NULL;
	for (int i = 0; i < 100000; i++) 
    {
		v.push_back(i);
		if (p != &v[0]) 
        {
			p = &v[0];
			num++;
		}
	}

	cout << "num:" << num << endl;
}

int main() 
{
	test01();
    
	system("pause");
	return 0;
}
```

总结：如果数据量较大，可以一开始利用reserve预留空间

#### 2.3 deque容器

##### 2.3.1 deque容器基本概念

**功能：**

* 双端数组，可以对头端进行插入删除操作

**deque与vector区别：**

* vector对于头部的插入删除效率低，数据量越大，效率越低
* deque相对而言，对头部的插入删除速度回比vector快
* vector访问元素时的速度会比deque快,这和两者内部实现有关

![说明: 2015-11-19_204101](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002-1547547642923.jpg)



deque内部工作原理:

deque内部有个**中控器**，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间

![clip_image002-1547547896341](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002-1547547896341.jpg)

* deque容器的迭代器也是支持随机访问的

##### 2.3.2 deque构造函数

**功能描述：**

* deque容器构造

**函数原型：**

* `deque<T>` deqT;                      //默认构造形式
* `deque(beg, end);`                  //构造函数将[beg, end)区间中的元素拷贝给本身。
* `deque(n, elem);`                    //构造函数将n个elem拷贝给本身。
* `deque(const deque &deq);`   //拷贝构造函数

**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//deque构造
void test01() 
{
	deque<int> d1; //无参构造函数
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);
	deque<int> d2(d1.begin(),d1.end());
	printDeque(d2);

	deque<int>d3(10,100);
	printDeque(d3);

	deque<int>d4 = d3;
	printDeque(d4);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

**总结：**deque容器和vector容器的构造方式几乎一致，灵活使用即可

##### 2.3.3 deque赋值操作

**功能描述：**

* 给deque容器进行赋值

**函数原型：**

* `deque& operator=(const deque &deq); `         //重载等号操作符


* `assign(beg, end);`                                           //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`                                             //将n个elem拷贝赋值给本身。

**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//赋值操作
void test01()
{
	deque<int> d1;
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);

	deque<int>d2;
	d2 = d1;
	printDeque(d2);

	deque<int>d3;
	d3.assign(d1.begin(), d1.end());
	printDeque(d3);

	deque<int>d4;
	d4.assign(10, 100);
	printDeque(d4);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：deque赋值操作也与vector相同，需熟练掌握

##### 2.3.4 deque大小操作

**功能描述：**

* 对deque容器的大小进行操作

**函数原型：**

* `deque.empty();`                       //判断容器是否为空

* `deque.size();`                         //返回容器中元素的个数

* `deque.resize(num);`                //重新指定容器的长度为num,若容器变长，则以默认值填充新位置。

  ​			                             //如果容器变短，则末尾超出容器长度的元素被删除。

* `deque.resize(num, elem);`     //重新指定容器的长度为num,若容器变长，则以elem值填充新位置。

  ​                                                     //如果容器变短，则末尾超出容器长度的元素被删除。


**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//大小操作
void test01()
{
	deque<int> d1;
	for (int i = 0; i < 10; i++)
	{
		d1.push_back(i);
	}
	printDeque(d1);
	//判断容器是否为空
	if (d1.empty()) 
    {
		cout << "d1为空!" << endl;
	}
	else 
    {
		cout << "d1不为空!" << endl;
		//统计大小
		cout << "d1的大小为：" << d1.size() << endl;
	}
	//重新指定大小
	d1.resize(15, 1);
	printDeque(d1);

	d1.resize(5);
	printDeque(d1);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* deque没有容量的概念
* 判断是否为空   --- empty
* 返回元素个数   --- size
* 重新指定个数   --- resize

##### 2.3.5 deque 插入和删除

**功能描述：**

* 向deque容器中插入和删除数据

**函数原型：**

两端插入操作：

- `push_back(elem);`          //在容器尾部添加一个数据
- `push_front(elem);`        //在容器头部插入一个数据
- `pop_back();`                   //删除容器最后一个数据
- `pop_front();`                 //删除容器第一个数据

指定位置操作：

* `insert(pos,elem);`         //在pos位置插入一个elem元素的拷贝，返回新数据的位置。

* `insert(pos,n,elem);`     //在pos位置插入n个elem数据，无返回值。

* `insert(pos,beg,end);`    //在pos位置插入[beg,end)区间的数据，无返回值。

* `clear();`                           //清空容器的所有数据

* `erase(beg,end);`             //删除[beg,end)区间的数据，返回下一个数据的位置。

* `erase(pos);`                    //删除pos位置的数据，返回下一个数据的位置。

**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//两端操作
void test01()
{
	deque<int> d;
	//尾插
	d.push_back(10);
	d.push_back(20);
	//头插
	d.push_front(100);
	d.push_front(200);

	printDeque(d);

	//尾删
	d.pop_back();
	//头删
	d.pop_front();
	printDeque(d);
}
//插入
void test02()
{
	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);
	printDeque(d);

	d.insert(d.begin(), 1000);
	printDeque(d);

	d.insert(d.begin(), 2,10000);
	printDeque(d);

	deque<int>d2;
	d2.push_back(1);
	d2.push_back(2);
	d2.push_back(3);

	d.insert(d.begin(), d2.begin(), d2.end());
	printDeque(d);
}
//删除
void test03()
{
	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);
	printDeque(d);

	d.erase(d.begin());
	printDeque(d);

	d.erase(d.begin(), d.end());
	d.clear();
	printDeque(d);
}
int main() 
{
	//test01();
	//test02();
    test03();
    
	system("pause");
	return 0;
}
```

总结：

* 插入和删除提供的位置是迭代器！
* 尾插   ---  push_back
* 尾删   ---  pop_back
* 头插   ---  push_front
* 头删   ---  pop_front

##### 2.3.6 deque 数据存取

**功能描述：**

* 对deque 中的数据的存取操作

**函数原型：**

- `at(int idx); `     //返回索引idx所指的数据
- `operator[]; `      //返回索引idx所指的数据
- `front(); `            //返回容器中第一个数据元素
- `back();`              //返回容器中最后一个数据元素

**示例：**

```C++
#include <deque>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//数据存取
void test01()
{
	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);

	for (int i = 0; i < d.size(); i++) 
    {
		cout << d[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < d.size(); i++) 
    {
		cout << d.at(i) << " ";
	}
	cout << endl;

	cout << "front:" << d.front() << endl;
	cout << "back:" << d.back() << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

- 除了用迭代器获取deque容器中元素，[ ]和at也可以
- front返回容器第一个元素
- back返回容器最后一个元素

##### 2.3.7  deque 排序

**功能描述：**

* 利用算法实现对deque容器进行排序

**算法：**

* `sort(iterator beg, iterator end)`  //对beg和end区间内元素进行排序

**示例：**

```C++
#include <deque>
#include <algorithm>

void printDeque(const deque<int>& d) 
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
void test01()
{
	deque<int> d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);

	printDeque(d);
	sort(d.begin(), d.end());
	printDeque(d);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：sort算法非常实用，使用时包含头文件 algorithm即可

#### 2.4 stack容器

##### 2.4.1 stack 基本概念

**概念：**stack是一种**先进后出**(First In Last Out,FILO)的数据结构，它只有一个出口

![说明: 2015-11-15_195707](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002-1547604555425.jpg)

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为

栈中进入数据称为  --- **入栈**  `push`

栈中弹出数据称为  --- **出栈**  `pop`

生活中的栈：

![img](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002.png)



![img](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002-1547605111510.jpg)



##### 2.4.2 stack 常用接口

功能描述：栈容器常用的对外接口

构造函数：

* `stack<T> stk;`                                 //stack采用模板类实现， stack对象的默认构造形式
* `stack(const stack &stk);`            //拷贝构造函数

赋值操作：

* `stack& operator=(const stack &stk);`           //重载等号操作符

数据存取：

* `push(elem);`      //向栈顶添加元素
* `pop();`                //从栈顶移除第一个元素
* `top(); `                //返回栈顶元素

大小操作：

* `empty();`            //判断堆栈是否为空
* `size(); `              //返回栈的大小

**示例：**

```C++
#include <stack>

//栈容器常用接口
void test01()
{
	//创建栈容器 栈容器必须符合先进后出
	stack<int> s;

	//向栈中添加元素，叫做 压栈 入栈
	s.push(10);
	s.push(20);
	s.push(30);

	while (!s.empty()) 
    {
		//输出栈顶元素
		cout << "栈顶元素为： " << s.top() << endl;
		//弹出栈顶元素
		s.pop();
	}
	cout << "栈的大小为：" << s.size() << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 入栈   --- push
* 出栈   --- pop
* 返回栈顶   --- top
* 判断栈是否为空   --- empty
* 返回栈大小   --- size

#### 2.5 queue 容器

##### 2.5.1 queue 基本概念

**概念：**Queue是一种**先进先出**(First In First Out,FIFO)的数据结构，它有两个出口



![说明: 2015-11-15_214429](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002-1547606475892.jpg)

队列容器允许从一端新增元素，从另一端移除元素

队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为

队列中进数据称为 --- **入队**    `push`

队列中出数据称为 --- **出队**    `pop`

生活中的队列：

![1547606785041](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\1547606785041.png)



##### 2.5.2 queue 常用接口

功能描述：栈容器常用的对外接口

构造函数：

- `queue<T> que;`                                 //queue采用模板类实现，queue对象的默认构造形式
- `queue(const queue &que);`            //拷贝构造函数

赋值操作：

- `queue& operator=(const queue &que);`           //重载等号操作符

数据存取：

- `push(elem);`                             //往队尾添加元素
- `pop();`                                      //从队头移除第一个元素
- `back();`                                    //返回最后一个元素
- `front(); `                                  //返回第一个元素

大小操作：

- `empty();`            //判断堆栈是否为空
- `size(); `              //返回栈的大小

**示例：**

```C++
#include <queue>
#include <string>
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};
void test01() 
{
	//创建队列
	queue<Person> q;

	//准备数据
	Person p1("唐僧", 30);
	Person p2("孙悟空", 1000);
	Person p3("猪八戒", 900);
	Person p4("沙僧", 800);

	//向队列中添加元素  入队操作
	q.push(p1);
	q.push(p2);
	q.push(p3);
	q.push(p4);

	//队列不提供迭代器，更不支持随机访问	
	while (!q.empty()) 
    {
		//输出队头元素
		cout << "队头元素-- 姓名： " << q.front().m_Name 
              << " 年龄： "<< q.front().m_Age << endl;
        
		cout << "队尾元素-- 姓名： " << q.back().m_Name  
              << " 年龄： " << q.back().m_Age << endl;
        
		cout << endl;
		//弹出队头元素
		q.pop();
	}
	cout << "队列大小为：" << q.size() << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

- 入队   --- push
- 出队   --- pop
- 返回队头元素   --- front
- 返回队尾元素   --- back
- 判断队是否为空   --- empty
- 返回队列大小   --- size

#### 2.6 list容器

##### 2.6.1 list基本概念

**功能：**将数据进行链式存储

**链表**（list）是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

链表的组成：链表由一系列**结点**组成

结点的组成：一个是存储数据元素的**数据域**，另一个是存储下一个结点地址的**指针域**

STL中的链表是一个双向循环链表



![说明: 2015-11-15_225145](E:\嵌入式\c全套教程2021\匠心精作C++从0到1入门编程-学习编程不再难\匠心精作C++从0到1入门编程-学习编程不再难资料\第5阶段-C++提高编程资料\提高编程能力资料\讲义\assets\clip_image002-1547608564071.jpg)

由于链表的存储方式并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于**双向迭代器**

list的优点：

* 采用动态存储分配，不会造成内存浪费和溢出
* 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点：

* 链表灵活，但是空间(指针域) 和 时间（遍历）额外耗费较大

List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的。

总结：STL中**List和vector是两个最常被使用的容器**，各有优缺点

##### 2.6.2  list构造函数

**功能描述：**

* 创建list容器

**函数原型：**

* `list<T> lst;`                               //list采用采用模板类实现,对象的默认构造形式：
* `list(beg,end);`                           //构造函数将[beg, end)区间中的元素拷贝给本身。
* `list(n,elem);`                             //构造函数将n个elem拷贝给本身。
* `list(const list &lst);`            //拷贝构造函数。

**示例：**

```C++
#include <list>

void printList(const list<int>& L) 
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	printList(L1);

	list<int>L2(L1.begin(),L1.end());
	printList(L2);

	list<int>L3(L2);
	printList(L3);

	list<int>L4(10, 1000);
	printList(L4);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：list构造方式同其他几个STL常用容器，熟练掌握即可

##### 2.6.3 list 赋值和交换

**功能描述：**

* 给list容器进行赋值，以及交换list容器

**函数原型：**

* `assign(beg, end);`            //将[beg, end)区间中的数据拷贝赋值给本身。
* `assign(n, elem);`              //将n个elem拷贝赋值给本身。
* `list& operator=(const list &lst);`         //重载等号操作符
* `swap(lst);`                         //将lst与本身的元素互换。

**示例：**

```C++
#include <list>

void printList(const list<int>& L) 
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//赋值和交换
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);
	printList(L1);

	//赋值
	list<int>L2;
	L2 = L1;
	printList(L2);

	list<int>L3;
	L3.assign(L2.begin(), L2.end());
	printList(L3);

	list<int>L4;
	L4.assign(10, 100);
	printList(L4);
}
//交换
void test02()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	list<int>L2;
	L2.assign(10, 100);

	cout << "交换前： " << endl;
	printList(L1);
	printList(L2);

	cout << endl;

	L1.swap(L2);

	cout << "交换后： " << endl;
	printList(L1);
	printList(L2);
}
int main() 
{
	//test01();
	test02();

	system("pause");
	return 0;
}
```

总结：list赋值和交换操作能够灵活运用即可

##### 2.6.4 list 大小操作

**功能描述：**

* 对list容器的大小进行操作

**函数原型：**

* `size(); `                             //返回容器中元素的个数

* `empty(); `                           //判断容器是否为空

* `resize(num);`                   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。

  ​					    //如果容器变短，则末尾超出容器长度的元素被删除。

* `resize(num, elem); `       //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。

   //如果容器变短，则末尾超出容器长度的元素被删除。

**示例：**

```C++
#include <list>

void printList(const list<int>& L) 
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//大小操作
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	if (L1.empty())
	{
		cout << "L1为空" << endl;
	}
	else
	{
		cout << "L1不为空" << endl;
		cout << "L1的大小为： " << L1.size() << endl;
	}
	//重新指定大小
	L1.resize(10);
	printList(L1);

	L1.resize(2);
	printList(L1);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

- 判断是否为空   --- empty
- 返回元素个数   --- size
- 重新指定个数   --- resize

##### 2.6.5 list 插入和删除

**功能描述：**

* 对list容器进行数据的插入和删除

**函数原型：**

* push_back(elem);//在容器尾部加入一个元素
* pop_back();//删除容器中最后一个元素
* push_front(elem);//在容器开头插入一个元素
* pop_front();//从容器开头移除第一个元素
* insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。
* insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
* insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
* clear();//移除容器的所有数据
* erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
* erase(pos);//删除pos位置的数据，返回下一个数据的位置。
* remove(elem);//删除容器中所有与elem值匹配的元素。

**示例：**

```C++
#include <list>

void printList(const list<int>& L) 
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
//插入和删除
void test01()
{
	list<int> L;
	//尾插
	L.push_back(10);
	L.push_back(20);
	L.push_back(30);
	//头插
	L.push_front(100);
	L.push_front(200);
	L.push_front(300);

	printList(L);

	//尾删
	L.pop_back();
	printList(L);

	//头删
	L.pop_front();
	printList(L);

	//插入
	list<int>::iterator it = L.begin();
	L.insert(++it, 1000);
	printList(L);

	//删除
	it = L.begin();
	L.erase(++it);
	printList(L);

	//移除
	L.push_back(10000);
	L.push_back(10000);
	L.push_back(10000);
	printList(L);
	L.remove(10000);
	printList(L);
    
    //清空
	L.clear();
	printList(L);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 尾插   --- push_back
* 尾删   --- pop_back
* 头插   --- push_front
* 头删   --- pop_front
* 插入   --- insert
* 删除   --- erase
* 移除   --- remove
* 清空   --- clear

##### 2.6.6 list 数据存取

**功能描述：**

* 对list容器中数据进行存取

**函数原型：**

* `front();`        //返回第一个元素。
* `back();`         //返回最后一个元素。

**示例：**

```C++
#include <list>

//数据存取
void test01()
{
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	L1.push_back(40);

	//cout << L1.at(0) << endl;//错误 不支持at访问数据
	//cout << L1[0] << endl; //错误  不支持[]方式访问数据
	cout << "第一个元素为： " << L1.front() << endl;
	cout << "最后一个元素为： " << L1.back() << endl;

	//list容器的迭代器是双向迭代器，不支持随机访问
	list<int>::iterator it = L1.begin();
	//it = it + 1;//错误，不可以跳跃访问，即使是+1
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* list容器中不可以通过[]或者at方式访问数据
* 返回第一个元素   --- front
* 返回最后一个元素   --- back

##### 2.6.7 list 反转和排序

**功能描述：**

* 将容器中的元素反转，以及将容器中的数据进行排序

**函数原型：**

* `reverse();`   //反转链表
* `sort();`        //链表排序

**示例：**

```C++
void printList(const list<int>& L) 
{
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << *it << " ";
	}
	cout << endl;
}
bool myCompare(int val1 , int val2)
{
	return val1 > val2;
}
//反转和排序
void test01()
{
	list<int> L;
	L.push_back(90);
	L.push_back(30);
	L.push_back(20);
	L.push_back(70);
	printList(L);

	//反转容器的元素
	L.reverse();
	printList(L);

	//排序
	L.sort(); //默认的排序规则 从小到大
	printList(L);

	L.sort(myCompare); //指定规则，从大到小
	printList(L);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 反转   --- reverse
* 排序   --- sort （成员函数）

##### 2.6.8 排序案例

案例描述：将Person自定义数据类型进行排序，Person中属性有姓名、年龄、身高

排序规则：按照年龄进行升序，如果年龄相同按照身高进行降序

**示例：**

```C++
#include <list>
#include <string>
class Person 
{
    public:
        Person(string name, int age , int height) 
        {
            m_Name = name;
            m_Age = age;
            m_Height = height;
        }
    public:
        string m_Name;  //姓名
        int m_Age;      //年龄
        int m_Height;   //身高
};
bool ComparePerson(Person& p1, Person& p2) 
{
	if (p1.m_Age == p2.m_Age) 
    {
		return p1.m_Height  > p2.m_Height;
	}
	else
	{
		return  p1.m_Age < p2.m_Age;
	}
}
void test01() 
{
	list<Person> L;

	Person p1("刘备", 35 , 175);
	Person p2("曹操", 45 , 180);
	Person p3("孙权", 40 , 170);
	Person p4("赵云", 25 , 190);
	Person p5("张飞", 35 , 160);
	Person p6("关羽", 35 , 200);

	L.push_back(p1);
	L.push_back(p2);
	L.push_back(p3);
	L.push_back(p4);
	L.push_back(p5);
	L.push_back(p6);

	for (list<Person>::iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age 
              << " 身高： " << it->m_Height << endl;
	}
	cout << "---------------------------------" << endl;
	L.sort(ComparePerson); //排序

	for (list<Person>::iterator it = L.begin(); it != L.end(); it++) 
    {
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age 
              << " 身高： " << it->m_Height << endl;
	}
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 对于自定义数据类型，必须要指定排序规则，否则编译器不知道如何进行排序


* 高级排序只是在排序规则上再进行一次逻辑规则制定，并不复杂

#### 2.7 set/ multiset 容器

##### 2.7.1 set基本概念

**简介：**

* 所有元素都会在插入时自动被排序

**本质：**

* set/multiset属于**关联式容器**，底层结构是用**二叉树**实现。

**set和multiset区别**：

* set不允许容器中有重复的元素
* multiset允许容器中有重复的元素

##### 2.7.2 set构造和赋值

功能描述：创建set容器以及赋值

构造：

* `set<T> st;`                        //默认构造函数：
* `set(const set &st);`       //拷贝构造函数

赋值：

* `set& operator=(const set &st);`    //重载等号操作符

**示例：**

```C++
#include <set>

void printSet(set<int> & s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}
//构造和赋值
void test01()
{
	set<int> s1;

	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	printSet(s1);

	//拷贝构造
	set<int>s2(s1);
	printSet(s2);

	//赋值
	set<int>s3;
	s3 = s2;
	printSet(s3);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* set容器插入数据时用insert
* set容器插入数据的数据会自动排序

##### 2.7.3 set大小和交换

**功能描述：**

* 统计set容器大小以及交换set容器

**函数原型：**

* `size();`          //返回容器中元素的数目
* `empty();`        //判断容器是否为空
* `swap(st);`      //交换两个集合容器

**示例：**

```C++
#include <set>

void printSet(set<int> & s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}
//大小
void test01()
{
	set<int> s1;
	
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);

	if (s1.empty())
	{
		cout << "s1为空" << endl;
	}
	else
	{
		cout << "s1不为空" << endl;
		cout << "s1的大小为： " << s1.size() << endl;
	}
}
//交换
void test02()
{
	set<int> s1;

	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);

	set<int> s2;

	s2.insert(100);
	s2.insert(300);
	s2.insert(200);
	s2.insert(400);

	cout << "交换前" << endl;
	printSet(s1);
	printSet(s2);
	cout << endl;

	cout << "交换后" << endl;
	s1.swap(s2);
	printSet(s1);
	printSet(s2);
}
int main() 
{
	//test01();
	test02();

	system("pause");
	return 0;
}
```

总结：

* 统计大小   --- size
* 判断是否为空   --- empty
* 交换容器   --- swap

##### 2.7.4 set插入和删除

**功能描述：**

* set容器进行插入数据和删除数据

**函数原型：**

* `insert(elem);`           //在容器中插入元素。
* `clear();`                    //清除所有元素
* `erase(pos);`              //删除pos迭代器所指的元素，返回下一个元素的迭代器。
* `erase(beg, end);`    //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
* `erase(elem);`            //删除容器中值为elem的元素。

**示例：**

```C++
#include <set>

void printSet(set<int> & s)
{
	for (set<int>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}
//插入和删除
void test01()
{
	set<int> s1;
	//插入
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	printSet(s1);

	//删除
	s1.erase(s1.begin());
	printSet(s1);

	s1.erase(30);
	printSet(s1);

	//清空
	//s1.erase(s1.begin(), s1.end());
	s1.clear();
	printSet(s1);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 插入   --- insert
* 删除   --- erase
* 清空   --- clear

##### 2.7.5 set查找和统计

**功能描述：**

* 对set容器进行查找数据以及统计数据

**函数原型：**

* `find(key);`                  //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
* `count(key);`                //统计key的元素个数

**示例：**

```C++
#include <set>

//查找和统计
void test01()
{
	set<int> s1;
	//插入
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(40);
	
	//查找
	set<int>::iterator pos = s1.find(30);

	if (pos != s1.end())
	{
		cout << "找到了元素 ： " << *pos << endl;
	}
	else
	{
		cout << "未找到元素" << endl;
	}

	//统计
	int num = s1.count(30);
	cout << "num = " << num << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 查找   ---  find    （返回的是迭代器）
* 统计   ---  count  （对于set，结果为0或者1）

##### 2.7.6 set和multiset区别

**学习目标：**

* 掌握set和multiset的区别

**区别：**

* set不可以插入重复数据，而multiset可以
* set插入数据的同时会返回插入结果，表示插入是否成功
* multiset不会检测数据，因此可以插入重复数据

**示例：**

```C++
#include <set>

//set和multiset区别
void test01()
{
	set<int> s;
	pair<set<int>::iterator, bool>  ret = s.insert(10);
	if (ret.second) 
    {
		cout << "第一次插入成功!" << endl;
	}
	else 
    {
		cout << "第一次插入失败!" << endl;
	}
	ret = s.insert(10);
	if (ret.second) 
    {
		cout << "第二次插入成功!" << endl;
	}
	else 
    {
		cout << "第二次插入失败!" << endl;
	}
    
	//multiset
	multiset<int> ms;
	ms.insert(10);
	ms.insert(10);

	for (multiset<int>::iterator it = ms.begin(); it != ms.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 如果不允许插入重复数据可以利用set
* 如果需要插入重复数据利用multiset

##### 2.7.7 pair对组创建

**功能描述：**

* 成对出现的数据，利用对组可以返回两个数据

**两种创建方式：**

* `pair<type, type> p ( value1, value2 );`
* `pair<type, type> p = make_pair( value1, value2 );`

**示例：**

```C++
#include <string>

//对组创建
void test01()
{
	pair<string, int> p(string("Tom"), 20);
	cout << "姓名： " <<  p.first << " 年龄： " << p.second << endl;

	pair<string, int> p2 = make_pair("Jerry", 10);
	cout << "姓名： " << p2.first << " 年龄： " << p2.second << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

两种方式都可以创建对组，记住一种即可

##### 2.7.8 set容器排序

学习目标：

* set容器默认排序规则为从小到大，掌握如何改变排序规则

主要技术点：

* 利用仿函数，可以改变排序规则

**示例一**   set存放内置数据类型

```C++
#include <set>

class MyCompare 
{
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};
void test01() 
{    
	set<int> s1;
	s1.insert(10);
	s1.insert(40);
	s1.insert(20);
	s1.insert(30);
	s1.insert(50);

	//默认从小到大
	for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;

	//指定排序规则
	set<int,MyCompare> s2;
	s2.insert(10);
	s2.insert(40);
	s2.insert(20);
	s2.insert(30);
	s2.insert(50);

	for (set<int, MyCompare>::iterator it = s2.begin(); it != s2.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：利用仿函数可以指定set容器的排序规则



**示例二** set存放自定义数据类型

```C++
#include <set>
#include <string>

class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}

	string m_Name;
	int m_Age;

};
class comparePerson
{
public:
	bool operator()(const Person& p1, const Person &p2)
	{
		//按照年龄进行排序  降序
		return p1.m_Age > p2.m_Age;
	}
};

void test01()
{
	set<Person, comparePerson> s;

	Person p1("刘备", 23);
	Person p2("关羽", 27);
	Person p3("张飞", 25);
	Person p4("赵云", 21);

	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	s.insert(p4);

	for (set<Person, comparePerson>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << "姓名： " << it->m_Name << " 年龄： " << it->m_Age << endl;
	}
}
int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：

对于自定义数据类型，set必须指定排序规则才可以插入数据

#### 2.8 map/ multimap容器

##### 2.8.1 map基本概念

**简介：**

* map中所有元素都是pair
* pair中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
* 所有元素都会根据元素的键值自动排序

**本质：**

* map/multimap属于**关联式容器**，底层结构是用二叉树实现。

**优点：**

* 可以根据key值快速找到value值

map和multimap**区别**：

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素


##### 2.8.2  map构造和赋值

**功能描述：**

* 对map容器进行构造和赋值操作

**函数原型：**

**构造：**

* `map<T1, T2> mp;`                     //map默认构造函数: 
* `map(const map &mp);`             //拷贝构造函数

**赋值：**

* `map& operator=(const map &mp);`    //重载等号操作符

**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}
void test01()
{
	map<int,int>m; //默认构造
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));
	printMap(m);

	map<int, int>m2(m); //拷贝构造
	printMap(m2);

	map<int, int>m3;
	m3 = m2; //赋值
	printMap(m3);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：map中所有元素都是成对出现，插入数据时候要使用对组

##### 2.8.3 map大小和交换

**功能描述：**

* 统计map容器大小以及交换map容器

函数原型：

- `size();`          //返回容器中元素的数目
- `empty();`        //判断容器是否为空
- `swap(st);`      //交换两个集合容器

**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}
void test01()
{
	map<int, int>m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	if (m.empty())
	{
		cout << "m为空" << endl;
	}
	else
	{
		cout << "m不为空" << endl;
		cout << "m的大小为： " << m.size() << endl;
	}
}
//交换
void test02()
{
	map<int, int>m;
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	map<int, int>m2;
	m2.insert(pair<int, int>(4, 100));
	m2.insert(pair<int, int>(5, 200));
	m2.insert(pair<int, int>(6, 300));

	cout << "交换前" << endl;
	printMap(m);
	printMap(m2);

	cout << "交换后" << endl;
	m.swap(m2);
	printMap(m);
	printMap(m2);
}
int main() 
{
	test01();
	test02();

	system("pause");
	return 0;
}
```

总结：

- 统计大小   --- size
- 判断是否为空   --- empty
- 交换容器   --- swap

##### 2.8.4 map插入和删除

**功能描述：**

- map容器进行插入数据和删除数据

**函数原型：**

- `insert(elem);`           //在容器中插入元素。
- `clear();`                    //清除所有元素
- `erase(pos);`              //删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);`    //删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
- `erase(key);`            //删除容器中值为key的元素。

**示例：**

```C++
#include <map>

void printMap(map<int,int>&m)
{
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "key = " << it->first << " value = " << it->second << endl;
	}
	cout << endl;
}
void test01()
{
	//插入
	map<int, int> m;
	//第一种插入方式
	m.insert(pair<int, int>(1, 10));
	//第二种插入方式
	m.insert(make_pair(2, 20));
	//第三种插入方式
	m.insert(map<int, int>::value_type(3, 30));
	//第四种插入方式
	m[4] = 40; 
	printMap(m);

	//删除
	m.erase(m.begin());
	printMap(m);

	m.erase(3);
	printMap(m);

	//清空
	m.erase(m.begin(),m.end());
	m.clear();
	printMap(m);
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* map插入方式很多，记住其一即可

- 插入   --- insert 
- 删除   --- erase
- 清空   --- clear

##### 2.8.5 map查找和统计

**功能描述：**

- 对map容器进行查找数据以及统计数据

**函数原型：**

- `find(key);`                  //查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
- `count(key);`                //统计key的元素个数

**示例：**

```C++
#include <map>

//查找和统计
void test01()
{
	map<int, int>m; 
	m.insert(pair<int, int>(1, 10));
	m.insert(pair<int, int>(2, 20));
	m.insert(pair<int, int>(3, 30));

	//查找
	map<int, int>::iterator pos = m.find(3);

	if (pos != m.end())
	{
		cout << "找到了元素 key = " << (*pos).first << " value = " << (*pos).second << endl;
	}
	else
	{
		cout << "未找到元素" << endl;
	}

	//统计
	int num = m.count(3);
	cout << "num = " << num << endl;
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

- 查找   ---  find    （返回的是迭代器）
- 统计   ---  count  （对于map，结果为0或者1）

##### 2.8.6 map容器排序

**学习目标：**

- map容器默认排序规则为 按照key值进行 从小到大排序，掌握如何改变排序规则

**主要技术点:**

- 利用仿函数，可以改变排序规则

**示例：**

```C++
#include <map>

class MyCompare 
{
    public:
        bool operator()(int v1, int v2) {
            return v1 > v2;
        }
};
void test01() 
{
	//默认从小到大排序
	//利用仿函数实现从大到小排序
	map<int, int, MyCompare> m;

	m.insert(make_pair(1, 10));
	m.insert(make_pair(2, 20));
	m.insert(make_pair(3, 30));
	m.insert(make_pair(4, 40));
	m.insert(make_pair(5, 50));

	for (map<int, int, MyCompare>::iterator it = m.begin(); it != m.end(); it++) {
		cout << "key:" << it->first << " value:" << it->second << endl;
	}
}
int main() 
{
	test01();

	system("pause");
	return 0;
}
```

总结：

* 利用仿函数可以指定map容器的排序规则
* 对于自定义数据类型，map必须要指定排序规则,同set容器















