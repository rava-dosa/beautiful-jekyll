---
layout: post
title: Dialogues on linux kernel-1
subtitle: Linux starter pack
tags: [tutorial, software, linux, kernel, os]
---
### Architecture of Linux kernel
![Linux-Architecture](https://i.pinimg.com/originals/a4/76/e5/a476e5ac785fa192712b24316bfaf3c3.gif){:class="img-responsive"}
### Before We start
This series is neither totally theoritical nor totally hands-on. It is amalgammation of both. Just the way I think. You will not become expert which should be implied.  
### Adding a new module in linux kernel
So linux kernel is a monolithic kernel. To be accurate it is Modular monolithic. Monolithic term in kernal is used in the sense of permission of code running there. If the whole kernel is running in ** ring 0 ** then the kernel will be called monoltihic. But if very few code is running in ring 0 then the kernel is called micro kernel architecture.  
So if a module crashes the whole kernel will crash in linux. So always spawn a VM if you are trying to add garbage in linux.
#### Hello World
##### file-name -> hello-1.c
```
#include <linux/module.h>
#include <linux/kernel.h>    

int init_module(void)
{
    printk(KERN_INFO "Hello world 1.\n");
    return 0;
}

void cleanup_module(void)
{
    printk(KERN_INFO "Goodbye world 1.\n");
}  

```
To compile this code you can use [makefile](https://github.com/rava-dosa/linux/tree/master/Linux-Hello%20world) provided in this repo with proper naming as it is in the repository.  
If you have done everything right till date then try
```
insmod ./hello-1.ko
```
Now you can check hello world being printed in /var/log/kern.log.  
To check do cat /var/log/kern.log.  
That's all folks.


