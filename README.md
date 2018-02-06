网上各种blog互相复制，没tm几个对的。  
以下内容均经过验证。

 1. 使用UltraISO制作iso的U盘启动时\pool\main\l、m文件夹中部分文件的文件名过长被截断，导致安装时出错。网上好多这样的问题， 这个问题应当是UltraISO设计的
 问题--当同时存在bridge，Joliet，iso9660三种文件命名时，UltraISO只能列出一种模式。  
       推荐使用Rufus。
  
 2. 如果安装过程中提示挂载cd-rom时，alt+F2，umount /media,然后alt+f1返回重试。  
 这个是安装内核的bug。
 
修改initrd.gz
cd newiso
mkdir initrd-tmp && cd initrd-tmp
解压initrd.gz
gzip -dc ../iso/install/initrd.gz | cpio -id
修改文件
vim var/lib/dpkg/info/cdrom-detect.postinst
		try_mount() {
		        local device=$1
		        local type=$2

		        local ret=1
添加这一行：	        umount /media || true
重新生成initrd.gz
find . | cpio --quiet  -o -H newc | gzip -9 > ../iso/install/initrd.gz	
懒得修改的话，就alt+F2，umount /media,然后alt+f1返回重试。

3. 特别注意分区的写法，详见seed文件。如果把分区表写进到seed文件中，在换行的\后面一定不要有任何字符，此坑踩了一下午，之后就把分区表独立成一个文件了；raid模式+lvm的分区表相信我写的是独一份。

4. 看isolinux/txt.cfg当中
 append file=/cdrom/preseed/ubuntu-server.seed initrd=/install/initrd.gz vga=normal console=ttyS1,115200n8 language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us quiet --
 为什么一定要把
language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us 
加到这里？不知道那些在preseed/ubuntu-server.seed定义语言和键盘的是怎么生效的，估计也只是转转帖子罢了。
在安装过程中在语言、键盘、国家在安装内核读取seed文件前进行设置，所以在seed文件中定义这些不会生效。debian的文档中有说。

5. 以上4点格外留意，都是我亲自验证并解决。剩余的自动化和串口设置参照文件。ttyS0 1 2？？ 这个得根据机器的实际情况设定。
6. 如果安装完卡开机画面，一般是显示设置的问题，进入恢复模式。
Edit /etc/default/grub as follows:
    GRUB_CMDLINE_LINUX_DEFULT="quiet nomodeset"【删掉splash，解决卡在显卡检测，显示器无输入】


- MBR自动化：  
isolinux/txt.cfg[正常安装]  
isolinux/adtxt.cfg[恢复模式]  
isolinux/isolinux.cfg  
timeout 30   \#3秒后自动执行，若为0则等待输入  
\# ui gfxboot bootlogo

- MBR模式串口：  
isolinux/txt.cfg[正常安装]  
isolinux/adtxt.cfg[恢复模式]    
    console=ttyS0,115200n8   
isolinux/isolinux.cfg   
serial 0 115200

- EFI自动化：  
boot/grub/grub.cfg  
 set timeout=1  
 set default=0  
 language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us 
 
- EFI串口：       
console=ttyS0,115200n8



