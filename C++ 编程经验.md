

# C++ 编程经验
```c++
// 三元运算符
var = (y < 10) ? 30 : 40;   

// 多维数组声明
int threedim[5][10][4];     
int a[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};  // 赋值
// 等同于
int a[3][4] = { 
 {0, 1, 2, 3},   
 {4, 5, 6, 7},
 {8, 9, 10, 11}
};

// balance 是一个指向 &balance[0] 的指针，即数组 balance 的第一个元素的地址。
// 因此，下面的程序片段把 p 赋值为 balance 的第一个元素的地址
double *p;
double balance[10];
p = balance;  // 指向数组的指针

// *(p+2) 等价于 balance[2]



```

各种数据类型的大小：

```c++
cout << "Size of char : " << sizeof(char) << endl;           // 1字节
cout << "Size of int : " << sizeof(int) << endl;             // 4字节
cout << "Size of short int : " << sizeof(short int) << endl; // 2字节
cout << "Size of long int : " << sizeof(long int) << endl;   // 4字节
cout << "Size of float : " << sizeof(float) << endl;         // 8字节
cout << "Size of double : " << sizeof(double) << endl;       // 2字节

cout << "Size of point : " << sizeof(int *) << endl;         // 所有类型的指针都是8字节
```

逗号`,`的优先级:

```c++
int i, j, c;

j = 10;
i = (j++, c=10, 100+j); // 括号内从左到右依次执行
cout << i << " " << c;
```

111 10



**常用函数一览：**

```c++
typeid(变量名).name()   // 显示变量类型

```





## 一. 函数编程

### <font color="red">1. 数据类型</font>

####  1.1 整型 ：  sizeof(long) >= sizeof(int) >= sizeof(short)

| 类型               | 位宽        | 范围                              | 备注                                     |
| ------------------ | ----------- | --------------------------------- | ---------------------------------------- |
| **char**           | **1字节**   | -128 ~ 127 或 0 ~ 255             | char默认是signed还是unsigned由编译器决定 |
| unsigned char      | 1字节       | 0 ~ 255                           |                                          |
| (signed) **short** | **2字节**   | -32768 ~ 32767                    |                                          |
| unsigned short     | 2字节       | 0 ~ 65535                         |                                          |
| (signed) **int**   | **4字节**   | -2147483648 ~ 2147483647          |                                          |
| unsigned int       | 4字节       | 0 ~ 4294967295                    |                                          |
| (signed) **long**  | **4/8字节** | int/long long                     | 64位的Linux上是8字节，其它是4字节        |
| unsigned long      | 4/8字节     | unsigned  int/ unsigned long long | 64位的Linux上是8字节，其它是4字节        |
| **long long**      | **8字节**   | 略                                |                                          |
| unsigned long long | 8字节       | 略                                |                                          |



#### 1.2 浮点型 ：

| 类型        | 位宽   | 范围                                      | 备注 |
| ----------- | ------ | ----------------------------------------- | ---- |
| float       | 4字节  | (+/-)3.4e(+/-)38 **(7位有效数字)**        |      |
| double      | 8字节  | (+/-)1.7e(+/-)308 **(15位有效数字)**      |      |
| long double | 16字节 | (+/-)1.18973e(+/-)4932 **(19位有效数字)** |      |



#### 1.3 其它：

| 类型      | 位宽    | 备注                                   |
| --------- | ------- | -------------------------------------- |
| void      | 0字节   |                                        |
| Bool      | 1字节   | 1/0                                    |
| 指针（*） | 4/8字节 | 在32位系统上是4字节，64位系统上是8字节 |



### <font color="red">2. 二进制表示</font>

----

> - **二进制: 0b**       
> - **八进制: 0**
> - **十六进制: 0x**

```c++
#include <iostream>
#include <bitset>
using namespace std;

int main()
{
    // 1.进制的表示
    int x = 0b1001;     // x = 9
    int t = 34;
    int y = 074;        // y = 60
    int z = 0xa3;       // z = 163
    char s[10];
    
    // 2.进制的输出
    // 下面的8是二进制位数，自定义为多少输出就是多少位，高位补0
    cout <<"二进制："<< bitset<8>(x) << endl;   // 00001001
    cout <<"八进制："<< oct << y << endl;       // 74
    cout <<"十六进制："<< hex << z << endl;     // a3

    /* C语言版本 */
    itoa(x, s, 2);//转成字符串，基数为2
    printf("二进制：%s\n", s);      // 1001
    printf("八进制：%o\n", y);      // 74
    printf("十进制：%d\n",t);       // 34
    printf("十六进制：%x\n", z);    // a3
    
    // 前面加个#就会显示对应进制的前缀
    printf("十六进制：%#x\n", z);   // 0xa3    

    return 0;
}
```



### <font color="red">3. 各种运算符&关键字</font>

#### 3.1 点运算符 `.` と 箭头运算符 `->`

---

> 点运算符 . ：对象的成员                            箭头运算符 -> ：指针指向的对象的成员

```c++
instance.name 等价于 p_instance->name
```

```C++
double a = 21.56; 
cout << "a : " << a << " (int) a :" << (int) a << endl; // 强制转换
```



#### 3.2 取地址运算符 `&` と 间接寻址运算符 `*`

---

> 取地址运算符 & ：返回操作数的内存地址                间接寻址运算符 * ： 返回操作数所指定地址的变量的值

```c++
int *p, var;   // 最标准的写法是将*号放在变量名字身上

var = 300;
p = &var; // 指针

cout << "Address of var: " << p << endl;
cout << "Value of var: " << *p << endl; // 间接寻址
```
Address of var: 0x61fe14
Value of var: 300



#### 3.3 `#define` 预处理器 と `const`关键字 と `static`关键字

---


1. **#define预处理器**用于在程序运行前明确变量的含义（相当于替换功能）

```c++
#define LENGTH 10       // 在以下程序中，可以使用LENGTH代替常量10
#define WIDTH  5
#define my_int int      // 在以下程序中，可以使用my_int代替关键字int
```

补充：

> **参数宏**
>
> 定义一个带有参数的宏

```c++
#define  宏名(参数列表)  表达式或者常量值
```

> **条件编译**

```c++
#include <iostream>
using namespace std;

#define DEBUG
#define MIN(a,b) (((a)<(b)) ? a : b) // 参数宏

int main ()
{
   int i, j;
   i = 100;
   j = 30;
#ifdef DEBUG
   cerr <<"Trace: Inside main function" << endl;
#endif

#if 0
    /* 这部分相当于注释掉 */
    ...
#endif

   cout <<"The minimum is " << MIN(i, j) << endl;

#ifdef DEBUG
   cerr <<"Trace: Coming out of main function" << endl;
#endif
    
    return 0;
}
```

输出：

```
Trace: Inside main function
Trace: Coming out of main function
The minimum is 30
```





2. **const关键字**用于定义<font color='red'>常量</font>（<font color='red'>不可被修改</font>，一个永远不变的量，比如一个具体的数字，字符）

```c++
const int LENGTH = 10;  // 变量名字最好大写
const int WIDTH  = 5;
```

补充：

> **const关键字与指针**

```c++
//以下两者等价，表示 *p 不可变。*p 表示的是指针变量 p 所指向的内存单元里面的内容，此时这个内容不可变；
const int *p
int const *p

//此时 const 修饰的是 p，所以 p 中存放的内存单元的地址不可变，而内存单元中的内容可变。即 p 的指向不可变，p 所指向的内存单元的内容可变；
int * const p

//*p 和 p 都被修饰了，那么 p 中存放的内存单元的地址和内存单元中的内容都不可变；
const int * const p
```

> **const修饰在函数名前面**
>
> 用const来修饰返回的指针或引用，保护指针指向的内容或引用的内容不被修改，也常用于运算符重载。归根究底就是<font color="red">*使得函数调用表达式不能作为左值。*</font>

```c++
class A {
private:
	int i;
public:
	A(){i=0;}
	int & get(){
		return i;
	}
};
 
void main(){
	A a;
	a.get()=1;            // 尝试修改a对象的数据成员为1，而且是用函数调用表达式作为左值。
	cout<<a.get()<<endl;  // 数据成员真的被改为1了，返回指针的情况也可以修改成员i的值
                          // 所以为了安全起见最好在返回值加上const，使得函数调用表达式不能作为左值
}
```

> **const修饰在函数名后面**（简称const函数）
>
> 表示是 C++ 常成员函数，该函数不能修改对象内的任何成员，只能发生读操作，不能发生写操作。
>
> <font color="red">*※ 切记const函数只能调用const函数，否则编译错误*</font>。

```c++
class People
{
 public:
    int talk(void);
    int eat(void) const; // const 成员函数
 private:
    int age;
   
};
int People::eat(void) const
{
    ++age;   // 编译错误，企图修改数据成员age
    talk();  // 编译错误，企图调用非const函数
    return age;
}
```





3. **static 关键字**：指示编译器，在该程序运行周期里**保持这个局部变量的存在，可以修改它**，但*不需要在每次它进入和离开作用域时进行创建和销毁*。类似的，当 static 用在类数据成员上时，该成员被称为**静态成员**，会导致该成员被类的所有对象共享。

```c++
static int count = 5;        // 全局变量 (全局静态变量)

// 函数定义
void func(void)
{
    static int i = 0;        // 局部静态变量

    i++;
    cout << "变量 i 为 " << i ;
    cout << " , 变量 count 为 " << count << std::endl;
}

int main()
{
    while(count--)
    {
       func();
    }
    return 0;
}
```

```
变量 i 为 1 , 变量 count 为 4
变量 i 为 2 , 变量 count 为 3
变量 i 为 3 , 变量 count 为 2
变量 i 为 4 , 变量 count 为 1
变量 i 为 5 , 变量 count 为 0
```

**static修饰符**也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。

所以此时，另一个文件中用**extern**声明的同名全局变量不会把该文件中用**static**修饰的全局变量的定义当做自己的定义。



#### 3.4 malloc函数 (C语言) と `new`关键字 (C++)

---

**malloc函数**：

> malloc的输出是一个指针，用来指向这片申请的内存

```c++
int *p = (int *)malloc(20);               // 申请20 bytes的空间, 并将返回的指针强制转换为指向int类型的指针
int *p = (int *)malloc(5*sizeof(int));    // 等同于上述代码

/* 申请动态内存 */
int *p_loc = (int *)malloc(sizeof(int));
int **p = &p_loc; 

free(p)
```



**new关键字**：

定义一个指向 double 类型的指针，然后请求内存，该内存在执行时被分配。

```c++
double* pvalue  = NULL;     // 初始化为 null 的指针
// cout << *pvalue << endl; // 此行报错

pvalue  = new double;       // 为变量请求内存
cout << *pvalue << endl;

*pvalue = 12.312;           // 在分配的地址存储值
cout << *pvalue << endl;

delete pvalue;              // 释放pvalue所指向的内存, 但指针pvalue还在
cout << *pvalue << endl;
```

输出：

```
6.83732e-317
12.312
6.83732e-317
```

如果自由存储区已被用完，可能无法成功分配内存。所以<font color='red'>上述代码并不地道</font>。

```C++
double* pvalue  = NULL;
if( !(pvalue  = new double ))
{
   cout << "Error: out of memory." <<endl;
   exit(1);
}

...

delete pvalue;             // 释放pvalue所指向的内存, 但指针pvalue还在
```

>  **malloc()** 函数在 C 语言中就出现了，在 C++ 中仍然存在，但建议尽量不要使用 malloc() 函数。
>
>  **new** 与 **malloc()**  函数相比，其主要的优点是，new 不只是分配了内存，它还**创建了对象**。见下例构造函数调用次数

```c++
class Box
{
   public:
      Box() {
         cout << "调用构造函数！" <<endl;
      }
      ~Box() {
         cout << "调用析构函数！" <<endl;
      }
};
 
int main( )
{
   Box* myBoxArray = new Box[4];  // new 不只是分配了内存，它还创建了对象
 
   delete [] myBoxArray;          // 删除数组内存和对象
   return 0;
}
```

输出：

```
调用构造函数！
调用构造函数！
调用构造函数！
调用构造函数！
调用析构函数！
调用析构函数！
调用析构函数！
调用析构函数！
```



**数组的动态内存分配**:





#### 3.5 extern：跨文件全局变量（变量声明）

---

> 使用extern修饰全局变量，提供一个全局变量的引用，可以跨文件访问。比如a.c中用extern声明一个全局变量，则其它文件中便可直接访问使用该变量。
>
> <font color='red'>可以这么理解，extern 是用来在另一个文件中声明一个全局变量或函数。</font>

当使用多个文件且只在其中一个文件中定义变量时（定义变量的文件在程序连接时是可用的），变量声明就显得非常有用。可以使用 **extern** 关键字在任何地方声明一个变量。虽然可以在 C++ 程序中多次声明一个变量，但变量只能在某个文件、函数或代码块中被定义一次。

```c++
#include <iostream>
using namespace std;
 
// 变量声明
extern int a, b;
extern int c;
extern float f;
extern void write_extern();
 
int main ()
{
  // 变量定义
  int a, b;
  int c;
  float f;
 
  // 实际初始化
  a = 10;
  b = 20;
  c = a + b;
 
  cout << c << endl ;
 
  f = 70.0/3.0;
  cout << f << endl ;
 
  return 0;
}
```



#### 3.6 inline (内联) 函数

---

如果一个函数是**内联**的，那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方，而不像普通函数一样进行现场保护、跳转执行然后恢复现场等操作，可以使程序效率提高。对内联函数进行任何修改，都需要重新编译函数的所有客户端。

```c++
inline void func(int arg);
```

内联函数一般都是1-5行的小函数。在使用内联函数时要留神：

- 1.在内联函数内不允许使用循环语句和开关语句；
- 2.内联函数的定义必须出现在内联函数第一次调用之前；
- 3.类结构中所在的类说明内部定义的函数是内联函数。



#### 3.7 volatile：可能会被意想不到的改变的变量

---

> 编译器不会去优化这个变量的值，优化器在用到这个变量时必须每次都小心地重新读取（即使它前面的指令刚刚从该处读取过数据)，而不是使用保存在寄存器里的备份。



#### 3.8 explicit：阻止构造函数进行隐式的类型转换

---





#### 3.10 左值引用& 和 右值引用&&

---

> 左值引用：不想拷贝，想直接对其进行修改





> 右值引用：一次性对象，不想拷贝，也不想对其进行修改，只想代入、输出等

```c++
for (auto &&i : arr)
{
    cout << i << " ";   // 只用于输出
}
```



















#### 其它

**register：寄存器变量 (C++11开始被弃用)**

---

> 使用register修饰的变量会被编译器尽量分配到寄存器里，提高访问效率。





### <font color="red">4. 各种语句结构</font>

---

#### 4.1 C++11 的for循环

```c++
int my_array[5] = {1, 2, 3, 4, 5};
// 每个数组元素乘于 2
for (int &x : my_array)  // 表示引用类型, 不是取地址运算符
{
    x *= 2;
    cout << x << " "; 
}
/* 
auto 类型也是 C++11 新标准中的，用来自动获取变量的类型
   for (auto item : my_array): 只读形式
   for (auto &item : my_array): 声明为引用形式, 可编辑形式
*/
for (auto &x : my_array) {
    x *= 2;
    cout << x << " "; 
}
```

2 4 6 8 10 4 8 12 16 20

```c++
string str("some string"); 
// range for 语句 
for(auto &c : str)
    cout << c << "|"; 
} 
```

s|o|m|e| |s|t|r|i|n|g|




#### 4.2 switch 语句

```c++
switch (expression)
{
    case constant:
        ...
        break;  // 否则直接跳到下面的case
    case constant:
        ...
    default:  // 可选的，所有case都不满足时
        statement(s);
}
```



#### 4.3 goto 可以用于一下子跳出多层嵌套

```c++
for(...) {
   for(...) {
      while(...) {
         if(...) goto stop;
         .
         .
         .
      }
   }
}
stop:
cout << "Error in program.\n";
```



#### 4.4 枚举

```c++
enum <类型名> {<枚举常量1>, <枚举常量2>, <枚举常量3>, ...};
```

例：

```c++
enum Season
{
    SPR = 0, SUM, AUT, WIN,
};

int main()
{
    Season s = AUT;        // C++不能使用数字进行赋值
    s = (Season)(s+1);     // C++不能直接加1, 需要类型转换
    if(s == WIN)
    {
        std::cout<<"这是冬天"<<std::endl;	   
    }
}
```





### <font color="red">5. 函数</font>


#### 5.1 传值调用 VS 指针调用 VS 引用调用

---

> 传值调用:   不会改变传入的参数

```c++
void swap(int x, int y)   // 该函数无效
{
   int temp;
 
   temp = x; 
   x = y; 
   y = temp;
 
   return;
}
```

> 指针调用：把参数的地址复制给形式参数

```c++
void swap(int *x, int *y)  // 这里的*表示声明函数参数为指针类型
{
   int temp;
   temp = *x;      // 间接寻址运算符*
   *x = *y;        // 间接寻址运算符*
   *y = temp;      // 间接寻址运算符*
 
   return;
}

swap(&a, &b);      // 调用方法
```

> 引用调用： 把引用的地址复制给形式参数

```c++
void swap(int &x, int &y)  // 这里的&表示声明函数参数为引用类型, 不是取地址运算符
{
   int temp;
   temp = x;     /* 保存地址 x 的值 */
   x = y;        /* 把 y 赋值给 x */
   y = temp;     /* 把 x 赋值给 y  */
 
   return;
}

swap(a, b);       // 调用方法
```



#### 5.2 返回变量的引用

---

```c++
int & get_p(int i){
    return arr[i];  // 返回第 i 个元素的引用
}

get_p(2) = 3;       // 改变第 3 个元素
```

当返回一个引用时，要注意被引用的对象不能超出作用域。

所以返回一个对局部变量的引用是不合法的，

但是，可以返回一个对静态变量的引用。

```c++
int & func() {
   int q;
   //! return q; // 在编译时发生错误
   static int x;
   return x;     // 安全，x 在函数作用域外依然是有效的
}
```









### <font color="red">6. 指针</font>


#### 6.1 Null 指针

---

在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 NULL 值是一个良好的编程习惯。

赋为 NULL 值的指针被称为**空指针**。

```c++
int  *ptr = NULL;  // ptr的值是0
```

**指针可以进行“+”，“-”，“++”，“--”运算。数组名是一个常指针，不能变化，修改数组名的值是非法的。**

```c++
int var[3] = {10, 100, 200};

for (int i = 0; i < MAX; i++)
{
    *var = i;    // 这是正确的语法
    var++;       // 这是不正确的
}
```

由于一个数组名对应一个指针常量，只要不改变数组的值，仍然可以用指针形式的表达式。

例如，下面是一个有效的语句，把 var[2] 赋值为 500：

```c++
*(var + 2) = 500;
```



#### 6.2 指向指针的指针（多级间接寻址）

---

![img](D:\技法\笔记图片\pointer_to_pointer.jpg)

```c++
int **pptr; // 指向指针的指针
```

例：

```C++
int  var;
int  *ptr;
int  **pptr;

var = 3000;

// 获取 var 的地址
ptr = &var;

// 使用运算符 & 获取 ptr 的地址
pptr = &ptr;

// 使用 pptr 获取值
cout << "var 值为 :" << var << endl;
cout << "*ptr 值为:" << *ptr << endl;
cout << "**pptr 值为:" << **pptr << endl;
```

var 值为 :3000
*ptr 值为:3000
\*\*pptr 值为:3000




#### 6.3 指针数组：指针的数组

----
```c++
int  var[3] = {10, 100, 200};
int *ptr[3];          // 指针数组

for (int i = 0; i < 3; i++)
{
    ptr[i] = &var[i]; // 赋值为整数的地址
}

for (int i = 0; i < 3; i++)
{
    cout << "Value of var[" << i << "] = ";
    cout << *ptr[i] << endl;
}
```

Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200

```c++
// C++规定char *前面必须有const, 以防修改String
const char *names[3] = {"Zara Ali", "Hina Ali", "Nuha Ali"}; 

// for range只读形式
for (auto p : names) 
{
    cout << p << endl; 
    // cout << *p << endl; 只会访问名为p的数组的第一个字符的内容
}
```
Zara Ali
Hina Ali
Nuha Ali



**int *p[n]** 为指针数组：即存放指针的数组，数组中有n个元素，每个元素为一个int型的指针；

**int (*p)[n] **为数组指针：即指向数组的指针，p为一个指针，指向一个包含n个int元素的数组。

数组和指针，在后面的为主语，前面的是定语，用来修饰主语。



#### 6.4 传递数组给函数的几种方式

---

###### 形式参数是一个指针

```c++
void myFunction(int *param, int size);  // 即传入一个指针
```

###### 形式参数是一个已定义大小的数组

```c++
void myFunction(int param[10]);
```

###### 形式参数是一个未定义大小的数组

```c++
void myFunction(int param[], int size);
```

给函数传递数组，数组会自动地退化为一个指向改数组首地址元素的指针。因此要同时传递数组的大小进入函数。



###### <font color='red'>返回数组的方式:</font>

```c++
int * myFunction();  // 即返回一个指针
```

C++ 不允许返回一个完整的数组作为函数的参数。

但是，可以通过指定不带索引的数组名来返回一个指向数组的指针。





### <font color='red'>7. 其它</font>

#### 7.1 C++输出

---







## 二. 面向对象编程

#### 1. 范围解析运算符 ::

---

```C++
class Box
{
   public:
      double length;         // 长度
      double breadth;        // 宽度
      double height;         // 高度
      double getVolume(void);// 返回体积
};

// 类外定义成员函数方式
double Box::getVolume(void)
{
    return length * breadth * height;
}
```



#### 2. this 指针

---

在 C++ 中，每一个对象都能通过 **this** 指针来访问自己的地址。

友元函数没有 **this** 指针，因为友元不是类的成员。只有成员函数才有 **this** 指针。



#### 3. 静态成员

---

当我们声明类的成员为静态时，这意味着无论创建多少个类的对象，静态成员都只有一个副本。静态成员即使在类对象不存在的情况下也能被调用

>  **静态成员在类的所有对象中是共享的。**

※ <font color='red'>不能把静态成员的初始化放置在类的定义中</font>，但是可以在类的外部通过使用范围解析运算符 **::** 来重新声明静态变量从而对它进行初始化

```c++
static int length;        // 静态成员变量
static int get_length();  // 静态成员函数

// 初始化类 Box 的静态成员变量
int Box::length = 0;
```



> **静态成员函数与普通成员函数的区别：**
>
> - 静态成员函数没有 this 指针，只能访问静态成员（包括静态成员变量和静态成员函数）。
> - 普通成员函数有 this 指针，可以访问类中的任意成员；而静态成员函数没有 this 指针。



#### 4. 析构函数

---

**析构函数**会在每次删除所创建的对象时执行。波浪号（~）作为前缀，它不会返回任何值，也**不能带有任何参数**。

析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。



#### 5. 拷贝构造函数

---

```c++
classname (const classname &obj) { // 形参必须是该类对象的const引用
   
    this->value = obj.value; // 拷贝值
}
```

拷贝构造函数通常用于：

- 通过使用另一个同类型的对象来初始化新创建的对象。
- 复制对象把它作为参数传递给函数。
- 复制对象，并从函数返回这个对象。

如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。

拷贝分为浅拷贝和深拷贝，深拷贝必须重新定义拷贝构造函数。

```c++
class Name
{
   public:
      int length;
      
      Name(int length)        // 构造函数
      {  
         this->length = length;
      }                  
      Name(const Name & item) // 拷贝构造函数
      {
         this->length = item.length;
      }

};                            // 注意这里要有;

int main()
{
    Name name1 = Name(12);
    Name name2 = Name(name1);
    
    cout << name2.length;
    return 0;
}
```

输出：

```
12
```



#### 6. 友元函数&友元类

---

类的**友元函数**是定义在类外部，**不是类的成员函数**，但有权访问类的所有私有（**private**）和保护（**protected**）成员。

```c++
friend void func(int arg);
friend class ClassTwo;
```



#### 7. 类，继承的三种方式

---

```c++
class Base {
 
   public:
  // 公有成员
 
   protected:
  // 受保护成员: 在派生类（即子类）中可以访问，类外不可访问
 
   private:
  // 默认都是私有成员: 只有类和友元函数可以访问, 不能被派生类访问
 
};
```

- 1.**public 继承：**<font color='red'>一模一样</font>

- 2.**protected 继承：**

  基类 public，protected，private -> 派生类 **protected**, protected, private

- 3.**private 继承：**<font color='red'>全变private</font>

  基类 public，protected，private ->派生类 **private**, **private**, private



**多继承**

```c++
Class <派生类名>: <继承方式1><基类名1>, <继承方式2><基类名2>, ...
```





#### 8. 重载运算符， 重载函数

---

重载运算符方法：

```c++
<类名> operator+(const <类名> &<变量名>);   // 两个<类名>实例相加，返回一个新的<类名>实例
```

可重载的运算符列表：

| 双目算术运算符 |            + (加)，-(减)，*(乘)，/(除)，% (取模)             |
| :------------: | :----------------------------------------------------------: |
|   关系运算符   | ==(等于)，!= (不等于)，< (小于)，> (大于>，<=(小于等于)，>=(大于等于) |
|   逻辑运算符   |             \|\|(逻辑或)，&&(逻辑与)，!(逻辑非)              |
|   单目运算符   |              + (正)，-(负)，*(指针)，&(取地址)               |
| 自增自减运算符 |                      ++(自增)，--(自减)                      |
|    位运算符    | \| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移) |
|   赋值运算符   |       =, +=, -=, *=, /= , % = , &=, \|=, ^=, <<=, >>=        |
| 空间申请与释放 |                new, delete, new[ ] , delete[]                |
|   其他运算符   | **()**(函数调用)，**->**(成员访问)，**,**(逗号)，**[]**(下标) |

下面是不可重载的运算符列表：

- **.**：成员访问运算符
- **.\***, **->\***：成员指针访问运算符
- **::**：域运算符
- **sizeof**：长度运算符
- **?:**：条件运算符
- **#**： 预处理符号



#### 9. 多态，virtual，虚函数(virtual function)，接口（抽象类）

---

<font color='red'> 主管写好纯虚函数，让属下继承该类后实现不同的版本，即可动态绑定不同的实现版本。</font>

```c++
class Shape {
   public:
      int area()
      {
         cout << "Parent class area" <<endl;
         return 0;
      }
};

class Rectangle: public Shape{
   public:
      int area ()
      { 
         cout << "Rectangle class area" <<endl;
         return 1; 
      }
};

class Triangle: public Shape{
   public:
      int area ()
      { 
         cout << "Triangle class area" <<endl;
         return 2; 
      }
};

int main( )
{
   Shape *shape;
   Rectangle rec;
   Triangle  tri;

   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();

   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();

   return 0;
}
```

输出：

```
Parent class area
Parent class area
```

导致错误输出的原因是，调用函数 area() 被编译器设置为基类中的版本，这就是所谓的**静态多态**，或**静态链接** - 函数调用在程序执行前就准备好了。有时候这也被称为**早绑定**，因为 area() 函数在程序编译期间就已经设置好了。

但现在，让我们对程序稍作修改，在 Shape 类中，area() 的声明前放置关键字 **virtual** :

```c++
class Shape {
   public:
      virtual int area()
      {
         cout << "Parent class area" <<endl;
         return 0;
      }
};
```

输出：

```c++
Rectangle class area
Triangle class area
```



**虚函数** 是在基类中使用关键字 **virtual** 声明的函数。

在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。

我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为**动态链接**，或**后期绑定**。



**纯虚函数** 是没有实现的虚函数，只给了虚函数的定义。纯虚函数是通过在声明中使用 "= 0" 来指定的。

```C++
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      // 纯虚函数(pure virtual function)
      virtual int area() = 0;  // = 0 告诉编译器, 函数没有主体, 该虚函数是纯虚函数。
};
```

父类声明了虚函数，子类可以声明一个同名函数（同参数）覆盖，如果不声明，就会默认执行父类的这个函数。

“纯虚函数”声明后如果子类不实现这个函数，编译器会报错，所以一定要实现。

基类含有“纯虚函数”就是抽象类，它可以派生子类，但是子类如果想实例化，则必须实现“纯虚函数”也就是重写它，否则编译器报错。



**接口（抽象类）**：如果类中至少有一个函数被声明为纯虚函数，则这个类就是**抽象类**。

设计**抽象类**（通常称为 ABC）的目的，是为了给其他类提供一个可以继承的适当的基类。

抽象类不能被用于实例化对象，它只能作为**接口**使用。如果试图实例化一个抽象类的对象，会导致编译错误。

可用于实例化对象的类被称为**具体类**。



## 三. 模板编程

#### 1. 函数模板

---

> C++最初引入模板时用的是 `class` 关键字，后来避免与声明类的 `class` 关键字混淆，引入了 `typename` 关键字，所以建议使用后者。

```c++
template<typename T>    // template<class T>
T add(T a, T b)
{
    return a + b;
}
```

使用方法：

```c++
cout << add<int>(1, 2) << endl;            // 3
cout << add<string>("A", "B") << endl;     // AB
```

> 还可用来指定函数中所使用的的变量的大小

```c++
template<size_t N>
void add(int(&data)[N]){
    for (size_t i=0; i < N; i++){
        data[i] = 0;
    }
}
```



#### 2. 类模板

---

```c++
template <class T>
class Stack {
  private:
    vector<T> elems;          // 元素
 
  public:
    void push(T const&);      // 入栈
    void pop();               // 出栈
    T top() const;            // 返回栈顶元素
    bool empty() const{       // 如果为空则返回真。
        return elems.empty();
    }
};
 
template <class T>
void Stack<T>::push (T const& elem)
{
    // 追加传入元素的副本
    elems.push_back(elem);   
}
```







## 四. C++标准库



| 类别       | 函数名             | 说明                                              |
| ---------- | ------------------ | ------------------------------------------------- |
|            |                    |                                                   |
| 大小       | `size()`           | 容器中元素的个数                                  |
|            | `max_size()`       | 容器中可以保存的最大元素数目                      |
|            | `empty()`          | 判断容器是否为空，如果为空返回true，否则返回false |
| 添加，删除 | `c.insert(args)`   |                                                   |
|            | `c.emplace(inits)` |                                                   |
|            | `c.erase(args)`    |                                                   |
|            | `c.clear()`        | 删除c中所有元素                                   |
|            |                    |                                                   |
|            |                    |                                                   |
|            |                    |                                                   |



### 1. 顺序容器

---

> 容器中元素的位置与加入容器时的顺序相对应

#### <font color="green">1.1 vector</font>（向量）

> 可变大小数组。除尾部之外的插入、删除可能很慢。

```c++
#include <vector>

vector<int> vec1;         //默认初始化，vec1为空
vector<int> vec2(vec1);   //使用vec1初始化vec2
vector<int> vec3(vec1.begin(),vec1.end());   //使用vec1初始化vec2
vector<int> vec4(10);     //10个值为0的元素
vector<int> vec5(10,4);   //10个值为4的元素
vector<string> vec7(10,"hello");    //10个值为hello的元素
```



| 用法                     | 作用                         |
| ------------------------ | ---------------------------- |
| `vec.begin(),vec.end()`  | 返回vector的首、尾**迭代器** |
| `vec.front(),vec.back()` | 返回vector的首、尾**元素**   |
| `vec.push_back()`        | 从vector末尾加入一个元素     |
| `vec.pop_back()`         | 从vector末尾删除一个元素     |
| resize()                 |                              |
| reserve()                |                              |
| capacity()               |                              |
| emplace_back()           |                              |



#### <font color="green">1.2 list</font>（列表）

> 双向链表。支持双向顺序访问，任何位置插入、删除都很快。



#### <font color="green">1.3 deque</font>（队列）

> 双端队列。快速随机访问，在头尾部插入、删除很快。



### 2. 关联容器

---

> 元素按照关键字进行保存，用于访问

#### <font color="green">2.1 map</font>（映射）

> 字典



#### <font color="green">2.2 multimap</font>（多重映射）

> 关键字可重复的字典



#### <font color="green">2.3 set</font>（集合）

> 集合



#### <font color="green">2.4 multiset</font>（多重集合）

> 关键字可重复的集合



### 3. 容器适配器

---

#### <font color="green">3.1 stack </font>（栈）

> 基于deque实现

| 用法        | 作用                    |
| ----------- | ----------------------- |
| `st.top()`  | 返回stack的栈顶元素     |
| `st.push()` | 从stack栈顶加入一个元素 |
| `st.pop()`  | 从stack栈顶弹出一个元素 |





#### <font color="green">3.2 queue</font>（队列）

> 基于deque实现

| 用法                 | 作用                    |
| -------------------- | ----------------------- |
| `q.front(),q.back()` | 返回queue的首、尾元素   |
| `q.push()`           | 从queue末尾加入一个元素 |
| `q.pop()`            | 从queue末尾删除一个元素 |





#### <font color="green">3.3 priority_queue </font>（优先级队列）

> 基于vector实现

| 用法       | 作用                             |
| ---------- | -------------------------------- |
| `q.top()`  | 返回priority_queue的首元素       |
| `q.push()` | 向priority_queue中加入一个元素   |
| `q.pop()`  | 从priority_queue末尾删除一个元素 |



### 4. 数据结构算法库

---



```c++
#include <algorithm>
```



| 用法     | 作用 |
| -------- | ---- |
| `sort()` | 排序 |
|          |      |
|          |      |



### 5. 数学库

---

#### 4.1 **\<cmath\>**

```c++
double log(double);           // 该函数返回参数的自然对数
double pow(double, double);   // 假设第一个参数为 x，第二个参数为 y，则该函数返回 x 的 y 次方
double sqrt(double);          // 该函数返回参数的平方根
double floor(double);         // 该函数返回一个小于或等于传入参数的最大整数

int abs(int);                 // 该函数返回整数的绝对值
double fabs(double);          // 该函数返回任意一个浮点数的绝对值
```







## C++ 四种范式

---

#### 1. 面向过程（Procedure Programming, PP）

面向过程编程（Procedure Programming, PP），也被称为命令式编程，也是大家最为熟悉的一种传统的编程方式。从本质上讲，它是“冯.诺伊曼机“运行机制的抽象，它的编程思维方式源于计算机指令的顺序排列。

面向过程编程的步骤：
首先，我们必须将待解问题的解决方案抽象为一系列概念化的步骤。然后通过编程的方式将这些步骤转化为程序指令集（算法），而这些指令按照一定的顺序排列，用来说明如何执行一个任务或解决一个问题。这就意味着，程序员必须要知道程序要完成什么，并且告诉计算机如何来进行所需的计算工作，包括每个细节操作。简言之，就是将计算机看作一个善始善终服从命令的装置。

代码如下：

```c++
int add(int a, int b) {
    return a + b;
}
```



#### 2. 面向对象（Object Oriented Programming, OOP）

面向对象编程（Object Oriented Programming, OOP），包含类、对象、封装、继承、多态、重载等机制，通过类和类之间的消息机制建模，提倡针对不同的场景问题构建不同的数据结构，通过方法的调用或消息的互通实现程序交互。就C++而言，OOP 的意思是利用类层级（class hierarchies）及虚函数进行编程，从而可以通过精制的接口操作各种类型的对象，并且程序本身也可以通过派生（derivation）进行功能增量扩展。

托马斯.库恩提出“科学的革命”的范式论之后，Robert Floyd在1979年图灵奖的颁奖演说中使用了编程范式一词。编程范式一般包括三个方面，以OOP为例：

包括三个方面，以OOP为例：
1. 学科的逻辑体系——规则范式：如类/对象、继承、动态绑定、方法改写、对象替换等等机制。
2. 心理认知因素——心理范式：按照面向对象编程之父Alan Kay的观点，“计算就是模拟”。OOP范式极其重视隐喻（metaphor）的价值，通过拟人化，按照自然的方式模拟自然。
3. 自然观/世界观——观念范式：强调程序的组织技术，视程序为松散耦合的对象/类的集合，以继承机制将类组织成一个层次结构，把程序运行视为相互服务的对象们之间的对话。

```c++
class AddClass {
public :
    int operator()(int a, int b) {               // 类中重载（）
        return a + b;
    }
};
```



#### 3. 泛型编程（Generic Programming，GP）

所谓泛型编程就是独立于任何特定类型的方式编写代码，使用泛型程序时，需要提供具体陈述实例所操作的类型或者值。我们经常用到STL容器、迭代器、和算法都是泛型编程的例子；

> 1. 模板是C++支持参数化多态的工具，使用模板可以使用户为类或者函数声明一种一般模式，使得类中的某些数据成员或者成员函数的参数、返回值取得任意类型；
> 2. 模板是一种对类型进行参数化的工具；
> 3. 通常有两种形式：函数模板和类模板；
> 4. 函数模板针对仅参数类型不同的函数；
> 5. 类模板针对仅数据成员和成员函数类型不同的类；
> 6. 使用模板的目的就是能够让程序员编写与类型无关的代码。比如编写了一个交换两个整型int 类型的swap函数，这个函数就只能实现int 型，对double，字符这些类型无法实现，要实现这些类型的交换就要重新编写另一个swap函数。使用模板的目的就是要让这程序的实现与类型无关，比如一个swap模板函数，即可以实现int 型，又可以实现double型的交换。

```c++
// 模板函数
template<typename T, typename U>
auto add3(T &&a, U &&b) -> decltype(a + b) {              // decltype为类型说明符
    return a + b;
}
```

其中上面程序`decltype(a + b)`中`decltype`为C++11新标准引进的类型说明符，它的作用是返回操作数的数据类型，编译器分析表达式得到它的类型，却不实际计算表达式的值。这样可以通过表达式推断出要定义的变量类型，但是不用该表达式的值初始化变量，有点类似auto，auto在初始化可以用，如下：

```c++
auto f=[](int a,int b){return a+b;};         // 与上面lambda表达式的声明是等价的
```



#### 4. 函数编程（Lambda 表达式实现）

函数式编程，是将程序描述为表达式和变换，以数学方程的形式建立模型，并且尽量避免可变的状态。C++11中引入了lambda表达式的概念。一个lambda表达式标识一个可调用的代码单元，我们可以将其理解为一个未命名的内联函数。

一个lambda表达式具有如下形式：

```
捕获列表->返回类型{函数体}
```

我们可以忽略参数列表、箭头和返回类型，但必须永远包含捕获列表和函数体。我们可以按照下面的方式定义一个实现add的lambda表达式：

```c++
auto add4 = [](int a, int b) -> int { return a+b };
```





## 豆知識：

---

##### 1. **指针的存在意义**：

#####                                                         提升代码的运行效率，减小内存占用。

   （比如两块内存的交换，这在函数传参操作时尤为有利，不用copy一个一样的变量区域进行操作）

##### 2. C++ 变量类型（表格）

|  类型   |                             描述                             |
| :-----: | :----------------------------------------------------------: |
|  bool   |                    存储值 true 或 false。                    |
|  char   |          通常是一个字符（八位）。这是一个整数类型。          |
|   int   |               对机器而言，整数的最自然的大小。               |
|  float  | 单精度浮点值。单精度是这样的格式，1位符号，8位指数，23位小数。![图片说明](D:\技法\笔记图片\C++float.png) |
| double  | 双精度浮点值。双精度是1位符号，11位指数，52位小数。![图片说明](D:\技法\笔记图片\C++double.png) |
|  void   |                       表示类型的缺失。                       |
| wchar_t |                         宽字符类型。                         |

其它类型：**枚举、指针、数组、引用、数据结构、类**等等

定义时没有被初始化的变量：带有静态存储持续时间的变量会被隐式初始化为 **NULL**（所有字节的值都是 0），其他所有变量的初始值是未定义的。

| 数据类型 | 初始化默认值 |
| :------: | :----------: |
|   int    |      0       |
|   char   |     '\0'     |
|  float   |      0       |
|  double  |      0       |
| pointer  |     NULL     |



##### 3. C++ 变量作用域

> - 在函数或一个代码块内部声明的变量，称为**局部变量**。
> - 在函数参数的定义中声明的变量，称为**形式参数**。
> - 在所有函数外部声明的变量，称为**全局变量**。

注意，同名局部变量的值会覆盖全局变量的值。



##### 4. C++ 修饰符类型

C++ 允许在 **char、int 和 double** 数据类型前放置修饰符。

修饰符用于改变基本类型的含义，所以它更能满足各种情境的需求。

| 修饰符类型 | 修饰对象 |
| :--------: | :------: |
|    **signed**        | long, short, int, char |
|     **unsigned**       | long, short, int, char |
|    **long**        | signed, unsigned, int, double |
|     **short**       | signed, unsigned, int |

C++ 允许使用速记符号来声明**无符号短整数**或**无符号长整数**。可以不写 int，只写单词 **unsigned、short** 或 **unsigned、long**，int 是隐含的。例如，下面的两个语句都声明了无符号整型变量。

```c++
unsigned x;  // 等同于下面
unsigned int y;
```

```c++
#include <iostream>
using namespace std;

/* 
 * 这个程序演示了有符号整数和无符号整数之间的差别
*/
int main()
{
   short int i;           // 有符号短整数
   short unsigned int j;  // 无符号短整数

   j = 50000;

   i = j;
   cout << i << " " << j;

   return 0;
}
```

当上面的程序运行时，会输出下列结果:

```
-15536 50000
```

上述结果中，无符号短整数 50,000 的位模式被解释为有符号短整数 -15,536。



##### 5. C++ 二进制运算符

A = 0011 1100

B = 0000 1101

| 运算符 |          描述          |                             实例                             |
| :----: | :--------------------: | :----------------------------------------------------------: |
|   &    |         AND 与         |                (A & B) 得 12，即为 0000 1100                 |
|   \|   |         OR 或          |                (A \| B) 得 61，即为 0011 1101                |
|   ^    |        XOR 异或        |                (A ^ B) 得 49，即为 0011 0001                 |
|   ~    | 非，即0变成1，1变成0。 | (~A ) 得 -61，即为 1100 0011，一个有符号二进制数的补码形式。 |
|   <<   |       左移运算符       |                A << 2 得 240，即为 1111 0000                 |
|   >>   |       右移运算符       |                 A >> 2 得 15，即为 0000 1111                 |



##### 6. C++ 随机数

关于随机数生成器，有两个相关的函数。一个是 **rand()**，该函数只返回一个伪随机数。生成随机数之前<font color='red'>必须</font>先调用 **srand()** 函数。

**srand()** 函数是随机数发生器的初始化函数。原型:

```c++
void srand(unsigned seed);
```

**用法:** 它初始化随机种子，会提供一个种子，这个种子会对应一个随机数，如果使用相同的种子后面的 rand() 函数会出现一样的随机数，如:` srand(1);` 直接使用 1 来初始化种子。

不过为了防止随机数每次重复，常常使用系统时间来初始化，即使用 time函数来获得系统时间，它的返回值为从 00:00:00 GMT, January 1, 1970 到现在所持续的秒数，然后将time_t型数据转化为(unsigned)型再传给srand函数，即: srand((unsigned) time(&t)); 还有一个经常用法，不需要定义time_t型t变量,即: `srand((unsigned) time(NULL)); `直接传入一个空指针，因为你的程序中往往并不需要经过参数获得的数据。

**进一步说明：**计算机并不能产生真正的随机数，而是已经编写好的一些无规则排列的数字存储在电脑里，把这些数字划分为若干相等的N份，并为每份加上一个编号用srand()函数获取这个编号，然后rand()就按顺序获取这些数字，当srand()的参数值固定的时候，rand()获得的数也是固定的，所以一般srand的参数用time(NULL)，因为系统的时间一直在变，所以rand()获得的数，也就一直在变，相当于是随机数了。只要用户或第三方不设置随机种子，那么在默认情况下随机种子来自系统时钟。如果想在一个程序中生成随机数序列，需要至多在生成随机数之前设置一次随机种子。

 **即：**只需在主程序开始处调用 `srand((unsigned)time(NULL)); `, 后面直接用rand就可以了。

不要在 for 等循环放置`srand((unsigned)time(NULL));`

```C++
#include <iostream>
#include <time.h>
#include <stdlib.h>

/* 
#include <ctime>
#include <cstdlib> // 也可以
*/

using namespace std;

int main ()
{
   // 设置种子
   srand( (unsigned)time(NULL) );
   
   // 输出 0 到 10 之间的随机数
   cout <<"随机数： " << rand() % 10 << endl; // 再除以一个数就变成了随机小数

   return 0;
}
```



随机小数的另一种方法(使用<random>库)：

```C++
#include <iostream>
#include <ctime>
#include <random>

using namespace std;
 
int main()
{
    // 设置种子
    default_random_engine e(time(0));
    
    // 生成 -1.5 到 3.5 之间的随机双精度小数
    uniform_real_distribution<double> u(-1.5,3.5);
    
    cout << "随机小数: " << u(e) << endl;

    return 0;
}
```

