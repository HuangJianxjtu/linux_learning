# Ubuntu20.04配置笔记

## 如何解决Ubuntu20.04与Windows系统时间不同步

>* `timedatectl set-local-rtc true`

## 在“Software & Updates”中，修改软件源为科大源，更新

## system monitor
>* `sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor`
>* `sudo apt update`
>* `sudo apt install indicator-sysmonitor`
>* 启动，右击设置"Run on startup"，添加network

## 显卡驱动
在“Software & Updates”的additional drivers安装即可，使用tested版本。安装后可能需要重启

>* 解决关机慢、有光标闪烁问题，[参考](https://blog.csdn.net/X_T_S/article/details/110144658)。似乎是因为英伟达显卡驱动引起的
>* **解决ubuntu搭配4k屏幕使用过程中的个别软件（搜狗输入法，qv2ray）显示过小问题:** 即设置4k分辨率150%缩放后，一些软件显示过小。[参考](https://unix.stackexchange.com/questions/433385/scaling-hidpi-issue-for-qt5-applications-under-gnome), 不知道还复现。。。。

## 中文输入法
按照sougou for linux的官方教程安装即可。如果系统找不到搜狗输入法，需要在“Input Method”中添加。

## 科学上网
>* 下载最新版的qv2ray(2.7.0)，[链接](https://github.com/Qv2ray/Qv2ray/releases/tag/v2.7.0)。并设置开机自动重启、自动连接；开机自动重启可能失效，此时需要手动添加到startup application中
>* 从github下载最新版的xray core到qv2ray的默认路径，[链接](https://github.com/XTLS/Xray-core/releases/tag/v1.5.5)。并通过检查。
>* 添加自己的翻墙服务器

## Google Chrome

>* 翻墙后很容易安装

## 更换Terminal

终端采用zsh和oh-my-zsh，美观且功能强大！！！ZSH, also called the Z shell

>* 首先，安装zsh：

  `sudo apt install zsh`

>* 将默认终端由bash换成zsh

`chsh -s /bin/zsh   #注意：不要使用sudo`

>* 安装oh-my-zsh 项目来帮我们配置 zsh，采用wget安装：

 ```bash
 cd ~/Downloads
 wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh
 chmod +x install.sh
 ./install.sh
 ```

>* 安装语法高亮插件和自动补全插件

```bash
git clone https://gitee.com/lxgyChen/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://gitee.com/pocmon/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
再将~/.zshrc中的`plugins=(git)`改为`plugins=(git zsh-syntax-highlighting zsh-autosuggestions)`, 最后运行`source ~/.zshrc`

>* 修改透明度为10%

>* 分屏工具teminator, `sudo apt install terminator`；在preference->profiles->general中，设置不显示titlebar；unfocused terminal font brightness设置成80%.


## 设置git
参考廖雪峰的[网站](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

## Latex环境
>* `sudo apt-get install texlive-full`
>* `sudo apt-get install texstudio`
>* [参考](https://blog.csdn.net/qq_41814939/article/details/82288145)，可以参考设置XeLaTeX编译引擎和中文支持包

## ROS 1.0

[清华源_Ubuntu20.04下安装ROS](https://blog.csdn.net/weixin_42525601/article/details/112198438), [科大源_Ubuntu20.04下安装ROS](https://icode.best/i/55863549473115)

## VScode

登录账号后，自动同步配置。TODO: 整理一下如何用`Setting Sync`手动同步

## Clion & Pycharm

>* CLion和PyCharm安装,[参考](https://blog.csdn.net/feimeng116/article/details/105898892)；注意安装路径, **统一安装在`~/bin_hj`下**; 
>* 使用CLion&Pycharm进行ROS编程，[参考](https://github.com/HuangJianxjtu/robotics_coding_learning/blob/master/ROS/env_setup/ros_IDE_env_setup.md)
>* 可能会遇到pycharm无法导入rospy，[参考](https://blog.csdn.net/weixin_44481159/article/details/112583202)

##  文献管理工具Zotero

>* 下载链接[Zotero 6 for Linux](https://www.zotero.org/download/)，下载得到一个压缩包
>* 解压到一个能长期保存的文件夹，我的是: `~/bin_hj`。可以直接运行该目录下的`zotero`文件，但不方便
>* 建立桌面快捷方式(**在Zotero_linux-x86_64路径下运行**)：
```bash
./set_launcher_icon
ln -s /PATH_TO_Zotero/Zotero_linux-x86_64/zotero.desktop ~/.local/share/applications/zotero.desktop  #使用绝对路径创建软链接，需要修改第一个文件路径
```
>* 之后zotero的配置参考[zotero_坚果云_setup](zotero_jianguoyun_setup.md)

## 日常工具
>* 下载工具transmission (ubuntu20.04自带)
>* 媒体播放器：Ubuntu自带或VLC

>* [ubunntu 20.04 mendeley的安装与卸载](https://blog.csdn.net/qq_33804792/article/details/117708336)
>* docker, [安装与使用](https://blog.csdn.net/leon_zeng0/article/details/113881191), [卸载](https://zhuanlan.zhihu.com/p/143156163)。 [docker使用教程-菜鸟](https://www.runoob.com/docker/docker-container-usage.html)。[docker生成Ubuntu16.04容器-安装ROSkinetic](https://blog.csdn.net/u010904547/article/details/108375005)

## TODO: 设置局域网内网络共享文件夹
主要用途是跟windows快速传文件，[参考](https://blog.csdn.net/willhu2008/article/details/121715002). 但感觉有点麻烦，而且如果设置允许匿名访问，则仅能读取，不能修改。。。 以后再说吧 懒。。。

## 库

* Eigen
    * `sudo apt install libeigen3-dev`; 
    * 查看Eigen库的版本，`gedit /usr/include/eigen3/Eigen/src/Core/util/Macros.h`
    * 头文件位置：`/usr/include/eigen3`

* Sophus
    * `git clone http://github.com/strasdat/Sophus
`， 用cmake安装

    * find_package(fmt)报警告; `git clone  https://github.com/fmtlib/fmt.git`， 使用cmake安装即可。fmt是Ubuntu20.04才有的，注意使用，否则会报错。
    * Pangolin, `git clone https://github.com/stevenlovegrove/Pangolin.git`， 使用cmake安装即可。
    * Sophus头文件的位置`/usr/local/include/sophus`

* Cere
    * 安装依赖：`sudo apt install liblapack-dev libsuitesparse-dev libcxsparse3 libgflags-dev libgoogle-glog-dev libgtest-dev`
    * `git clone https://github.com/ceres-solver/ceres-solver.git`,使用cmake安装即可
    * 头文件位置：`/usr/local/include/ceres`; 库文件`/usr/local/lib/libceres.a`

* g2o
    * 安装依赖: `sudo apt install qt5-qmake qt5-default libqglviewer-dev-qt5 libsuitesparse-dev libcxsparse3 libcholmod3`
    * `git clone https://github.com/RainerKuemmerle/g2o.git`, 使用cmake安装即可
    * 头文件位置：`/usr/local/include/g2o/`; 库文件在`/usr/local/lib`下

* OpenCV3
    * 使用源码安装，[网址](https://opencv.org/releases/)。使用cmake安装，使用版本3.4.15
    * 头文件位置：`/usr/local/include/opencv2`
    * 查看opencv版本` pkg-config --modversion opencv`