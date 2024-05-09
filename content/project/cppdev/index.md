---
title: "Msys2 for Windows C++ Devlopment"
summary: Msys2 and VScode configuration for Cpp Dev
tags:
  - CS
  - Software
date: 2024-05-09
---

MSYS2 is a software distribution and a building platform for Windows, based on a fork of Cygwin (a Unix-like environment for Windows) that aims to provide better native functionality. It's designed to facilitate the creation and management of Windows software with a collection of tools and libraries from the GNU and Unix worlds.

Here are some key aspects of MSYS2:

1. **Package Management**: MSYS2 uses Pacman, the package manager from Arch Linux, to handle package installations, updates, and removals. This system allows users to easily manage packages and maintain their development environment.

2. **Three Subsystems**: MSYS2 offers three distinct subsystems:
   - **MSYS**: This subsystem provides a POSIX shell and utilities, primarily for building software.
   - **MINGW-w64**: These environments (one for 32-bit and one for 64-bit) are designed for native Windows software development. They provide more Windows-friendly versions of GCC and other tools.
   - **UCRT64**: A relatively new environment that targets the Universal C Runtime in Windows.

3. **Cross-Platform Functionality**: While maintaining compatibility with Windows, MSYS2 allows developers to use Unix-like tools and build systems which makes it easier to port applications from Linux to Windows.

4. **Development Environment**: Developers often use MSYS2 as an environment to compile, build, and test their applications directly on Windows, especially when working with open-source software or when porting applications from Unix/Linux systems.

Overall, MSYS2 is a versatile tool that enhances the productivity of developers on Windows by integrating Unix-like toolchains and workflows into a Windows setting.

## Download Msys2
[https://www.msys2.org/](https://www.msys2.org/)

## Run Mingw64 Environment and Install MinGW-w64

1. Update all the installed software

    ```shell
    pacman -Syu
    ```
2. Install MinGW-x86_64-toolchain and build-essential
    ```shell
    pacman -S --needed base-devel mingw-w64-x86_64-toolchain
    ```
3. Install CMake
    ```shell
    pacman -S mingw-w64-x86_64-cmake
    ```

## Scripts for Testing CMake and GCC

* main.cpp file

    ```c
    #include <stdio.h>
    #include <stdlib.h>

    /**
    * power - Calculate the power of number.
    * @param base: Base value.
    * @param exponent: Exponent value.
    *
    * @return base raised to the power exponent.
    */
    double power(double base, int exponent)
    {
        int result = base;
        int i;

        if (exponent == 0)
        {
            return 1;
        }

        for (i = 1; i < exponent; ++i)
        {
            result = result * base;
        }

        return result;
    }

    int main(int argc, char *argv[])
    {
        if (argc < 3)
        {
            printf("Usage: %s base exponent \n", argv[0]);
            return 1;
        }
        double base = atof(argv[1]);
        int exponent = atoi(argv[2]);
        double result = power(base, exponent);
        printf("%g ^ %d is %g\n", base, exponent, result);
        return 0;
    }

    ```

* CMakeLists.txt
    ```CMake
    cmake_minimum_required (VERSION 2.8)
    # Project Info
    project (Demo1)
    # add generate target
    add_executable(Demo main.cpp)
    ```

## Build and Run the test files
```shell
# create the target binary file directory
mkdir build
cd build
# generate makefile
cmake ..
# build the project
cmake --build .
# Run the Demo.exe file
. Demo.exe 5 4
# the output is 625
```