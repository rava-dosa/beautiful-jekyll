---
layout: post
title: Why my golang code panicked and gave segfault ?
subtitle: 
tags: [golang]
---

### Content
1. Why it happened ?
2. How to fix this ?
3. Where I have been for the last four months ?

### Why it happened ?
So in peter-pan's dreamland my highly concurrent http server worked just fine. Peter pan deployed the golang binary on the server and it never gave any problem, but Ubuntu-16.04 is far from peter pan's dreamland. Me and my friend wrote a golang code, which did a single job of uploading an image to aws s3. While we were benchmarking the code using apache benchmark, on 300 concurrent connection the code ran just fine. But as we increased the no of connection to 400, our code started giving segfault. We checked the main memory for out of memory error. But even that wasn't the problem. Tinker bell came and asked us to google the error. But search results were of no use. I wished the fairy for a solution. She whispered what if there is a limit on total no of open socket. Being a fairy she was really close to correct. There was a limit on total no of open files. So we thought okay let's increase the limit. And it worked magically. 

### How to fix this issue ?
So there are these limits. 

```
    1. find open files limit per process: ulimit -n
    2. find hard limit on no of open files: ulimit -Hn
    3. count all opened files by all processes: lsof | wc -l
    4. get maximum allowed number of open files: cat /proc/sys/fs/file-max  
```
Open this file, /etc/security/limits.conf, add these two lines
```
* soft nofile 65535
* hard nofile 65535
```
Now open these files, /etc/systemd/user.conf and  /etc/systemd/system.conf , add this line

```
DefaultLimitNOFILE=65535
```
Now do a sudo reboot. Now you have reached peter pan's dreamland. 

### Where I have been for the last four months ?
So the sixth semester at kgp cse is known to take a toll on your daily life. So there was a lot of coding assignments. And after doing all those and attending classes the eagerness to read and write a blog just vanished. After the end of semester I went to internship at barclays, Pune. That was a great experience. And Let's just say my stay at pune turned out to be quite eventful.  
### That's all folks.