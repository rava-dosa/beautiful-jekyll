---
layout: post
title: Thinking in an map-reduce way
subtitle: Trying to learn big data
tags: [big-data, software, distributed-system]
---
### General Architecture
![MapReduce](https://www.networkershome.com/wp-content/uploads/2017/10/nh-big-data-hadoop.jpg){:class="img-responsive"}
### Before We start
It's totally theoritical I have no practical experience in this topic. I want that(practical experience) that's why I am writing this blog &#x1f609; . I am **not** copying content from anywhere.

### Preprocessing
As per however small my perspective is the I think writing mapper and reducer is not the big-deal, What the most most important thing is data preprocessing, that is how you are gonna provide data to the jar file. To preprocess the data you have to think about what your key value will be, and how your data is. You can't expect your data to be clean and formatted, and you have to take care of every edge case there. Just imagine you are doing matrix multiplication but if some of your individual no's is greater than you ram total address space then, you might be in big trouble(I tried copying once 1 hd movie by opening it in sublime and the ctl+a, ctrl+c, it's not pleasent), then you have to precalculate your multiplication step as well put it as key and by using job control, chain two map reduce task. I went a little offroad above,as you give one one line to one mapper so how would you like to provide that one line, what if for matrix multiplication you provide one line such as, **a[1,1] b[1,1] b[2,1]................ a[1,2] b[1,1] b[2,1] ................** now as you know both matrix parameters you can be assured that (we can use bash variable to store local variable as well but there is distributed cache as well) one mapper can write key value pair as **Key<1,1> a[1,1]b[1,1], Key<1,1> a[1,2]b[2,1], ..........................** And in reducer step you can just add all the key value pair to get something like **Key<1,1> sumof(i=>1-n)a[1,i]b[i,1]**. 

### Mapper and reducer
I thought I will write this individually but it got included above. 
### When you should not use map reduce ?
* When you have data whose processing requires use of previous data them map-reduce algorithm might not be the best choice.The memory model for MapReduce is SIMD(Single instruction multiple data.).
* Graph processing
* If synchronization is required in shared data(that is changing common variables).  
Now here spark and apache giraph comes to scene.

### Note
Later I realised that this methodology in some case might not work with data streaming and online algorithms. 
# That's all folks.
