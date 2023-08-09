# C++入门

## 关键字

## 命名空间

<u>在C/C++中，变量、函数和后面要学到的类都是大量存在的，这些变量、函数和类的名称将都存 在于全局作用域中，可能会导致很多冲突。使用命名空间的目的是对标识符的名称进行本地化， 以避免命名冲突或名字污染，namespace关键字的出现就是针对这种问题的。</u>

```c++
#include <stdio.h>
#include <stdlib.h>
int rand = 10;
// C语言没办法解决类似这样的命名冲突问题，所以C++提出了namespace来解决
int main()
{
 printf("%d\n", rand);
return 0;
}
// 编译后后报错：error C2365: “rand”: 重定义；以前的定义是“函数”
```

> 命名空间的定义：定义命名空间，需要使用到namespace关键字，后面跟命名空间的名字，然后接一对{}即可，{} 中即为命名空间的成员。
>
> ```c++
> // cc是命名空间的名字，一般开发中是用项目名字做命名空间名。
> // 我们上课用的是bit，大家下去以后自己练习用自己名字缩写即可，如张三：zs
> // 1. 正常的命名空间定义
> namespace cc
> {
>  // 命名空间中可以定义变量/函数/类型
>  int rand = 10;
>  int Add(int left, int right)
>  {
>  	return left + right;
>  }
>     struct Node
>  {
>  	struct Node* next;
>  	int val;
>  };
> }
> //2. 命名空间可以嵌套
> // test.cpp
> namespace N1
> {
>     int a;
>     int b;
>     int Add(int left, int right)
>  {
>      return left + right;
>  }
> namespace N2
>  {
>      int c;
>      int d;
>      int Sub(int left, int right)
>      {
>          return left - right;
>      }
>  }
> }
> //3. 同一个工程中允许存在多个相同名称的命名空间,编译器最后会合成同一个命名空间中。
> // ps：一个工程中的test.h和上面test.cpp中两个N1会被合并成一个
> // test.h
> namespace N1
> {
> int Mul(int left, int right)
>  {
>      return left * right;
>  }
> }
> ```
>
> 注意：一个命名空间就定义了一个新的作用域，命名空间中的所有内容都局限于该命名空间中

命名空间的使用：

```C++
//1、指定 ：：加命名空间名称及作用域限定符
int main()
{
    printf("%d\n", N::a);
    return 0;    
}
//2、部分  using N::b;使用using将命名空间中某个成员引入
using N::b;
int main()
{
    printf("%d\n", N::a);
    printf("%d\n", b);
    return 0;    
}
//3、全局 using namespce N;使用using namespace 命名空间名称 引入
using namespce N;
int main()
{
    printf("%d\n", N::a);
    printf("%d\n", b);
    Add(10, 20);
    return 0;    
}
```

## 缺省参数

**缺省参数概念：**<u>缺省参数是声明或定义函数时为函数的参数指定一个缺省值。在调用该函数时，如果没有指定实 参则采用该形参的缺省值，否则使用指定的实参。</u>

```c++
void Func(int a = 0)//参数的默认值
{
	cout<<a<<endl;
}
int main()
{
    Func();     // 没有传参时，使用参数的默认值
    Func(10);   // 传参时，使用指定的实参
	return 0;
}
```

> 缺省参数分类：
>
> 全缺省参数
>
> 半缺省参数
>
> 举例：
>
> ```c++
> //全缺省
> void Func(int a = 10, int b = 20, int c = 30)
> {
> 	cout << "a = " << a << endl;
> 	cout << "b = " << b << endl;
> 	cout << "c = " << c << endl;
> 	cout << endl;
> }
> 
> int main()
> {
> 	Func(1, 2, 3);
> 	Func(1, 2);
> 	Func(1);
> 	Func();
> 	//使用缺省值，必须从右往左连续使用
> 	//Func(,2,);
> 	//Func(, , 3);
> 	return 0;
> }
> // 半缺省
> // 1、必须从右往左连续缺省
> //2、缺省值必须是常量或者全局变量
> //3、C语言不支持（编译器不支持）
> //缺省参数不能在函数声明和定义中同时出现，只出现在函数声明中
> void Func(int a, int b = 10, int c = 20)
> {
>     cout << "a = " << a << endl;
>     cout << "b = " << b << endl;
>     cout << "c = " << c << endl;
> }
> ```
>
> - 缺省参数应用：
>
>   ```c++
>   struct Stack
>   {
>   	int* a;
>   	int top;
>   	int capacity;
>   };
>   
>   void StackInit(struct Stack* ps, int defaultCapacity = 4)
>   {
>   	ps->a = (int*)malloc(sizeof(int)*defaultCapacity);
>   	if (ps->a == NULL)
>   	{
>   		perror("malloc fail");
>   		exit(-1);
>   	}
>   	ps->top = 0;
>   	ps->capacity = defaultCapacity;
>   }
>   
>   int main()
>   {
>   	Stack st1; // 最多要存100个数
>   	StackInit(&st1, 100);
>   
>   	Stack st2; // 不知道多少数据,则使用系统默认数值4先开辟4个空间defaultCapacity
>   	StackInit(&st2);
>   	return 0;
>   }
>   ```

## 函数重载

> 1. **函数重载：**<u>是函数的一种特殊情况，C++允许在同一作用域中声明几个功能类似的**同名函数**，这 些同名函数的形参列表**(参数个数 或 类型 或 类型顺序)**不同，常用来处理实现功能类似数据类型 不同的问题。</u>
>
> 2. 通过下面我们可以看出gcc的函数修饰后名字不变。而g++的函数修饰后变成【_Z+函数长度 +函数名+类型首字母】。
>
>    gcc:
>
>    ![image-20230809142159795](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230809142159795.png)
>
>    g++:
>
>    <img src="C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230809142318928.png" alt="image-20230809142318928" />
>
>    <img src="C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230809142338982.png" alt="image-20230809142338982" style="zoom: 50%;" />
>
>    ![image-20230809142454690](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230809142454690.png)
>
>    <img src="C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230809142507648.png" alt="image-20230809142507648" style="zoom:50%;" />
>
>    **结论：**<u>在linux下，采用g++编译完成后，函数名字的修饰发生改变，编译器将函数参数类型信息添加到修改后的名字中。</u>

## 引用

1. 常引用

   ```c++
   void TestConstRef()
   {
       //不能扩大引用权限，可以缩小或者保持权限
       const int a = 10;
       //int& ra = a;   // 该语句编译时会出错，a为常量 扩大权限
       const int& ra = a;//保持权限
       // int& b = 10; // 该语句编译时会出错，b为常量
       const int& b = 10;
       
       int c = 10;
       const int& cd = c;//缩小权限
   
       double d = 12.34;
       //int& rd = d; // 该语句编译时会出错，类型不同
       const int& rd = d;
   }
   ```

2. 传值、传引用

   ~~如果函数返回时，出了函数作用域，如果返回对象还在(还没还给系统)，则可以使用 引用返回，如果已经还给系统了，则必须使用传值返回。~~

   传值、传引用效率比较

   

3. ***指针与引用的区别***

   > 1. 引用概念上定义一个变量的别名，指针存储一个变量地址
   > 2. 引用在定义时必须初始化，指针没有要求
   > 3. 引用在初始化时引用一个实体后，就不能再引用其他实体，而指针可以在任何时候指向任何 一个同类型实体
   > 4. 没有NULL引用，但有NULL指针
   > 5. 在sizeof中含义不同：引用结果为引用类型的大小，但指针始终是地址空间所占字节个数(32 位平台下占4个字节)
   > 6. 引用自加即引用的实体增加1，指针自加即指针向后偏移一个类型的大小
   > 7. 有多级指针，但是没有多级引用
   > 8. 访问实体方式不同，指针需要显式解引用，引用编译器自己处理
   > 9. 引用比指针使用起来相对更安全

## 内联函数

以inline修饰的函数叫做内联函数，编译时C++编译器会在调用内联函数的地方展开，没有函数调用建立栈帧的开销，内联函数提升程序运行的效率。

![image-20230809204242790](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230809204242790.png)

```c++
typeid(B).name()//获取变量类型C
```