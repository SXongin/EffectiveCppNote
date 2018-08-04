# Design and Declarations

## Item 18 Make interfaces easy to use correctly and hard to use incorrectly

- 好的接口很容易被正确使用，不容易被误用。你应该在你的所有接口中努力达成这些性质。
- “促进正确使用”的办法包括接口的一致性，以及与内置类型的行为兼容。
- “阻止误用”的办法包括建立新类型、限制类型上的操作，束缚对象值，以及消除客户的资源管理责任。
- tr1::shared_ptr 支持定制型删除器（custom deleter）。这可防范DLL问题，可被用来自动解除互斥锁（mutexes；见条款14）等等。（ps：C++11 提供了 std::shared_ptr)。

## Item 19 Treat class design as type design

- **新 type 的对象应该如何被创建和销毁**？这会影响到你的 class 的构造函数和析构函数以及内存分配函数和释放函数（operator new，operator new[]，operator delete 和 operator delete[]——见第8章）的设计，当然前提是如果你打算撰写它们。
- **对象的初始化和对象的赋值应该有什么样的差别**？这个答案决定了你的构造函数和赋值（assignment）操作符的行为，以及其间的差异。很重要的是别混淆了”是“初始化”和“赋值”，因为它们对应于不同的函数调用（见条款4）。
- **新 type 的对象如果被 passed by value（以值传递），意味着什么**？记住，copy 构造函数用来定义一个 type 的 pass-by-value 该如何实现。
- **什么是新 type 的合法值？**对 class 的成员变量而言，通常只有某些数值集是有效的。那些数值集决定了你的 class 必须维护的约束条件（invariants），也就决定了你的成员函数（特别是构造函数、赋值操作符和所谓的“setter”函数）必须进行的错误检查工作。它也影响函数抛出的异常、以及（极少被使用的）函数异常明细列？（exception specifications）。
- **你的新 type 需要配合某个继承图系（inheritance graph）吗**？如果你继承自某些既有的classes，你就受到那些 classes 的设计的束缚，特别是受到“它们的函数是 virtual 或 non-virtual”的影响（见条款 34 和条款 36）。如果你允许其他 classes 继承你的 class，那会影响你所声明的函数——尤其是析构函数——是否为 virtual（见条款7）。
- **你的新 type 需要什么样的转换**？你的 type 生存于其他一海票 types 之间，因而彼此间该有转换行为吗？如果你希望允许类型 T1 之物被隐式转换为类型 T2 之物，就必须在 class T1 内写一个类型转换函数（operator T2）或在 class T2 内写一个 non-explicit-one-argument 构造函数。如果你只允许 explicit 构造函数存在，就得写出专门负责执行转换的函数，且不得为类型转换操作符（type conversion operators）或 non-explicit-one-argument 构造函数（条款 15 由隐式和显式转换函数的范例。）
- **什么样的操作符和函数对此新 type 而言是合理的**？这个问题的答案决定你将为你的 class 声明哪些函数。其中某些该是 member 函数，某些则否（见条款 23，24，46）。
- **什么样的标准函数应该驳回**？那些正是你必须声明为 private 者（见条款 6）。
- **谁该取用新 type 的成员**？这个提问可以帮助你决定哪个成员为 public，哪个为 protected， 哪个为private。它也帮助你决定哪一个 classes 和/或 functions 应该是 friends,以及将它们嵌套于另一个之内是否合理。
- **什么是新 type 的“未声明接口”（undeclared interface）**？它对效率、异常安全性（见条款 29）以及资源运用（例如多任务锁定和动态内存）提供何种保证？你在这些方面提供的保证将为你的 class 实现代码加上相应的约束条件。
- **你的新 type 有多么一般化**？或许你其实并非定义一个新 type，而是定义一整个 types 家族。果真如此你就不该定义一个新 class，而是定义一个新的 class template。
- **你真的需要一个新 type 吗**？如果只是定义新的 derived class 以便为既有的 class 添加机能，那么说不定单纯定义一个或多个 non-member 函数或 templates，更能够达到目标。

## Item 20 Prefer pass-by-reference-to-const to pass-by-value

- 尽量以 pass-by-reference-to-const 替换 pass-by-value。前者通常比较高效，并可避免切割问题（slicing problem）。
- 以上规则并不适用于内置类型，以及 STL 的迭代器和函数对象。对它们而言，pass-by-value 往往比较适当。

## Item 21 Don't try to return a reference when you must return an object

- 绝不要返回 pointer 或 reference 指向一个 local stack 对象， 或返回 reference 指向一个 heap-allocated 对象， 或返回 pointer 或 reference 指向一个 local static 对象而有可能同时需要多个这样的对象。条款 4 已经为“在单线程环境中合理返回reference 指向一个 local static 对象”提供了一份设计实例。

## Item 22 Declare data members private

- 切记将成员变量声明为 private。这可赋予客户访问数据的一致性、可细微划分访问控制、允诺约束条件获得保证，并提供 class 作者以充分的实现弹性。
- protected 并不比 public 更具有封装性。

## Item 23 Prefer non-member non-friend functions to member functions

- 宁可拿 non-member non-friend 函数替换 member 函数。这样做可以增加封装性、包裹弹性（packaging flexibility）和机能扩充性。

## Item 24 Declare non-member functions when type conversions should apply to all parameters

- 如果你需要为某个函数的所有参数（包括被 this 指针所指的那个隐喻参数）进行类型转换，那么这个函数必须是个non-member。ps：也尽量不要是 friend 函数。

## Item 25 Consider support for  a non-throwing swap

- 当 std::swap 对你的类型效率不高时，提供一个 swap 成员函数，并确定这个函数不抛出异常。
- 如果你提供一个 member swap，也该提供一个 non-member swap 用来调用前者。对于class（而非 templates），也请特化 std::swap。
- 调用 swap 时应针对 std::swap 使用 using 声明式，然后调用 swap 并且不带任何“命名空间资格修饰”。为“用户自定义类型”进行 std templates 全特化是好的， 但千万不要尝试在 std 内加入某些对 std 而言全新的东西。
