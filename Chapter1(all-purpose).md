# Accustoming Yourself to C++

## Item 1 View C++ as a federation of languages

## Item 2 Prefer consts, enums, and inlines to #defines

- 对于单纯常量，最好以 const 对象或者 enums 替换 #defines。
- 对于形似函数的宏（macros），最好改用 inline 函数替换 #defines。

## Item 3 Use const whenever possible

- 将某些东西声明为 const 可帮助编辑器侦测出错误用法。const 可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。
- 编译器强制实施 bitwise constness， 但你编写的程序时应该使用“概念上的常量性”（conceptual constness）。
- 当 const 和 non-const 成员函数有着实质等价的实现时，令 non-const 版本调用 const 版本可避免代码重复。

## Item 4 Make sure that objects are initialized before they're used

- 为内置型对象进行手工初始化，因为C++不保证初始化它们。
- 构造函数最好使用成员初值列（member initialization list），而不要在构造函数本体内使用赋值操作（assignment）。初值列列出的成员变量，其排列次序应该和它们在class中的声明次序相同。
- 为免除“跨编译单元之初始化次序”问题，请以 local static 对象替换 non-local static 对象。