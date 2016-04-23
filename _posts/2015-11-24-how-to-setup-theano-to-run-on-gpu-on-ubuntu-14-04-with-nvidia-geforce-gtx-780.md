---
id: 618
title: How to Setup Theano to Run on GPU on Ubuntu 14.04 with Nvidia Geforce GTX 780
date: 2015-11-24T13:32:19+00:00
author: lo
layout: post
guid: /?p=618
permalink: /how-to-setup-theano-to-run-on-gpu-on-ubuntu-14-04-with-nvidia-geforce-gtx-780/
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
categories:
  - In Practice
tags:
  - Advice
  - GPU
  - I Hate Linux
  - Linux
  - Nvidia
  - Tutorial
  - Ubuntu
  - Ubuntu 14.04
---
Documenting the steps how to setup Theano to run on GPU on Ubuntu 14.04 so I can refer to this in the future. With this successfully installed, you can run Keras, convnet, Theano, etc properly.

# Why This Article?

Have you ever met those painful issues just trying to setup your GPU for scientific programming:

  * Getting a blinking cursor after installing the &#8220;latest&#8221; Nvidia drivers?
  * Getting a blinking cursor after installing the &#8220;latest&#8221; CUDA toolkit?
  * Login loop in Ubuntu login screen?
  * NVRM: rm\_init\_adapter(0) failed?
  * NVIDIA: could not open the device file /dev/nvidia0 (input/output error)
  * &#8230;

If yes, then read on.

<span style="color: #ff0000;"><strong>I cannot guarantee your OS does not break after running the steps below, so please backup everything, run as your own risk.</strong></span>

You may want to read the _Things That Does Not Work For Me_ part below first before following the _instructions_.

# Specifications

My server has the following specifications:

  * OS: Ubuntu 14.04 LTS, X64
  * GPU: Nvidia Geforce GTX 780
  * Fresh install of Ubuntu 14.04 LTS

# Instructions

If you fail in the middle of any of this steps, you are advised to restart and follow the steps carefully again. Use common sense. and if you don&#8217;t know how to fix it, too bad,

1. Download latest (or very recent) Nvidia display driver for Linux:

<pre>wget http://us.download.nvidia.com/XFree86/Linux-x86_64/352.63/NVIDIA-Linux-x86_64-352.63.run</pre>

2. Download latest (or very recent) CUDA Installation Run file for Ubuntu 14.04:

<pre>wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run</pre>

3. Get into recovery mode to install the files, because you cannot install your display driver with X server on.

<pre>reboot</pre>

4. Once you are in GRUB, select &#8220;Advanced Options for Ubuntu&#8221;.

5. In the recovery mode, select &#8220;network&#8221;, press yes. Once the networking has been enabled, this will also set your drive to read/write mode.

6. In the recovery mode, select &#8220;root&#8221; to get a root prompt.

7. Go to where you downloaded the previous files at, and **run NVIDIA-Linux-x86_64-352.63.run **.

<pre>chmod +x NVIDIA-Linux-x86_64-352.63.run # Make it executable

./NVIDIA-Linux-x86_64-352.63.run # Install Nvidia display driver</pre>

For yes/no questions, take &#8220;yes&#8221;. For locations, press &#8220;enter&#8221; to take default values.

8. Setup your environment variables.

Make sure your LD\_LIBRARY\_PATH, LIBRARY_PATH and PATH includes the places it needs to include:

<pre># ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-7.5/lib64:
export LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-7.5/lib64:
export PATH=$PATH:/usr/local/cuda-7.5/bin</pre>

Don&#8217;t forget to:

<pre>$ source ~./.bashrc</pre>

8. **Run nvidia-smi** to confirm that your setup is correct.

<pre>nvidia-smi</pre>

nvidia-smi should be on your path already, so this should work. If it doesn&#8217;t:

  * You must have done something wrong, better go back to step 1. and do it all over again.
  * You can see if it&#8217;s at /usr/bin/nvidia-smi .

You should then see something similar to this:

<pre>Mon Nov 23 21:01:17 2015 
+------------------------------------------------------+ 
| NVIDIA-SMI 352.39 Driver Version: 352.39 | 
|-------------------------------+----------------------+----------------------+
| GPU Name Persistence-M| Bus-Id Disp.A | Volatile Uncorr. ECC |
| Fan Temp Perf Pwr:Usage/Cap| Memory-Usage | GPU-Util Compute M. |
|===============================+======================+======================|
| 0 GeForce GTX 780 Off | 0000:01:00.0 N/A | N/A |
| 34% 33C P0 N/A / N/A | 137MiB / 3068MiB | N/A Default |
+-------------------------------+----------------------+----------------------+
 
+-----------------------------------------------------------------------------+
| Processes: GPU Memory |
| GPU PID Type Process name Usage |
|=============================================================================|
| 0 Not Supported |
+-----------------------------------------------------------------------------+</pre>

9. Go to where you downloaded the previous files at, and **run cuda\_7.5.18\_linux.run** .

<pre>chmod +x cuda_7.5.18_linux.run # Make it executable

./cuda_7.5.18_linux.run # Install Nvidia display driver</pre>

For yes/no questions, take &#8220;yes&#8221;. For locations, press &#8220;enter&#8221; to take default values.

10. **Re-run nvidia-smi** just to be sure everything is still working well.

<pre>nvidia-smi</pre>

You should then (again) see something similar to this:

<pre>Mon Nov 23 21:08:56 2015 
+------------------------------------------------------+ 
| NVIDIA-SMI 352.39 Driver Version: 352.39 | 
|-------------------------------+----------------------+----------------------+
| GPU Name Persistence-M| Bus-Id Disp.A | Volatile Uncorr. ECC |
| Fan Temp Perf Pwr:Usage/Cap| Memory-Usage | GPU-Util Compute M. |
|===============================+======================+======================|
| 0 GeForce GTX 780 Off | 0000:01:00.0 N/A | N/A |
| 34% 28C P8 N/A / N/A | 271MiB / 3068MiB | N/A Default |
+-------------------------------+----------------------+----------------------+
 
+-----------------------------------------------------------------------------+
| Processes: GPU Memory |
| GPU PID Type Process name Usage |
|=============================================================================|
| 0 Not Supported |
+-----------------------------------------------------------------------------+</pre>

11. Reboot now back to desktop.

<pre>reboot</pre>

12. Time to install Theano and python, basically the first part of <a href="http://deeplearning.net/software/theano/install_ubuntu.html" target="_blank">this guide</a>. Run the following commands:

<pre>sudo apt-get install python-numpy python-scipy python-dev python-pip python-nose g++ libopenblas-dev git
sudo pip install Theano</pre>

13. **Copy and paste the below code into check1.py**. Now we need to make sure GPU is indeed working. I am copying the instructions from <a href="http://deeplearning.net/software/theano/tutorial/using_gpu.html" target="_blank">deeplearning.net</a>.

<pre><span class="kn">from</span> <span class="nn">theano</span> <span class="kn">import</span> <span class="n">function</span><span class="p">,</span> <span class="n">config</span><span class="p">,</span> <span class="n">shared</span><span class="p">,</span> <span class="n">sandbox</span>
<span class="kn">import</span> <span class="nn">theano.tensor</span> <span class="kn">as</span> <span class="nn">T</span>
<span class="kn">import</span> <span class="nn">numpy</span>
<span class="kn">import</span> <span class="nn">time</span>

<span class="n">vlen</span> <span class="o">=</span> <span class="mi">10</span> <span class="o">*</span> <span class="mi">30</span> <span class="o">*</span> <span class="mi">768</span>  <span class="c"># 10 x #cores x # threads per core</span>
<span class="n">iters</span> <span class="o">=</span> <span class="mi">1000</span>

<span class="n">rng</span> <span class="o">=</span> <span class="n">numpy</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">RandomState</span><span class="p">(</span><span class="mi">22</span><span class="p">)</span>
<span class="n">x</span> <span class="o">=</span> <span class="n">shared</span><span class="p">(</span><span class="n">numpy</span><span class="o">.</span><span class="n">asarray</span><span class="p">(</span><span class="n">rng</span><span class="o">.</span><span class="n">rand</span><span class="p">(</span><span class="n">vlen</span><span class="p">),</span> <span class="n">config</span><span class="o">.</span><span class="n">floatX</span><span class="p">))</span>
<span class="n">f</span> <span class="o">=</span> <span class="n">function</span><span class="p">([],</span> <span class="n">T</span><span class="o">.</span><span class="n">exp</span><span class="p">(</span><span class="n">x</span><span class="p">))</span>
<span class="k">print</span><span class="p">(</span><span class="n">f</span><span class="o">.</span><span class="n">maker</span><span class="o">.</span><span class="n">fgraph</span><span class="o">.</span><span class="n">toposort</span><span class="p">())</span>
<span class="n">t0</span> <span class="o">=</span> <span class="n">time</span><span class="o">.</span><span class="n">time</span><span class="p">()</span>
<span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">xrange</span><span class="p">(</span><span class="n">iters</span><span class="p">):</span>
    <span class="n">r</span> <span class="o">=</span> <span class="n">f</span><span class="p">()</span>
<span class="n">t1</span> <span class="o">=</span> <span class="n">time</span><span class="o">.</span><span class="n">time</span><span class="p">()</span>
<span class="k">print</span><span class="p">(</span><span class="s">"Looping </span><span class="si">%d</span><span class="s"> times took </span><span class="si">%f</span><span class="s"> seconds"</span> <span class="o">%</span> <span class="p">(</span><span class="n">iters</span><span class="p">,</span> <span class="n">t1</span> <span class="o">-</span> <span class="n">t0</span><span class="p">))</span>
<span class="k">print</span><span class="p">(</span><span class="s">"Result is </span><span class="si">%s</span><span class="s">"</span> <span class="o">%</span> <span class="p">(</span><span class="n">r</span><span class="p">,))</span>
<span class="k">if</span> <span class="n">numpy</span><span class="o">.</span><span class="n">any</span><span class="p">([</span><span class="nb">isinstance</span><span class="p">(</span><span class="n">x</span><span class="o">.</span><span class="n">op</span><span class="p">,</span> <span class="n">T</span><span class="o">.</span><span class="n">Elemwise</span><span class="p">)</span> <span class="k">for</span> <span class="n">x</span> <span class="ow">in</span> <span class="n">f</span><span class="o">.</span><span class="n">maker</span><span class="o">.</span><span class="n">fgraph</span><span class="o">.</span><span class="n">toposort</span><span class="p">()]):</span>
    <span class="k">print</span><span class="p">(</span><span class="s">'Used the cpu'</span><span class="p">)</span>
<span class="k">else</span><span class="p">:</span>
    <span class="k">print</span><span class="p">(</span><span class="s">'Used the gpu'</span><span class="p">)</span></pre>

14. **Run the check1.py to confirm that Theano is working correctly with CPU**.

<pre>THEANO_FLAGS=mode=FAST_RUN,device=cpu,floatX=float32 python check1.py</pre>

This should give:

<pre>[Elemwise{exp,no_inplace}(&lt;TensorType(float32, vector)&gt;)]
Looping 1000 times took 1.999963 seconds
Result is [ 1.23178029 1.61879337 1.52278066 ..., 2.20771813 2.29967761
 1.62323284]
<strong>Used the cpu</strong></pre>

15. **Run the check.py to confirm that Theano is working correctly with GPU**.

<pre>THEANO_FLAGS=mode=FAST_RUN,device=gpu,floatX=float32 python check1.py</pre>

This should give:

<pre>Using gpu device 0: GeForce GTX 780
[GpuElemwise{exp,no_inplace}(&lt;CudaNdarrayType(float32, vector)&gt;), HostFromGpu(GpuElemwise{exp,no_inplace}.0)]
Looping 1000 times took 0.583838 seconds
Result is [ 1.23178029 1.61879349 1.52278066 ..., 2.20771813 2.29967761
 1.62323296]
<strong>Used the gpu</strong></pre>

Note that: computation time has decreased, and it is indeed using GPU now.

16. Done! If you reached here and did not meet any issues, I hope you can comment and confirm that this does help you, so others can trust this guide. If you have still spent hours/days, **blame Linux.** =]

# Things That Does Not Work For Me

1. Installing current nvidia drivers from apt-get.

<pre>$ apt-get install nvidia-current</pre>

Normally you would think apt-get install the latest nvidia drivers would be the best to go. Nope, after the installation 1) nvidia-smi does not work 2) I cannot get back into desktop 3) get error messages like RmInitAdapter failed in dmesg 4) no monitor found in /var/log/Xorg.0.log

2. Install CUDA from apt-get.

<pre>$ apt-get install nvidia-cuda-toolkit</pre>

The same as 1., equally disastrous: 1) nvidia-smi does not work 2) I cannot get back into desktop 3) get error messages like RmInitAdapter failed in dmesg 4) no monitor found in /var/log/Xorg.0.log

3. Installing current nvidia from another &#8220;defacto&#8221; repository.

<pre>sudo apt-add-repository ppa:ubuntu-x-swat/x-updates
sudo apt-get update
sudo apt-get install nvidia-current</pre>

The same as 1., equally disastrous: 1) nvidia-smi does not work 2) I cannot get back into desktop 3) get error messages like RmInitAdapter failed in dmesg 4) no monitor found in /var/log/Xorg.0.log

4. Anything about Bumblebee.

Not sure what this is, just not related. Forget about it.

5. Also, I don&#8217;t have advice trying to repair a broken Ubuntu if you did anything of the above. But maybe you can try removing nvidia drivers and reinstalling the desktop, but doesn&#8217;t work for me either.

<pre>apt-get remove --purge nvidia*
apt-get install --reinstall ubuntu-desktop unity
apt-get install nvidia-current</pre>

6. Using any nvidia driver like 340 or 304 version. They do not work.

_**The only thing that works for me is a fresh reinstall of Ubuntu 14.04. Nothing else.**_

# Final Words

The <a href="http://deeplearning.net/software/theano/install_ubuntu.html" target="_blank">installation guide</a> from DeepLearning.net is very helpful, but it contains poison advice that messed up my display of my OS completely.

BTW, ever heard of **&#8220;Linux is user-friendly but it is very selective of its friends.&#8221;**? I know I am certainly not its friend&#8230; 😉