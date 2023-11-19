# Getting familiar with the C compiler

I'm really excited today because I recently acquired the STM32 Nucleo development board! I'm currently working my way through _Bare Metal C_ by Steve Oualline where I'm diving deeper into the C compiler.

Compilation is an incredibly complex branch of computer science. 

# Introduction to compilation

High-level programming languages allow humans to give instructions to computers that resemble human languages. Consider the following example of statements and output written in Python.

>>> message = "Hello, World!"

>>> print(message)

Hello, World!

>>>

 You don't need to be a computer scientist to understand what's going on here. This block of code creates and prints a simple message to the screen. 

#include <stdio.h>

int main()

{

    printf("Hello, World!\n")