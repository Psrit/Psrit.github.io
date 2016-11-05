---
title: C++总结
date: 2016-11-04 22:36:51
tags:
---



-------

### 程序

- 源程序：用源语言写的，有待翻译的程序；
- 目标程序；源程序通过翻译程序加工以后生成的机器语言程序；
- 可执行程序：连接目标程序以及库中的某些文件，生成的一个可执行文件（如 .exe on Windows）

### 三种不同类型的翻译程序

- 汇编程序：将汇编语言源程序翻译成目标程序；
- 编译程序：将高级语言源程序翻译成目标程序；
- 解释程序：将高级语言源程序翻译成机器指令，边翻译边执行。

### 进制转换

- $R$进制到十进制：
$$
(b\_k b\_{k - 1} \cdots b\_0 . b\_{-1} \cdots b\_{-n})\_R = \sum\_{i = -n}^k b\_i R^i
$$
如 $(11111111.11)\_2=1×2^7+1×2^6+1×2^5+1×2^4\\\\+1×2^3+1×2^2+1×2^1+1×2^0+1×2^{-1}+1×2^{-2} =(255.75)\_{10}$
- 十进制到$R$进制：
  - 整数：
    除以$R$取余
  - 小数：
    乘以$R​$取整：
    $$
    \left\.\begin{alignedat}{2}
    0\. & 3125 && \times 2 = {\color{red} 0}.625 \\\\
    0\. & 625   && \times 2 = {\color{red} 1}.25 \\\\
    {\color{blue}0}. & 25     && \times 2 = {\color{red} 0}.5 \\\\
    0\. & 5       && \times 2 = {\color{red} 1}.0 \\\\
    \end{alignedat}\right\\}
    \Rightarrow 0.3125\_{10} = 0.0101\_{2}
    $$
- 二、八、十六进制快速转换：
  - 1 位八进制数相当于3 位二进制数；
  - 1 位十六进制数相当于4 位二进制数。如：
  $$
  \mathrm{
  (1011010.10)\_2 = (\underline{001}\ \underline{011}\ \underline{010} . \underline{100})\_2 = (132.4)\_8 \\\\
  (1011010.10)\_2 = (\underline{0101}\ \underline{1010} . \underline{1000})\_2 = (5A.8)\_{16} \\\\
  (F7)\_{16} = (\underline{1111}\ \underline{0111})\_2 = (11110111)\_2
  }
  $$

### 二进制数的编码

- 原码：
  - 符号-绝对值表示：0 → +; 1 → -
  - 缺点：0的表示不唯一（000...0 == 100...0）；进行四则运算时，符号位须单独处理，运算规则复杂。
- 反码（求补码时的中间码）：
  - 正数的反码与原码表示相同；
  - 负数的反码与原码有如下关系：
    符号位不变(仍用1 表示)，其余各位取反(0 变1，1 变0)，例如：
    $$
    \begin{alignedat}{2}
    X &= & \ -&\ 1100110 \\\\
    [X]\_原 & = & 1&\ 1100110 \\\\
    [X]\_反 & = & 1&\ 0011001
    \end{alignedat}
    $$
- 补码：
  - 正数的补码与原码相同；
  - 负数的补码由该数反码的末位加 1 求得，之前的$X$的补码应为：
    $$
    [X]\_补 = 1\ 0011010
    $$
  - 对补码求补即得原码。
  - 优点：（没体验过……）
    - 0 的表示唯一（即100...0 == 000...0）；
    - 符号位可作为数值参加运算；
    - 补码运算的结果仍为补码。

### 实数的浮点表示以及字符的表示

- 实数$N$用浮点形式可以表示为：$N = M \times 2 ^ E$，$E$称为幂，$N$称为阶码，$M$是$N$的尾数。
- ASCII 码是一种常用的西文字符编码：用7位二进制数表示一个字符，最多可以表示$2 ^ 7 = 128$个字符。

------

### C++基本数据类型

- 对于各种整数类型，ISO C++规定它们的大小满足：
  (signed/unsigned) char ≤ (unsigned) short int ≤ (unsigned) int ≤
  (unsigned) long int ≤ long long int
- 字符串类型
  - 有字符串常量，基本类型中没有字符串变量；
  - 采用字符数组存储字符串（C 风格的字符串）或标准C++类库中的String 类（C++风格的字符串）
- 各基本类型的取值范围

|        类型名         | 长度（字节） |                   取值范围                   |
| :----------------: | :----: | :--------------------------------------: |
|        bool        |   1    |               false, true                |
|        char        |   1    |                -128 ~ 127                |
|    signed char     |   1    |                -128 ~ 127                |
|   unsigned char    |   1    |                 0 ~ 255                  |
|   (signed) short   |   2    |              -32768 ~ 32767              |
|   unsigned short   |   2    |                0 ~ 65535                 |
| (signed) int/long  |   4    |           -2 ^ 31 ~ 2 ^ 31 - 1           |
| unsigned int/long  |   4    |              0 ~ 2 ^ 32 - 1              |
|     long long      |   8    |           -2 ^ 63 ~ 2 ^ 63 - 1           |
| unsigned long long |   8    |              0 ~ 2 ^ 64 - 1              |
|       float        |   4    | (绝对值范围, 下同) 3.4 × 10 ^ (-38) ~ 3.4 × 10 ^ 38 |
|       double       |   8    |    1.7 × 10 ^ (-308) ~ 1.7 × 10 ^ 308    |
|    long double     |   8    |    1.7 × 10 ^ (-308) ~ 1.7 × 10 ^ 308    |
注：以上针对32位处理器。64位机器中long == long long，其他类型没有考证过。

### C++常量

- 整数常量：如12，010，0xfff
  - 前缀：十进制：无；八进制：0；十六进制：0x
  - 后缀：L (or l)：类型至少是long；LL (or ll)：类型是long long；U (u)：unsigned类型
- 浮点常量：如12.5，-12.5，0.345E+2，-34.4E-3
  - 指数形式下，整数和小数部分可省略其一
  - 默认是double型。添加后缀 F (f) 可使其成为float型，如12.3f
- 字符和字符串：
  - C++转义字符列表：看书吧（P26）。这里不浪费纸张了，节约资源从小事做起。
  - 字符串常量："CHINA"。每个字符一个字节，别忘了末尾有'\0'。
  - 可以添加前缀以改变字符（串）常量的类型：

| 前缀   | 含义                  | 类型       |
| ---- | :------------------ | -------- |
| u    | Unicode 16字符        | char16_t |
| U    | Unicode 32字符        | char32_t |
| L    | 宽字符(What's that???) | wchar_t  |
| u8   | UTF-8 (仅用于字符串字面常量)  | char     |

### C++变量

- 定义：`TYPE var1, var2, ... varn; `。**变量的定义就是在给变量命名的时候分配内存空间。**
- 初始化：

  ```c++
  int a = 0;
  int a(0);
  int a = {0}; // 列表初始化，此时不允许信息丢失，如用double初始化int是不可以的（报错？），下同
  int a{0};
  ```

  未经初始化的变量，其值可能是**随机的**。
  
### C++符号常量

- 定义：`const TYPE const_var = value;` or `TYPE const const_var = value;`
- 符号常量在定义时一定要同时初始化，在程序中不能改变其值。

### 一些运算符

- 逗号运算：`expr1, expr2, ..., exprN`。逐次求解，最终结果为`exprN`的值。
  例如：`a = 3 * 5, a * 4`，最终结果为60.
- 关系运算：`<, <=, >, >=`和`==, !=`（前四个优先级高于后两个；前四个彼此之间优先级相同，后两个亦如是）
- 逻辑运算：
  - 优先级排序：`! > && > ||`
  - 短路特性：
    - `expr1 && expr2`：expr1为false则直接返回false，不再求解expr2；为true则求解expr2，以expr2的结果作为最终结果；
    - `expr1 || expr2`：expr1为true则直接返回true，不再求解expr2；为false则求解expr2，以expr2的结果作为最终结果。
- 条件运算符：`boolean_expr ? expr1 : expr2`（优先级高于赋值运算符，低于逻辑运算符；返回值类型是expr1和expr2中较高的类型）
- `sizeof`：`sizeof(TYPE)` or `sizeof expr`，给出TYPE类型或expr的类型所占的字节数。
- 位运算和它们的“奇技淫巧”：
  `char a = ... ; a = a & 0xfe;`：a的最低一位设为0；
  `char c = ... ; int a = ... ; c = a & 0xff;`：取出a的最低一个字节；
  `int a = ... ; a = a | 0xff;`：将a的最低一字节各位全设为1；
  `0111 1010 ^ 0000 1111 == 0111 0101`：与0异或保持原值，与1异或取反；
  `~`：人家是单纯的按位取反运算啦~；
  `<<`：**低位补0**，高位舍弃；
  `>>`：低位舍弃，**高位补**`0 if 是无符号数 else 符号位`（是不是很熟悉的表达式:eyes:<img src="https://worldvectorlogo.com/logos/python-5.svg" style="width:3%; height:3%;" />）
  运算符优先级的问题见课本P34。

### 数据I/O流类库操纵符 (需要iomanip)

| 操纵符名                | 含义                                       |
| ------------------- | ---------------------------------------- |
| `dec`               | 数值数据采用十进制表示                              |
| `hex`               | 十六进制                                     |
| `oct`               | 八进制                                      |
| `ws`                | 提取空白符                                    |
| `endl`              | 插入换行符，并刷新流                               |
| `ends`              | 插入空字符                                    |
| `setprecision(int)` | 设置浮点数的小数位数（包括小数点）                        |
| `setw(int)`         | 设置域宽                                     |
| `std::fixed`        | the default display notation for floating-point numbers |

例如：

```c++
#include <iostream>
int main( ) 
{
    using namespace std;
    float i = 1.1F, pi = 3.1415;
    cout << setw(5) << setprecision(3) << pi << endl; // print: _3.14 ?
    cout << /*fixed <<*/ i << endl;   // fixed is the default // print: 1.1
    cout << scientific << i << endl; // print: 1.100000e+000
    cout.precision( 8 );
    cout << fixed << i << endl; // ?
    cout << scientific << i << endl; // ?
}
```

### 基本控制结构

- if-else: 

    ```c++
    if (expr1)
        statement1 
    [else if (expr2)
        statement2
    else
        statement3]
    ```

  The **else** clause of an **if...else** statement is associated with the closest previous **if** statement in the same scope that does not have a corresponding **else** statement. (就进匹配原则)
- switch-case: 

    ```c++
    switch (表达式)
    { 
    case 常量表达式 1: 
        语句1 // Here {} is not needed. 
    case 常量表达式 2: 语句2
    ┆
    case 常量表达式 n: 语句n
    default: 语句n+1
    }
    ```

- while:

    ```c++
    while (expr)
        statement
    ```

    ```c++
    do
        statement
    while (expr)
    ```

- for: 

    ```c++
    for (init_expression; cond_expression; loop_expression)
        statement
    ```

    ```c++
    for (iterate_var : expression)
        statement 
    ```

### 自定义类型 (typedef, using, enum, auto, decltype)

- typedef和using: 

  ```c++
  typedef existed_type your_identifier1, your_identifier2, ...;
  // e.g. typedef double Area, Volume;
  using your_identifier = existed_type;
  // e.g. using Area = double; using Volume = double;
  ```

- enum: 

  ```c++
  enum name {enum_list}; // semicolon!!!
  ```
  
  - 枚举元素具有默认值，也可以在声明时另行指定枚举元素的值；
  - 枚举值可以进行关系运算；
  - 整数值不能直接赋给枚举变量，如需要将整数赋值给枚举变量，应进行强制类型转换；
  - 枚举值可以赋给整型变量。
- auto和decltype: (讲道理这里需要继续学习。Take a cup of Java and enjoy C++. <img src="https://worldvectorlogo.com/logos/java-14.svg" style="width:3%; height:3%;" />)

  ```c++
  auto val = val1 + val2; // val.getClass() == (val1 + val2).getClass()
  decltype(i) j = 2; // j.getClass() == i.getClass()
  ```

-------

### 函数关键字 (inline, constexpr)

- inline: 
  使用内联函数可以使程序更快，因为它们消除了与函数调用关联的开销（编译时在调用处用函数体进行替换，节省了参数传递、控制转移等开销）。 内联展开的函数可以进行无法对普通函数使用的代码优化。编译器将内联扩展选项和关键字视为建议，不保证会对函数进行内联。
  虽然内联函数类似于宏（因为在编译时进行调用会展开函数代码），但内联函数是通过编译器分析的，而宏是通过预处理器展开的。 因此，二者存在很多重大差异，详见：[MSDN: 内联函数 (C++)](https://msdn.microsoft.com/zh-cn/library/bw1hbe6y.aspx)。
  “严格的”内联函数应当满足（其实写个 inline 函数不满足这些也是可以的……这时编译器可能不会将其内联化；我的数据结构作业都是这样的……）：
  - 内联函数体不能有循环语句和 switch 语句；
  - 内联函数的定义必须出现在第一次被调用之前；
  - 对内联函数不能进行异常接口声明。
- constexpr (if you use Visual Studio, VS2015 is required at least): 
  关键字 constexpr 于 C++11 中引入并于 C++14 中得到改善。它表示*常数表达式*。与 const 相同，它可应用于变量，因此如果任何代码试图修改该值，均将引发编译器错误。与 const 不同，constexpr 也可应用于函数和类构造函数。
  constexpr 指示值或返回值是常数，并且**如果可能，将在编译时计算值或返回值**。每当需要 const 整数时（如在模板参数和数组声明中），均可使用 constexpr 整数值。当可以在编译时（而非运行时）计算某个值时，使用 constexpr 可以使程序运行速度更快、占用内存更少。
  为限制计算编译时常量的复杂性及其编译时间的潜在影响，C++14 标准要求常数表达式 (constant expression) 中所涉及的类型限定为文本类型，即一个 constexpr 变量或函数必须返回一个**文本类型**。所谓文本类型，是可在编译时确定其布局的类型 (whose layout can be determined at compile time)。以下均为文本类型：
  1. void
  2. 标量类型
  3. 引用
  4. void、标量类型或引用的数组
  5. 具有普通析构函数，以及一个或多个不是移动或复制构造函数的 constexpr 构造函数的类 (A class that has a trivial destructor, and one or more constexpr constructors that are not move or copy constructors. )。此外，其所有非静态数据成员和基类必须是文本类型且不可变 (not volatile)。
  - 就**变量**而言，const 变量的初始化可以延迟到运行时，而 **constexpr 变量必须在编译时进行初始化**。如：

    ```c++
    constexpr float x = 42.0;
    constexpr float y{108};
    constexpr float z = exp(5, 3);
    constexpr int i; // Error! Not initialized
    int j = 0;
    constexpr int k = j + 1; //Error! j not a constant expression
    ```

  - 就**函数**而言，constexpr 函数是在使用需要它的代码时，可以在编译时计算其返回值的函数。 **constexpr 函数必须只接受并返回文本类型**。**当其参数为 constexpr 值并且在编译时使用代码需要返回值时**（例如，初始化一个 constexpr 变量或提供一个非类型模板参数），它会生成**编译时常量** (a compile-time constant)。使用非 constexpr 参数调用时，或编译时不需要其值时，它将与普通函数 (a regular function) 一样，在运行时生成一个值（此双重行为使你无需编写同一函数的 constexpr 和非 constexpr 版本）。
    此外，根据课件 constexpr 修饰的函数在其所有参数都是 constexpr 时，一定返回 constexpr (really?) ；函数体中必须有且仅一条 return 语句。constexpr 变量例如：

    ```c++
    int nonconst_var = 100;
    const int const_var1 = 2;
    const int const_var2 = nonconst_var;

    constexpr int constexpr_var1 = 3 + const_var1 * 4; - 正确 
    constexpr int constexpr_var2 = 3 + nonconst_var * 4;
    constexpr int constexpr_var3 = 3 + const_var2 * 4;
    // constexpr的变量的值必须是编译器在编译的时候就可以确定的。上例中因为nonconst_var的值在语法上来讲，运行期间可能被更改，所以编译期间无法确定，不属于常数表达式。因为const_var2是由非常数表达式来初始化的，所以const_var2也不是常数表达式。但const_var2本身的声明，定义及初始化是合法的。constexpr比const更严格，用来初始化constexpr_var2和constexpr_var3的也都不是常数表达式，所以他们的定义都是错误的。
    ```

### 函数调用

若函数定义在调用点之前，可以不另外声明；
若函数定义在调用点之后，必须要在调用函数前声明函数原型（在调用函数**前**就可以了）：
函数原型：`RETURN_TYPE func(TYPE1 [val1], ...); // Note the semicolon!!! `

### 引用

定义一个引用时，必须同时对它进行**初始化**，**使它指向一个已存在的对象**（这个对象可以没有被初始化）。一旦**一个引用被初始化**后，就不能改为指向其他对象。

### 默认参数值

有默认参数的形参必须列在形参列表最右，即默认参数值右边不能有/无默认值的参数。 调用时，实参与形参从左向右依次结合。

```c++
int add(int x = 5, int y = 6); // 默认参数值必须在这里给出
int main() {
    add();
}
int add(int x, int y) { // 这里不能再指定默认值了
    return x + y;
}
```

```c++
int add(int x = 5, int y = 6) { // 默认参数值必须在这里给出
    return x + y;
}
int main() {
    add();
}
```

-------

### 类的成员

- 可以为数据成员提供一个类内初始值。创建对象时，类内初始值用于初始化数据成员，没有初始值的成员将被默认初始化。
- 访问控制：

| 类型        | 对象内部函数和数据            | 同类型其他对象 | 子类   | 外部函数和外部类               |
| --------- | -------------------- | ------- | ---- | ---------------------- |
| public    | 可以访问（直接使用成员名访问；本列下同） |         | 可以访问 | 可以访问（使用`对象名.成员名`的方式访问） |
| protected | 可以访问                 |         |      | 不可访问                   |
| private   | 可以访问                 |         |      | 不可访问                   |

- 构造函数
  - 在对象创建时被自动调用。
  - 默认构造函数是调用时不需要实参的构造函数，有以下两种形式（二者不可同时出现在类中，否则编译错误）：
    - 参数表为空；
    - 全部参数都有默认值。
  - 若类中**未定义构造函数**，编译器将在需要时**自动生成**一个默认构造函数，其
    - 参数列表为空，不为数据成员设置初始值；
    - 如果类内定义了成员的初始值，则使用类内定义的初始值；
    - 如果没有定义类内的初始值，则以默认方式初始化（调用数据成员的默认构造函数）；
    - 基本数据类型的数据，默认初始化的值不确定。
  - 若类中**定义了构造函数**，默认情况下编译器不会为类生成隐含的默认构造函数，这时建立对象时必须给出初始值以作为调用构造函数的实参。若仍希望编译器隐含地生成默认构造函数，可以使用`Clock() =default`。
  - 所谓**委托构造函数**，是使用类的其他构造函数执行初始化过程的构造函数。如

    ```c++
    Clock(int h, int m, int s): hour(h), minute(m), secong(s) {}
    Clock(): Clock(0, 0, 0) {} // delegate constructor
    ```

    有点像这样：

    ```java
    public class Employee extends Person {
        protected int employeeNumber;
        protected String workPhoneNumber;
    	
        public Employee(String aName, String aPhoneNumber, String anAddress, int aNumber, String aWorkPhoneNumber){
            //显式调用父类构造方法Person(String, String, String)
            super(aName, aPhoneNumber, anAddress);
            employeeNumber = aNumber;
            workPhoneNumber = aWorkPhoneNumber;
        }
        public Employee(int aNumber, String aWorkPhoneNumber){
            //隐含调用父类构造方法Person() { this("", "", ""); }
            employeeNumber = aNumber;
            workPhoneNumber = aWorkPhoneNumber;
            //或使用"this("", "", "", aNumber, aWorkPhoneNumber);"替代以上两句
        }
        public Employee(){
            //隐含调用父类构造方法Person()
            this(0, ""); // 看见没！
        }
    } // 有点跑题了o.O
    ```

  - **复制构造函数**是以本类已存在的对象的引用为形参，初始化同类型的新对象的构造函数。若程序中没有声明复制构造函数，则编译器会**自动生成**一个隐含的复制构造函数（如不希望编译器自作多情，在C++98里可以将复制构造函数声明为 private 并且不提供函数实现；在C++11里，用`=delete`指示编译器不生成默认的复制构造函数）
    在以下三种情况下，复制构造函数会被调用：
    - 定义一个对象时，用本类另一个对象作为初始值；
    - 若函数形参是类的对象，调用函数时，将使用实参对象初始化形参对象；
    - 如果函数的返回值是类的对象，函数执行完成返回主调函数时，将使用 return 语句中的对象初始化一个临时无名对象，传递给主调函数，此时发生复制构造。（这种情况也可以通过移动构造避免不必要的复制）
  - **移动构造函数**：`class_name(class_name &&)`
    && 是右值引用；函数返回的临时变量是右值。
- 析构函数
  是在对象的生存期结束的时刻由系统自动调用的，先完成对象被删除前的一些清理工作，然后再释放此对象所属的空间。如果程序中未声明析构函数，编译器将**自动产生**一个默认的析构函数，其函数体为空。析构函数**不接受任何参数**。

### 类的组合

- 构造组合类对象时的初始化次序：
  - 首先对构造函数初始化列表中列出的成员（包括基本类型成员和对象成员）进行始化，**初始化次序是成员在类体中定义的次序**。
    - 成员对象构造函数调用顺序：按对象成员的声明顺序，先声明者先构造；
    - 初始化列表中未出现的成员对象，调用默认构造函数（即无形参的）初始化。
  - 处理完初始化列表之后，再执行构造函数的函数体。

### 前向引用声明

像这样：

```c++
class B; // 前向引用声明
class A {
public:
	void f(B b);
};
class B{
public：
	void g(A a);
};
```

但是，在提供一个完整的类声明之前，不能声明该类的对象，也不能在内联成员函数中使用该类的对象。当使用前向引用声明时，只能使用被声明的符号，而不能涉及类的任何细节。具体见教材P121~122。

### 结构体和联合体

如果一个结构体全部数据成员都是公有的，并且没有用户自定义的构造函数，没有基类和虚函数，这个结构体的变量可以这么赋值：
`STRUCT_NAME myStruct = { data_member_1_val, data_member_2_val, ...}`
关于联合体，见教材P130~133（尤其是例4-8）。

### 枚举类

```c++
// enum class enumeration_identifier [:underlying_type] { enumerator_list };
// e.g.
enum class Side { Right, Left };
Side s = Side::Right;
```

-------

### 作用域

- 函数原型作用域
  函数**原型**（不是函数定义？）中的参数，作用域为 '(' 到 ')' 之间。
- 局部作用域
  函数的形参、在块中声明的标识符；其作用域**自声明处起**，限于块中。具有局部作用域的变量也称为局部变量。
- 类作用域
  类的成员具有类作用域，其范围包括类体和非内联成员函数的函数体。
  如果在类作用域以外访问类的成员，要通过类名（访问静态成员），或者该类的对
  象名、对象引用、对象指针（访问非静态成员）。
- 文件作用域
  不在前述各个作用域中出现的声明，就具有文件作用域，这样声明的标识符其作用
  域**开始于声明点**，结束于文件尾。
- <del>命名空间作用域（？）</del>
  <del>具有命名空间作用域的变量也称为全局变量。</del>

### 可见性

- 如果某个标识符在外层中声明，且在内层中没有同一标识符的声明，则该标识符在内层可见。
- 对于两个嵌套的作用域，如果在内层作用域内声明了与外层作用域中同名的标识符，则外层作用域的标识符在内层不可见。

### 生存期

- 静态生存期
  - 如果对象的生存期与程序的运行期相同，则称它具有静态生存期。
  - 在文件作用域中声明的对象具有这种生存期。
  - 在函数内部声明静态生存期对象，要冠以关键字 static。如

    ```c++
    void f() { static int a = 1; int b = 2; ... } // f也可能是main
    ```

    其中 a 具有**全局寿命，局部可见**，且只有在程序**第一次**进入函数时被初始化。
- 动态生存期
  - 块作用域中声明的，没有用static修饰的对象是动态生存期的对象（习惯称局部生存期对象）。
  - 开始于程序执行到**声明点**时，结束于命名该标识符的作用域结束处。
  - 上例中的 b 是局部变量，具有动态生存期，每次进入函数时都会被初始化。
- 定义时未指定初值的基本类型静态生存期变量，会被赋予0值初始化；对于动态生存期变量，不指定初值则初值不确定。

### 类的静态成员

- 静态数据成员 (static) ：在类内声明，但必须在**类外**定义和初始化（使用类名限定）。
- 静态函数成员 (static) ：主要用于处理该类的静态数据成员，可以直接访问该类的静态数据和静态函数成员；若要访问非静态成员，要通过**对象名**（类名也不行）来访问。

### 类的友元

- 友元函数：
  - 友元函数是在类声明中由关键字 friend 修饰说明的非成员函数，在它的函数体中能够通过对象名访问 private 和 protected成员。
  - 作用：增加灵活性，使程序员可以在封装和快速性方面做合理选择。
  - 访问对象中的成员必须通过对象名。调用时和普通的类外函数一样。
- 友元类：
  - 若一个类 A 为另一个类 B 的友元类，则类 A 的所有成员都能访问类 B 的私有和保护成员，类 A 的所有成员函数都是 B 类的友元函数。
  - 声明语法：将友元类名在另一个类中使用friend修饰说明。如：

    ```c++
    class B {
    	friend class A;
    	...
    }
    ```

### const

- 常对象：`const 类名 对象名;` or `类名 const 对象名;`
  用 const 进行修饰的对象，必须进行初始化，不能被更新：
- 常成员：
  用 const 进行修饰的类成员，分为常数据成员和常函数成员。
  - 常成员函数：`类型说明符 函数名(参数表) const;`
    不更新对象的数据成员。因为 const 是函数类型的一个组成部分，在函数实现时也要带 const: `类型说明符 函数名(参数表) const { ... }`。也正是因此，const 关键字可以被用于参与对重载函数的区分。
    通过常对象只能调用它的常成员函数。
  - 常数据成员：
    如果在一个类中说明了常数据成员，那么任何函数都不能对该成员赋值。构造函数对该数据成员进行初始化，只能通过初始化列表。可见教程P165~166例5-8. 
- 常引用：`const 类型说明符& 引用名`
  被引用的对象不能被更新；如果用常引用做形参，便不会意外地发生对实参的更改。
- 常数组：数组元素不能被更新（详见第6章）：`类型说明符 const 数组名[大小]...`
- 常指针（指向常量的指针）：不能通过指向常量的指针改变所指对象的值，但指针本身可以改变，可以指向另外的对象。如：

  ```c++
  int a;
  const int *p1 = &a; //p1是指向常量的指针
  int b;
  p1 = &b; //正确，p1本身的值可以改变
  *p1 = 1; //编译时出错，不能通过p1改变所指的对象
  b = 2; // *p1 == 2 ?
  ```

- 指针（类型的）常量：指针本身的值不能被改变，即指针指向固定位置。

  ```c++
  int a = 1, b = 2;
  int* const p2 = &a;
  p2 = &b; //错误，p2是指针常量，值不能改变
  *p2 = b; // 可以 ? 即(*p2 = 2) ?
  ```

### 多文件结构

一个工程可以划分为多个源文件：
- 类声明文件（.h文件）（其实还有一种.hpp文件，它将类的声明和实现放在一起；此文件有些格式上的限制）
- 类实现文件（.cpp文件）：需要#include相关的头文件
- 类的使用文件（main()所在的.cpp文件）：需要#include相关的头文件

### 外部变量

- 如果一个变量除了在定义它的源文件中可以使用外，还能被其它文件使用，那么就称这个变量是外部变量。
- 文件作用域中定义的变量，默认情况下都是外部变量，但在其它文件中如果需要使用这一变量，需要在这些文件中用extern关键字加以声明。可见教材P170~171. 

### 外部函数

- 在所有类之外声明的函数（也就是非成员函数），都是具有文件作用域的。
- 这样的函数都可以在不同的编译单元中被调用，只要在调用之前进行引用性声明（即声明函数原型）即可。也可以在声明函数原型或定义函数时用extern修饰，其效果与不加修饰的默认状态是一样的。

--------

### 数组和指针

- 指针（型）函数：`存储类型 数据类型 *函数名(...) { // 函数体语句 }`
  若函数的返回值是指针，该函数就是指针类型的函数。
  不要将非静态局部地址用作函数的返回值。如：在子函数中定义局部变量后将其地址返回给主函数，就是非法地址。
  在子函数中通过动态内存分配 new 操作取得的内存地址返回给主函数是合法有效
  的，但是内存分配和释放不在同一级别，要注意不能忘记释放，避免内存泄漏。
- 指向函数的指针（函数指针）：`存储类型 数据类型 (*函数指针名)(...) { // 函数体 };`
  函数指针指向的是程序代码存储区。
- 动态内存分配：
  - `new 类型名T(初始化参数列表)`：
    在程序执行期间，申请用于存放T类型对象的内存空间，并依初值列表赋以
    初值。不给出初始化参数列表时，会调用默认构造函数。
    结果值：成功：T类型的指针，指向新分配的内存；失败：抛出异常。
  - `delete 指针p`：p 必须是由 new 生成的。
  - `new 类型名T [ 数组长度 ]`
  - `delete[] 数组名p`：p 必须是用new分配得到的数组首地址。

### 深复制与浅复制

- 浅层复制： 实现对象间数据元素的一一对应复制。这时复制的指针与原指针指向同一块内存，可能造成程序逻辑的错误（对同一变量重复操作）甚至内存空间被多次释放（重复 delete）的运行错误。
- 深层复制：当被复制的对象数据成员是指针类型时，不是复制该指针成员本身，而是将指针所指对象进行复制。

-------

### 其他

#### rand()

```c++
// #include <stdlib.h>
int rand(void) // returns a pseudorandom integer in the range 0 to RAND_MAX (32767).
```

#### srand()

```c++
// #include <stdlib.h>
void srand(unsigned int seed);
// we can use: srand((unsigned)time(NULL));
```

#### time()

```c++
// C: #include <time.h>
// C++: #include <ctime> or <time.h>
/*
 * Return the time as seconds elapsed since midnight, January 1, 1970, 
 * or -1 in the case of an error.
 *
 * timer: The return value is stored in the location given by timer. 
 * timer may be NULL, in which case the return value is not stored.
 */
time_t time(time_t *timer);
```

#### 函数的可变参数: initializer_list<T>

initializer_list<T>: 是一种标准库类型，用于表示某种特定类型的值的数组。其对象中的元素永远是常量值，我们无法改变其对象中元素的值。Copying an **initializer_list** creates a second instance of a list pointing to the same objects; the underlying objects are not copied. （浅复制）
例如：

```c++
#include <initializer_list>
#include <iostream>

int main()
{
    using namespace std;
    // Create an empty initializer_list c0
    initializer_list <int> c0;

    // Create an initializer_list c1 with 1 element
    initializer_list <int> c1{ 3 };

    // Create an initializer_list c2 with 5 elements 
    initializer_list <int> c2{ 5, 4, 3, 2, 1 };

    // Create a copy, initializer_list c3, of initializer_list c2
    initializer_list <int> c3(c2);

    // Create a initializer_list c4 by copying the range c3[_First, _Last)
    const int* c3_ptr = c3.begin();
    c3_ptr++;
    c3_ptr++;
    initializer_list <int> c4(c3.begin(), c3_ptr);

    // Move initializer_list c4 to initializer_list c5
    initializer_list <int> c5(move(c4)); // move ???

    cout << "c1 =";
    for (auto c : c1)
        cout << " " << c;
    cout << endl; // c1 = 3

    cout << "c2 =";
    for (auto c : c2)
        cout << " " << c;
    cout << endl; // c2 = 5 4 3 2 1

    cout << "c3 =";
    for (auto c : c3)
        cout << " " << c;
    cout << endl; // c3 = 5 4 3 2 1

    cout << "c4 =";
    for (auto c : c4)
        cout << " " << c;
    cout << endl; // c4 = 5 4

    cout << "c5 =";
    for (auto c : c5)
        cout << " " << c;
    cout << endl; // c5 = 5 4
}
```

#### getline()

需要string. 

```c++
string s;
getline(cin, s); // 输入整行字符串;
getline(cin, s, ','); // 使用其它分隔符(',')作为字符串结束的标志. 
```

#### UML

在UML中，虚线箭头表示依赖，实线空心三角形表示继承，实线空心菱形表示共享聚集，实线实心菱形表示组成聚集。其他的见课本。