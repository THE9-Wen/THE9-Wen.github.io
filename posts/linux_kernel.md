linux运行的硬件基础
寄存器
1. 通用寄存器
  EAX：累加器
  EBX：基地址寄存器
  ECX：计数
  EDX：存放数据
  EBP：基指针
  ESP: 堆栈指针
  ESI：源变址
  EDI：目标变址
2. 段寄存器
  CS: 代码段
  DS: 数据段
  SS: 堆栈段
  ES: 其他段
3. 状态和控制寄存器
  状态和控制寄存器是由标志寄存器（EFLAGS）、指令指针（EIP）和 4 个控制寄存器组成
  - 指令指针寄存器和标志寄存器
  - 控制寄存器
4. 系统地址寄存器
  保存操作徙动要保护的信息和地址转换表信息
5. 调试寄存器和测试寄存器

内存地址
物理地址和虚拟地址：在 8086 的实模式下，把某一段寄存器左移 4 位，然后与地址 ADDR 相加后被直接送到内
存总线上，这个相加后的地址就是内存单元的物理地址，而程序中的这个地址就叫逻辑地址
（或叫虚地址）。

