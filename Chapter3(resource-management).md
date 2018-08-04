# Resource Management

## Item 13 Use objects to manage resources

- 为防止资源泄露，请使用 RAII 对象，它们在构造函数中获得资源并在析构函数中释放资源。
- 两个常被使用的 RAII classes 分别是 tr1::shared_ptr 和 auto_ptr。前者通常是较佳选择，因为其 copy 行为较为直观。若选择 auto_ptr, 负责会使它（被复制物）指向 null。（ps：C++11 标准下可以使用 std::shared_ptr 和 std::uniq_ptr 代替。）

## Item 14 Think carefully about copying behavior in resource-managing classes

- 复制 RAII 对象必须一并复制它所管理的资源，所以资源的 copying 行为决定 RAII 对象的 copying 行为。
- 普遍而常见的 RAII class copying 行为是：抑制 copying、施行引用计数法（reference counting）、（ps：复制底部资源和转移底部资源的所有权）。

## Item 15 Provide access to raw resources in resource-managing classes

- APIs 往往要求访问原始资源（raw resources），所以每一个 RAII class 应该提供一个“取得其所管理之资源”的方法。
- 对原始资源的访问可能经由显示转换或隐式转换。一般而言显示转换比较安全，但隐式转换对客户比较方便。

## Item 16 Use the same form in corresponding uses of new and delete

- 如果你在 new 表达式中使用 []，必须要在相应的 delete 表达式中也使用 []。如果你在 new 表达式中不使用 []，一定不要在相应的 delete 表达式中使用 []。

## Item 17 Store newed objects in smart pointers in standalone statements

- 以独立语句将 newed 对象存储于（置入）智能指针内。如果不这样做，一旦异常抛出，有可能导致难以察觉的资源泄露。
