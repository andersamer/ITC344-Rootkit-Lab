# Introduction

Welcome to the rootkit lab! Rootkits are a powerful hacking tool that grants attackers persistent access to the kernel of a system. Since kernels have access to every part of the system (not just files, but memory management and more!). 

Essentially, rootkits are pieces of code that are usually designed to give hackers instant root access to a system.In this lab, we are going to be focusing on Linux rootkits that use *kernel modules* to run arbitrary code on the kernel.

This lab was based heavily off of TheXcellerator's two rootkit-centered blog posts, though they do things a little bit differently than us. **Make sure you read through these posts and reference back to them when you get stuck. They are a great source of information and will help you solve many of your problems.**

- [Linux Rootkits Part 1: Introduction and Workflow](https://xcellerator.github.io/posts/linux_rootkits_01/)

- [Linux Rootkits Part 2: Ftrace and Function Hooking](https://xcellerator.github.io/posts/linux_rootkits_02/)

And one ***IMPORTANT*** warning (especially for Linux users!): ***You should only add your kernel modules to your VM's kernel. Doing this on your own machine can seriuosly mess up your system.***

With that out of the way, let's get started!

# Part 0: Setting up the VM.

Due to kernels being more secure and using different processes now, these rootkits need to be set up in an older enviroment. We will be using Ubuntu 20.04 as our testing enviroment. 

### Downloading the right image

Using your Proxmox enviroment and the knowledge you gained on how to create a new VM, create a new VM using Ubuntu 20.04 as the operating system.

Here is a download link for the .iso

https://releases.ubuntu.com/focal/ubuntu-20.04.6-desktop-amd64.iso

Set up this VM as normal, giving it the recommended amount of RAM and storage. 

### Downloading the right dependencies

Once you have your VM installed, we need to download some packages to our machine so we can write our rootkits. 

Open up a shell and run this command:

```
sudo apt update; sudo apt install git build-essential linux-headers-$(uname -r)
```

This will download the correct packages so that the C code we will be using to make our rootkits can work.

# Part 1: "Hello-World!" Rootkit

Make sure to read [the Xcellerator's first blog post](https://xcellerator.github.io/posts/linux_rootkits_01/) before beginning this part of the lab. Some of the set up will be repeated, so just double check you have everything correct and you're good to go!

You read it? Great! Lets get going!

For part 1, we are keeping it simple. **Your task is to compile a rootkit that logs a message of your choosing to the the kernel logs.**

The Xcellerator's first blog post will walk you through the code of the kernel, how to compile it, how to add the kernel module using `insmod`, and how to check the kernel logs using `dmesg`. Click [here](https://xcellerator.github.io/posts/linux_rootkits_01/#building-kernel-modules) to see the section of the blog post that corresponds with part 1. 

For part 1, you will need to:

1. Copy the code for the "Hello World!" kernel module.

2. Modify the code so that something other than `"Hello World!" is displayed in the kernel logs.

3. Create a `Makefile` to compile the code into a kernel object file (`.ko`).

4. Compile the code.

5. Insert your module to the kernel using `insmod [filename].ko`.

6. Check the syslogs using `dmesg` to verify that your custom message was printed to the logs.

Once you finish this part of the lab, take a screenshot of the `"Hello World!"` log statement to include in your writeup.

# Part 2: Directory Hiding Rootkit

In [the Xcellerator's first article](https://xcellerator.github.io/posts/linux_rootkits_01/#what-is-a-kernel-mode-rootkit), they state that **function hooking** is an essential part of kernel rootkits.

> "Essentially, we take a function in memory that performs some action we want to influence (listing directory contents, sending a signal to a process, etc) and write our own version."

That is the main objective of part 2. Using the code provided to you in the git repository, **your task is to build a rootkit that will hide directories whose named begins with a certian keyword.** The code for this module, its Makefile, and other useful information can all be found in [this Github repository](https://github.com/xcellerator/linux_kernel_hacking/tree/master/3_RootkitTechniques/3.4_hiding_directories).

For part 2, you will need to:

1. Copy the [code](https://github.com/xcellerator/linux_kernel_hacking/blob/master/3_RootkitTechniques/3.4_hiding_directories/rootkit.c) for the directory hiding kernel module.

2. Modify the code to use a key word of your choice. (**Hint:** look for the `PREFIX` macro!)

3. Compile the code using the [Makefile](https://github.com/xcellerator/linux_kernel_hacking/blob/master/3_RootkitTechniques/3.4_hiding_directories/Makefile) provided in the Xcellerator's Github repo.

4. Insert the kernel module using `insmod`.

5. Create some directories whose names match your keyword. Try using `ls` to list them. Do they show up?

Once you get your rootkit working, take screenshots of your demo for your writeup.

# Part 3: Kill Rootkit

Now that you have your Directory Hiding Rootkit working, it's time to move on to something a little more malicious: `kill` commands. If you are unfamiliar with the `kill` command, it's used in conjunction with a number associated with a process to immediately end it. 

For more context, read TheXcellerator's post [here](https://xcellerator.github.io/posts/linux_rootkits_03/).

For this part, you will need to:

1. Copy the [code](https://github.com/xcellerator/linux_kernel_hacking/blob/master/3_RootkitTechniques/3.2_kill_signalling/rootkit.c) for the kill signaling rootkit.

2. Add the `set_root` function; 
```c
void set_root(void)
{
    struct cred *root;
    root = prepare_creds();

    if (root == NULL)
        return;

    root->uid.val = root->gid.val = 0;
    root->euid.val = root->egid.val = 0;
    root->suid.val = root->sgid.val = 0;
    root->fsuid.val = root->fsgid.val = 0;

    commit_creds(root);
}
```

3. Complete the hook and ftrace like you did in Part 2.

4. Compile the code into a makefile.

5. Insert the rootkit using `insmod`.

6. Send signal `64` to any process (i.e. `kill -64 1`).

7. Use the command `id` to see the result of your hard work.

Congratulations! You are root! 

# Deliverables
- Screenshot of "Hello-world" message
- Screenshots of directories before and after hiding
- Screenshot of `id` command to show you are root
