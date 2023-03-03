# VSCode 配置

主要配置C++、Python的ROS开发环境

* 如何快速打开VSCode的命令面板(Command Palette): Crtl+Shift+P

## 1.VSCode的配置

主要分为全局配置和工作空间配置。全局配置存储在`.config/Code/User/settings.json`， 该文件会被自动同步到VSCode服务器。工作空间配置存储在每个工作空间中的.vscode文件夹中，这个文件夹不会自动同步。

>* 如何同步VSCode配置：[VSCode同步设置插件-Settings Sync](https://blog.csdn.net/l1724979351/article/details/119039113)

## 2.工作空间配置

主要包含一下三个文件。只要配置c_cpp_properties.json即可，然后用CMake插件调试（Crtl + F5）。 不用去配置繁琐的launch.json和task.json. [有用的官方教程！](https://github.com/microsoft/vscode-cmake-tools/blob/main/docs/how-to.md#create-a-new-project)；[参考1](https://www.guyuehome.com/20977)
* c_cpp_properties.json: C++配置文件
* launch.json: 启动代码调试相关的配置文件
* tasks.json: 自定义任务列表

## 3.测试用例

此目录下已经包含了普通c++工程和c++ ros工程，能运行、调试这两个工程说明配置完成了。注意，ros工程下面的.vscode/c_cpp_properties.json已经配好，能找到ros头文件，可用作模板。

## 4. C++环境配置（代码提示、补全、变量查找等）

* 所需插件：C/C++
* 需要在.vscode文件夹下生成**正确的**c_cpp_properties.json, 才能实现C++代码提示、补全、变量查找等功能。
* **生成默认c_cpp_properties.json文件的方法**：在vscode命令面板中找到`C/C++: Edit configurations (UI)`. 此时其中的includePath只有${default}，即**用户在全局settings.json定义的C_Cpp.default.includePath路径**。根据需要稍微修改c_cpp_properties.json，参考**测试用例**
* **在全局settings.json定义的C_Cpp.default.includePath路径**：C_Cpp.default.compilerPath和C_Cpp.default.includePath.  [参考：在全局设置中设定c++默认头文件，免得每个工程配置一遍](https://blog.csdn.net/wbvalid/article/details/115001149). 常用的includePath如下:
 ```json
    "${workspaceFolder}/**",      //当前工作空间下头文件
    "usr/include/**",             //系统默认c++头文件，如标准库iostream
    "/usr/local/include/**"       //用户自己安装库的头文件,如opencv
    "/opt/ros/noetic/include/**"  //ROS自带的头文件(ubuntu20.04)
    "Path_to_ROS_Workspace/devel/include/**"    //自定义的ROS包
  ```
## 4. 插件

>* [sftp](https://zhuanlan.zhihu.com/p/73218983)
