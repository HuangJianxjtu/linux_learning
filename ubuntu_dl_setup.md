# Ubuntu深度学习环境配置

## 1.Nvidia GPU driver, CUDA and cnDNN, openCV

用途： 使用GPU加速Deep Learning计算； CUDA加速...

必须都安装前三者后，才能使用GPU来加速TensorFlow, Torch/PyTorch等DL框架

### 0. 推荐的版本（均已测试）

* ubuntu 14.04+1080ti

    CUDA 8.0(.deb方式安装), cudnn v5, openCV 2.4.13

    **注意**：在cmake opencv的源文件时，由于cuda8.0不支持老的显卡版本，因此需要加上相应参数：`cmake -D CUDA_GENARATION=Kepler ../`, 否则会报错:`nvcc fatal : Unsupported gpu architecture 'compute_11'`

* ubuntu 18.04+1650(Dell-inpiration-7590 laptop)

    CUDA 10.2(.deb方式安装,会自动安装nvidia driver440.33), cudnn v7.6.5, openCV 3.4.8

     **注意**：CUDA 10以上安装Torch时,需要用[这个仓库](https://github.com/nagadomi/distro)，不要用[官方的](http://torch.ch/docs/getting-started.html#_)

     **BUG**: Torch的cunn装不上

### 1.1 安装Nvidia GPU driver(如果使用.deb文件安装cuda，则可跳过这一步)

* 安装方法：

    一般直接使用Ubuntu中Software&Update里面的Nvidia驱动。否则，需要手动安装（网上找教程吧）。测试成功的例子：
  * i7-7700+1080ti台式机：ubuntu16.04/14.04， nvidia driver 384.130
  * i7-9750H+GTX1650笔记本(dell-7950):ubuntu18.04, nvidia driver 440.33或430

    注：最好选常用的显卡，比如1080ti等。 GTX1650在ubuntu16.04中很难安装驱动，尝试了无数次后放弃，故选择ubuntu18.04

* 测试Nvidia GPU driver是否安装成功

    能正常运行nvidia-settings和nvidia-smi,则说明安装成功。
    进一步确认： setting里面的details，看看是否显示nvidia显卡

* 显卡切换

    ```bash
    sudo prime-select nvidia # 切换到nvidia显卡
    sudo prime-select intel  # 切换到intel显卡
    sudo prime-select query  # 查看当前使用的显卡
    ```

    reboot

* nvidia显卡使用情况查看

    `watch -n 1 nvidia-smi`

* 卸载nvidia显卡驱动（特别是在无法进入图形界面时）

  * Crtl + Alt + F3， 进入tty命令行模式
  * sudo apt-get purge nvidia-*

 * * *

### 1.2 安装CUDA

* 下载CUDA

    [官网](https://developer.nvidia.com/cuda-toolkit-archive)下载安装文件。**应注意cuda版本和其对应的nvidia驱动版本，否则可能导致无法进入图形界面**
  * deb(local)文件: 在安装CUDA的同时，会自动下载、安装最新显卡驱动。缺点：安装的显卡驱动有时和系统不兼容，需要自行修复
  * runfile(local，.run文件, **不推荐**)：可以选择不安装显卡驱动。需要先关闭xserver,比较麻烦，因此不太建议使用

* 安装

    ```bash
    # .deb文件安装
    sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
    sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
    sudo apt-get update
    sudo apt-get install cuda

    # .run文件安装
    sudo sh cuda_9.0.176_384.81_linux.run
    # Follow the command-line prompts
    ```

    重启电脑，看看是否能正常进入图形界面。若不能需要进入tty模式，卸载cuda，并重试

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
    ```

* 测试（即查看cudnn的版本）

    `cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`

    若运行正常则说明安装正确

* 卸载

    若删除cuda，cuDNN也会被跟着删除的

 * * *
