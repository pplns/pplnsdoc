树莓派系统安装说明
===
***Author***: Neal Zhang

***Email***: Loong@pplns.com

===
目录
----
*   [准备工作] (#准备工作)
*   [安装步骤] (#安装步骤)
*   [登录系统] (#登录系统)

准备工作
----
    1.硬件准备：树莓派3、32G内存卡、读卡器、电源线、电源插头。

    2.软件准备：Win32DiskImager-0.9.5-install.exe、2016-09-23-raspbian-jessie.img

    3.下载链接：

    https://www.raspberrypi.org/downloads/raspbian/

    https://sourceforge.net/projects/win32diskimager/

安装步骤
----
    1.在常用安装软件目录下，新建目录ImageWriter,双击Win32DiskImager-0.9.5-install.exe,将Win32DiskImager安装到ImageWriter目录。

    2.打开Win32DiskImager安装目录，双击Win32DiskImager.exe或双击桌面快捷方式Win32DiskImager，打开软件，将2016-09-23-raspbian-jessie.img镜像按路径导入，单击Write,等待写入完毕。

    3.待出现Write Successful弹框，证明写入镜像成功，至此树莓派系统安装完毕。

登录系统
----
    1.将内存卡从电脑正确弹出，安装至树莓派。

    2.将网线插入树莓派，使个人PC和树莓派处于同一局域网之内。

    3.将树莓派连接显示器，待系统首次登录自动初始化完毕后，自动进入系统桌面。

    4.打开Shell操作界面，输入ifconfig查看此时树莓派IP。

    5.使用ssh远程登录工具putty或xshell登录至树莓派即可在个人PC端操作树莓派。

    6.树莓派系统默认用户名和密码分别是：pi和raspberry。    
    
