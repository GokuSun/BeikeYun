# BeikeYun
资料来源：https://www.jianshu.com/p/21d3954231dc （配置、版本、以及焊接线序等都有介绍，很详细，本文不在赘述，本文主要总结一下刷机过程以及拷机过程遇到的一些问题）  

1:如何看贝壳云版本
拆机（只有三个螺丝，拆了之后，用平口螺丝刀撬开即可，大力出奇迹），拆开外壳后将主板1个螺丝下了之后就可以看到主板背面的标签，A3或A4版本一目了然，A3版本不带电阻无需拆解，其他版本要把usb附近的6个电阻干掉  

2：刷机  
刷机的时候焊接usb3根线，红线不用焊接，如果焊接了，插上usb就会开机  
那么在刷机的时候就需要先短接maskrom点，然后再插入usb，然后再插入电源  
否则刷机软件一直会显示发现一个adb设备，也就是没有进入maskrom，而是进入了系统  
四根线的红色为VCC 白色为DATA- 绿色为DATA+ 黑色为GND  

贝壳云速度测试命令，需先挂在硬盘，然后进入挂在的分区再输入命令  
time dd bs=1M count=1024 if=/dev/zero of=test conv=fsync  

3：拷机  
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

拷机命令  
docker run --rm 80x86/cpuminer-multi:latest cpuminer -t20 --benchmark -a cryptolight  
