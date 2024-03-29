### RISC与CISC指令集
**RISC(Reduced Instruction Set Computing)**-精简指令集计算机和**CISC(Complex Instruction Set Computing)**-复杂指令集计算机是当前CPU的两种架构
|  | CISC | RISC |
| --- | --- | --- |
| 指令字长 | 指令字长不固定，复杂指令较多 | 指令字长固定，指令集精简 |
| 访存 | 访存指令不限制，多个指令周期 | 访存指令仅限Load/Store指令，一个指令周期 |
| 控制方式 | 微程序控制 | 组合逻辑控制 |
| 寄存器数量 | 内部通用寄存器数量较少 | 内部通用寄存器数量较多 |

### 流水线技术
若将一重复的处理过程**分解为若干子过程**，每个子过程都可在专用设备构成的流水线功能段上实现，并可与其他子过程**同时运行**，则这种技术称为流水线技术。  
分为五个阶段1、取指2、（IF）译码/读寄存器（ID）3、执行/计算地址（EX）4、访存（MEM）对存储器读写5、写回（WB）对寄存器写
| 问题类型 | 问题描述 | 解决方法 |
| --- | --- | --- |
| 流水线瓶颈 | 流水线中有速度慢的段 | 再分成几个段，或是资源重复 |
| 资源相关 | 多个指令在同一时钟周期争用同一功能部件 | 延迟指令执行，增加硬件资源 |
| 数据相关 | 前一条指令的结果影响了后续指令的执行顺序，RAW,WAW,WAR | 采用数据旁路，暂停 |
- **RAW(Read After Write)写后读**：后面指令用到前面指令所写的数据，**正常指令执行会出现的（写回是最后一个）**
- WAW(Write After Write)：两条指令写同一个单元
- WAR(Write After Read)：后面指令覆盖前面指令所读的单元

### CPU访存过程
虚拟地址->物理地址，虚拟地址为：页目录下标 + 页表下标 + 页内偏移
**CPU（虚拟地址）->TLB/页表（获得物理地址）->[Cache/主存] / 磁盘（得到物理地址对应的数据）**  
注意辅存与CPU直接不能直接通信，缺页中断时要先由硬盘调入主存
- TLB命中/Page命中：这个虚拟地址可以直接转换为物理地址（**虚页号->实页号**）
- 因此不存在TLB命中而页表不命中的情况。
- Cache命中：可以直接拿走我要的信息（**物理地址->数据**）
- 页表不命中，信息一定不在主存中。

### TLB和Cache的区别
- 快表是**页表**的一部分副本（地址映射）
- Cache是**内存数据**的一部分副本（数据内容）

### Cache可以加速地址映射吗
可以

### 存储器层次结构
随机访问存储器RAM
静态读写存储器(SRAM)：存取速度快
动态读写存储器(DRAM)：存储容量大
### 磁盘操作
平均存取时间=寻道时间+旋转延迟时间（旋转->转半圈的时间）+传输时间
### 汇编程序员可见
PC、PSW、通用寄存器、基址寄存器（多道程序）
### 寻址方式
- **基址**寻址（基址寄存器BR+偏移量A）面向系统，**多道程序**；A可变
- **变址**寻址（变址寄存器IX+偏移量A）面向用户，循环（**数组**）；A不可变

### 中断
中断指**程序运行中出现的紧急事件**的整个过程。程序运行过程中，系统外部、系统内部或者现行程序本身若出现紧急事件，处理机立即**中止现行程序的运行，自动转入相应的处理程序**(中断服务程序)，待处理完后，再返回原来的程序运行，这整个过程称为程序中断。  
外部设备向CPU发出的中断：
- **中断响应优先级**是由硬件排队线路或中断查询程序的查询顺序决定的，**不可动态改变**；
- **中断处理优先级**可以由**中断屏蔽字来改变**

### 异常
1. **硬故障中断**（硬件、终止）
- 存储器校验错、总线错误、Cache缺失
- Cache缺失硬件解决
2. **程序性异常**（软件、故障）
- 整除0、溢出、越界、缺页
- 内中断以及虚存失效如缺页

### 局部性原理
局部性原理有两种表现形式：
- 时间局部性（Temporal Locality）：如果程序中的某条指令一旦被执行，则不久的将来该指令可能再次被执行。
- 空间局部性（Spatial Locality）：一旦程序访问了某个存储单元，则在不久的将来，其附近的存储单元也最有可能被访问。
