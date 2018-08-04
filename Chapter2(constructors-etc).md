# Constructors, Destructors, and Assignment Operators

## Item 5 Know what functions C++ silently writes and calls

- 编译器可以暗自为 class 创建 default 构造函数、copy 构造函数、copy assignment 操作符，以及析构函数。

## Item 6 Explicitly disallow the use of compiler-generated functions you not want

- 为驳回编译器自动（暗自）提供的机能，可将相应的成员函数声明为 private 并且不予实现。使用像 Uncopyable 这样的 base class 也是一种方法。

## Item 7 Declare destructors virtual in polymorphic base classes

- Polymorphic （带有多态性质的）base classes 应该声明一个 virtual 析构函数。如果 class 带有任何 virtual 函数，他就应该拥有一个 virtual 析构函数。
- Classes 的设计目的如果不是作为 base classes 使用，或不是为了具备多态性（polymorphically），就不该声明 virtual 析构函数。
- 抽象类（abstract classes）应该有一个纯虚（pure virtual）析构函数，并提供一个定义。

## Item 8 Prevent exceptions from leaving destructors

- 析构函数绝对不要吐出异常。如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下它们（不传播）或结束程序。
- 如果客户需要对某个操作函数运行期间抛出的异常作出反应，那么class应该提供一个普通函数（而非在析构函数中）执行该操作。

## Item 9 Never call virtual functions during construction of destruction

- 在构造和析构期间不要调用 virtual 函数，因为这类调用从不下降至 derived class （比起当前执行构造函数和析构函数的那层）。

## Item 10 Have assignment operators return  a reference to *this

- 令赋值（assignment）操作符返回一个 reference to *this.

## Item 11 Handle assignment to self in operator=

- 确保当对象自我赋值时 operator= 有良好的行为。其中技术包括比较“来源对象”和“目标对象”的地址、精心周到的语句顺序、以及 copy-and-swap。
- 确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确。

## Item 12 Copy all parts of an object

- Copying 函数应确保复制“对象内的所有成员变量”以及“所有 base class 成分”。不要尝试以某个 copying 函数实现另一个 copying 函数。应该将共同机能放进第三个函数中，并由两个 copying 函数共同调用。
