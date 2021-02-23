---
title: C programming language
published: true
categories: note
---

{% raw %}

## C语言简介

### 起源

Dennis Ritch & Ken Thompson在开发UNIX操作系统时设计了C语言，基于Thompson发明的B语言

### 优点

高效性，可移植性，强大而灵活，面向程序员(享受自由,承担责任)

### 应用

嵌入式开发, 科学编程, 操作系统, 编译器

### 使用步骤

定义程序目的，设计程序，编写代码，编译，运行程序，测试和调试程序，维护和修改程序。

### 编程机制

源代码编译为目标代码，链接库代码和启动代码W为可执行代码



## 数据

### 基本数据类型

| 有符号整形                   | 大小    |
| ---------------------------- | ------- |
| `int`                        | >=16bit |
| `short` / `short Int`        | >=16bit |
| `long` / `long int`          | >=32bit |
| `long long` /`long long int` | >=64bit |

无符号整形：unsigned前缀，表示的范围更大

| 实浮点数      | 精度 |
| ------------- | ---- |
| `float`       | >=6  |
| `double`      | >=10 |
| `long double` |      |

精度大小相关定义<limits.h><float.h>

字符类型：`char`

+ `ctype.h`中定义了很多有用的字符函数, 包括`is...()`检测和`to...()`转化

布尔类型：`_Bool`(unsigned int)

虚数`<complex.h>`

可移植类型`<stdint.h>` `<inttype.h>`

*查看自己机器中的类型大小*

```c
#include<stdio.h>

int main(void)
{
    printf("Type short has a size of %zd bytes.\n", sizeof(short));
    printf("Type int has a size of %zd bytes.\n", sizeof(int));
    printf("Type long has a size of %zd bytes.\n", sizeof(long));
    printf("Type long long has a size of %zd bytes.\n", sizeof(long long));
    printf("Type float has a size of %zd bytes.\n", sizeof(float));
    printf("Type double has a size of %zd bytes.\n", sizeof(double));
    printf("Type long double has a size of %zd bytes.\n", sizeof(long double));
    printf("Type char has a size of %zd bytes.\n", sizeof(char));
    printf("Type _Bool has a size of %zd bytes.\n", sizeof(_Bool));
    return 0;
}
```

```bash
Type short has a size of 2 bytes.
Type int has a size of 4 bytes.
Type long has a size of 8 bytes.
Type long long has a size of 8 bytes.
Type float has a size of 4 bytes.
Type double has a size of 8 bytes.
Type long double has a size of 16 bytes.
Type char has a size of 1 bytes.
Type _Bool has a size of 1 bytes.
```

*类型转换*

涉及两种类型运算，分别转换为更高级

计算最终结果转换为被赋值变量的类型

强制类型转换: `(type) expression`

### 数组和指针

#### 数组

声明：

```c
type name [size];
```

数组名是数组首元素的地址

`arry = &arry[0]`

初始化：

```c
type arry[n] = {var1,var2,[3]=var3};
```

C定义数组使借用指针的概念

`arry[n]=*(arry+n)`

多维数组：

```c
type arry[m][n] = {{...},{...},{...}};
//声明指向内含两个int的多维数组
int (* ptr) [2];
```

声明一个指向N唯数组的指针时，只能省略最左方括号的值

变长数组：

使用变量表示数组长度

```c
int arry[col][row];
//在函数原型中使用变长数组
//只有在变长数组中函数原型参数名才有用
type function(int m, int n, int arry[m][n]);
type function(int, int, int arry[*][*]);
```

复合字面量：提供临时值时使用

`(type){var_list}`

#### 指针

指针表示数据对象的地址

声明：

```c
type * ptr;
```

赋值：使用取地址符`&`获取数据对象地址

接引用：使用接引用符`*`获取储存在地址上的数据对象

指针递增表示增加一个存储单位

指针相减可得到两个元素之间的距离

### 字符串和字符串函数

#### 字符串

*字符串*是以空字符`\0`结尾的char类型数组

使用双引号`""`括起来的内容被视为指向该字符串储存位置的指针

字符串存储在*静态存储区*, 初始化数组获得字符串的副本, 初始化指针只获得地址

编译器可以使用内存中一个副本来表示所有相同的字符串字面量,建议初始化指针为字符串字面量时使用`const`限定符

```c
const char * strng = "this is a string";
//使用数组声明字符串可以省略大小
char strng[] = "this is a string";
```

#### 字符串函数

```c
#include<string.h>
//获取字符串的长度,不包含末尾空字符
size_t strlen(const char *s)

//拼接字符串,把第二个参数拼接到第一个参数后面,返回第一个参数地址,第二个参数不变
char *strcat(char *dest, const char *src)
//拼接字符串,第三个参数检查字符串大小
char *strncat(char *dest, const char *src, size_t n)
//可以使用strlen()获取字符串剩余空间,size为字符串数组大小,减一为末尾的空字符
strncat(dest, src, size-strlen(dest)-1)
    
//字符串比较,相等则返回1
int strcmp(const char *s1, const char *s2)
//比较前n个字符
int strncmp(const char *s1, const char *s2, size_t n)

//字符串复制,返回第一个参数地址
char *strcpy(char *dest, const char *src)
//复制前n个字符
char *strncpy(char *dest, const char *src, size_t n)


#include<stdio.h>
//printf输出到字符串中
int sprintf(char *s, const char *format, ...)
    
    
#include<stdlib.h>
//转换字符串为数字，nptr为输入字符串位置，endptr被用作储存输入数字结束地址，十进制base=10
long strtol(const char *nptr, char **endptr, int base)
```

自定义字符串查找函数

```c
//返回出现查找字符串的位置
char *search_string(char *target, char *source)
{
    char *return_ptr=NULL;
    int i = 0;
    int j;
    while (source[i] != '\0')
    {
        if (source[i] == target[0])
        {
            return_ptr = &source[i];
            j = 0;
            while (source[i + j] && target[j]){
                if (source[i + j] != target[j]){
                    return_ptr = NULL;
                    break;
                }
                j++;
            }
        }
        i++;
    }
    return return_ptr;
}
```

### 结构和其他数据形式

#### 结构

提高数据表达能力，定义新的类型

结构声明（模板）

```c
struct name {
    type property_name;
    ...
}/*variable*/;
```

声明结构变量

```c
struct name variable;
```

初始化

```c
struct name variable = {
    var1,
    .property = var2,
    ...
};
```

访问结构成员

```c
struct_var.property
struct_ptr->property
```

结构字面量

```c
struct_var = (struct name){var1, var2...}
```

伸缩型数组成员

```c
struct flex {
    int arry[];
};
//usage:
struct flex * ptr = malloc(sizeof(flex) + 5 * sizeof(int));
ptr->arry[4] = 0;
```

  匿名结构

```c
struct person {
    int id;
    struct {char first[20], char last[20]};
};

struct person ted = {9328, {"Ted", "Grass"}};
puts(ted.first);
```

保存结构（文件）

```c
#include<stdio.h>
fwrite(&struct_name, sizeof(struct_name), 1, fp); 
```

#### 联合

在同一内存空间中储存不同的数据类型

声明

```c
union hold {
    int digit;
    char letter;
    double bigfl;
};
```

初始化与访问

```c
zunion hold val;
union hold foo = {.int = 1};
union hold * ptr;

val.digit = 1;
val.letter = 'A';

ptr = & val;
ptr->int = 0;
```

匿名联合

```c
//example:
struct owner {
    char socsecurity[12];
	...
};
struct leasecompany {
    char name[40];
	char headquearters[40];
    ...
};
struct car_data{
    char make[15];
    int status;
    union {
        struct owner owncar;
        struct leasecompany leasecar;
    };
    ...
}

struct car_data flits;
flits.owncar.socsecurity = "SHAFLK";
```

#### 枚举

声明符号名称来表示整形常量(0, 1, 2...)，提高可读性

```c
enum spectrum {red, yellow, greem, blue};
enum spectrum color;
color = blue;
```

默认值：可以手动指定，默认从零或上一个指定值开始递增

````c
enum level {low = 100, medium = 500, high =2000};
````

### typedef

```c
typedef struct{
    double real;
    double imag;
}complex;
```

### 复杂的声明

`[]` `()`  优先级相同，从左向右结合，大于`*`

```c
int * risks[10];
//包含10个元素的数组，元素为指向int的指针

int (* risks)[10];
//指针，指包含10个元素的数组，元素为int

int * goods[10][5];
//10*5的数组，元素为指向int的指针

char (* flump[3])(int);
//3个元素的数组，元素为指针，指向参数为int返回char的函数
```

指向函数的指针：写出函数原型，函数名前加`*`外带括号`()`

函数名是函数的地址

```c
void f(char *);
void (* pf)(char *);
pf = f;
//usage:
pf("parameter");
//or
(* pf)("parameter");
```

### 位字段

该结构为每个字段提供标签，确定该字段宽度。例：包含4个1位的字段，存储在一个int中

```c
struct {
    unsigned int autfd : 1;
    unsigned int bldfc : 1;
    unsigned int undln : 1;
    unsigned int itals : 1;
} prnt;
```

使用标签访问：

```c
prnt.autfd = 0;
prnt.bldfc = 1;
```

使用未命名字段填充，使用0宽度字段强制对齐

```c
struct {
    unsigned int field1 : 1;
    unsigned int 		: 2;
    unsigned int field2 : 1;
    unsigned int 		: 0;
    unsigned int field3 : 1;
} stuff;
```

例子：使用位字段表示一个方框的属性

```c
#include<stdbool.h>
struct box_props {
    bool opaque				  : 1;
    unsigned int fill_color   : 3;
    unsigned int			  : 4;
    bool show_border		  : 2;
    unsigned int border_color : 3;
    unsigned int border_style : 2;
    unsigned int			  : 2;
}
```

## 输入输出

### 格式化输入输出

#### printf

`printf`返回打印字符个数

| 转换说明 | 输出                                  |
|:----:|:----------------------------------- |
| %a   | 浮点数、六进制数、p计数法                       |
| %A   | 浮点数、六进制数、p计数法                       |
| %c   | 单个字符                                |
| %d   | 有符号十进制整数                            |
| %e   | 浮点数，e计数法                            |
| %E   | 浮点数，e计数法                            |
| %f   | 浮点数，十进制计数法                          |
| %g   | 根据值的不同，自动选择%e或%f，%e用于指数小于-4或大于等于精度时 |
| %G   | 根据值的不同，自动选择%E或%f，%E用于指数小于-4或大于等于精度时 |
| %i   | 有符号十进制整数                            |
| %o   | 无符号八进制整数                            |
| %p   | 指针                                  |
| %s   | 字符串                                 |
| %u   | 无符号十进制整数                            |
| %x   | 无符号十六进制郑虎，使用十六进制数0f                 |
| %X   | 无符号十六进制郑虎，使用十六进制数0F                 |
| %%   | 打印一个百分号                             |

在%和转换说明之间插入修饰符可修饰基本的转换说明

| 修饰符 | 含义                                                         |
| :----: | ------------------------------------------------------------ |
|  标记  | - 左对齐，+ 添加符号，空格 前导空格符号占位，# 把结果转化为另一种形式，0 前导0填充宽度 |
|  数字  | 最小字符宽度                                                 |
| .数字  | 精度：对于%eEf小数点后精度，对于%gG有效数字最大位数，对于%s打印字符最大数量，对于整形表示待打印数字最小位数 |
|   h    | short int / unsigned short int                               |
|   hh   | signed char / unsigned char                                  |
|   l    | long int / unsigned long int                                 |
|   ll   | long long int /unsigned long long int                        |
|   L    | long double                                                  |
|   t    | ptrdiff_t(两个指针的差值)                                    |
|   z    | size_t(sizeof的返回值)                                       |
|   j    | intmax_t或unitmax_t定义<stdint.h>                            |

打印长的字符串：`\`+`Enter`在一个字符串中分行，`"string1"` `“string2”`直接相连 

#### scanf

`scanf`返回成功读取对象个数

|  转换说明   | 含义                                             |
| :---------: | ------------------------------------------------ |
|     %c      | 输入解释为字符                                   |
|     %d      | 输入解释为有符号十进制整数                       |
| %e %f %g %a | 输入解释为浮点数                                 |
| %E %F %G %A | 输入解释为浮点数                                 |
|     %i      | 输入解释为有符号十进制整数                       |
|     %o      | 输入解释为有符号八进制数                         |
|     %p      | 输入解释为指针                                   |
|     %s      | 输入解释为字符串(从第一个非空白到下一个空白之间) |
|     %u      | 输入解释为无符号十进制整数                       |
|    %x %X    | 输入解释为有符号十六进制整数                     |

|      修饰符       | 含义                       |
| :---------------: | -------------------------- |
|         *         | 抑制赋值(提供变量代替参数) |
|       数字        | 最大输入宽度               |
| hh ll h l L j z t | 存储为相应的类型           |

### 字符输入输出

用户输入字符被收集存储在*缓冲区*中，按下`Enter`程序才可以使用用户输入的字符

程序从缓冲区中获取单个字符的函数：

```c
getchar()
putchar()
```

`<stdio.h>`中定义的`EOF`表示文件末尾

按下`Enter`发送输入, 这一动作也传递了`\n`换行符, 解决方案如下

```c
//flash the cache
while (getchar()!='\n')
	continue;
```

### 字符串输入输出

````c
char *fgets(char *s, int n, FILE *stream)
````

+ 第一个参数指定存储位置
+ 第二个参数指定最大输入长度, 读取n-1个字符, 或读取到第一个换行符, 存储换行符, 以空字符结尾
+ 第三个参数指定输入来源(stdin)
+ 成功返回指向第一个参数的指针, 失败返回`NULL`
+ 遇到`EOF`返回`NULL`

````c
int puts(const char *s)
````

+ 输出字符串到屏幕上
+ 遇到控空字符`\0`停止
+ 末尾自动添加换行

````c
int fputs(const char *s, FILE *stream)
````

+ 输出字符到指定文件上(stdout)

使用getchar()和putchar()可以自定义输入输出函数

```c
#include <stdio.h>

//string input function(modified version of fgets)
//char out of length-->ignore & flash the catch
//'\n' in the end-->replace to '\0'
//st: output place
//n: length of the string
char *s_gets(char *st, int n)
{
    char *ret_val;
    int i = 0;

    ret_val = fgets(st, n, stdin);
    if (ret_val)
    //if (ret_val!=NULL)
    {
        //checkout the end of the input
        while (st[i] != '\n' && st[i] != '\0')
            i++;
        if (st[i] == '\n'){
            //input end with a "Enter":
            //replace the '\n' to '\0'
            st[i]='\0';
        }
        else{
            //input end with out of length:
            //remove the input reminded in catch
            while (getchar()!='\n')
                continue;
            //alert the user
            printf("The input is out of length\n");
        }
    }
    return ret_val;
    //return: the return of fgets
    //error / EOF-->NULL
    //succeed-->the address of the first parameter
}
```

### 文件输入输出

文本模式：文件内容转化为C模式，使用 \n 表示换行

二进制模式：显示文件底层内容，mac使用 \r 换行，MS-DOS使用 \r\n 换行

标准文件：stdin：标准输入(键盘)，stdout：标准输出(屏幕)，stderr：标准错误流(屏幕)

1. 打开文件

   ```c
   //标准输入输出库包含fopen()的函数原型
   #include<stdio.h>
   
   //文件指针，指向打开的文件，打开失败返回NULL
   FILE *fp;
   //fopen打开文件，返回文件指针
   fp = fopen("path/file name","mode")
   ```

   | 模式字符串                                                   | 含义                                                   |
   | ------------------------------------------------------------ | ------------------------------------------------------ |
   | `"r"`                                                        | 读模式                                                 |
   | `"w"`                                                        | 写模式，现有文件长度截为0，不存在则创建一个新文件      |
   | `"a"`                                                        | 写模式，在现有文件末尾添加内容，不存在则创建一个新文件 |
   | `"r+"`                                                       | 更新模式，可读可写                                     |
   | `"w+"`                                                       | 更新模式，现有文件长度截为0，不存在则创建一个新文件    |
   | `"a+"`                                                       | 更新模式，现有文件末尾添加内容，不存在则创建一个新文件 |
   | `"rb"` `"wb"` `"ab"` `"rb+"` `"r+b"` `"wb+"` `"w+b"` `"ab+"` `"a+b"` | 与上一模式相同，以二进制模式打开                       |
   | `"wx"` `"wbx"` `"w+x"` `"wb+x"` `"w+bx"`                     | 文件已存在则打开失败，以独占模式打开                   |

2. 读写文件

   字符输入输出：

   ```c
   int getc(FILE *stream)
   ```

   ```c
   int putc(int c, FILE *stream)
   ```

   格式化输入输出：

   ```c
   int fprintf(FILE *stream, const char *format, ...) 
   ```

   + 返回打印字符个数
   + 第一个参数指定输出位置
   + format与printf相同

   ```c
   int fscanf(FILE *stream, const char *format, ...)
   ```

   + 返回成功读取个数
   + 第一个参数指定读取位置
   + format与scanf相同

   字符串输入输出：

   ````c
   char *fgets(char *s, int n, FILE *stream)
   ````

   + 第一个参数指定存储位置
   + 第二个参数指定最大输入长度, 读取n-1个字符, 或读取到第一个换行符, 存储换行符, 以空字符结尾
   + 第三个参数指定输入来源(stdin)
   + 成功返回指向第一个参数的指针, 失败返回`NULL`
   + 遇到`EOF`返回`NULL`

   ````c
   int fputs(const char *s, FILE *stream)
   ````

   + 输出字符到指定文件上(stdout)

   随机读取：

   ```c
   int fseek(FILE *stream, long off, int whence)
   ```

   + fseek移动文件指针
   + off: 偏移量
   + whence：`SEEK_SET` 文件开头，`SEEK_CUR` 当前位置，`SEEK_END` 文件末尾
   + 成功返回0，失败返回-1

   ```c
   long ftell(FILE *stream)
   ```

   + ftell告诉当前文件指针与开头距离

   ```c
   void rewind(FILE * fp);
   ```

   + 文件指针移动到文件开头

   其他：

   ```c
   int ungetc(int c, FILE *fp)
   //把指定字符放回输入缓冲区
       
   int fflush(FILE *fp)
   //输出缓冲区中所有的未写入数据被发送到fp指定的输出文件
   
   int setvbuf(FILE * restrict fp, char * restrict buf, int mod, size_t size)
   //创建一个供标注io函数替换使用的缓冲区
   //buf为NULL自动创建缓冲区
   //mode：_IOFBF完全缓冲，_IONBF无缓冲，_IOLBF缓冲
   
   size_t fwrite(const void* restrict ptr, size_t size, size_t nmemb, FILE * restrict fp)
   //二进制写入
   //ptr指向待写入数据
   //nmemb表示待写入数据块的数量
       
   size_t fread(void * restrict ptr, size_t size, size_t nmemb, FILE * restrict fp)
   //ptr指向储存读出数据的地址
   //返回成功读取数量nmember
   
   int feof(FILE *fp)
   //输入调用检测到文件末尾，返回一个非零值
   
   int ferror(FILE *fp)
   //读或写出现错误时，返回一个非零值
   ```

3. 关闭文件

   ```c
   //关闭文件，检查是否成功关闭，成功则返回0
   if (fclose(fp) != 0)
       printf("Error in closing file\n");
   ```



## 语句、运算符、表达式

*语句*是一条完整的计算机指令，简单语句使用一个分号结尾；复合语句使用{花括号}括起

运算对象是*运算符*操作的对象，*表达式*由运算对象和运算符组成，每个表达式都有一个值

表达式的*副作用*是对文件或对象的修改，所有副作用在*序列点*之前发生(；或完整表达式)

| 赋值运算符 | 含义                                                         |
| :--------: | ------------------------------------------------------------ |
|    `=`     | 赋值表达式，*左值*(用于标识或定位存储位置)可用在赋值表达式的左侧 |
|    `+=`    | +的增强赋值                                                  |
|    `-=`    | -的增强赋值                                                  |
|    `*=`    | *的增强赋值                                                  |
|    `/=`    | /的增强赋值                                                  |
|    `%=`    | %的增强赋值                                                  |

| 算数运算符 | 含义                       |
| :--------: | -------------------------- |
| `+ - * /`  | 四则运算                   |
|    `%`     | 取模                       |
|  `++ --`   | 递增递减，分为前缀后缀模式 |

| 关系运算符 | 含义(返回bool值) |
| :--------: | ---------------- |
|    `<`     | 小于             |
|    `<=`    | 小于等于         |
|    `>`     | 大于             |
|    `>=`    | 大于等于         |
|    `*`    | 相等             |
|    `!=`    | 不等             |

| 逻辑运算符 | 含义 |
| :--------: | ---- |
|    `&&`    | 和   |
|    `||`    | 或   |
|    `!`     | 非   |

| 其他运算符 | 含义                                                        |
| :--------: | ----------------------------------------------------------- |
|  `sizeof`  | 对象占用字节大小，返回size_t                                |
|  `(type)`  | 强制类型转换                                                |
|    `,`     | 连接多个表达式，最左侧先求值，表达式的值为最右侧表达式的值< |
|    `?:`    | 条件运算符：expression ? expression_true : expression_false |
| `_Alignof` | 返回一个对象的对其要求，返回size_t                          |

| 位操作 | 含义     |
| :----: | -------- |
|  `~`   | 按位取反 |
|  `&`   | 按位与   |
|  `|`   | 按位或   |
|  `^`   | 按位异或 |
|  `<<`  | 左移     |
|  `>>`  | 右移     |

位操作应用：

````c
//int to binary string
#include<limits.h>

char * itobs(int n, char * ps)
{
    const static int size = CHAR_BIT * sizeof(int);
    for (int i = size - 1; i >= 0; i--, n >>= 1)
		ps[i] = (1 & n) + '0';
    return ps;
}
````

```c
//invert last n bits of a binary int
int invert_end(int num, int bits)
{
    //00000111...111
    //     |<--n-->|
    int mask = 0;
    int bitval = 1;
    while (bits-- > 0)
    {
		mask |= bitval;
        bitval <<= 1;
    }
    return num ^ musk;
}
```

## 流程控制

### 循环

```c
while (expression)
    statement
```

```c
//使用逗号运算符，包含更多信息
for (initialize; test; update)
    statement
```

```c
do
    statement
while (expression);
```

循环辅助语句：

`break`  终止循环，进行下一阶段

`continue`  跳过本次迭代，进行下一轮迭代

### 分支

```c
if (expression1)
    statement1
else if (expression2)
    statement2
else
    statement3
```

### 跳转

```c
switch (expression)
{
    case label1: statement1 //使用break跳出switch
    case label2: statement2
    default: statement3
}
```

```c
goto label;
...
label: statement
```



## 函数

*函数原型*定义函数参数类型和返回类型, 供编译器使用, 可使用`void`类型表示没有参数或返回类型

```c
//variable name can be ignored
type function_name(type1 var1,type2 var2)
```

*函数*是完成特定任务的独立程序代码单元

````c
type function_name(type1 var1,type2 var2)
	statement
````

使用`return`语句从函数中返回值

### 编译多源代码文件的程序

1. 函数原型和已定义的字符常量放在头文件中`.h`

2. 导入头文件

   ```c
   #include"myhead.h"
   ```

3. `cc`编译源代码为目标代码`.o`

   ```
   $cc source1.c source2.c
   ```

4. `gcc`链接目标代码和目标代码或源代码

   ```bash
   gcc target1.o target2.o source3.c
   ```

### main函数

`main`函数是c程序中第一个执行的函数

+ 命令行参数

  ```c
  int main(int argc, char *agrv [])
  ```

  `argc`为传递参数个数

  `agrv`为参数列表，第一个参数为程序名称，参数使用空格分开

+ 返回值

  main返回0表示成功执行

  ````c
  int main(void)
  {
      return 0;
  }
  ````

  返回非零值表示执行失败，可使用exit函数在任何地方退出程序

  使用`<stdlib.h>`中定义的`EXIT_SUCCESS` `EXIT_FAILURE`提供返回值

  ```c
  exit(EXIT_SUCCESS);
  exit(EXIT_FAILURE);
  ```

### 内联函数

具有内部链接的函数可以成为内联函数

应该定义在首次使用它的文件中，相当于函数原型

使用函数说明符`inline`

例：一个常用的清空缓存函数

```c
inline static falshcache(void)
{
    while (getchar() != '\n')
        continue;
}
```

### 函数说明符

函数说明符`_Noreturn`表示函数不会把控制返回主调程序

## 存储类别

### 变量性质

+ 作用域
  1. 块作用域
  2. 函数作用域
  3. 函数原型作用域
  4. 文件作用域
+ 链接
  1. 外部链接：可以在多文件程序中使用
  2. 内部链接：只能在一个翻译单元中使用
  3. 无链接
+ 存储期
  1. 静态存储器，存在于*程序*执行期间
  2. 线程存储器，存在于*线程*执行期间
  3. 自动存储器，存在于*块*的执行期间
  4. 动态分配存储器

### 存储类别

| 存储类别     | 存储期 | 作用域 | 链接 | 声明方式                         |
| :----------- | :----- | ------ | ---- | -------------------------------- |
| 自动         | 自动   | 块     | 无   | 块内                             |
| 寄存器       | 自动   | 块     | 无   | 块内, 使用关键字`register`       |
| 静态外部链接 | 静态   | 文件   | 外部 | 所有函数体外                     |
| 静态内部链接 | 静态   | 文件   | 内部 | 所有函数体外，使用关键字`static` |
| 静态无连接   | 静态   | 块     | 无   | 块内, 使用关键字`static`         |

1. 自动变量

   显式声明起提示作用或覆盖外部变量, 使用关键字`auto`

   内层块会隐藏外层块的定义

2. 寄存器变量

   请求寄存器变量, 使用关键字`register`

   寄存器变量无法获取地址

3. 块作用域的静态变量

   在*块内*使用关键字`static`声明静态变量

   该变量只在编译时初始化一次

4. 外部链接的静态变量

   在所有函数体之外声明变量, 具有外部链接

   引用其他源代码文件中的外部变量, 必须使用关键字`extern`进行引用式声明

   外部变量只能声明一次, 且必须在定义该变量时进行

5. 内部链接的静态变量

   在所有函数体外, 使用关键字`static`声明

6. 动态分配，内存管理

   ```c
   #include<stdlib.h>
   //传递所需的字节数
   //返回指向动态分配内存块的首字节地址
   void *malloc(size_t size)
   //例：分配一块内存储存十个字符
   char *strng;
   strng = (char *)malloc(sizeof(char)*10);
   
   //释放malloc分配的内存
   void free(void *ptr)
   ```

### ANSI C 限定符

1. `const`

   限定变量不被修改

   + 在指针中使用

     ```c
     //不能指向别的地方
     type * const ptr = &var;
     //不能修改被指向的值
     const type * ptr = &var;
     ```

   + 对全局数据使用

     ```c
     //常量储存在文件中, 其他源文件使用extern引用, 共同编译
     //file1.c
     const double PI = 3.1415926535;
     //file2.c
     extern const double PI;
     ```

     ```c
     //在头文件中定义，其他文件包含该头文件
     //file1.h
     static const double PI = 3.1415926535;
     //file2.h
     #include "file1.h"
     ```

2. `volatile`

   告诉编译器，代理可以改变变量的值

   用于编译器优化

3. `restrict`

   只能用于指针，表示该指针是改变指向变量的唯一且初始的方式

   用于编译器优化

4. `_Atomic`

   一个线程对其进行操作时，其他线程不能访问

   用于多线程编程

5. `_alignas(type/size)`

   指定一个变量或类型的对齐值

## C预处理器

### 明示常量

`#define`

```c
#define 宏 替换体
```

+ 预处理器使用宏等价的替换文本进行替换程序中的宏
+ 嵌套替换
+ 不替换双引号中的宏

```c
//在类函数宏中使用参数
#define FUNC(X,Y) (X*Y)
```

+ 原理：对参数直接替换

+ 参数名：如果`x`是一个宏形参，`#x`转换为字符串“x”的形参名

+ 粘合：粘合运算符`##`把两个记号合成一个记号

+ 变参宏：参数为`...`，替换部分为`__VA_ARGS__`， 只能替换最后一个参数

### 文件包含

`#include`

+ 包含头文件：

  ```c
  #include<stdio.h>//在系统标准目录中查找
  #include"myio.h"//在当前目录或绝对路径中查找
  ```

+ 使用头文件

  1. 明示常量
  2. 宏函数
  3. 函数声明
  4. 结构模板定义
  5. 类型定义
  6. 共享变量（在一个源文件中定义，在头文件中引用式声明，包含该头文件的一系列函数可以使用）

### 其他指令

+ 取消定义

  `#undef` 取消已经定义的 #define 指令，无法取消定义部分预定义宏

+ 条件编译

  `#ifdef` `#else` `#endif` `#ifndef`

  根据标识符是否定义选择编译

  例：用于避免重复包含

  ```c
  #ifndef NAME_H_
  	#define NAME_H_
  	//the content of the head file
  #endif
  ```

  `#if` `#elif` `#else` `#endif`

  可使用C的关系运算符和逻辑运算符选择编译

  例：增加可移植性

  ```c
  #if defined (IBMPC)
  	#include<ibmpc.h>
  #elif defined (MAC)
  	#include<mac.h>
  #elif defined (WIN)
  	#include<win.h>
  #else
  	#include<general.h>
  #endif
  ```

+ 重置`__LINE__` 和 `__FILE__` 宏报告的行号和文件名

  ```c
  #line 10 "filename.c"
  ```

+ 编译时报错`#error`

  ```c
  #error error message
  ```

+ 修改编译器设置`#pragma`

### 预定义宏

| 宏                 | 含义                                               |
| ------------------ | -------------------------------------------------- |
| `__DATE__`         | 预处理日期                                         |
| `__FILE__`         | 当前源代码文件的文件名的字符串字面量               |
| `__LINE__`         | 当前源代码文件中行号的整形常量                     |
| `__STDC__`         | 设置为1时表明事项遵循c标准                         |
| `__STDC_HOSTED__`  | 本机环境设置为1， 否则设置为0                      |
| `__STDC_VERSION__` | 支持C99标准设置为199991L，支持C11标准设置为201112L |
| `__TIME__`         | 翻译代码的时间，格式为“hh:mm:ss”                   |

### 泛型编程

例：检测输入值的类型

```c
#define MYTYPE(X) _Generic((X),\
	int: "int",\
	float: "float",\
	double: "double",\
	default: "other",\
)
```

例：数学库中的类型变体

```c
#define SQRT(X) _Generic((X),\
	long double: sqrtl, \
	float: sqrtf,\
	default: sqrt\
)(X)

#define RAD_TO_DEG (180/(4*atanl(1)))

#define SIN(X) _Generic((X),\
	long double: sinl((X)/RAD_TO_DEG),\
	float: sinf((X)/RAD_TO_DEG),\
	default: sin((X)/RAD_TO_DEG)\
)
```

## 常用C库

### 数学库`#include<math.h>`

```bash
$gcc source.c -o target.o -lm
//使用-lm选项链接数学库
```

|           三角函數            |              描述               |
| :---------------------------: | :-----------------------------: |
|      double sin(double);      |              正弦               |
|      double cos(double);      |              余弦               |
|      double tan(double);      |              正切               |
|     double asin(double);      |        反正弦（0 ~ pi）         |
|      double acos(double)      |     反余弦（-pi/2 ~ pi/2）      |
|     double atan(double);      |     反正切（-pi/2 ~ pi/2）      |
| double atan2(double, double); | 返回正弦值为y/x的角度（-pi~pi） |
|     double sinh(double);      |            双曲正弦             |
|     double cosh(double);      |            双曲余弦             |
|     double tanh(double);      |            双曲正切             |

|          指数对数           |   描述   |
| :-------------------------: | :------: |
|     double exp(double);     | 指数函数 |
|    double sqrt(double);     | 开平方根 |
|     double log(double);     | 自然对数 |
|    double log10(double);    | 常用对数 |
| double pow(double, double); | x的y次幂 |
|  float powf(float, float);  | x的y次幂 |

|       取整函數        |  描述  |
| :-------------------: | :----: |
| double ceil(double);  | 上取整 |
| double floor(double); | 下取整 |

|            绝对值            |       描述       |
| :--------------------------: | :--------------: |
|        int abs(int);         |  求整型的绝对值  |
|     double fabs(double);     | 求浮点类型绝对值 |
| double cabs(struct complex); |  求复数的绝对值  |

|             取余数             |                    描述                    |
| :----------------------------: | :----------------------------------------: |
| double modf(double, double *); | 将参数的整数部分通过指针回传，返回小数部分 |
|  double fmod(double, double);  |            返回两参数相除的余数            |

### 通用工具库`#include<stdlib.h>`

1. `atexit(void (* fp)(void))` `exit()`

   atexit 注册函数，执行exit时按照注册倒序执行

   exit从任意函数退出，并执行清理工作

2. 排序

   ```c
   void qsort(void *base, size_t nmemb, size_t size,
              int (*compar)(const void * p1, const void * p2));
   ```

   + base指向待排序数组首元素

   + 待排序项的数量

   + 待排序元素大小

   + 自定义比较函数

     + p1小于p2返回-1

     + p1等于p2返回0

     + p1大于p2返回1

   例：自定义比较函数，从小到大排序

   ```c
   int compar(const void *p1, const void *p2)
   {
       const double *a1 = (const double *)p1;
       const double *a2 = (const double *)p1;
       if (*a1 < *a2)
           return -1;
       else if (*a1 == *a2)
           return 0;
       else
           return 1;
   }
   ```

   例：自定义比较函数，比较两个名称

   ```c
   struct name{
       char first[20];
       chat last[20];
   }
   #include<string.h>
   int comp(const void *p1, const void *p2)
   {
       const struct * a1 = (const struct name)p1;
       const struct * a2 = (const struct name)p2;
       int res;
       res =strcmp(a1->las, a2->last);
       if (res != 0)
       	return res;
       else
   		return strcmp(a1->first, a2->first)
   }
   ```

### 断言库`#include<assert.h>`

断言是为了标识程序中某些条件为真的关键位置，不符合条件调用`abort()`终止程序

```c
assert(expression);
```

在引用断言库之前， `#define NDEBUG` 禁用程序中的assert()语句

```
_Static_assert(expression, "error message")
```

在预处理时断言一个整形常量

### 拷贝数组 `#include<string.h>`

```c
void * memcpy(void * restrict s1, const void * restrict s2, size_t n);
void * memmove(void * s1, const void * s2, size_t n)
```

拷贝s2指向位置的n个字节到s1，并返回s1的值

使用memcpy应该确保两个数列没有重合位置，memmove使用缓冲区，不用考虑重合

### 可变参数`#include<stdarg.h>`

1. 提供一个使用省略号的函数

   ```c
   void f1(int n, ...);
   void f2(...);//没有形参名，无效
   void f3(int x,double y, ...)
   void f4(int x, ..., double y);//无效
   ```

   使用 parmN 表述省略号前的形参，表示参数数量

2. 在函数定义中穿件一个`va_list`类型的变量

3. 用宏`va_start(va_list, parmN)`把该变量初始化成为一个参数列表

4. 用宏`va_arg(va_list, type)`访问参数列表，逐个检索

5. 用宏`va_end(va_list)`完成清理操作

+ 用宏`va_copy(va_list,va_list)`复制参数列表

例：多项求和

```c
#include<stdarg.h>
double sum(int n, ...)
{
    double total = 0;
    ar_list al;
    
    va_start(al, n);
    for (int i = 0; i < n; i++)
        total += va_arg(al, double);
    
    va_end(al);
    return total;
}
```

{% endraw %}
