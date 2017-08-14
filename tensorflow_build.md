# 텐서플로우 Build from sources 요약

기본적으로 텐서플로우 공식 홈페이지의 [Installing TensorFlow from Sources] (https://www.tensorflow.org/install/install_sources) 의 절차를 따름. 다만 옵션들이 살짝 달라짐.

<code>
$ git clone https://github.com/tensorflow/tensorflow
</code>
<br />
<code>
$ cd tensorflow
</code>
<br />
<code>
$ git checkout r1.2 
</code>
(뒤의 버전을 설치할 버전으로 바꿈)
<br />

<code>
$ sudo pip3 install six numpy wheel   
</code>
<br />
파이썬을 Homebrew 를 통해 Python3 를 설치해서 쓰기 때문에 <code>pip3</code> 를 사용함.
<br />

<code>
$ brew install coreutils
</code>
<br />

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

(No GPU)<br />
<code>
$ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-msse4.2 //tensorflow/tools/pip_package:build_pip_package
</code>
<br />

(GPU)<br />
<code>
$ bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-mfpmath=both --copt=-msse4.2 --config=cuda -k //tensorflow/tools/pip_package:build_pip_package
</code>
<br />

<code>
$ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
</code>

<code>
$ sudo pip3 install /tmp/tensorflow_pkg/tensorflow-1.2.1-cp36-cp36m-macosx_10_12_x86_64.whl
</code><br />
(파일 이름은 폴더를 확인하여 교체)
<br />

- Tensorflow GPU Mac build 참고  
<https://medium.com/@mattias.arro/installing-tensorflow-1-2-from-sources-with-gpu-support-on-macos-4f2c5cab8186>



