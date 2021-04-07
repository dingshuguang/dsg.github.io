---
title: 华为交换机清除console密码
date: 2021-4-1 13:11
tags: switch
top_img: https://res.cloudinary.com/ddsg/image/upload/c_scale,w_1920/v1593394961/taylor-vick-M5tzZtFCOfs-unsplash_rpv2ax.jpg
cover: https://res.cloudinary.com/ddsg/image/upload/c_scale,w_1920/v1593394961/taylor-vick-M5tzZtFCOfs-unsplash_rpv2ax.jpg
---

文章来源华为官网，记录在这，方便查看。
## 恢复Console口密码

设备提供如下方法恢复Console口密码。
方法一：通过STelnet/Telnet登录设备修改Console口密码。
方法二：在BootROM下配置清除Console口密码启动后，修改Console口密码。
方法三：在BootROM菜单下取消设置下次启动配置文件，设备以空配置启动后修改Console口密码。

> 说明：
请优先使用方法一，如果STelnet/Telnet密码也忘记了再使用其它两个方法。使用Telnet协议存在安全风险，建议用户使用STelnet V2登录设备。
在方法一不可用的情况下优先使用方法二。
S1720GFR、S2720、S2750、S5700LI、S5700S-LI、S5720S-12TP-PWR-LI-AC和S5700S-28P-PWR-LI-AC为BootROM菜单，S5710-X-LI、S5700S-28X-LI-AC、S5700S-52X-LI-AC、S5720SI、S5720S-SI、S5720EI、S5720HI、S5720LI、S5720S-LI、S6720EI和S6720S-EI为BootLoad菜单。
以下设备回显仅作举例，不同设备，不同版本，回显可能会稍有不同，请以设备的实际显示为准。

### 方法一  通过STelnet/Telnet登录设备修改Console口密码

以下涉及的命令行及回显信息以STelnet登录设备修改Console口密码为例。
如果用户拥有STelnet账号，并且具有3级或更高的权限，则可以通过STelnet登录到设备后修改Console口密码，然后保存配置。

使用STelnet账号登录设备，并确认当前账号有3级或更高的权限。
使用display users命令查看当前设备所有登录用户。其中带“+”标记行为当前用户，记录对应的编号VTY1。

```
<HUAWEI> display users
  User-Intf    Delay    Type   Network Address     AuthenStatus    AuthorcmdFlag
  129 VTY 0   00:23:36  TEL    10.135.18.67              pass           no        Username : Unspecified

+ 130 VTY 1   01:20:36  TEL    10.135.18.91              pass           no        Username : Unspecified

  131 VTY 2   00:00:00  TEL    10.135.18.54              pass           no        Username : Unspecified
```

使用display user-interface命令可以显示所有用户的权限，确定VTY1对应的等级为15，有权限修改Console口密码。

```
<HUAWEI> display user-interface
  Idx  Type     Tx/Rx      Modem Privi ActualPrivi Auth  Int
  0    CON 0    9600       -     15    -           P     -
+ 129  VTY 0               -     15    15          P     -
+ 130  VTY 1               -     15    15          P     -
+ 131  VTY 2               -     15    -           P     -
  132  VTY 3               -     15    15          P     -
......
```

修改Console用户的密码，以修改为密码认证，密码为“huawei@123”为例。

```
<HUAWEI> system-view
[HUAWEI] user-interface console 0
[HUAWEI-ui-console0] authentication-mode password
[HUAWEI-ui-console0] set authentication password cipher huawei@123
[HUAWEI-ui-console0] return
```

为了防止重启后配置丢失，保存配置。

### 方法二  在BootROM下清除Console口密码登录后，修改Console口密码

设备的BootROM提供了清除Console口密码的功能，可以在用户使用Console口登录的时候跳过密码检查。这样系统启动后除了不需要输入Console密码外，与正常启动相同，也会完成所有配置加载。设备启动后重新配置验证方式和Console口密码，然后保存配置。

>注意：

要进入到BootROM菜单需要重启设备，会导致业务中断，请视具体情况做好设备备份，并尽量选择业务量较少的时间操作。
清除Console口密码登录后请马上配置新的密码。
在此操作过程中不要对设备进行下电。

用串口线连接设备，然后重启。当出现“Press Ctrl+B to enter BootROM menu...”（V200R002及V200R003版本）或者“Press Ctrl+B or Ctrl+E to enter BootROM menu ...”（V200R005及以后发布的版本）打印信息时，按下“Ctrl+B”或者“Ctrl+E”并键入密码（缺省为“Admin@huawei.com”，V100R006C03之前版本可能为“huawei”），进入BootROM主菜单。

清除Console口登录密码。

```
 BootROM  MENU

    1. Boot with default mode
    2. Enter serial submenu
    3. Enter startup submenu
    4. Enter ethernet submenu
    5. Enter filesystem submenu
    6. Modify BootROM password   //V200R006及之前版本：Modify BootROM password V200R007及之后版本：Enter password submenu
    7. Clear password for console user
    8. Reboot
    (Press Ctrl+E to enter diag menu) 

Enter your choice(1-8): 7

Note: Clear password for console user? Yes or No(Y/N): y

Clear password for console user successfully. Choose "1" to boot, then set a new password.
Note: Do not choose "8. Reboot" or power off the device, otherwise this operation will not take effect.
```

根据设备的提示，在BootROM主菜单下选择“1”启动设备。
完成系统启动后，通过Console口登录时不需要认证，登录后配置Console口密码，以修改为密码认证，并修改密码为“huawei@123”为例。

```
<HUAWEI> system-view
[HUAWEI] user-interface console 0
[HUAWEI-ui-console0] authentication-mode password
[HUAWEI-ui-console0] set authentication password cipher huawei@123
[HUAWEI-ui-console0] return
```

为了防止重启后配置丢失，保存配置。


### 方法三  在BootROM菜单下取消设置下次启动配置文件，设备以空配置启动后修改Console口密码

在BootROM下取消设置下次启动配置文件，设备会以空配置（出厂配置）启动。启动后将原配置文件导出，手动修改其中关于Console口登录的配置。将修改后的配置文件重新上传至设备，配置设备以修改后的配置文件启动，重启后的设备将无需输入Console口登录密码。（以下举例以Console口下配置了Password认证为例，其它认证方式以设备实际情况为准。）

用串口线连接设备，然后重启，当出现“Press Ctrl+B to enter BootROM menu...”（V200R002及V200R003版本）或者“Press Ctrl+B or Ctrl+E to enter BootROM menu ...”（V200R005及以后发布的版本）打印信息时，按下“Ctrl+B”或者“Ctrl+E”并键入密码（缺省为“Admin@huawei.com”，V100R006C03之前版本可能为“huawei”）后进入BootROM主菜单。

清空启动配置文件，使设备以空配置启动。

```
 BootROM  MENU

    1. Boot with default mode
    2. Enter serial submenu
    3. Enter startup submenu
    4. Enter ethernet submenu
    5. Enter filesystem submenu
    6. Modify BootROM password   //V200R006及之前版本：Modify BootROM password V200R007及之后版本：Enter password submenu
    7. Clear password for console user
    8. Reboot
    (Press Ctrl+E to enter diag menu) 

Enter your choice(1-8): 3

       Startup Configuration Submenu

    1. Display startup configuration
    2. Modify startup configuration
    3. Return to main menu


Enter your choice(1-3): 2

Note: startup file field can not be cleared
'.'=clear field; =quit; Enter=use current configuration

startup type(1: Flash)
  current: 1
  new    :

Flash startup file (can not be cleared)
  current: HUAWEI-v200r008c00.cc
  new    :

saved-configuration file
  current: vrpcfg.zip
  new    : .          //清空当前值

patch package
  current:
  new    :

       Startup Configuration Submenu

    1. Display startup configuration
    2. Modify startup configuration
    3. Return to main menu

Enter your choice(1-3): 3  
```

在BootROM主菜单下选择“1”启动设备。
完成系统启动后，设备会恢复为出厂配置。V200R009及之前版本通过Console口登录时，提示设置Console口密码，以密码为“huawei@123”为例。

```
An initial password is required for the first login via the console.
Continue to set it? [Y/N]:y
Set a password and keep it safe. Otherwise you will not be able to login via the console.

Please configure the login password (8-16)
Enter Password:     //输入huawei@123
Confirm Password:     //再次输入huawei@123
```

V200R010及之后版本通过Console口登录时，提示输入Console口的默认用户名和密码，之后会提示修改密码，此时必须修改密码，以密码为“huawei@123”为例。

```
Login authentication                                                            
                                                                                
                                                                           
Username:admin                                                                  
Password:     //输入admin@huawei.com                                                                       
Warning: The default password poses security risks.                             
The password needs to be changed. Change now? [Y/N]: y                          
Please enter old password:     //输入admin@huawei.com                                                      
Please enter new password:     //输入huawei@123                                                      
Please confirm new password:     //再次输入huawei@123                                                    
The password has been changed successfully. 
```

恢复原配置。当前设备为默认的出厂配置，若希望恢复设备原配置，又不想保留原配置文件中关于Console密码的配置，可以下载原配置文件到PC上，手动删除Console下配置后，上传至设备，指定为下次启动文件，重启设备。

配置设备为FTP服务器。
```
<HUAWEI> system-view
[HUAWEI] ftp server enable
Info: The FTP server is already enabled.
[HUAWEI] vlan 10
[HUAWEI-vlan10]  interface vlanif 10   //配置VLANIF10作为管理接口。
[HUAWEI-Vlanif10] ip address 10.110.24.254 24
[HUAWEI-Vlanif10] quit
[HUAWEI] interface gigabitethernet 0/0/10   //GE0/0/10为使用Web网管登录Switch的PC与Switch相连的物理接口编号，请按照实际现网情况进行选择。
[HUAWEI-GigabitEthernet0/0/10] port link-type access
[HUAWEI-GigabitEthernet0/0/10] port default vlan 10
[HUAWEI-GigabitEthernet0/0/10] quit
[HUAWEI] aaa
[HUAWEI-aaa] local-user huawei password irreversible-cipher huawei@123
[HUAWEI-aaa] local-user huawei ftp-directory flash:
[HUAWEI-aaa] local-user huawei service-type ftp
[HUAWEI-aaa] local-user huawei privilege level 15
```
下载原配置文件vrpcfg.zip到PC。
```
C:\Documents and Setting\Administrator> ftp 10.110.24.254
连接到 10.110.24.254。
220 FTP service ready.
用户 (10.110.24.254:(none)): huawei
331 Password required for huawei.
密码:
230 User logged in.
ftp> get vrpcfg.zip
200 Port command okay.
150 Opening ASCII mode data connection for directory list.
226 Transfer complete.
ftp: 收到 981 字节，用时 0.01秒 981000.00千字节/秒.
```
在PC上解压后使用文本编辑工具（建议用系统自带的文本编辑工具）打开并删除Console口认证配置，删除后重新压缩成vrpcfg.zip文件。需删除的配置如下所示：
```
#
user-interface maximum-vty 15
user-interface con 0
 authentication-mode password         //需手动删除
 set authentication password cipher %@%@:*IB+w7j~""GlU$0-;\#m@Jw%@%@      //需手动删除
#
user-interface con 0
 authentication-mode aaa     //需手动删除
 user privilege level 15     //需手动删除
```

将修改后的配置文件保存后，上传到设备，替换原配置文件。
```
ftp> put vrpcfg.zip
200 Port command okay.
150 Opening ASCII mode data connection for directory list.
226 Transfer complete.
ftp: 发送 981 字节，用时 0.00Seconds 978000.00千字节/秒.
```
设置修改后的配置文件为下一次启动配置文件，选择不保存配置重启设备。
```
<HUAWEI> startup saved-configuration vrpcfg.zip
Info: Succeeded in setting the configuration for booting system.
<HUAWEI> reboot fast
System will reboot! Continue ? [y/n]:y
```
重启后，会再次提醒设置Console登录密码，输入一个安全并方便自己记忆的密码后，回车，进入命令行界面。
