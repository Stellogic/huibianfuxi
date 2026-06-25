# AGENTS.md

本仓库是《汇编语言》期末复习资料。回答问题、整理笔记或生成代码时，默认按课件中的当前课程版本处理。

## 当前课程汇编版本

- 主线：`x86-64` 汇编。
- 汇编器：`GAS`（GNU Assembler）。
- 语法：Intel 语法，而不是 AT&T 语法。
- 常用文件：`.s` 源文件、`.o` 目标文件、可执行文件。
- 常用工具：`gcc`、`as`、`ld`、`objdump`、`gdb`。
- 常用程序框架：`.intel_syntax noprefix`、`.data`、`.text`、`.globl main`、`main:`、`ret`。

## 答题口径

- 默认使用 `x86-64 + GAS + Intel syntax` 写代码和讲解。
- 访问全局变量/静态数据时，优先按课件口径使用 `label[rip]` 这类 RIP 相对寻址。
- 函数参数按题面平台判断：Win64 常见 `RCX/RDX/R8/R9`；Linux x86-64 常见 `RDI/RSI/RDX/RCX/R8/R9`。
- MASM、16 位/32 位 DOS、`.asm/.obj/.exe`、`PROC/ENDP`、`int 21h` 主要来自旧题或往年卷，只做识别和对比；除非题目明确要求，不要把它们当成本届默认编程写法。
- ARM/鲲鹏/AArch64 主要用于概念题、选择题和架构对比；编程题主线仍是 x86-64 GAS。
