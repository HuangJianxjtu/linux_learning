# Ubuntu20.04重装系统后的软件安装指南

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

## 中文输入法
按照sougou for linux的官方教程安装即可

## 科学上网
>* 在software中安装qv2ray，并设置开机自动重启、自动连接；开机自动重启可能失效，此时需要手动添加到startup application中
>* 从github下载v2ray core到qv2ray的默认路径，并通过检查。
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

git clone https://gitee.com/han8gui/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
再将~/.zshrc中的`plugins=(git)`改为`plugins=(git zsh-syntax-highlighting zsh-autosuggestions)`, 最后运行`source ~/.zshrc`

>* 修改透明度为10%

>* 分屏工具teminator, `sudo apt install terminator`；在preference->profiles->general中，设置不显示titlebar；unfocused terminal font brightness设置成80%.


## 设置git
参考廖雪峰的[网站](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)

## 日常工具
>* 下载工具transmission, ubuntu20.04自带
>* 媒体播放器：Ubuntu自带或VLC
>* VScode:登录账号后，自动同步配置
>* [ros清华源_Ubuntu20.04下安装ROS](https://blog.csdn.net/weixin_42525601/article/details/112198438)
>* 安装CLion, 注意安装路径,[参考](https://blog.csdn.net/feimeng116/article/details/105898892); 使用CLion进行ROS编程，[参考](https://github.com/HuangJianxjtu/ros_learning/blob/master/env_setup/ros_IDE_env_setup.md)
>* 安装PyCharm, 注意安装路径，参考CLion的安装；使用PyCharm进行ROS编程也参考CLion中的教程。但要注意解决[解决pycharm无法导入rospy](https://blog.csdn.net/weixin_44481159/article/details/112583202)，这篇文章有一处小毛病，就是noetic默认使用python3编程，找到相应文件路径即可。
>* [ubunntu 20.04 mendeley的安装与卸载](https://blog.csdn.net/qq_33804792/article/details/117708336)