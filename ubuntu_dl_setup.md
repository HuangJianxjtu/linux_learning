# Ubuntu深度学习环境配置

## 1.Nvidia GPU driver, CUDA and cnDNN, openCV

用途： 使用GPU加速Deep Learning计算； CUDA加速...

必须都安装前三者后，才能使用GPU来加速TensorFlow, PyTorch/Torch等DL框架

### 1.1 推荐的版本（均已测试）

* ubuntu 14.04+1080ti台式机

    CUDA 8.0(.deb方式安装), cudnn v5, openCV 2.4.13

    **注意**：在cmake opencv的源文件时，由于cuda8.0不支持老的显卡版本，因此需要加上相应参数：`cmake -D CUDA_GENARATION=Kepler ../`, 否则会报错:`nvcc fatal : Unsupported gpu architecture 'compute_11'`

* ubuntu 18.04+1650(Dell-inpiration-7590 laptop)

 >>* CUDA_10.2 + nvidia_driver_440.33(.deb方式安装), cudnn v7.6.5, openCV 3.4.8
 >>* 安装较为容易

* ubuntu 16.04+1650(Dell-inpiration-7590 laptop)

 >>* CUDA_10.1.168 + nvidia_driver_418.67(.deb方式安装), cudnn v7.6.4, openCV 3.4.8
 >>* 安装非常麻烦。上面几乎是唯一安装成功的版本！
  
### 1.1 安装Nvidia GPU driver(如果使用.deb文件安装cuda，则可跳过这一步)

* 一般的安装方法：

    一般直接使用Ubuntu中Software&Update里面的Nvidia驱动。否则，需要手动安装（网上找教程吧）。测试成功的例子：
  * i7-7700+1080ti台式机：ubuntu16.04/14.04， nvidia driver 384.130
  * i7-9750H+GTX1650笔记本(dell-7950):ubuntu18.04, nvidia driver 440.33或430

    注：最好选常用的显卡，比如1080ti等

* PPA方式安装

  参考[此文](https://blog.csdn.net/L_Y_Fei/article/details/101113785)

* 测试Nvidia GPU driver是否安装成功

    能正常运行nvidia-settings和nvidia-smi,则说明安装成功。
    进一步确认： setting里面的details，看看是否显示nvidia显卡

* 显卡切换

    ```bash
    sudo prime-select nvidia # 切换到nvidia显卡
    sudo prime-select intel  # 切换到intel显卡
    sudo prime-select query  # 查看当前使用的显卡
    sudo reboot
    ```

* nvidia显卡使用情况查看

    `watch -n 1 nvidia-smi`

* 卸载nvidia显卡驱动（特别是在无法进入图形界面时）

  * Crtl + Alt + F1， 进入tty命令行模式
  * `sudo apt-get purge nvidia-*`

 * * *

### 1.2 安装CUDA

* 下载CUDA

    [官网](https://developer.nvidia.com/cuda-toolkit-archive)下载安装文件。**应注意cuda版本和其对应的nvidia驱动版本，否则可能导致无法进入图形界面**
  * deb(local)文件: 在安装CUDA的同时，会自动下载、安装最新显卡驱动。缺点：安装的显卡驱动有时和系统不兼容，需要自行修复
  * runfile(local，.run文件, **不推荐**)：可以选择不安装显卡驱动。需要先关闭xserver,比较麻烦，因此不太建议使用

* .deb方式安装

>>* 禁用nouveau模块

`sudo gedit /etc/modprobe.d/blacklist.conf`

在末尾加入

```txt
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist rivatv
blacklist nvidiafb
```

`sudo update-initramfs -u`

>>* 重启。

通过`Crtl + Alt + F1`， 进入tty命令行模式（返回桌面：`Ctrl + Alt + F7`）。
通过`lsmod | grep nouveau`查看是否在用nouveau驱动, 如果没有任何返回则说明没有使用。运行：

```bash
sudo service lightdm stop # 关闭桌面管理器lightdm
# 进入.deb安装文件所在的文件夹
sudo dpkg -i cuda-repo-ubuntu1604-***.deb
sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
sudo service lightdm start # 开启桌面管理器lightdm
sudo reboot
```

看看是否能正常进入图形界面。若不能需要进入tty模式，卸载cuda，重试

* 测试

    `cd /usr/local/cuda-10.2/samples`

    `sudo make -j12`

    注：1.此处是cuda10.2; 2.此处是12核CPU. 下同

    如果编译成功，最后会显示`Finished building CUDA samples`。在该文件夹下会产生一个bin目录，存放刚刚编译产生的可执行文件，尝试运行他们！

* 查看CUDA版本

    `cat /usr/local/cuda/version.txt`

* 配置CUDA相关环境变量

    在~/.bashrc末尾添加(Tensorflow官方安装历程要求注意的是:配置PATH和LD_LIBRARY_PATH和CUDA_HOME环境变量.)：

    ```bash
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
    export PATH=$PATH:/usr/local/cuda/bin
    export CUDA_HOME=$CUDA_HOME:/usr/local/cuda
    ```

* CUDA版本切换

    /usr/local/cuda是一个软链接，它指向我们指定的cuda版本（e.g. /usr/local/cuda-10.2). 因此，要切换CUDA版本，只需要把原来的软链接删除，再建立新的软链接即可。例如：

    ```bash
    cd /usr/local
    sudo rm -rf cuda
    sudo ln -s cuda-10.2 cuda
    ```

    用`ls -all`可以看到软链接关系

* 卸载（还没有较好的办法）

    简单粗暴：`sudo rm -rf /usr/local/cuda`

    正规：

    ```bash
    sudo apt-get remove cuda
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt autoremove     # 此时会把cuda和对应nvidia driver卸载
    sudo reboot
    ```

 * * *

### 1.3 安装cuDNN

* 下载

    [官网](https://developer.nvidia.com/rdp/cudnn-archive)下载`cuDNN Library for Linux`(即.tgz文件), 需要登陆会员才能下载（微信等），在当前目录下解压得到一个cuda文件夹，然后运行:

    ```bash
    sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
    sudo chmod a+r /usr/local/cuda/include/cudnn.h
    sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
    sudo cp cuda/include/cudnn_version.h /usr/local/cuda/include/ #对于v7以后，否则编译报错
    sudo chmod a+r /usr/local/cuda/include/cudnn_version.h  #对于v7以后，否则编译报错
    ```

* 测试（即查看cudnn的版本）

    `cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`

    若运行正常则说明安装正确

* 卸载

    若删除cuda，cuDNN也会被跟着删除的

 * * *

## 2. Anaconda

推荐使用anaconda来维护编程环境，比较方便。anaconda自动安装了jupyter noteboook. 但注意anaconda可能会跟ROS环境冲突。
* 下载

    可以用[清华源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)，也可以用[官方源](https://www.anaconda.com/products/individual)

* **安装anaconda(.sh文件)时，不要使用sudo**

    命令：`bash ***.sh`; 为了维护原来ROS的环境，不要让anaconda修改~/.bashrc 或者~/.zshrc
    >* 使用该命令在终端中激活anacoda环境： `source ~/anaconda3/bin/activate`


* 卸载

    [官方教程](https://docs.continuum.io/anaconda/install/uninstall/)

    删除~/annaconda文件夹；
    删除~/.bashrc ~/.zshrc中关于annaconda的设置


* 源
>* 添加清华源

    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
    

>* 显示安装通道
    `conda config --set show_channel_urls yes`
>* 回复默认设置
    `conda config --remove-key channels`


## 3. Tensorflow

注意有些程序需要在比较低版本的tensorflow上运行，下面是安装指定版本tensorflow的方法。如果不指定版本号，则默认安装最新版
>* `conda create -n tf python=3.6`
>* `source activate tf`
>* `conda install tensorflow-gpu=1.14`

* 测试是否安装成功

    在Python中import tensorflow; 对于1.14版本，再测试slim = tensorflow.contrib.slim

* python通过requirement文件安装环境，[链接](https://zhuanlan.zhihu.com/p/69058584)

## 4. 带CUDA加速的OpenCV

* 下载[opencv](https://opencv.org/releases/)和[opencv_contrib](https://github.com/opencv/opencv_contrib/releases)源码，使用cmake安装。opencv和opencv_contrib的版本要一致
* `mkdir build && cd build`
* cmake编译指令：`cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/home/jian/Libs/opencv/opencv-3.4.14/install -D WITH_TBB=OFF -D WITH_IPP=OFF -D WITH_1394=OFF -D BUILD_WITH_DEBUG_INFO=OFF -D BUILD_DOCS=OFF -D INSTALL_C_EXAMPLES=OFF -D INSTALL_PYTHON_EXAMPLES=OFF -D BUILD_EXAMPLES=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D WITH_QT=OFF -D WITH_GTK=ON -D WITH_OPENGL=ON -D OPENCV_EXTRA_MODULES_PATH=/home/jian/Downloads/opencv_contrib-3.4.14/modules -D WITH_V4L=ON  -D WITH_FFMPEG=ON -D WITH_XINE=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D OPENCV_GENERATE_PKGCONFIG=ON -D WITH_CUDA=ON -D CUDA_ARCH_BIN="8.6" -D CUDA_ARCH_PTX="" -D BUILD_opencv_cudacodec=OFF ../`
* 注意上述指令要根据具体情况修改三处地方：
    * `/home/jian/Libs/opencv/opencv-3.4.14/install`是指定安装位置，最好都装在自行建立的Libs中
    * `/home/jian/Downloads/opencv_contrib-3.4.14/modules`是opencv_contrib的位置
    * `CUDA_ARCH_BIN="8.6"`是GPU的算力， [查询方法](https://blog.csdn.net/CSS360/article/details/109696878)
