# Ubuntu16.04重装系统后的软件环境安装指南

如何解决Ubuntu与Windows双系统时间不同步:它自己好了。。。
BUG:安装NVIDIA显卡驱动总是失败(GTX 1650)

## 1.搜狗中文输入法

  >* (1)install sougou package(sogoupinyin_2.2.0.0108_amd64.deb);
  >* (2)打开System Setting,找到Language Support，将键盘输入法系统由默认的iBus改成fcitx;
  >* (3)reboot;
  >* (4)点击右上角输入法小图标,选择config，去掉勾，点击左下角小加号，找到Sogou Pinyin添加即可
  >* 若在菜单栏的右上角出现两个输入法图标，则：$sudo apt-get remove fcitx-ui-qimpanel;再重启
  >* 搜狗拼音的简体、繁体相互转换：Ctrl+Shift+F

## 2.system monitor

  >* $sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor
  >* $sudo apt-get update
  >* $sudo apt-get install indicator-sysmonitor
启动，右击设置"Run on startup"

## 3.Shadowsocks

  >* $sudo add-apt-repository ppa:hzwhuang/ss-qt5 
  >* $sudo apt-get update 
  >* $sudo apt-get install shadowsocks-qt5
  >* 在Startup Application里面设置shadowsocks开机自动启动。

## 4.Google Chrome

  >* install Chrome using package(google-chrome-stable_current_amd64.deb);
  >* then, remove "http://dl.google.com/linux/chrome/deb/" from the software source!
  >* 打开shadowsocks,设置全局代理；然后，登录google账号，更新Chrome的设置(这个需要一些时间，约10分钟)。
  >* 插件1：Adblock Plus的设置
  >* 插件2：SwitchyOmega auto-switch的设置：github上已经保存了设置文件

## 5.更换Terminal(optional)

终端采用zsh和oh-my-zsh，既美观又简单易用，主要是能提高你的逼格！！！

  >* 首先，安装zsh：

    >>* sudo apt-get install zsh

  >* 安装oh-my-zsh 项目来帮我们配置 zsh，采用wget安装：

    >* wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

  >* 将终端由bash换成zsh

      >>* 在~/.bashrc的最后一行写上zsh（同理，如果要从zsh切换到bash,就在~/.bashrc中去掉zsh）;source ~/.bashrc

  >* 修改透明度为10%

  >* 命令行自动提示：没多大用，用Tab即可

## 6.好看的主题(optional)

  >* 先安装unity-tweak-tool

    >>* sudo apt-get install unity-tweak-tool

  >* 安装Flatabulous主题

    >>* sudo add-apt-repository ppa:noobslab/themes
    >>* sudo apt-get update
    >>* sudo apt-get install flatabulous-theme

  >* 安装扁平化的图标

    >>* sudo add-apt-repository ppa:noobslab/icons
    >>* sudo apt-get update
    >>* sudo apt-get install ultra-flat-icons

  >* 安装完成后，打开unity-tweak-tool软件.修改主题为Flatabulous,修改图标为Ultra-flat

## 7.安装docky(optional)

  >* sudo apt-get install docky
  >* 卸载：sudo apt-get remove docky
  >* [参考博客](https://www.jianshu.com/p/4bd2d9b1af41)
  >* 小技巧：如果想要删除docky上的图标(除了docky本身)，直接用鼠标将其拖出即可；此外，如果要将一个图标添加到docky,只需要打开这个软件，再在docky上右击，选择"pin to dock"
  >* 也可以使用cairo-dock，会玩的话, 足以媲美macOS自带的dock.缺点是不能正常显示一些软件的图标，如VSCode,需要手动设置

## 8.安装WPS(optional)

  >* 直接从官网下载安装包安装
  >* 解决缺少字体问题：[参考博客](https://blog.csdn.net/com_stu_zhang/article/details/81285794)
  >* 字体文件已备份

## 9.清理不必要的软件

  >* sudo apt-get update 
  >* sudo apt-get upgrade
  >* sudo apt-get remove firefox
  >* sudo apt-get remove libreoffice-common 
  >* sudo apt-get remove unity-webapps-common 
  >* sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot 
  >* sudo apt-get remove gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install
  >* sudo apt-get remove onboard deja-dup 

## 10.VScode

  >* 直接从官网下载安装

## 11.ROS-kinetic

  >* 先设置国内的镜像源
  >* $sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
  >* $wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -
  >* 然后继续按官网(www.ros.org)的安装教程继续安装。
  >* 再测试turtlesim和Gazebo是否能正常工作。
  >* 解决gazebo打不开world的解决方法：

  ```bash
  cd ~/.gazebo/
  mkdir -p models
  cd ~/.gazebo/models/
  wget http://file.ncnynl.com/ros/gazebo_models.txt
  wget -i gazebo_models.txt（下载这些模型大概需要1小时！为了节省时间，已经备份模型了）
  ls model.tar.g* | xargs -n1 tar xzvf
  ```

## 12.其他

  >* ubuntu下好用的视频播放器：smplayer
  >* PDF工具：mendeley, foxit reader
  >* 在ubuntu16.04上，NVIDIA GTX 1650显卡驱动安装失败。但根据网上的说过，GTX 1650显卡驱动安装成功的案例：ubuntu18.04 + nvidia430.09. 之后有需要可以试试
  >* how to solve the problem "Error in REST request" in Gazebo in Ubuntu18.04? Answer: change ~/.ignition/fuel/config.yaml as following.
    url: https://api.ignitionfuel.org
to
    url: https://api.ignitionrobotics.org
  >* Chinese input for Ubuntu18.04: 1.在setting里面点"install/Remove Languages", 点击安装Chinese; 2. 重启; 3.添加"Chinese(Intelligent Pinyin). win+空格切换输入法



