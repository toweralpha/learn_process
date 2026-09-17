# learn_process

A personal learning-notes repository. Each document here records one topic I worked
through myself — the concepts, the mental model, and the mistakes worth remembering —
written so that it can be re-read later without re-deriving everything from scratch.

> 个人学习笔记仓库。每份文档记录一个我完整学过的主题：概念、思维模型，以及值得记下来的坑，
> 目的是以后能直接重读，而不用重新推导一遍。

---

## Contents / 目录

| File | Topic | 文件 | 主题 |
| --- | --- | --- | --- |
| [`指针与结构体_学习讲义.pdf`](指针与结构体_学习讲义.pdf) | C pointers, structs & memory layout (reverse-engineering oriented) | 同上 | C 语言指针、结构体与内存布局（面向逆向工程） |

---

## `指针与结构体_学习讲义.pdf`

### What it is / 这是什么

A 39-page Chinese-language study guide (讲义) on **C pointers, structs and memory
layout**, written from a **reverse-engineering** point of view. It starts from the
lowest-level prerequisites and works up to reading real decompiler output.

一份 39 页的中文学习讲义，主题是 **C 语言指针、结构体与内存布局**，
视角是 **逆向工程**。内容从最底层的前置知识讲起，一路推到能读懂真实的反编译代码。

### Why it exists / 为什么有这份文档

It was written to explain two lines of decompiled code that a reverse-engineering
course presents very early, and which look intimidating at first sight:

这份文档是为了讲清一段逆向课程中出现得很早、第一眼看起来很吓人的反编译代码：

```c
*(_BYTE *)(*(_QWORD *)(a1 + 8208) + 4120LL)
(const char *)(a1 + 4112)
```

Those two lines only use three ideas — **bytes, pointers, structs** — but they need a
solid foundation to unpack. The guide builds that foundation, then takes the two lines
apart layer by layer and reconstructs the C `struct` they imply.

这两行只用到三个概念 —— **字节、指针、结构体** —— 但要讲透它们需要扎实的地基。
文档先把地基打好，再把这它们逐层拆开，并反推出它们对应的 C 结构体定义。

### What's inside / 内容结构

| Chapter | Content |
| --- | --- |
| **Ch. 0 — Prerequisite checklist** | A 10-item self-check list (number bases, bytes, addresses, endianness, `sizeof`, `&`, arrays, debugger basics), a bonus tier, and 3 minimal verification experiments to run |
| **Ch. 1 — Memory, bytes, endianness** | Memory as an addressed byte array, hex↔type-width table, `0x2010 = 8208`, little/big-endian memory diagrams, `union` verification |
| **Ch. 2 — Types, sizes, layout** | The three meanings of a pointer's type, a 4-platform size table (`long` and pointer differences), memory views of integers / floats / pointers / structs |
| **Ch. 3 — Pointer core concepts** | Declaration & dereference, pointer size, pointer arithmetic ("`+1` moves how far?"), array decay, multi-level pointers, `const` combinations, function pointers & vtables, strings |
| **Ch. 4 — Structs, alignment, padding** | Alignment rules, a step-by-step `sizeof` derivation, memory diagrams, reordering to save space, `offsetof`, nested offsets, bit-fields and `#pragma pack` |
| **Ch. 5 — Decompiled code taken apart** | The two target lines decomposed into 5 layers, type inference from `_BYTE`/`_WORD`/`_DWORD`/`_QWORD` widths, reconstructing the struct, cross-checking against assembly (`mov` vs `lea`), and a runnable reproduction |
| **Ch. 6 — Self-test** | 20 common points of confusion answered one by one, 5 exercises with full solutions and offset derivations, a study roadmap, and a CN/EN glossary |

**Prerequisites:** basic C syntax (variables, `if`, `for`, functions) — no prior pointer
or memory knowledge assumed.
**Goal:** be able to read struct member access and pointer dereferences in IDA / Ghidra output.

**前置要求：** 会 C 的基本语法（变量、`if`、`for`、函数）即可，不要求学过指针或内存。
**目标：** 能看懂 IDA / Ghidra 反编译结果里的结构体成员访问与指针解引用。

### How to use it / 怎么用

1. Read the Chapter 0 checklist and skip whatever you can already do.
2. Chapters 1–4 are the foundation — **compile and run every example**, don't just read.
3. Chapter 5 is the payoff: it peels the target code open layer by layer.
4. Take Chapter 6's self-test before looking at the answers.

1. 先看第 0 章的前置清单，已掌握的条目直接跳过。
2. 第 1~4 章是地基，**每个例子都要实际编译运行**，不要只读。
3. 第 5 章是全篇的落点，把目标代码逐层剥开。
4. 第 6 章先自己做自测题，再看答案。

All code examples are short, self-contained C files (labelled `exp1.c` … `exp14.c`) and
were written to be compiled and inspected in a debugger.

文档里所有代码示例都是简短、可直接编译的 C 程序（编号 `exp1.c` … `exp14.c`），
建议配合调试器观察内存。

---

## Repository layout / 仓库结构

```
learn_process/
├── README.md
└── 指针与结构体_学习讲义.pdf      # the study guide / 学习讲义本体
```

---

## A note on how the PDF was made / 关于 PDF 的生成方式

The PDF is typeset from source, not exported from Word or a browser. The content is
written as a small Python document model (headings, paragraphs, code blocks, tables,
call-out boxes), which is then laid out and written by **[fpdf2](https://pypi.org/project/fpdf2/)**
with the fonts subset-embedded. The build scripts live in a separate local directory.

这份 PDF 由源码排版生成，不是从 Word 或浏览器导出的。内容写成一个小型 Python 文档模型
（标题、段落、代码块、表格、提示框），再交给 **[fpdf2](https://pypi.org/project/fpdf2/)**
排版输出并做字体子集嵌入。生成脚本存放在本地另一个目录中。

Only the glyphs actually used are embedded, which keeps the file at roughly 510 KB while
staying fully self-contained, searchable, and copy-paste friendly.

只嵌入实际使用到的字形，因此文件约 510 KB，同时完全自包含、文字可搜索可复制。

---

## License / 许可

Personal study notes, shared as-is. Feel free to read and learn from them.
个人学习笔记，按原样分享，欢迎阅读参考。
