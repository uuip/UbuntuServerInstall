网上各种blog互相复制，没tm几个对的。
以下内容均经过验证。

 1. 使用UltraISO制作iso的U盘启动时\pool\main\l、m文件夹中部分文件的文件名过长被截断，导致安装时出错。网上好多这样的问题， 这个问题应当是UltraISO设计时的。具体参看。后面完善。
  
      推荐使用Rufus。
  
 2. 如果安装过程中提示挂载cd-rom时，alt+F2，umount /media,然后alt+f1返回重试。  
 这个是安装内核的bug。后面列出我怎么修改的。待完善。


- 自动化：  
isolinux/txt.cfg[正常安装]  
isolinux/adtxt.cfg[恢复模式]  
language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us isolinux/isolinux.cfg
timeout 30 3秒后自动执行，若为0则等待输入  
\# ui gfxboot bootlogo

- 串口：  
isolinux/txt.cfg[正常安装]  
isolinux/adtxt.cfg[恢复模式]   
    console=ttyS0,115200n8   
isolinux/isolinux.cfg   
serial 0 115200

- EFI自动化：  
boot/grub/grub.cfg set timeout=1 set default=0 language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us 
- EFI串口：     
console=ttyS0,115200n8
