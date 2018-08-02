---
layout: post
title: How to install tensorflow-gpu without shitting on your computer ?
subtitle: Sorry for the word "Sh******"
tags: [tutorial, software, installation]
---
### Once upon a time
There was a operating system called Ubuntu 14.04, I tried installing tensorflow gpu on that.
And I was stuck in login loop as Tom cruise was stuck in time loop in the movie "The Edge of tomorrow". I couldn't get out of login loop until I installed Ubuntu 16.04
### Want to know how was my experience with ubuntu 16.04
In a very few[**sarcastic laugh**] steps I was able to install it.  
1. sudo apt-get --purge remove nvidia-*
2. sudo apt-get autoremove
3. sudo apt-get update
4. sudo gedit /etc/modprobe.d/blacklist-nouveau.conf  
**paste** 
``` 
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
5. `echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf`
6. sudo update-initramfs -u
7. sudo apt-get update
8. sudo apt-get upgrade -y
9. sudo apt-get dist-upgrade -y
10. sudo apt-get install build-essential
11. sudo apt-get install linux-image-extra-virtual
12. sudo apt-get install linux-source
13. sudo apt-get source linux-image-$(uname -r)
14. sudo apt-get install linux-headers-$(uname -r)
15. wget -O cuda_8_linux.run https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_375.26_linux-run
16. sudo chmod +x cuda_8_linux.run
```
#in tty ctrl+alt+f1 or ctrl + alt +f6
sudo service lightdm stop 
#or if you are using gnome 3 then
sudo service gdm3 stop
sudo ./cuda_8_linux.run
```
17. sudo service gdm3 stop
18. sudo ./cuda_8_linux.run
```
#Do you accept the previously read EULA?
#accept
#Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 367.48?
#n (we installed drivers previously)
#Install the CUDA 8.0 Toolkit?
#y
#Enter Toolkit Location:
# /usr/local/cuda-8.0 (enter)
# Do you wish to run the installation with ‚sudo’?
# y
# Do you want to install a symbolic link at /usr/local/cuda?
# y 
# Install the CUDA 8.0 Samples?
# y 
# Enter CUDA Samples Location:
# enter 
# Install cuDNN
# go to website and download cudnn-8.1 https://developer.nvidia.com/cudnn
```
19. tar -zxvf cudnn-8.1-linux-x64-v5.1.tgz 

20. copy libs to /usr/local/cuda folder
21. sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
22. sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
23. sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
24. sudo add-apt-repository ppa:graphics-drivers/ppa
25. sudo apt-get update
26. sudo apt-get install nvidia-375
27. sudo apt-get install libcupti-dev
###### Once nvidia driver is installed, restart the computer. You can verify the driver using the following command.
28. cat /proc/driver/nvidia/version
29. echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
30. sudo apt-get install python-numpy python-dev python-pip python-wheel
######Download Bazel
31. chmod +x bazel-0.5.2-installer-linux-x86_64.sh
33. ./bazel-0.5.2-installer-linux-x86_64.sh --user
######paste in bashrc
34. export PATH="$PATH:$HOME/bin"
###### https://www.anaconda.com/download/ install anaconda
###### Add these two lines in gedit ~/.bashrc
35. export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64"
36. export CUDA_HOME=/usr/local/cuda
37. conda create -n tensorflow
38. source activate tensorflow
39. pip install --ignore-installed --upgrade \ https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.0.0-cp27-none-linux_x86_64.whl

### But as time has passed by..............
Things have changed a lot.Now on new ubuntu 18.04 you don't need to do all this, Things have become relatively easy:  
(**Even though I installed conda packages first**)  
1. sudo apt-get --purge remove nvidia-*
2. sudo apt-get autoremove
3. sudo add-apt-repository ppa:graphics-drivers/ppa
4. sudo apt-get update
5. sudo apt-get install nvidia-375
6. sudo reboot
7. wget https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh
7.1. bash Anaconda3-5.2.0-Linux-x86_64.sh  
**While installation,when asked about adding path,say yes.**  
8. source ~/.bashrc
9. conda update conda
10. conda update anaconda
11. conda update python
12. conda update --all
13. conda create --name tf-gpu
14. source activate tf-gpu
15. conda install tensorflow-gpu  
`I tried this and it worked. So now if you want to check if gpu device is recognized or not:`
16. python  
16.1. import tensorflow as tf
```
with tf.device('/gpu:0'):
    a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a')
    b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b')  
    c = tf.matmul(a, b)  
with tf.Session() as sess:  
    print (sess.run(c))
```
17. That's it you are done with installation.
18. [Ref](https://gist.github.com/rava-dosa/75a04514ad6864b1eb0eee6c9821143a)


