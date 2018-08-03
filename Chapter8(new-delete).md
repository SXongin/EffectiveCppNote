# Customizing new and delete

## Item 49 Understand the behavior of the new-handler

- set_new_handler 允许客户指定一个函数，在内存分配无法获得满足时被调用。
- Nothrow new 是一个颇为局限的工具，因为它只适用于内存分配；后续的构造函数调用还是可能抛出异常。
- 一个设计良好的 new-handler 函数应该做到以下事件之一：**让更多内存可以被使用**、**安装另一个 new-handler**、**卸除 new-handler**、**抛出 bad_alloc（或派生自 bad_alloc）的异常**或者**不返回**。

## Item 50 Understand when it makes sense to replace new and delete

- 有许多理由需要写个自定义的 new 和 delete，包括改善性能、对 heap 运用错误进行调试、收集 heap 实用信息。
- 改善性能包括这几个方面
    1. 为了增加分配和归还的速度
    2. 为了降低缺省内存管理器带来的空间额外开销
    3. 为了弥补缺省分配器中的非最佳对位（suboptimal alignment)
    4. 为了将相关对象成簇集中（比如为了降低“内存页错误”的概率）
    5. 为了获得非传统的行为

## Item 51 Adhere to convention when writing new and delete

- operator new 应该内含一个无穷循环，并在其中尝试分配内存，如果它无法满足内存需求，就该调用 new-handler。它也应该有能力处理 0 bytes 申请。Class 专属版本则应该还处理“比正确大小更大的（错误）申请”。
- operator delete 应该在收到 null 指针时不做任何事。Class 专属版本则还应该处理“比正确大小更大的（错误）申请”。

## Item 52 Write placement delete if you write placement new

- 当你写一个 palcement operator new，请确定也写出了对应的 placement operator delete。如果没有这样做，你的程序可能会发生隐微而时断时续的内存泄露。
- 当你声明 placement new 和 placement delete，请不要无意识（非故意）地遮掩了它们地正常版本。