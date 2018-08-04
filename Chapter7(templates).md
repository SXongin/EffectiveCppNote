# Templates and Generic Programming

## Item 41 Understand implicit interfaces and compile-time polymorphism

- classes 和 templates 都支持接口（interfaces）和多态（polymorphism）。
- 对 classes 而言接口时显示的（explicit），以函数签名为中心。多态则是通过virtual函数发生于运行期。
- 对 template 参数而言，接口是隐式的（implicit），奠基于有效表达式。多态则是是通过 template 具现化和函数重载解析（function overloading resolution）发生于编译期。

## Item 42 Understand the two meanings of typename

- 声明 template 参数时，前缀关键字 class 和 typename 可互换。
- 请使用关键字 typename 标识嵌套从属类型名称；但不得在 base class lists（基类列）或 member initialization list（成员初值列）内以它作为 base class 修饰符。

## Item 43 Know how to access names in templatized base classes

- 可在 derived class templates 内通过“this->”指涉 base class templates 内的成员名称，或籍由有一个 using 明白写出的“base class 资格修饰符”完成。

## Item 44 Factor parameter-independent code out of templates

- Templates 生成多个 classes 和多个函数，所以任何 template 代码都不该与某个造成膨胀的 template 参数产生相依关系。
- 因非类型模板参数（non-type template parameters）而造成的代码膨胀，往往可消除，做法是以函数参数或 class 成员变量替换 template 参数。
- 因参数类型（type parameters）而造成的代码膨胀，往往可降低，做法是让带有完全相同二进制表述（binary representations）的具现类型（instantiation types）共享实现码。

## Item 45 Use member function templates to accept "all compatible types"

- 请使用 member function templates（成员函数模板）生成“可接受所有兼容类型”的函数。
- 如果你声明 member templates 用于“泛化 copy 构造”或“泛化 assignment 操作”，你还是需要声明正常的 copy 构造函数和 copy assignment 操作符。

## Item 46 Define non-member functions inside templates when type conversions are desired

- 当我们编写一个 class template，而它所提供之 “与此 template 相关的”函数支持“所有参数之隐式类型转换”时，请将那些函数定义为“class template 内部的 friend 函数”。sx：可以使用 friend 函数再调用一个 helper 函数。

## Item 47 Use traits classes for information about types

- Traits classes 使得“类型相关信息”在编译期可用。他们以 templates 和“templates 特化完成实现。
- 整合重载技术（overloading）后，traits classes 有可能在编译期对类型执行 if…else 测试。

## Item 48 Be aware of template metaprogramming

- Template metaprogramming（TMP，模板元编程）可将工作由运行期移往编译期，因而得以实现早期错误侦测和更高的执行效率。
- TMP 可被用来生成“基于政策选择组合”（based on combinations of policy choices）的客户定制代码，也可用来避免生成对某些特殊类型并不适合的代码。
- ps：TMP 可用于确保量度单位正确、优化矩阵运算和生成客户定制之设计模式。
