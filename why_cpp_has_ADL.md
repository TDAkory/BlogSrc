# Why C++ has Argument-dependent lookup

```shell
xxx.cc:100: error: reference to 'sort' is ambiguous
    sort(vec_.begin(), vec_.end(), std::less<double>());

yyy.h:5 note: candidate found by name lookup is 'sort'
namespace sort{
          ^
```

问题的来源，是在一个复杂项目的编译时，由于新引入的一个库的文件`xxx.cc:100`包含一句`sort`语句，报出了如上的编译错误。编译器发现有多个不同的`sort`名字候选，无法确定调用哪一个，按照编译器的提示，它首先找到的是一个位于`yyy.h:5`名为 `sort` 的命名空间。其中 `xxx.cc` 是库的源文件，而 `yyy.h` 是复杂项目自身的源文件。

这里引起了我们的兴趣：

* 编译器为什么会找到一个命名空间，什么是 name lookup ？
* 为什么库和复杂项目单独编译的时候都没有问题 ？

## 什么是 name lookup

> [Name lookup](https://en.cppreference.com/w/cpp/language/lookup)

按照定义，名称查找是这样一个过程：当程序中遇到一个名称时，将其与引入该名称的声明关联起来。它确保了代码中的每个名称都能正确地关联到其声明。这个过程包括非限定名称查找和限定名称查找，以及在需要时的参数依赖查找和模板参数推导：

* 非限定名称查找([Unqualified name lookup](https://en.cppreference.com/w/cpp/language/unqualified_lookup))：当使用未限定的名称时（如std），编译器会在全局或命名空间作用域内查找该名称的声明。对于函数名来说，非限定名称查找还包括参数依赖查找。
* 限定名称查找([Qualified name lookup](https://en.cppreference.com/w/cpp/language/qualified_lookup))：当名称前有明确的命名空间或作用域限定符时（如std::cout），编译器会在指定的命名空间或作用域内查找。
* 参数依赖查找（ADL）：在函数调用时，如果函数名称未限定，编译器还会在函数参数类型的命名空间中查找可能的函数声明。
* 重载解析：如果名称查找找到了多个具有相同名称的声明，编译器将根据上下文和参数类型来选择最合适的声明。

对于函数和函数模板名称，名称查找可以将多个声明与同一名称关联起来，并且可能从参数依赖查找中获得额外的声明（模板参数推导也可能适用），这一组声明集被传递给重载解析，来选择最终要使用的声明。完成选择之后，才会考虑成员访问规则，即其仅在名称查找和重载解析之后考虑。

对于所有其他名称（变量、命名空间、类等），名称查找只能将多个声明关联到同一个实体，否则它必须产生单一声明，以便程序能够编译。在作用域中查找名称时，会找到该名称的所有声明，有一个例外，被称为“struct hack”或“类型/非类型隐藏。

> 什么是 struct hack
>
> 同一作用域内的名称冲突：在C++中，如果在同一作用域内，一个名称被用作不同类型的声明，比如一部分声明是类型（如类、结构体、联合体或枚举），而另一部分声明是非类型（如变量、非静态数据成员或枚举器），这时会发生名称冲突。
>
> 当名称冲突发生时，如果类型名称（类、结构体、联合体或枚举）不是通过typedef声明的，那么这个类型名称在查找时会被隐藏。这意味着，当你尝试使用这个名称时，编译器会首先查找非类型名称。
>
> 尽管发生了名称冲突，但C++编译器不会报错，因为这种隐藏是有意为之的，以允许类型和非类型名称共存于同一作用域。

```c
// 要访问被隐藏的类型名称，你必须使用详细类型说明符（elaborated type specifier）。这通常涉及到使用作用域运算符::来指定完整的类型名称。例如，如果你有一个名为MyType的类和同名的变量MyType，你可以使用::MyType来指代类类型
class MyType {};

int MyType = 10;  // 同一个作用域内，MyType作为变量名

// 访问类类型，需要使用作用域运算符
MyType::MyType instance;  // 正确，访问类MyType
```

## 非限定名称查找

## 限定名称查找

## 参数依赖查找

## 回到问题

## 为什么C++会有ADL

## 参考引用

- [Name lookup](https://en.cppreference.com/w/cpp/language/lookup)
