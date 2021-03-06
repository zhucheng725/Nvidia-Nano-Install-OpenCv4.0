# Nvidia-Nano-Install-OpenCv4.0

### STEP1: Download The OpenCV4 Resource

Give the website to download:
https://opencv.org/releases/   #Opencv4
and 
https://codeload.github.com/opencv/opencv_contrib/zip/4.1.0  #Opencv4 extra contrib

### STEP2:Install the requirement

```
$sudo apt-get update
$sudo apt-get install -y build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
$sudo apt-get install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
$sudo apt-get install -y python2.7-dev python3.6-dev python-dev python-numpy python3-numpy
$sudo apt-get install -y libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
$sudo apt-get install -y libv4l-dev v4l-utils qv4l2 v4l2ucp
$sudo apt-get install -y curl
$sudo apt-get update
```

But in my nano, you can do this following:
```
sudo add-apt-repository "deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe"
sudo apt update
sudo apt install libjasper1 libjasper-dev
```
or download the deb:
https://packages.ubuntu.com/zh-cn/xenial/arm64/libjasper-dev/download



### STEP3:START
make dir in the opencv-4.1.0 floder named "release" #mkdir release
<br>
```cd release/```
<br>
```
cmake -D WITH_CUDA=ON -D CUDA_ARCH_BIN="5.3" -D CUDA_ARCH_PTX="" -D OPENCV_EXTRA_MODULES_PATH=/home/kirito/procedure/opencv4/opencv_contrib-4.1.0/modules -D WITH_GSTREAMER=ON -D WITH_LIBV4L=ON -D BUILD_opencv_python2=ON -D BUILD_opencv_python3=ON -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF -D CMAKE_BUILD_TYPE=RELEASE  -D WITH_IPP=OFF  -D CMAKE_INSTALL_PREFIX=/usr/local ..
```
Before do that cmake, I try other ways such as :
```
cmake -D WITH_CUDA=ON -D CUDA_ARCH_BIN="5.3" -D CUDA_ARCH_PTX="" -D OPENCV_EXTRA_MODULES_PATH=/home/kirito/procedure/opencv4/opencv_contrib-4.1.0/modules -D WITH_GSTREAMER=ON -D WITH_LIBV4L=ON -D BUILD_opencv_python2=ON -D BUILD_opencv_python3=ON -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF -D CMAKE_BUILD_TYPE=RELEASE  -D CMAKE_INSTALL_PREFIX=/usr/local ..
```
without -D WITH_IPP=OFF and failed.
I try to download the <br>

https://raw.githubusercontent.com/opencv/opencv_3rdparty/ippicv/master_20170822/ippicv/ippicv_2017u3_lnx_intel64_general_20170822.tgz<br>
and extrat it to the path: 
```
opencv-4.1.0/3rdparty/ippicv.
```
Althought I extrat it and put it to the path, I see some fail list after $cmake.
Anyways , I put it in the path.
and 
```
$make -j3 #also you can just $make 
```
Maybe you will see the result:<br>
```
Killed
CMake Error at cuda_compile_1_generated_pyrlk.cu.o.RELEASE.cmake:281 (message):
Error generating file
/home/kirito/procedure/opencv4/opencv-4.1.0/release/modules/cudaoptflow/CMakeFiles/cuda_compile_1.dir/src/cuda/./cuda_compile_1_generated_pyrlk.cu.o


modules/cudaoptflow/CMakeFiles/opencv_cudaoptflow.dir/build.make:675: recipe for target 'modules/cudaoptflow/CMakeFiles/cuda_compile_1.dir/src/cuda/cuda_compile_1_generated_pyrlk.cu.o' failed
make[2]: *** [modules/cudaoptflow/CMakeFiles/cuda_compile_1.dir/src/cuda/cuda_compile_1_generated_pyrlk.cu.o] Error 1
CMakeFiles/Makefile2:9900: recipe for target 'modules/cudaoptflow/CMakeFiles/opencv_cudaoptflow.dir/all' failed
make[1]: *** [modules/cudaoptflow/CMakeFiles/opencv_cudaoptflow.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....

[ 94%] Built target opencv_stitching
Makefile:162: recipe for target 'all' failed
make: *** [all] Error 2
```
I think maybe the nano do not have enough RAM or very hot during the cmake.<br>
https://devtalk.nvidia.com/default/topic/1049296/jetson-nano/how-to-install-opencv-python-for-python3-6/post/5329472/#.<br>
So you can set some requirements before $make:
```
$ sudo fallocate -l 4.0G /swapfile # this is the difference
$ sudo chmod 600 /swapfile
$ sudo mkswap /swapfile
$ sudo swapon /swapfile
```
It will cost a long time about 3-4 hours.
After it , you will very excited to see the result.
![image](https://github.com/zhucheng725/Nvidia-Nano-Install-OpenCv4.0/blob/master/1.png)
and 
```
$sudo make install 
$sudo apt-get install -y python-opencv python3-opencv
```
![iamge](https://github.com/zhucheng725/Nvidia-Nano-Install-OpenCv4.0/blob/master/2.png)<br>



Also you can use .sh to complete this thing as follow:
```
https://github.com/AastaNV/JEP/blob/master/script/install_opencv4.0.0_Nano.sh
```

### STEP4:OTHERS
If meet this error:  fatal error: opencv2/photo.hpp: No such file or directory<br>
I follow this link:https://blog.csdn.net/keith_bb/article/details/52685231<br>
If you meet this error again you can do this command:<br>
```
sudo apt-get install libopencv-dev
```
I would meet some errors such as can not find the opencv.hpp directory.
Then I would try to write the CMakeLists.txt and build the 'build' file.
The CMakeLists.txt as follow:
```
cmake_minimum_required(VERSION 2.8)
project(tld)

set(CMAKE_CXX_FLAGS "-std=c++11")

find_package(OpenCV 4.1 REQUIRED)

include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(tld tld.cpp)
target_link_libraries(tld ${OpenCV_LIBS})
```
Then follow this command:<br>
```
cd build
cmake ..
make
./tld
```
Maybe would help you.





