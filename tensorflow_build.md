# 텐서플로우 Build from sources 요약

기본적으로 텐서플로우 공식 홈페이지의 [Installing TensorFlow from Sources](https://www.tensorflow.org/install/install_sources) 의 절차를 따름. 다만 옵션들이 살짝 달라짐.

<p>
<code>
$ git clone https://github.com/tensorflow/tensorflow
</code>
</p>

<p>
<code>
$ cd tensorflow
</code>
<br />
<code>
$ git checkout r1.3 
</code>
<br />
(뒤의 버전을 설치할 버전으로 바꿈)
</p>

<p>
<code>
$ sudo pip3 install six numpy wheel   
</code>
<br />
파이썬을 Homebrew 를 통해 Python3 를 설치해서 쓰기 때문에 <code>pip3</code> 를 사용함.
</p>

<p>
<code>
$ brew install coreutils
</code>
</p>

<p>
<code>
$ cd tensorflow  
</code>
<br />

<code>
$ ./configure
</code>
<br />

이 때 configure 를 위한 각종 옵션들을 입력해야 하는데, 이때 주의하여 입력할 것.  
특히 python interpreter 의 위치와 site-packages 의 위치에 유의할 것.
</p>

<p>
(No GPU)<br />
<code>
$ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-msse4.2 //tensorflow/tools/pip_package:build_pip_package
</code>
</p>

<p>
(GPU)<br />
<code>
$ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --copt=-msse4.2 --config=cuda -k //tensorflow/tools/pip_package:build_pip_package
</code>
</p>

<p>
<code>
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
</code>
</p>

<p>
<code>
$ sudo pip3 install /tmp/tensorflow_pkg/tensorflow-1.2.1-cp36-cp36m-macosx_10_12_x86_64.whl
</code><br />
(파일 이름은 폴더를 확인하여 교체)
</p>


#### Tensorflow GPU Mac build 참고  
Tensorflow 1.2.1 with NVIDIA CUDA GPU support on macOS Sierra 10.12 (<https://gist.github.com/jvicenti/d64fc03a57b7a2ceb117634bdb5dabfb>)
 
The version ('80100') of the host compiler ('Apple clang') is not supported (<https://github.com/torch/torch7/issues/1008>)  
: Install Xcode Commnad Line Tools ver.8.2  

<p>
<code>
$ sudo xcode-select --switch /Library/Developer/CommandLineTools
</code>
</p>

NVIDIA CUDA Installation Guide for Mac OS X  
(<http://docs.nvidia.com/cuda/cuda-installation-guide-mac-os-x/index.html#xcode-version>)  
: For OSX 10.12, Xcode 8.2 and Apple LLVM 8.0.0 should be used.  

OSX Sierra + Titan Xp eGPU. Tensorflow 1.2.1. + CUDA 8 + cuDNN 6  
(<https://gist.github.com/danbarnes333/486fc02f98046048f7d6bfd5908d561a>)  

<https://gist.github.com/myh1000/3fbb42928d94a083f6eaed28883ef659>  

<https://medium.com/@mattias.arro/installing-tensorflow-1-2-from-sources-with-gpu-support-on-macos-4f2c5cab8186>  

<https://gist.github.com/Mistobaan/dd32287eeb6859c6668d>  

<https://github.com/tensorflow/tensorflow/issues/11859>



