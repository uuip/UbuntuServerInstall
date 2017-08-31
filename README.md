1.使用光盘iso文件制作U盘启动时iso:\pool\main\l文件夹，m文件夹中部分文件的文件名过长被截断。
2.如果提示挂载cd-rom时，alt+F2，umount /media,然后返回alt+f1重试


自动化：
isolinux/txt.cfg[正常安装]
isolinux/adtxt.cfg[恢复模式]
	language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us
isolinux/isolinux.cfg
	timeout 30   3秒后自动执行，若为0则等待输入
	# ui gfxboot bootlogo

串口：
isolinux/txt.cfg[正常安装]
isolinux/adtxt.cfg[恢复模式]
	vga=normal console=ttyS0,115200n8
isolinux/isolinux.cfg
	serial 0 115200

EFI自动化：boot/grub/grub.cfg
set timeout=1
set default=0
language=en country=CN localechooser/preferred-locale=en_US.UTF-8 keymap=us
EFI串口：
vga=normal console=ttyS0,115200n8
