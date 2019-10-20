# 深度探索C/C++数组名与指针

指针是C/C++的特色，而数组名与指针有着太多相似之处，甚至在多数情况下，数组名可以当作指针来使用。因此可能存在着这样一种误解：数组名就是指针。而实际上，数组名并非是指针，它有着更为深层的意义。

### 一、数组名不是指针

#### 1. 用sizeof判断数组名与指针的长度不相同

```c++
#include <iostream>
using namespace std;
int main() {
    char str[20] = "hello world!";
    char *p = str;
    cout << sizeof(str) << endl;
    cout << sizeof(p) << endl;
    return 0;
}
```

输出结果为：

```c++
20
8
```

因为在64位系统中，指针的长度为8，所以程序第7行输出的结果为8；而程序中第6行输出的结果并不是8，而是20。因此可以证明数组名并不是指针。



### 二、数组名很像指针

#### 1. 数组名的确代表了数组第一个元素的地址

```c++
#include <iostream>
using namespace std;
int main() {
    int array[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int *p = array;
    cout << array << endl;
    cout << p << endl;
    cout << *array << endl;
    cout << *p << endl;
    return 0;
}
```

输出结果为：

```c++
0x7ffee7fad4e0
0x7ffee7fad4e0
1
1
```

从代码中可以发现，数组array与指针p都存放着数组元素1所在的地址，且对它们进行提领操作（dereference）都能得到元素1。

#### 2. 数组名可作为指针参数传递到函数中

```c++
#include <cstring>
#include <iostream>
using namespace std;
int main() {
    char str1[20] = "hello world!";
    char str2[20];
    strcpy(str2, str1);
    cout << "str1: " << str1 << endl;
    cout << "str2: " << str2 << endl;
    return 0;
}
```

输出结果为：

```c++
str1: hello world!
str2: hello world!
```

C标准库函数strcpy的函数原型为：

```c
char *strcpy(char *__dst, const char *__src);
```

从中可以看出，strcpy函数接收两个char类型的指针变量，在以上代码中char类型数组的数组名也可以作为char指针传入函数中。



### 三、数组名究竟是什么

#### 1. 数组名指代一种数据结构

```c++
int array[10];
cout << sizeof(array) << endl;
```

程序第2行输出结果为10，这表明array代表了一种含有10个int类型数据的一种数据结构。这种数据类型的大小为10；

#### 2. 数组名可作为指针常量

```c++
char str1[20] = "hello world!";
str1++;
```

上诉代码是不能编译通过的，原因就在于数组名可以转化为指向其指代实体的指针常量，其指向的地址是不能被改变的。

#### 3. 数组名可能会失去其指代数据结构的内涵

```c++
#include <iostream>
using namespace std;
void ArraySize(char str[]) {
    cout << sizeof(str) << endl;
}
int main() {
    char str[20] = "hello world!";
    ArraySize(str);
    return 0;
}
```

输出结果为8，而不是数组的大小20；可以看出，数组名作为函数形参时，沦为了一个普通指针。



### 四、结论

-   **数组名指代一种数据结构**
-   **数组名可作为指针常量**
-   **数组名可能会失去其指代数据结构的内涵**
    -   **数组名作为函数形参时，在函数体内失去了本身的特性，沦为了一个普通指针**
    -   **在失去其内涵的同时，也失去了其作为常量指针的特性，可以对其做算术运算**