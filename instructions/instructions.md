# Introduction

Welcome to the rootkit lab! Rootkits are a powerful hacking tool that grants attackers persistent access to the kernel of a system. Since kernels have access to every part of the system (not just files, but memory management and more!). 

Essentially, rootkits are pieces of code that are usually designed to give hackers instant root access to a system.In this lab, we are going to be focusing on Linux rootkits that use *kernel modules* to run arbitrary code on the kernel.

This lab was based heavily off of TheXcellerator's two rootkit-centered blog posts. While they do things a little bit differently than us. **Make sure you read through these posts and reference back to them when you get stuck. They are a great source of information and will help you solve many of your problems.**

- [Linux Rootkits Part 1: Introduction and Workflow](https://xcellerator.github.io/posts/linux_rootkits_01/)

- [Linux Rootkits Part 2: Ftrace and Function Hooking](https://xcellerator.github.io/posts/linux_rootkits_02/)

With that out of the way, let's get started!

# Part 1: "Hello-World!" Rootkit

Make sure to read [the Xcellerator's first blog post](https://xcellerator.github.io/posts/linux_rootkits_01/) before beginning this part of the lab.

For part 1, we are keeping it simple. **Your main task is to build a rootkit that logs "Hello World!" to the the kernel logs.**

[The Xcellerator's first blog post](https://xcellerator.github.io/posts/linux_rootkits_01/#what-is-a-kernel-mode-rootkit) will walk you through how to write the kernel module, how to compile it, how to add the kernel module using `insmod`, and how to check the kernel logs using `dmesg`. C

Remember, ***you should only add your kernel modules to your VM's kernel. Doing this on your own machine can seriuosly mess up your system.***

# Part 2: Directory Hiding Rootkit

In [the Xcellerator's first article](https://xcellerator.github.io/posts/linux_rootkits_01/#what-is-a-kernel-mode-rootkit), they state that *function hooking* is an essential part of kernel rootkits.

> "Essentially, we take a function in memory that performs some action we want to influence (listing directory contents, sending a signal to a process, etc) and write our own version."

That is the main objective of part 2. Using the code provided to you in the git repository, **your task is to build a rootkit that will hide directories that contain a certian keyword.**

# Multi Rootkit

