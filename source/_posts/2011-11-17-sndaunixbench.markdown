---
layout: post
title: "盛大云主机的unixbench测试报告"
date: 2011-11-17 07:59
comments: true
categories: tech
---
![banner](/images/pics/P2iSG.jpg "banner")

<!--more-->

盛大云主机，默认送了100块钱，选了最小配置的那个，40几块包月，以为能用2个月，结果发现IP地址每天要收1块钱，坑爹啊！难道还有人用vps不用公网IP的吗？？？！！！

用unixbench跑了一把，结果大家看吧！

```

#    #  #    #  #  #    #          #####   ######  #    #   ####   #    #
#    #  ##   #  #   #  #           #    #  #       ##   #  #    #  #    #
#    #  # #  #  #    ##            #####   #####   # #  #  #       ######
#    #  #  # #  #    ##            #    #  #       #  # #  #       #    #
#    #  #   ##  #   #  #           #    #  #       #   ##  #    #  #    #
 ####   #    #  #  #    #          #####   ######  #    #   ####   #    #

Version 5.1.3                      Based on the Byte Magazine Unix Benchmark

Multi-CPU version                  Version 5 revisions by Ian Smith,
Sunnyvale, CA, USA
January 13, 2011                   johantheghost at yahoo period com

1 x Dhrystone 2 using register variables  1 2 3 4 5 6 7 8 9 10

1 x Double-Precision Whetstone  1 2 3 4 5 6 7 8 9 10

1 x Execl Throughput  1 2 3

1 x File Copy 1024 bufsize 2000 maxblocks  1 2 3

1 x File Copy 256 bufsize 500 maxblocks  1 2 3

1 x File Copy 4096 bufsize 8000 maxblocks  1 2 3

1 x Pipe Throughput  1 2 3 4 5 6 7 8 9 10

1 x Pipe-based Context Switching  1 2 3 4 5 6 7 8 9 10

1 x Process Creation  1 2 3

1 x System Call Overhead  1 2 3 4 5 6 7 8 9 10

1 x Shell Scripts (1 concurrent)  1 2 3

1 x Shell Scripts (8 concurrent)  1 2 3

========================================================================
BYTE UNIX Benchmarks (Version 5.1.3)

System: localhost.localdomain: GNU/Linux
OS: GNU/Linux — 2.6.18-274.el5xen — #1 SMP Fri Jul 22 06:14:51 EDT 2011
Machine: i686 (i386)
Language: en_US.utf8 (charmap=”UTF-8″, collate=”UTF-8″)
CPU 0: AMD Opteron(tm) Processor 6172 (5253.6 bogomips)
Hyper-Threading, MMX, AMD MMX, Physical Address Ext, SYSCALL/SYSRET
17:54:15 up 2 days, 23:48,  1 user,  load average: 0.02, 0.01, 0.00; runlevel 3

————————————————————————
Benchmark Run: 四 11月 17 2011 17:54:15 – 18:22:09
1 CPU in system; running 1 parallel copy of tests

Dhrystone 2 using register variables        6316364.6 lps   (10.0 s, 7 samples)
Double-Precision Whetstone                     1561.1 MWIPS (10.0 s, 7 samples)
Execl Throughput                               1730.2 lps   (30.0 s, 2 samples)
File Copy 1024 bufsize 2000 maxblocks        396524.4 KBps  (30.0 s, 2 samples)
File Copy 256 bufsize 500 maxblocks          124786.0 KBps  (30.0 s, 2 samples)
File Copy 4096 bufsize 8000 maxblocks        728175.7 KBps  (30.0 s, 2 samples)
Pipe Throughput                              800622.9 lps   (10.0 s, 7 samples)
Pipe-based Context Switching                  90301.6 lps   (10.0 s, 7 samples)
Process Creation                               2931.9 lps   (30.0 s, 2 samples)
Shell Scripts (1 concurrent)                   2687.7 lpm   (60.0 s, 2 samples)
Shell Scripts (8 concurrent)                    370.7 lpm   (60.1 s, 2 samples)
System Call Overhead                         721836.9 lps   (10.0 s, 7 samples)

System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0    6316364.6    541.2
Double-Precision Whetstone                       55.0       1561.1    283.8
Execl Throughput                                 43.0       1730.2    402.4
File Copy 1024 bufsize 2000 maxblocks          3960.0     396524.4   1001.3
File Copy 256 bufsize 500 maxblocks            1655.0     124786.0    754.0
File Copy 4096 bufsize 8000 maxblocks          5800.0     728175.7   1255.5
Pipe Throughput                               12440.0     800622.9    643.6
Pipe-based Context Switching                   4000.0      90301.6    225.8
Process Creation                                126.0       2931.9    232.7
Shell Scripts (1 concurrent)                     42.4       2687.7    633.9
Shell Scripts (8 concurrent)                      6.0        370.7    617.9
System Call Overhead                          15000.0     721836.9    481.2
========
System Benchmarks Index Score                                         518.0

```

######EOF
