C语言进阶

## C语言1.0

整形存储

![image-20230315164753431](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230315164753431.png)

<u>0X11223344数据由高位->低位</u>

大端存储 ：低位放在高地址，高位放在低地址

小端存储 ：低位放在低地址，高位放在高地址

 整形家族 limits.h

***<u>浮点型存储</u>*** float.h

![image-20230322164857340](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230322164857340.png)

![image-20230322165212481](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230322165212481.png)

***<u>指针的进阶</u>***

常量字符串不可以修改 放在内存的只读数据区

字符指针 

数组指针

函数指针 ![image-20230323153308803](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230323153308803.png)

![image-20230323155322362](C:\Users\14521\AppData\Roaming\Typora\typora-user-images\image-20230323155322362.png) 

函数指针数组

***<u>！！！回调函数（函数指针用来实现回调函数）</u>***

回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个 函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数。回调函数不是由该函数 的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进 行响应。





