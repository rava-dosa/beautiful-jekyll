---
layout: post
title: Dialogues on linux kernel-2
subtitle: Adding a new Syscall
tags: [tutorial, software, linux, kernel, os]
---
### Architecture of Linux kernel
![Linux-Architecture](https://i.pinimg.com/originals/a4/76/e5/a476e5ac785fa192712b24316bfaf3c3.gif){:class="img-responsive"}
### Before We start
This series is neither totally theoritical nor totally hands-on. It is amalgammation of both. Just the way I think. You will not become expert which should be implied.  
### What is Syscall ?
System call is the programmatic way in which a computer program requests a service from the kernel of the operating system it is executed on. This may include hardware-related services (for example, accessing a hard disk drive), creation and execution of new processes, and communication with integral kernel services such as process scheduling. [Ref](https://en.wikipedia.org/wiki/System_call)
### Some Syscall
Straight from **the Source**.

{% highlight c %}
#ifndef CONFIG_ARCH_HAS_SYSCALL_WRAPPER
// File name -> /home/user/linux-4.18.1/include/linux/syscalls.h
asmlinkage long sys_io_setup(unsigned nr_reqs, aio_context_t __user *ctx);
asmlinkage long sys_io_destroy(aio_context_t ctx);
asmlinkage long sys_io_submit(aio_context_t, long,
			struct iocb __user * __user *);
asmlinkage long sys_io_cancel(aio_context_t ctx_id, struct iocb __user *iocb,
			      struct io_event __user *result);
asmlinkage long sys_io_getevents(aio_context_t ctx_id,
				long min_nr,
				long nr,
				struct io_event __user *events,
				struct timespec __user *timeout);
asmlinkage long sys_io_pgetevents(aio_context_t ctx_id,
				long min_nr,
				long nr,
				struct io_event __user *events,
				struct timespec __user *timeout,
				const struct __aio_sigset *sig);

/* fs/xattr.c */
asmlinkage long sys_setxattr(const char __user *path, const char __user *name,
			     const void __user *value, size_t size, int flags);
asmlinkage long sys_lsetxattr(const char __user *path, const char __user *name,
			      const void __user *value, size_t size, int flags);
asmlinkage long sys_fsetxattr(int fd, const char __user *name,
			      const void __user *value, size_t size, int flags);
asmlinkage long sys_getxattr(const char __user *path, const char __user *name,
			     void __user *value, size_t size);
asmlinkage long sys_lgetxattr(const char __user *path, const char __user *name,
			      void __user *value, size_t size);
asmlinkage long sys_fgetxattr(int fd, const char __user *name,
			      void __user *value, size_t size);
asmlinkage long sys_listxattr(const char __user *path, char __user *list,
			      size_t size);
asmlinkage long sys_llistxattr(const char __user *path, char __user *list,
			       size_t size);
asmlinkage long sys_flistxattr(int fd, char __user *list, size_t size);
asmlinkage long sys_removexattr(const char __user *path,
				const char __user *name);
asmlinkage long sys_lremovexattr(const char __user *path,
				 const char __user *name);
asmlinkage long sys_fremovexattr(int fd, const char __user *name);

/* fs/dcache.c */
asmlinkage long sys_getcwd(char __user *buf, unsigned long size);

/* fs/cookies.c */
asmlinkage long sys_lookup_dcookie(u64 cookie64, char __user *buf, size_t len);

/* fs/eventfd.c */
asmlinkage long sys_eventfd2(unsigned int count, int flags);

/* fs/eventpoll.c */
asmlinkage long sys_epoll_create1(int flags);
asmlinkage long sys_epoll_ctl(int epfd, int op, int fd,
				struct epoll_event __user *event);
asmlinkage long sys_epoll_pwait(int epfd, struct epoll_event __user *events,
				int maxevents, int timeout,
				const sigset_t __user *sigmask,
				size_t sigsetsize);

/* fs/fcntl.c */
asmlinkage long sys_dup(unsigned int fildes);
{% endhighlight %}

### So is adding a new one tough ?
No not so much, but the pain of compiling will definitely kill you(took me 3 hrs to compile a single syscall change in kernel). 

### Writing a new syscall 
First download kernel source code.

```
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.18.1.tar.xz
sudo -s
tar -xvf linux-4.18.1.tar.xz -C /usr/src/
cd /usr/src/linux-4.18.1
mkdir helloworld
cd helloworld
nano helloworld.c

```
In helloworld.c

{% highlight c %}
#include <linux/kernel.h>
 
asmlinkage long sys_helloworld(void)
{
    printk("Hello world\n");
    return 0;
}
{% endhighlight %}

Now make a makefile

```
nano makefile.c
# add this
obj-y := helloworld.o
```
After saving this makefile
```
cd ..
```

Now open makefile and go to line no 947 and add

```
 helloworld/ 
```

Now open file syscall.h by 

```
nano include/linux/syscalls.h
```

Now go to line 908 and add these lines

```
asmlinkage long sys_helloworld(void);
```
open syscall_64.tbl 

```
nano /arch/x86/entry/syscalls/syscall_64.tbl

```

Now go to line 346 and you will feel like adding 

```
335	64	helloworld			__x64_sys_helloworld
```
I added this line compiled and got error. 

```
335	64	helloworld			sys_helloworld
```

By adding this line it compiled successfully. I might be wrong you can try that line as well if it works then I would really like to know but it didn't work last time. And I might be wrong.

```
cp /boot/config-$(uname -r) .config
make menuconfig
# exit immediately after a semi colorful menu opens.
make
make modules_install install
```

Yup! you did it yourself. Restart your computer and Now you have added a syscall. Now if you want to check if syscall works or not then make some file name try.c

```
#include <stdio.h>
#include <linux/kernel.h>
#include <sys/syscall.h>
#include <unistd.h>
int main(){
long int s = syscall(335);
printf("Syscall returned %ld \n",s);
return 0;
}
```

Now compile this programm with gcc try.c and then run it by ./a.out. if it prints syscall returned 0, then your syscall worked else not. 

```
dmesg
# kernel log, you will see hellowold printed in last
```

That's it folks.
