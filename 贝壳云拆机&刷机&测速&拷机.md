# BeikeYun
一、贝壳云基本情况
贝壳云P1是一款矿难机，配置如下：  
RK3328 四核A53 1.5Ghz  
1G DDR3  
8G emmc  
USB3.0 x4 (GL3520)  
RTL8211F 千兆phy  
HDMI  
  
二、如何拆机并看贝壳云版本
贝壳云拆机很简单：底座只有三个螺丝，用十字螺丝刀拆了之后，用平口螺丝刀撬开即可，大力出奇迹！拆开外壳后将主板1个螺丝下了之后就可以看到主板背面的标签，A3或A4版本一目了然，也可以在包装盒上背面查看，A3版本一般不带电阻无需拆解（也有带电阻的，这种情况下可以先刷机后测试U口速度，如果速度达不到100M以上，再拆电阻不迟），其他版本有可能要把每个USB附近的6个电阻全部干掉(合计24个电阻)  
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/查看贝壳云版本号.jpg)   
   
三、如何拆掉电阻（此步骤在刷机后测试U口速度之后做，经过部分玩友测试，有些不需要拆解电阻也可以达到3.0的传输速度）
参考下面的图1和图2拆掉对应电阻即可，建议用镊子翘掉即可（不建议用烙铁），注意不要翘掉其他电器元件，一个比较可行的方式：你使劲的方向要除了这个电阻，不要有其他的原件，以防止误伤友军
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/贝壳云拆电阻示意图1.jpg)  
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/贝壳云拆电阻示意图2.jpg)   
  
四、如何刷机
刷机的时候先焊接USB 3根线，红线不用焊接，如果焊接了，插上USB就会开机，那么在刷机的时候就需要先短接maskrom点，然后再插入USB，再插入电源，否则刷机软件一直会显示发现一个adb设备，也就是没有进入maskrom，而是进入了Android系统。
四根线的红色为VCC 白色为DATA- 绿色为DATA+ 黑色为GND  
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/贝壳云刷机焊点示意图.jpg)   
具体刷机步骤参考：https://www.jianshu.com/p/21d3954231dc （配置、版本、需要的文件以及焊接线序等都有介绍，很详细，本文不在赘述，本文主要总结一下刷机过程以及拷机过程遇到的一些问题）  
灯大的软件下载网址：http://rom.nanodm.net/beikeyun/tool/win/ 刷机基本工具包含AndroidTool及DriverAssitant   
  
五、贝壳云USB速度测试：.
首先关机状态下插上移动硬盘（建议用固态或者好点的机械硬盘，如果硬盘用了几十年，本身传输速度就不行了，会影响测试结果的判断）    
查看贝壳云IP地址：做好如上步骤后，贝壳云上电，有显示器可以插上显示器（插入显示器后，输入用户名密码默认用户名为root，密码为1234，首次登录会提示要修改密码，修改即可，修改后使用ifconfig或者ip address查看IP地址），如果没有显示器，可以在路由器中查看IP地址  
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/Linux查看IP地址命令.jpg)  
  
然后用putty.exe登录贝壳云  
输入lsblk或者fdisk -l或者cd /dev 然后ls -l查看当前硬盘是sd几，一般是sda 
然后格式化硬盘为ext4格式（这种格式测试比较准确，ntfs格式不是内核模式的传输，而是经过转换的，因此速度不准，貌似灯大就这么说的）  
输入如下命令格式化：  
mkfs.ext4 /dev/sda  
在本地路径下建立一个文件夹，用于映射磁盘   
mkdir /media/ceshi   
挂载硬盘   
mount -t ext4 /dev/sda /media/ceshi    
挂在硬盘后一定要进入挂载的文件夹再测试速度，否则你是在测试贝壳云emmc的速度！  
cd /media/ceshi
输入命令测试速度  
time dd bs=1M count=1024 if=/dev/zero of=test conv=fsync  

如下图：
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/测试贝壳云U口速度.jpg)  
六：贝壳云拷机测试  
拷机需要先安装dockerCE，先看群作业里的docker安装方式  
主要在Debian 安装 Docker CE这一章  
我直接使用了使用脚本自动安装  
使用脚本自动安装  
在测试或开发环境中 Docker 官方为了简化安装流程，提供了一套便捷的安装脚本，Debian 系统上可以使用这套脚本安装：  
$ curl -fsSL get.docker.com -o get-docker.sh  
$ sudo sh get-docker.sh --mirror Aliyun  
执行这个命令后，脚本就会自动的将一切准备工作做好，并且把 Docker CE 的Edge 版本安装在系统中。  
启动 Docker CE  
$ sudo systemctl enable docker  
$ sudo systemctl start docker  
Debian 7 Wheezy 请使用以下命令启动  
$ sudo service docker start  
建立 docker 用户组  
默认情况下， docker 命令会使用 Unix socket 与 Docker 引擎通讯。而只有root 用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 root 用户。因此，更好地做法是将需要使用 docker 的用户加入 docker 用户组。  
建立 docker 组：  
$ sudo groupadd docker  
将当前用户加入 docker 组：  
$ sudo usermod -aG docker $USER  
退出当前终端并重新登录，进行如下测试。  
测试 Docker 是否安装正确   
$ docker run hello-world  
Unable to find image 'hello-world:latest' locally  
latest: Pulling from library/hello-world  
d1725b59e92d: Pull complete  
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest  
Hello from Docker!  
This message shows that your installation appears to be working correctly.  
To generate this message, Docker took the following steps:  
1. The Docker client contacted the Docker daemon.  
2. The Docker daemon pulled the "hello-world" image from the Docker Hub.(amd64)  
3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently readin
g.  
4. The Docker daemon streamed that output to the Docker client,which sent it to your terminal. To try something more ambitious, you can run an Ubuntu container with:  
$ docker run -it ubuntu bash  
Share images, automate workflows, and more with a free Docker ID:https://hub.docker.com/  
For more examples and ideas, visit:  
https://docs.docker.com/get-started/  
若能正常输出以上信息，则说明安装成功。  

拷机命令：       
想要：Load average 20输入如下命令  
docker run --rm 80x86/cpuminer-multi:latest cpuminer -t20 --benchmark -a cryptolight    
想要：Load average 80输入如下命令  
docker run --rm 80x86/cpuminer-multi:latest cpuminer -t80 --benchmark -a cryptolight    
然后新开一个ssh窗口连接贝壳云，输入htop查看cpu温度      
![Image](https://github.com/GokuSun/BeikeYun/blob/master/images/Loadaverage20测试界面.jpg)  



