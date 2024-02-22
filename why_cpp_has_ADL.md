# Why C++ has Argument-dependent lookup

```shell
xxx.cc:100: error: reference to 'sort' is ambiguous
    sort(vec_.begin(), vec_end(), std::less<double>());

yyy.h:5 note: candidate found by name lookup is 'sort'
namespace sort{
          ^
```

## 参考引用

- [Name lookup](https://en.cppreference.com/w/cpp/language/lookup)