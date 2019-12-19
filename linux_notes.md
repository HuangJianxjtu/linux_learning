# Linux Notes

## 1.远程连接

### 1.1 命令行连接 -- SSH

* server端--局域网或在有固定IP的服务器

    server即我们要远程控制的电脑。确认开启ssh server服务：
    `$service sshd status`

    若显示`Unit sshd.service could not be found`, 则需安装ssh-server:

    ```bash
    sudo apt-get update
    sudo apt-get install openssh-server
    sudo service ssh restart    # 重启ssh服务
    ```

    能够ping通server的IP则说明ssh service正常

* server端--无固定IP的外网连接（内网穿透）

  * 使用[ngrok](https://dashboard.ngrok.com/get-started),用github账号登陆，可以得到random的公网地址。启动ngrok:
    `$./ngrok tcp 22`
  * 用阿里云，自己搭建ngrok服务器，[参考1](https://www.zhihu.com/question/27771692),[参考2](https://www.jianshu.com/p/d35962b0dba4)

* client端

    `ssh USER@_IP_ -p [port]`

    在linux中，如果不写端口号，则默认是22

### 1.2 图形界面连接

推荐使用Teamviewer,不推荐VNC

使用`ssh -X`进行图形界面传输，待尝试

## 2.电脑间文件传输

* scp

    scp -- secure cp. 传输更加安全。适合单个文件快捷传输。 [command](https://www.runoob.com/linux/linux-comm-scp.html):

    `scp [可选参数] file_source file_target`

    example:

    ```bash
    scp root@www.runoob.com:/home/root/others/music /home/space/music/1.mp3
    #scp 命令使用端口号 4588
    scp -P 4588 remote@www.runoob.com:/usr/local/sin.sh /home/administrator
    ```

* ftp

传输数据快，适合大文件传输。通常用来做网盘，[待尝试](https://www.cnblogs.com/hexige/p/7809481.html)


sudo apt install /path/to/package/name.deb

默认：
对整个屏幕截图： PrintScreen
对活动窗口截图： Alt+PrintScreen
对任意矩形截图： Shift+PrintScreen
以上三个快捷键再加上Ctrl，就会默认复制截图到粘贴板