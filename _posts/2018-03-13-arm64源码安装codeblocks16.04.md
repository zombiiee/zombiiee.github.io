# ARM64位系统下源码安装codeblocks16.01

最近公司买了一大批Nvidia Jetson TX1和Nvidia Jetson TX2，安装了ubuntu16.04的系统后，发现安装codeblocks时只能安装13.12的版本，不能安装16.01版本，但是13.12版本各种bug，闪退等等的。迫于无奈，最后只能使用源码的形式安装。

[安装包下载地址 密码：51pf](https://pan.baidu.com/s/1a1aEEkEAdo6TuvuRIN7R7w)

## 安装步骤

由于直接源码安装codeblocks16.01后不能通过双击cpb文件打开工程，所以在这里我们需要先安装codeblocks13.12。

```
sudo add-apt-repository ppa:damien-moore/codeblocks-stable 
sudo apt-get update  
sudo apt-get install codeblocks  
```

此时在终端执行codeblocks命令你会发现打开的codeblocks是13.12版本的。

接下来在Home目录下新建devel目录，然后把之前下载的两个压缩包拷贝到devel目录下并解压，如图：

![](http://p5itp74xg.bkt.clouddn.com/image/arm64_codeblocks16.04/07.png)

### 安装wxGTK库

在devel目录打开终端，依次执行以下命令：

```
sudo apt-get install libgtk2.0-dev 
```
![](http://p5itp74xg.bkt.clouddn.com/image/arm64_codeblocks16.04/wxGTK_install01.png)
```
cd wxGTK-2.8.12/
sudo cp /usr/share/misc/config.{sub,guess} .
mkdir build_gtk2_shared_monolithic_unicode
cd build_gtk2_shared_monolithic_unicode 
../configure --prefix=/opt/wx/2.8--enable-xrc--enable-monolithic --enable-unicode
sudo make -j4
sudo make install
cd..
sudo cp wxwin.m4 /usr/share/aclocal
```



### 安装codeblocks16.04

返回到devel目录下，依次执行以下命令：

```
cd codeblocks-16.01.release
./bootstrap 
```
![](http://p5itp74xg.bkt.clouddn.com/image/arm64_codeblocks16.04/codeblock_install01.png)
```
cd /opt/wx
sudo cp -r 2.8--enable-xrc--enable-monolithic/ 2.8
# 切换到codeblocks-16.01.release目录
./configure -with-wx-config=/opt/wx/2.8/bin/wx-config
```
![](http://p5itp74xg.bkt.clouddn.com/image/arm64_codeblocks16.04/codeblock_install02.png)
```
sudo make -j4 （这步会比较耗时）
sudo make install
sudo gedit ~/.bashrc (在最后一行添加：export LD_LIBRARY_PATH=/opt/wx/2.8/lib)
```
![](http://p5itp74xg.bkt.clouddn.com/image/arm64_codeblocks16.04/codeblock_install03.png)
```
source ~/.bashrc
sudo gedit /etc/profile (在最后添加：export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/wx/2.8/lib)
```
![](http://p5itp74xg.bkt.clouddn.com/image/arm64_codeblocks16.04/codeblock_install04.png)
```
source /etc/profile
sudo ldconfig
```

到这一步codeblocks就安装完成了，此时在终端输入`codeblocks`你会发现启动的是16.01版本的了，此时可以在左侧任务栏右键单击codeblocks图标选择`Lock to Launcher`来把图标锁定在任务栏，以后可以通过单击任务栏中的codeblocks图标来启动codeblocks而不用每次在终端中启动。。

最后为了让双击cpb文件是启动是16.01版本而不是13.12版本的，就需要把13.12版本的codeblocks卸载掉。在终端中输入：

```
sudo apt-get autoremove codeblocks
```

接下来就大功告成了，为了避免一些小bug，比如单击任务栏图标不能启动codeblocks等，最好重启一下系统。