---
title: 'Non-msvc Rust Install on Windows 11'
summary: This blog will record my study through the book called The Elements of Computing Systems whose main contents are HDL, Assembler, VM and OS.
tags:
  - CS
date: 2024-06-02
# external_link: https://qianzehao123.github.io/posts/2023/07/From-Nand-To-Tetris/
---

When setting up the Rust programming environment on Windows, I opted to use the GNU GCC compiler to reduce disk usage instead of Microsoft's MSVC. However, one thing that has puzzled me is that many people use MSYS2 to install GCC. In fact, Rust's installer can directly install the GNU toolchain. Here is my installation method:

# Install Rust Package

![](./1.png)

![](./2.png)

# Choose Manually install the prerequisites

![](./3.png)

![](./4.png)

![](./5.png)

# Key Step: Choose the Toolchain

```
x86_64-pc-windows-gnu
```

![](./6.png)

![](./7.png)