# 텐서플로우 Build from sources 절차
------------------------

* 먼저 OSX 버전에 따라 Xcode 버전과 Apple LLVM 버전을 맞춰줘야 함.  
: For OSX 10.12, Xcode 8.2 and Apple LLVM 8.0.0 should be used.  
: NVIDIA CUDA Installation Guide for Mac OS X 페이지의 정보를 확인할 것.  
(<http://docs.nvidia.com/cuda/cuda-installation-guide-mac-os-x/index.html#xcode-version>)  
: 이를 위해서는 Xcode command line tools 를 8.2 버전을 설치해야 함. 설치 후

```
$ sudo xcode-select --switch /Library/Developer/CommandLineTools
```
를 통해 새로 설치된 버전 선택. llvm --version 을 통해 버전을 확인.  


* CUDA 가 제대로 잘 설치되었는지 확인할 것.   
CUDA Toolkit 과 cuDNN 버전도 확인할 것(현재는 CUDA 8.0, cuDNN 6 버전).  
CUDA Toolkit Download: <https://developer.nvidia.com/cuda-downloads>  
cuDNN Download: <https://developer.nvidia.com/cudnn>  


* ~/.bash_profile 파일에서 CUDA 경로 설정을 해줘야 함.

```
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib:$LD_LIBRARY_PATH
export DYLD_LIBRARY_PATH=/usr/local/cuda/lib:$DYLD_LIBRARY_PATH
```

* disable SIP  
(<http://osxarena.com/2015/10/guide-details-apples-system-integrity-protection-sip-for-hackintosh/>)  

```
$ csrutil status 
# 결과가 disabled 로 나오면 된 것.
```   



* **ld: library not found for -lgomp**  
위와 같은 오류를 방지하기 위해서 아래 명령을 실행.  

```
$ brew install llvm
$ ln -s /usr/local/opt/llvm/bin/clang /usr/local/bin/clang-omp
$ ln -s /usr/local/Cellar/llvm/4.0.1/lib/libgomp.dylib /usr/local/lib/libgomp.dylib
```


* 빌드 과정은 텐서플로우 공식 홈페이지의 [Installing TensorFlow from Sources](https://www.tensorflow.org/install/install_sources) 의 절차를 따름. 다만 옵션들이 살짝 달라짐.

```
$ git clone https://github.com/tensorflow/tensorflow

$ cd tensorflow
$ git checkout r1.3  # 뒤의 버전을 설치할 버전으로 바꿈

$ sudo pip3 install six numpy wheel   
# 파이썬을 Homebrew 를 통해 Python3 를 설치해서 쓰기 때문에 pip3 를 사용함.
$ brew install coreutils

$ cd tensorflow  
$ ./configure

```

* 이 때 configure 를 위한 각종 옵션들을 입력해야 하는데, 이때 주의하여 입력할 것.  

```
(configure options)

the location of python: /usr/local/bin/python3
Python library paths: /usr/local/lib/python3.6/site-packages
CUDA support? y
use clang as CUDA compiler? N
CUDA SDK version: 8.0
location where CUDA toolkit is installed: /Developer/NVIDIA/CUDA-8.0 (or defualt /usr/local/cuda)
cuDNN version: 6
the location where cuDNN  library is installed: /Developer/NVIDIA/CUDA-8.0 (or defualt /usr/local/cuda)
Cuda compute capabilities: 6.1
```



* (No GPU)

```
$ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-msse4.2 //tensorflow/tools/pip_package:build_pip_package
```


* (GPU)

```
$ bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
```

* pip 설치

```
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```
```
$ sudo pip3 install /tmp/tensorflow_pkg/tensorflow-1.3.0-cp36-cp36m-macosx_10_12_x86_64.whl #파일 이름은 폴더를 확인하여 교체
```

-----------------------------

#### Tensorflow GPU Mac build 참고  

Tensorflow 1.2.1 with NVIDIA CUDA GPU support on macOS Sierra 10.12 (<https://gist.github.com/jvicenti/d64fc03a57b7a2ceb117634bdb5dabfb>)
 
The version ('80100') of the host compiler ('Apple clang') is not supported (<https://github.com/torch/torch7/issues/1008>)  
: Install Xcode Commnad Line Tools ver.8.2  

```
$ sudo xcode-select --switch /Library/Developer/CommandLineTools
```

NVIDIA CUDA Installation Guide for Mac OS X  
(<http://docs.nvidia.com/cuda/cuda-installation-guide-mac-os-x/index.html#xcode-version>)  
: For OSX 10.12, Xcode 8.2 and Apple LLVM 8.0.0 should be used.  

OSX Sierra + Titan Xp eGPU. Tensorflow 1.2.1. + CUDA 8 + cuDNN 6  
(<https://gist.github.com/danbarnes333/486fc02f98046048f7d6bfd5908d561a>)  

Tensorflow 1.2.1 버전 맥에서 GPU 포함하여 컴파일 하기  
(<http://chaoswith.blogspot.kr/2017/07/tensorflow-121-gpu.html>)  

<https://medium.com/@mattias.arro/installing-tensorflow-1-2-from-sources-with-gpu-support-on-macos-4f2c5cab8186>  

<https://gist.github.com/Mistobaan/dd32287eeb6859c6668d>  

<https://github.com/tensorflow/tensorflow/issues/11859>



