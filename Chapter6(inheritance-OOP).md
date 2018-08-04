# Inheritance and Object-Oriented Design

## Item 32 Make Sure public inheritance models "is-a"

- “public 继承”意味着 is-a。适用于 base classes 身上的每一件事情一定也适用于 derived classes 身上，因为每一个 derived class 对象也都是一个 base class 对象。

## Item 33 Avoid hiding inherited names

- derived classes 内的名称会遮掩 base classes 内的名称。在 public 继承下从来没有人希望如此。
- 为了让被遮盖的名称再见天日，可使用 using 声明式或转交函数（forwarding functions）。

## Item 34 Differentiate between inheritance of interface and inheritance of implementation

- 接口继承和实现继承不同。在 public 继承之下，derived classes 总是继承 base class 的接口。
- pure virtual 函数只具体指定接口继承。
- 简朴的（非纯）impure virtual 函数具体指定接口继承及缺省实现继承。sx：也可以通过 pure virtual 函数声明及一个 protected non-virtual 函数 或者 pure virtual 函数的定义来实现，这种情况下需要 derived classes 主动要求使用默认实现。
- non-virtual 函数具体指定接口继承以及强制性实现继承。

## Item 35 Consider alternatives to virtual functions

- virtual 函数的替代方案包括 NVI（non-virtual interface） 手法及 Strategy 设计模式的多种形成。NVI 手法自身式一个特殊形式的 Template Method 设计模式。sx：NVI 以 public non-virtual 成员函数包裹较低访问性（private 或 protected）的 virtual 函数。还可以将 virtual 函数替换成“函数指针成员变量”或者函数对象（function objection）或者另一个继承体系内的 virtual 函数。
- 将机能从成员函数移到 class 外部函数，带来的一个缺点是，非成员函数无法访问 class 的non-public 成员。
- tr1::function 对象的行为就像一般函数指针。这样的对象可接纳“与给定之目标签名式（target signature）兼容”的所有可调用物（callable entities）。sx：C++11 标准下可以使用 std::function 代替。

## Item 36 Never redefine an inherited non-virtual function

- 绝对不要重新定义继承而来的 non-virtual 函数。

## Item 37 Never redefine a function's inherited default parameter value

- 绝对不要重新定义一个继承而来的 virtual 函数的缺省参数值，因为缺省参数值都是静态绑定，而 virtual 函数——你唯一应该覆写的东西——却是动态绑定。

## Item 38 Model "has-a" or "is-implemented-in-terms-of" through composition

- 复合（composition）的意义和 public 继承完全不同。
- 在应用域（application domain），复合意味 has-a（有一个）。在实现域（implementation domain），复合意味 is-implemented-in-terms-of（根据某物实现出）。

## Item 39 Use private inheritance judiciously

- Private 继承意味 is-implemented-in-terms-of（根据某物实现出）。它通常比复合（composition）的级别低。但是当 derived class 需要访问 protected base class 的成员，或需要重新定义继承而来的 virtual 函数时，这么设计是合理的。
- 和复合（composition）不同，private 继承可以造成 empty base 最优化。这对致力于“对象尺寸最小化”的程序库开发者而言，可能很重要。

## Item 40 Use multiple inheritance judiciously

- 多重继承比单一继承复杂。它可能导致新的歧义性，以及对 virtual 继承的需要。
- virtual 继承会增加大小、速度、初始化（及赋值）复杂度等等成本。如果virtual base classes 不带任何数据，将是最具有使用价值的情况。
- 多重继承的确由正当用途。其中一个情节涉及“public 继承某个 Interface class”和“private 继承某个协助实现的class”的两相组合。
