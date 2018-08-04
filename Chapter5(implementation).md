# Implementation

## Item 26 Postpone variable definitions as long as possible

- 尽可能延后变量定义式的出现。这样做可增加程序的清晰度并改善程序效率。ps：循环内使用变量应该比较赋值操作和构造及析构函数的成本，决定循环外定义还是循环内定义。

## Item 27 Minimize casting

- 如果可以，尽量避免转型，特别是在注重效率的代码中避免 dynamic_casts， 如果有个设计需要转型动作，试着发展无需转型的替代设计。
- 如果转型是必要的，试着将它隐藏于某个函数背后，客户随后可以调用该函数，而不需将转型放进他们自己的代码内。
- 宁可使用 C++-style（新式）转型，不要使用旧式转型。前者很容易辨识出来，而且也比较有着分门别类的职掌。

## Item 28 Avoid returning "handles" to object internals

- 避免返回 handles（包括 references、指针、迭代器）指向对象内部。遵守这个条款可增加封装性，帮助 const 成员函数的行为像个 const，并将发生“虚吊号码牌”（dangling handles）的可能性降至最低。

## Item 29 Strive for exception-safe code

- 异常安全函数（Exception-safe functions）即使发生异常也不会泄露资源或允许任何数据结构败坏。这样的函数区分为三种可能的保证：基本型、强烈型、不抛异常型。
- “强烈保证”往往能够以copy-and-swap 实现出来，但“强烈保证”并非对所有函数都可实现或具备现实意义。
- 函数提供的“异常安全保证”通常最高只等于其所调用之各个函数的“异常安全保证”中的最弱者。

## Item  30 Understand the ins and outs of  inlining

- 将大多数 inlining 限制在小型、被频繁调用的函数身上。这可使日后的调试过程和二进制升级（binary upgradability）更容易，也可使潜在的代码膨胀问题最小化，是程序的速度提升机会最大化。
- 不要只因为 function templates 出现在头文件上，就将他们声明为 inline。

## Item 31 Minimize compilation dependencies between files

- 支持“编译依存性最小化”的一般构想是：相较于声明式，不要相依于定义式。基于此构想的两个手段是 Handle classes 和 Interface classes。
- 程序库头文件应该以“完全且仅有声明式”（full and declaration-only forms）的形式存在；这种做法不论是否涉及 templates 都适用。
