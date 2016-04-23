---
id: 632
title: Why is Keras Running So Slow?
date: 2015-12-05T16:31:15+00:00
author: lo
layout: post
guid: /?p=632
permalink: /why-is-keras-running-so-slow/
nkweb_code_in_head:
  - default
nkweb_Use_Custom_js:
  - default
nkweb_Custom_js:
  - 
nkweb_Use_Custom_Values:
  - default
nkweb_Custom_Values:
  - 
nkweb_Use_Custom:
  - 'false'
nkweb_Custom_Code:
  - 
dsq_thread_id:
  - 4752582828
categories:
  - In Practice
  - Machine Learning
tags:
  - Advice
  - I Hate Linux
  - Keras
  - Linux
  - Theano
  - Tutorial
---
[<img class="aligncenter size-full wp-image-640" src="/wp-content/uploads/2015/12/meme.jpg" alt="meme" width="512" height="340" />](/wp-content/uploads/2015/12/meme.jpg)

More notes for myself&#8230; so it may not be helpful for you who bumped into here. 😉

# Why This Article?

<a href="/how-to-setup-theano-to-run-on-gpu-on-ubuntu-14-04-with-nvidia-geforce-gtx-780/" target="_blank">Setting Theano correctly</a> **is not enough** to ensure you can run deep learning software correctly. In our case, it will be Keras, and it can slow to a crawl if not setup properly.

Again, there could be many causes but I try to outline a clean step what I did, the performance I run a good setup, so you can compare. Hopefully you can glean some places where you did wrong.

# Specifications

My server has the following specifications finished running the steps outlined <a href="/how-to-setup-theano-to-run-on-gpu-on-ubuntu-14-04-with-nvidia-geforce-gtx-780/" target="_blank">here</a>.

  * OS: Ubuntu 14.04 LTS, X64
  * GPU: Nvidia Geforce GTX 780
  * Ubuntu 14.04 LTS
  * CUDA 7.5
  * Theano 0.7.0
  * Numpy 1.8.2
  * Kera 0.2.0
  * Scipy 0.13.3
  * NVIDIA-SMI 352.39
  * Graphics Driver Version: 352.39

# Instructions

1. Make sure your Theano configuration file, located at ~/.theanorc, is correct:

<pre>$ cat ~/.theanorc
[global]
floatX = float32
device = gpu
optimizer = fast_run

[lib]
cnmem = 0.8

[nvcc]
fastmath = True

[blas]
ldflags = -llapack -lblas</pre>

Use this to see your Theano settings in runtime and make sure it matches what you have above. Only some output is shown since it is very long:

<pre>$ python -c <span class="s1">'import theano; print theano.config'
</span>
Using gpu device 0: GeForce GTX 780 (CNMeM is enabled)
floatX (('float64', 'float32', 'float16')) 
 Doc: Default floating-point precision for python casts.

Note: float16 support is experimental, use at your own risk.
 Value: float32

warn_float64 (('ignore', 'warn', 'raise', 'pdb')) 
 Doc: Do an action when a tensor variable with float64 dtype is created. They can't be run on the GPU with the current(old) gpu back-end and are slow with gamer GPUs.
 Value: ignore

...
...
...

</pre>

2. Install Theano bleeding edge (0.7.0), since the Keras examples needs &#8216;relu&#8217;.

<pre>pip install --upgrade --no-deps git+git://github.com/Theano/Theano.git</pre>

Detailed instructions [here](http://deeplearning.net/software/theano/install.html#bleeding-edge-install-instructions).

3. Get Keras source and run the mnist_mlp.py to check the performance.

<pre>$ git clone https://github.com/fchollet/keras.git
Cloning into 'keras'...
remote: Counting objects: 6572, done.
remote: Total 6572 (delta 0), reused 0 (delta 0), pack-reused 6572
Receiving objects: 100% (6572/6572), 1.29 MiB | 894.00 KiB/s, done.
Resolving deltas: 100% (4571/4571), done.
Checking connectivity... done.</pre>

<pre>$ cd keras/examples
$ $ time python mnist_mlp.py 
Using Theano backend.
Using gpu device 0: GeForce GTX 780 (CNMeM is enabled)
60000 train samples
10000 test samples
Train on 60000 samples, validate on 10000 samples
Epoch 1/20
0s - loss: 0.2805 - acc: 0.9148 - val_loss: 0.1165 - val_acc: 0.9636
Epoch 2/20
0s - loss: 0.1151 - acc: 0.9651 - val_loss: 0.0960 - val_acc: 0.9685
Epoch 3/20
0s - loss: 0.0800 - acc: 0.9754 - val_loss: 0.0670 - val_acc: 0.9787
Epoch 4/20
0s - loss: 0.0624 - acc: 0.9806 - val_loss: 0.0703 - val_acc: 0.9775
Epoch 5/20
0s - loss: 0.0506 - acc: 0.9837 - val_loss: 0.0622 - val_acc: 0.9795
Epoch 6/20
0s - loss: 0.0414 - acc: 0.9867 - val_loss: 0.0641 - val_acc: 0.9803
Epoch 7/20
0s - loss: 0.0347 - acc: 0.9892 - val_loss: 0.0665 - val_acc: 0.9802
Epoch 8/20
0s - loss: 0.0295 - acc: 0.9906 - val_loss: 0.0769 - val_acc: 0.9789
Epoch 9/20
0s - loss: 0.0258 - acc: 0.9915 - val_loss: 0.0586 - val_acc: 0.9830
Epoch 10/20
0s - loss: 0.0215 - acc: 0.9928 - val_loss: 0.0577 - val_acc: 0.9841
Epoch 11/20
1s - loss: 0.0197 - acc: 0.9932 - val_loss: 0.0605 - val_acc: 0.9844
Epoch 12/20
0s - loss: 0.0180 - acc: 0.9940 - val_loss: 0.0560 - val_acc: 0.9863
Epoch 13/20
0s - loss: 0.0163 - acc: 0.9945 - val_loss: 0.0630 - val_acc: 0.9838
Epoch 14/20
0s - loss: 0.0136 - acc: 0.9956 - val_loss: 0.0608 - val_acc: 0.9857
Epoch 15/20
0s - loss: 0.0130 - acc: 0.9958 - val_loss: 0.0616 - val_acc: 0.9838
Epoch 16/20
0s - loss: 0.0114 - acc: 0.9960 - val_loss: 0.0584 - val_acc: 0.9854
Epoch 17/20
0s - loss: 0.0098 - acc: 0.9967 - val_loss: 0.0672 - val_acc: 0.9849
Epoch 18/20
0s - loss: 0.0106 - acc: 0.9964 - val_loss: 0.0678 - val_acc: 0.9846
Epoch 19/20
0s - loss: 0.0082 - acc: 0.9974 - val_loss: 0.0749 - val_acc: 0.9835
Epoch 20/20
0s - loss: 0.0085 - acc: 0.9971 - val_loss: 0.0685 - val_acc: 0.9843
Test score: 0.0685058810504
Test accuracy: 0.9843

<span style="color: #00ff00;"><strong>real 0m24.560s</strong></span>
user 0m23.216s
sys 0m1.328s


</pre>

As you can see, the whole run takes only 25 seconds, and it may take even less or maybe 2 minutes for you. Anything longer than that looks strange and you should inspect.

4. Install <a href="https://developer.nvidia.com/cuDNN" target="_blank">CuDNN</a> if you are using ConvNet. The basic implementations of convolution in Theano are significantly slower.

Downloading CuDNN is problematic, because you have to register an account on Nvidia and wait for hours or days for manual approval. Someone uploaded a version of CuDNN 6.5 for download on Google Drive <a href="http://deeplearning.net/software/theano/library/sandbox/cuda/dnn.html" target="_blank">here</a> if you don&#8217;t want to wait.

Once you have it, just unzip the tgz file.

<pre>$ tar zxvf cudnn-6.5-linux-x64-v2.tgz
$ cd cudnn-6.5-linux-x64-v2
$ ls
cudnn.h CUDNN_License.pdf INSTALL.txt libcudnn.so libcudnn.so.6.5 libcudnn.so.6.5.48 libcudnn_static.a
$ pwd
/home/echio/src/cudnn-6.5-linux-x64-v2</pre>

5. Make sure your CUDA and CuDNN are both accessible to Theano.

To check if your Theano is using CuDNN. Run this Python code below:

    # Run this python code
    from theano.sandbox.cuda.dnn import *
    print(dnn_available())
    print(dnn_available.msg)

I also captured the environment variables, replace echio with your username:

<pre>$ echo $CPATH

$ echo $LD_LIBRARY_PATH
/usr/local/cuda-7.5/lib64:
$ echo $LIBRARY_PATH
/usr/local/cuda-7.5/lib64:
$ python
Python 2.7.6 (default, Jun 22 2015, 17:58:13) 
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
&gt;&gt;&gt; from theano.sandbox.cuda.dnn import *
Using gpu device 0: GeForce GTX 780 (CNMeM is enabled)
&gt;&gt;&gt; print(dnn_available())
False
&gt;&gt;&gt; print(dnn_available.msg)
<strong>Theano can not compile with cuDNN. We got this error:</strong>
<strong>/tmp/try_flags_sbkMKM.c:5:19: fatal error: cudnn.h: No such file or directory</strong>
 #include &lt;cudnn.h&gt;
 ^
compilation terminated.</pre>

<a href="https://goo.gl/bTW22Q" target="_blank">Googling the error message</a> doesn&#8217;t help too much.

You need to add the location of the 3. into CPATH, LD\_LIBRARY\_PATH and LIBRARY_PATH. This is what my .bashrc looks like this (replace echio with your username):

<pre># ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-7.5/lib64:/home/echio/src/cudnn-6.5-linux-x64-v2:
export LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-7.5/lib64:/home/echio/src/cudnn-6.5-linux-x64-v2:
export CPATH=$CPATH:/home/echio/src/cudnn-6.5-linux-x64-v2:
export PATH=$PATH:/usr/local/cuda-7.5/bin</pre>

If you instead see this error message:

<pre>ERROR (theano.sandbox.cuda): Failed to compile cuda_ndarray.cu: libcublas.so.7.5: cannot open shared object file: No such file or directory</pre>

You probably didn&#8217;t have CUDA environment variables setup properly. See the above ~/.bashrc lines for correct setup.

6. Run this code again.

    # Run this python code
    from theano.sandbox.cuda.dnn import *
    print(dnn_available())
    print(dnn_available.msg)

You should see below when executed in a Python REPL.

<pre>&gt;&gt;&gt; from theano.sandbox.cuda.dnn import *
Using gpu device 0: GeForce GTX 780 (CNMeM is enabled)
&gt;&gt;&gt; print(dnn_available())
True
&gt;&gt;&gt; print(dnn_available.msg)
None</pre>

This is good! Re-run your Keras code and hopefully it will be fast this time&#8230;

# Conclusion

This may or may not solve your problem, but it certainly solved some of my problems. You will probably have to learn to debug things a bit to figure out how to get it to run well.

#### References

  * <a href="http://deeplearning.net/software/theano/library/config.html" target="_blank">http://deeplearning.net/software/theano/library/config.html</a>
  * <a href="http://deeplearning.net/software/theano/library/sandbox/cuda/dnn.html" target="_blank">http://deeplearning.net/software/theano/library/sandbox/cuda/dnn.html</a>
  * <a href="https://groups.google.com/forum/#!topic/keras-users/EAbpVHJBvGQ" target="_blank">https://groups.google.com/forum/#!topic/keras-users/EAbpVHJBvGQ</a>

&nbsp;