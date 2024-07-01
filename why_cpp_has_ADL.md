# Why C++ has Argument-dependent lookup

```shell
xxx.cc:100: error: reference to 'sort' is ambiguous
    sort(vec_.begin(), vec_.end(), std::less<double>());

yyy.h:5 note: candidate found by name lookup is 'sort'
namespace sort{
          ^
```

问题的来源，是在一个复杂项目的编译时，由于新引入的一个库包含一句`sort`语句，报出了如上的编译错误。编译器发现有多个不同的`sort`名字候选，无法确定调用哪一个，按照编译器的提示，它首先找到的是一个名为 sort 的命名空间。这里引起了我们的兴趣，编译器为什么会找到一个命名空间，什么是 name lookup ？

## 什么是 name lookup

> [Name lookup](https://en.cppreference.com/w/cpp/language/lookup)

按照定义，名称查找是这样一个过程：当程序中遇到一个名称时，将其与引入该名称的声明关联起来。它确保了代码中的每个名称都能正确地关联到其声明。这个过程包括非限定名称查找和限定名称查找，以及在需要时的参数依赖查找和模板参数推导：

* 非限定名称查找([Unqualified name lookup](https://en.cppreference.com/w/cpp/language/unqualified_lookup))：当使用未限定的名称时（如std），编译器会在全局或命名空间作用域内查找该名称的声明。对于函数名来说，非限定名称查找还包括参数依赖查找。
* 限定名称查找([Qualified name lookup](https://en.cppreference.com/w/cpp/language/qualified_lookup))：当名称前有明确的命名空间或作用域限定符时（如std::cout），编译器会在指定的命名空间或作用域内查找。
* 参数依赖查找（ADL）：在函数调用时，如果函数名称未限定，编译器还会在函数参数类型的命名空间中查找可能的函数声明。
* 重载解析：如果名称查找找到了多个具有相同名称的声明，编译器将根据上下文和参数类型来选择最合适的声明。

## 参考引用

- [Name lookup](https://en.cppreference.com/w/cpp/language/lookup)