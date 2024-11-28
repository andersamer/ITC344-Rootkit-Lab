# Introduction

Welcome to the rootkit lab! Rootkits are a powerful hacking tool that grants attackers persistent access to the kernel of a system. Since kernels have access to every part of the system (not just files, but memory management and more!). 

Essentially, rootkits are pieces of code that are usually designed to give hackers instant root access to a system.In this lab, we are going to be focusing on Linux rootkits that use *kernel modules* to run arbitrary code on the kernel.

This lab was based heavily off of TheXcellerator's two rootkit-centered blog posts, though they do things a little bit differently than us. **Make sure you read through these posts and reference back to them when you get stuck. They are a great source of information and will help you solve many of your problems.**

- [Linux Rootkits Part 1: Introduction and Workflow](https://xcellerator.github.io/posts/linux_rootkits_01/)

- [Linux Rootkits Part 2: Ftrace and Function Hooking](https://xcellerator.github.io/posts/linux_rootkits_02/)

And one ***IMPORTANT*** warning (especially for Linux users!): ***You should only add your kernel modules to your VM's kernel. Doing this on your own machine can seriuosly mess up your system.***

With that out of the way, let's get started!

# Part 1: "Hello-World!" Rootkit

Make sure to read [the Xcellerator's first blog post](https://xcellerator.github.io/posts/linux_rootkits_01/) before beginning this part of the lab.

For part 1, we are keeping it simple. **Your task is to compile a rootkit that logs a message of your choosing to the the kernel logs.**

The Xcellerator's first blog post will walk you through the code of the kernel, how to compile it, how to add the kernel module using `insmod`, and how to check the kernel logs using `dmesg`. Click [here](https://xcellerator.github.io/posts/linux_rootkits_01/#building-kernel-modules) to see the section of the blog post that corresponds with part 1. 

For part 1, you will need to:

1. Copy the code for the "Hello World!" kernel module.

2. Modify the code so that something other than `"Hello World!" is displayed in the kernel logs.

3. Create a `Makefile` to compile the code into a kernel object file (`.ko`).

4. Compile the code.

5. Insert your module to the kernel using `insmod [filename].ko`.

6. Check the syslogs using `dmesg` to verify that your custom message was printed to the logs.

6. ???

7. Profit.

Once you finish this part of the lab, take a screenshot of the `"Hello World!"` log statement to include in your writeup.

# Part 2: Directory Hiding Rootkit

In [the Xcellerator's first article](https://xcellerator.github.io/posts/linux_rootkits_01/#what-is-a-kernel-mode-rootkit), they state that **function hooking** is an essential part of kernel rootkits.

> "Essentially, we take a function in memory that performs some action we want to influence (listing directory contents, sending a signal to a process, etc) and write our own version."

That is the main objective of part 2. Using the code provided to you in the git repository, **your task is to build a rootkit that will hide directories whose named begins with a certian keyword.** The code for this module, its Makefile, and other useful information can all be found in [this Github repository](https://github.com/xcellerator/linux_kernel_hacking/tree/master/3_RootkitTechniques/3.4_hiding_directories).

For part 2, you will need to:

1. Copy the [code](https://github.com/xcellerator/linux_kernel_hacking/blob/master/3_RootkitTechniques/3.4_hiding_directories/rootkit.c) for the directory hiding kernel module.

2. Modify the code to use a key word of your choice. (**Hint:** look for the `PREFIX` macro!)

3. Compile the code using the [Makefile](https://github.com/xcellerator/linux_kernel_hacking/blob/master/3_RootkitTechniques/3.4_hiding_directories/Makefile) provided in the Xcellerator's Github repo.

4.Insert the kernel module using `insmod`.

5. Create some directories whose names match your keyword. Try using `ls` to list them. Do they show up?

Once you get your rootkit working, take screenshots of your demo for your writeup.

# Multi Rootkit

