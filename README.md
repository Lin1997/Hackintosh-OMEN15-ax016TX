原帖地址:http://bbs.pcbeta.com/viewthread-1758714-1-1.html

一、简介
[插图]
我之前已经在暗影2上成功安装了10.12.6，最近Apple发布了macOS High Sierra 10.13，正好赶上十一假期，就抽时间升级并写了这个帖子。为了跳过DSDT+SSDT手动修改的过程、方便新手，本帖使用了Hotpatch。
旧帖链接:http://bbs.pcbeta.com/viewthread-1729147-1-1.html

1.机器的配置如下:
型号	OMEN 15-ax016TX
CPU	i5-6300HQ
核显	HD530
独显	NVIDIA GeForce GTX 960M	(无法使用，HotPatch中已屏蔽)
内存	8G DDR4 2133
BIOS版本	F.38 Rev.A

2.目前完善情况:
已工作
CPU原生电源管理	（开启HWP，空闲0.8GHz~睿频3.2Ghz）
电池(修复电量不减的问题)
WIFI [拆机更换了BCM94352z](2.4G+5G) +蓝牙 (Airdrop、handoff未尝试)
亮度调节(快捷键正常)
睡眠/唤醒
显卡
声卡(AppleALC+CodeCommand解决唤醒无声)
摄像头
触控板
USB 2.0/3.0
有线网卡(驱动、内建)
读卡器

[插图]

现存问题
	下面是未解决的问题，大家有解决方案的话记得通知我哦。
1.HDMI输出(暗影2的HDMI口好像是通过独显的)
2.从Win10重启进macOS,否则kernel_task进程会占用大量资源，导致系统卡顿(关机后切换系统无问题)
3.屏幕亮度较低时感觉会有轻微闪烁感(错觉???):
http://bbs.pcbeta.com/viewthread-1738261-1-1.html

3.注意事项
1.自带的无线网卡无解，我拆机更换了BCM94352z.
[插图]
2.本帖附件中的X86PlatformPluginInjector.kext是针对i5-6300HQ的，其他CPU型号可以自己按下面的”完善系统”部分给出的教程自制.
3.在机械硬盘上不要用APFS格式(推荐安装到固态硬盘)，Apple官方对机械硬盘支持不佳，会导致卡顿。
4.如果想从Win10切换到macOS，请关机后切换，不要重启切换。(见上面提到的”现存问题”)
5.如果在尝试本教程前已经安装了macOS，而且发现电池电量不减，请拔掉电源适配器，长按电源键20s.
6.iMessage和FaceTime功能需要注入白果信息，请自行搜索教程。

二、安装教程
1.BIOS设置
开机后连按F10进入BIOS设置，先恢复默认设置。然后将”安全启动”关闭,将”传统模式”打开。

2.制作安装盘
1.论坛上下载带clover的原版安装镜像(.dmg格式的)，这里提供一个(感谢:…).
2.下载TransMac，打开并插入U盘(自行备份文件)，右键单击U盘 选”Format Disk For Mac”格式化U盘。然后再次右键单击U盘，选中”Restore with Disk Image”，然后选择下载好的安装镜像写入U盘.
3.打开写入镜像的U盘的EFI分区，删除所有文件，替换成下文给的EFI附件(要解压).

3.安装系统
1.关机重启，连按F9，选择从U盘启动。进入Clover引导界面后用方向键选中安装盘，然后敲击空格键，用方向键与回车选中”Verbose(-v)”。再用方向键和回车选择“Boot macOS with selected options”
2.等待进入安装界面。(如果失败，请记录下失败前最后的代码上论坛提问)
3.格式化硬盘，并将系统安装到硬盘(整个过程全中文，在这就不赘述了)

4.完善系统
1.额外kext安装：
(a) 打开Finder,菜单栏选”前往”->”前往文件夹”，输入/Library/Extensions
(b) 将附件解压的到的EFI/LE里的AppleBacklightInjector.kext和X86PlatformPluginInjector.kext复制到刚才打开的目录里。
(注:附件的X86PlatformPluginInjector.kext只适用于i5-6300HQ,其它CPU参考教程 http://bbs.pcbeta.com/viewthread-1737021-1-1.html制作并安装)
(c) 打开终端，输入命令”sudo kextcache -i /“，回车并输入密码。重启。
2.解决macOS与Win10时间不同步:
因为Windows和macOS看待CMOS记录时钟的方式不一样，两者会出现时间不同步。一般的解决方法都是修改Windows注册表:定位到HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation\，添加一个名为"RealTimeIsUniversal"的DWORD项，把值设为1
3.多指手势设置(感谢syscl):
“双指从右边缘向左滑动”和”三指下滑”快捷键设置方法:
系统偏好设置-键盘-快捷键 里: 
(a) Mission Control里双击”显示通知中心”右边的快捷键部分,然后双指从右边缘向左滑动，会自动填入快捷键; 
(b) Lanunchpad 与 Dock里双击”显示Launchpad”右边的快捷键部分,然后三指下滑，会自动填入快捷键.

三、其它
1.常用文件/工具
(a) 论坛内的Clover下载地址(不定期更新)(感谢 丶鸭梨大大。):
http://bbs.pcbeta.com/viewthread-1754489-1-1.html
(b) 常用驱动下载地址(放到EFI/Clover/Kexts/Other里):
https://bitbucket.org/RehabMan/
(c) Clover Configuator(Clover配置文件修改工具) 下载地址:
http://mackie100projects.altervista.org/download-clover-configurator/

2.常用教程
感谢各位大神的帖子和帮助！
(a) 开启完整HWP(SpeedShift)电源管理特性(感谢syscl)
http://bbs.pcbeta.com/viewthread-1737021-1-1.html
(b) [授权翻译] 使用补丁修改DSDT/SSDT [DSDT/SSDT综合教程](感谢daxuexinsheng)
http://bbs.pcbeta.com/viewthread-1571455-1-1.html
(c) 暗影精灵2修改DSDT+SSDT的过程(我的帖子)
http://bbs.pcbeta.com/viewthread-1729147-1-1.html
(d) [修改DSDT+SSDT]屏蔽双显卡笔记本的独显(感谢daxuexinsheng)
http://bbs.pcbeta.com/viewthread-1598800-1-1.html
(e) [重新整理逻辑] 电量补丁制作教程(感谢daxuexinsheng)
http://bbs.pcbeta.com/viewthread-1595139-1-1.html
(f) Lilu 相关内容说明 & 插件列表(感谢 口袋妖怪heart)
http://bbs.pcbeta.com/viewthread-1741470-1-1.html
(g) BCM94352z驱动教程(英语)
https://www.tonymacx86.com/threads/guide-airport-pcie-half-mini-v2.104850/

