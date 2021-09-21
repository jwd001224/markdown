# C++
## 前言
需要一定的C语言的基础知识，比如判断语句（if），循环语句（for,while）,数据类型，等。  

IDE推荐：CLion  
编译器：GCC或G++  
调试器：GDB  
## C++基础入门

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

### 关键字

1. const  
- 1.1 const含义  
常类型是指使用类型修饰符`const`说明的类型，常类型的变量或对象的值是不能被赋值改变的。  
- 1.2 const作用  
  - 1.2.1 const于数据类型，指针：    
    ```
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
  - 1.2.2 const于函数  
    ```
    const int func1(); // 本身无意义
    const int* func2(); // 指针指向的内容不能改变，返回的内容不可改变
    int* const func2(); // 指针本身不能改变，返回内容的地址不可改变

    void func(const int var); // 传递过来的参数var的值不能改变
    void func(int* const var); // 指针本身不可变，var的地址不可改变，值可以变
    ```
  - 1.2.3 const于类  
    ```
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
    ```
    class Apple{
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
- 1.3 const的意义    
  - 作用相当于`#define`，不同的是`#define`作用于全局。
  - 提高程序的健壮性  
2. static  
- 2.1 stztic含义  
   当与不同类型一起使用时，Static关键字具有不同的含义。我们可以使用static关键字： 
- 2.2 静态变量： 函数中的变量，类中的变量  
  - 2.1.1 函数中的静态变量    
    当变量声明为static时，空间将在程序的生命周期内分配。即当变量声明为static，内存确定，即使多次调用，系统也不会为其分配新内存，其中的内容不会消失重置。  
    ```
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
  - 2.1.2 类中的静态变量  
    由于声明为static的变量只被初始化一次，因为它们在单独的静态存储中分配了空间，不属于任何一个类，因此类中的静态变量由对象共享。对于不同的对象，不能有相同静态变量的多个副本。也是因为这个原因，静态变量不能使用构造函数初始化。  
    ```
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
- 2.3 静态类的成员： 类对象和类中的函数  
  - 2.2.1 类对象为静态  
    静态类对象始终贯穿程序，在程序结束时，释放内存。
  - 2.2.2 类中的静态函数  
    （1）静态成员函数只能访问静态类成员或其他静态成员函数，它们无法访问类中的非静态成员或成员函数。  
    （2）成员函数和成员对象分开存储。  
3. this指针  
- 1.1 this指针的用处  
    （1）一个对象的this指针不占用类的内存。  
    （2）this指针指向本类  
    （3）this作用域是在类内部，作用于非静态成员变量，使用时无需定义。
- 1.2 this指针的使用  
    （1）在类的非静态成员函数中返回类对象本身的时候，直接使用 `return *this`。  
    （2）当参数与成员变量名相同时，如`this->n = n` （不能写成 `n = n`)。  
- 1.3 this指针程序举例：  
    ```
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
            B& a_add(B *b)//如果返回值，将返回备份对象
            {
                this->b += b->b;
                return *this;//this本身为指向本类的指针，为本类的地址
            }
            int b;
    };
    int A::a1 = 0;
    void test01()
    {
        A a;
        a.func02(10);
        cout << "A:" << a.a2 << endl;
    }
    void test02()
    {
        B b1(20);
        cout << "B:" << b1.b << endl;

        b1.a_add(b1)->a_add(b1)->a_add(b1);
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
4. inline  
- 4.1 inline的用处  
    （1）内联能提高函数效率，但并不是所有的函数都定义成内联函数！内联是以代码膨胀(复制)为代价，仅仅省去了函数调用的开销，从而提高函数的执行效率。   
    （2）如果执行函数体内代码的时间相比于函数调用的开销较大，那么效率的收获会更少！  
    （3）每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。
- 4.2 以下情况不宜用内联：  
    （1）如果函数体内的代码比较长，使得内联将导致内存消耗代价比较高。  
    （2）如果函数体内出现循环，那么执行函数体内代码的时间要比函数调用的开销大。
5. 指针和引用  
- 5.1 指针  
  - 5.1.1 指针的含义  
    指针`type *p =  &a;`  
    p ==> 保存a的地址；*p ==> 保存a的内容；  
  - 5.1.2 指针变量的语法  
    - 变量：
        ```
        int a = 10;
        int *p = &a;//可以不用初始化
        ```
        或
        ```
        int a = 10;
        int *p = NULL;//可以不用初始化
        p = &a;
        ```
    - 数组：  
        ```
        int a[10];
        int *p = a;//a为a[10]的首地址
        ```
        或
        ```
        int a[10];
        int *p = NULL;//可以不用初始化
        p = a;
        ```
    - 函数： 
        ```
        int a;
        int b[10];
        int *c = &a;
        int* func(int *p){
            return p;//返回*p的地址
        }
        int *p = func(&a);返回a的地址
        func(b);
        func(c);
        ```
- 5.2 引用  
  - 5.2.1 引用的含义  
  （1）起别名  
  （2）一个变量只能作为一个变量的别名
  - 5.2.2 引用的语法
    - 变量
    ```
    int a = 10;
    int &b = a;//必须初始化
    //a,b的地址相同
    ```
    - 函数
    ```
    int a;
    int *c = &a;
    int func(int &p){
        return p;//返回*p的地址
    }
    int *p = func(a);返回a的地址
    func(b);
    func(*c);
    ```

6. new和malloc
- 6.1 栈区和堆区区别： 
  - 堆和栈中的存储内容：栈存局部变量、函数参数等。堆存储使用new、malloc申请的变量等；   
  - 申请方式：栈内存由系统分配，堆内存由自己申请；
  - 申请后系统的响应：  
    栈————只要栈的剩余空间大于所申请空间，系统将自动为程序提供内存，否则将报异常提示栈溢出。  
    堆————首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表 中删除，并将该结点的空间分配给程序；  
  - 申请大小的限制：Windows下栈的大小一般是2M，堆的容量较大；
  - 申请效率的比较：
    栈由系统自动分配，速度较快。  
    堆使用new、malloc等分配，较慢；   
- 6.2 new



