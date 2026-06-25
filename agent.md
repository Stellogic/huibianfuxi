# Agent Notes

本仓库是《汇编语言》期末复习资料。回答问题、整理笔记或补充资料时，默认按下面课程口径处理。

## 当前正在学习的汇编版本

根据课件口径，本课程当前主线是 **x86-64 架构下的 GAS 汇编，使用 Intel 语法**。

- 默认写代码、讲题、整理模板时，使用 `x86-64 + GAS + Intel syntax`。
- 源文件通常是 `.s`，程序框架常见 `.intel_syntax noprefix`、`.data`、`.text`、`.globl main`、`main:`。
- 工具链默认是 GNU 工具链：`gcc`、`as`、`ld`、`objdump`、`gdb`。
- 课件中 Windows x86-64 和 Linux x86-64 都会出现；函数参数、栈对齐、调用约定必须看题面平台。
- 旧题里的 16 位/32 位、DOS、MASM、`.asm/.obj/.exe`、`PROC/ENDP`、`int 21h` 只作为识别和对比，不作为本届编程题默认写法。
- ARM/鲲鹏/AArch64 主要用于概念题、选择题和架构对比，大题与编程默认仍按 x86-64 GAS 处理。

## 课程主线

- 主体架构：x86-64。
- 主体汇编器：GAS（GNU Assembler）。
- 主体语法风格：Intel 语法，而不是 AT&T 语法。
- 主体开发环境/工具链：`gcc`、`as`、`ld`、`objdump`、`gdb`。
- 章节 PDF 中同时出现 Windows + x86-64 和 Linux + x86-64；其中很多课堂示例是 64 位 Windows + x86-64，常见 `winio64.a`、`.seh_proc`、Win64 ABI 参数寄存器等。
- Linux + x86-64 也会讲，尤其用于 ABI 对比、反汇编或工具链说明。答题时先看题目/代码给的平台，不要无条件套 Linux ABI。
- 常见命令：
  - `gcc -S -Og -g -masm=intel -o xxx.s xxx.c`
  - `gcc -c -o xxx.o xxx.s`
  - `gcc -o xxx xxx.o ./lib/winio64.a`
  - `objdump -d -M intel xxx.o`
  - `gdb` 中常用 `set disassembly-flavor intel`

## GAS 重点口径

- GAS 源文件通常是 `.s`。
- 代码段用 `.text`，数据段用 `.data`。
- 全局符号用 `.globl` 或 `.global`。
- 外部符号可用 `.extern`，但 GAS 中直接引用外部符号通常也能交给链接器处理。
- Intel 语法要点：
  - 操作数顺序是“目的操作数, 源操作数”。
  - 寄存器不写 `%`。
  - 立即数不写 `$`。
  - 内存类型常写 `byte ptr`、`word ptr`、`dword ptr`、`qword ptr`。
  - 64 位全局/静态数据访问常见 `label[rip]`。
- GAS 注释：
  - `#` 单行注释。
  - `/* ... */` 多行注释。
  - 分号 `;` 不是本课程 GAS 主口径的注释符。

## 考试/复习优先级

- 优先掌握 x86-64 + GAS + Intel 语法。
- 编程题要求尽量写完整程序框架，通常包含 `.data`、`.text`、`.globl main`、`main:`、必要子程序和 `ret`。
- C 与汇编互推是重点，尤其是：
  - `gcc -S -masm=intel` 生成的汇编。
  - Win64 ABI 参数传递：前 4 个整数/指针参数通常是 `RCX`、`RDX`、`R8`、`R9`，调用者还要预留 32 字节 shadow space，返回值在 `RAX/EAX`。
  - x86-64 Linux ABI 参数传递：前 6 个整数/指针参数通常是 `RDI`、`RSI`、`RDX`、`RCX`、`R8`、`R9`，返回值在 `RAX/EAX`。
  - `call`、`ret`、`push`、`pop` 对 `RSP` 的影响。
  - 分支、循环、数组寻址、移位、乘除法、标志位、小端存储。
- 多模块和宏汇编也按 GAS 口径：
  - 源文件包含：`.include "sub.s"`，相当于文本插入。
  - 模块连接：各源文件分别汇编/编译成 `.o`，再链接。
  - 导出符号：`.globl func`。
  - 宏：`.macro` 开始，`.endm` 结束，宏是汇编时展开，不是运行时 `call/ret`。

## MASM 的位置

- MASM 是 Microsoft Macro Assembler，主要出现在旧 16/32 位 x86 或旧真题语境中。
- 本课程当前复习主线不是 MASM，不需要按 MASM 深入展开。
- 需要能识别常见对照：
  - `PUBLIC func` 约等于 GAS 的 `.globl func`。
  - `EXTRN func:NEAR` 约等于 GAS 的 `.extern func`。
  - `INCLUDE sub.asm` 约等于 GAS 的 `.include "sub.s"`。
  - `PROC` / `ENDP` 是 MASM 的过程开始/结束写法。
- 如果题目明显出现 `.model`、`.code`、`.data`、`PROC`、`ENDP`、`PUBLIC`、`EXTRN`、`16 位 DOS`、`8086`、`BP/EBP` 栈参数等，再按旧 MASM/16/32 位题面识别处理。
- 不要把 MASM 的 `0ffh`、`;` 注释、`PROC/ENDP` 等写法混进本届 GAS 64 位编程题答案里，除非题目明确要求旧语法。

## ARM/鲲鹏 的位置

- ARM/鲲鹏主要用于概念题、选择题、架构对比和少量反汇编对比。
- 章节 PDF 第 1 章明确把 x86-64 作为熟悉对象、ARM/鲲鹏作为了解对象；录音笔记也说明 x86 与 ARM/鲲鹏对比可能选择题考，大题不以鲲鹏为主。
- 记忆重点：
  - x86 是 CISC，指令长度可变。
  - ARM/AArch64 是 RISC/Load-Store 风格，A64 指令通常固定 4 字节。
  - 鲲鹏 920 属于 ARMv8/AArch64 相关语境。
  - ARM64 参数/返回值常见 `x0`/`w0` 等寄存器，但本课程编程主线仍优先 x86-64 GAS。

## 回答用户时的默认策略

- 如果用户问“这个要不要掌握”，按优先级回答：GAS/x86-64 是主线；MASM 旧题识别；ARM/鲲鹏概念对比。
- 如果用户贴汇编代码但没有说明平台，优先判断是否符合 GAS Intel x86-64；遇到 `PROC`、`EXTRN`、`PUBLIC`、`0ffh`、`;` 等再提示可能是 MASM/旧题。
- 给编程答案时，默认使用 GAS Intel x86-64 风格。
- 涉及函数参数时，先判断平台：出现 `winio64.a`、`.seh_proc`、`RCX/RDX/R8/R9`、shadow space 多半按 Win64；出现 `RDI/RSI/RDX/RCX/R8/R9` 多半按 Linux x86-64。
- 给考试背诵版解释时，少讲工具细节，多讲“考点判断、易错点、怎么背”。

## PDF 核对记录

- 已按章节 PDF 课件本体核对，不只看 `.md` 笔记。
- 第 1 章 PDF：课程目标写明熟悉 x86-64、了解鲲鹏处理器；熟悉 GAS 常用汇编指令；示例出现 `gcc -Og -S -masm=intel`、`objdump -d -M intel`。
- 第 2 章 PDF：教学目标包含“能利用 GCC 实现 C 语言转换为汇编语言、目标文件和可执行文件”“能写出 GAS 汇编语言程序框架”。
- 第 10 章 PDF：示例使用 `.intel_syntax noprefix`、`.data`、`.text`、`.globl main`，并用 `gcc/as` 与 `winio64.a` / `io_linux64.a`。
- 第 13 章 PDF：明确写“GAS 支持的多模块方法：模块连接、子程序库、源文件包含”，并说明 `.global/.globl`、`.extern`；同时列出 Win64 ABI 与 Linux ABI。
- 第 14 章 PDF：实验要求是模块化程序设计与宏汇编，文件为 `.s`，子程序库示例为 `libmysub.a`，路径含 `ZZUGAS`。
