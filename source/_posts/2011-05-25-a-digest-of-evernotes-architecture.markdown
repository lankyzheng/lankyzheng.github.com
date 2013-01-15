---
layout: post
title: "[中英对照]A Digest of Evernote’s Architecture：Evernote架构概要"
date: 2011-05-25 07:41
comments: true
categories: tech
---
原文地址：[http://blog.evernote.com/tech/2011/05/17/architectural-digest/](http://blog.evernote.com/tech/2011/05/17/architectural-digest/)

Let’s get things started with a coarse-grained overview of the physical makeup of the Evernote service. I won’t go into a lot of detail on each component here; we’ll aim to talk about the interesting bits in separate posts later.

咱们先从Evernote服务物理构成的初步概况开始。偶不会深入到各个部分的细节中去；偶们计划将来另开新帖来讨论有意思的要点。

<!--more-->

![image01](http://blog.evernote.com/tech/files/2011/05/evernote-highlevel-architecture1-282x300.png)

Starting at the top-left corner of the diagram, all stats as of May 17th, 2011 …

从图中左上角开始，所有状态都是2011年5月17日的。。。

**Networking:** Virtually all traffic to and from Evernote comes to www.evernote.com via HTTPS port 443. This includes all “web” activity, but also all client synchronization via our [Thrift](http://thrift.apache.org/)-based [service API](https://www.evernote.com/about/developer/api/).  Altogether, that produces up to 150 million HTTPS requests per day, with peak traffic around 250 Mbps. (Unfortunately for our semi-nocturnal Operations team, this daily peak tends to arrive around 6:30 am, Pacific time.)

**网络：** 实际上到www.evernote.com进出Evernote的所有流量都是走HTTPS的443端口的。包括了所有web活动，也包括所有通过我们基于Thrift的API的客户端同步。总共产生每天一亿五千万次HTTPS请求，峰值流量大约250Mbps。（对我们夜猫子运维团队而言，不幸的是峰值总在太平洋时间早上6:30左右出现。）

We use BGP to direct traffic through fully independent network feeds from our primary (NTT) and secondary (Level 3) providers. This is filtered through Vyatta on the way to the new A10 load balancers that we deployed in January when we hit the limit of SSL performance for our old balancers. We’re comfortably handling existing traffic using one AX 2500 plus a failover box, but we’re preparing to test their N+1 configuration in our staging cluster to prepare for future growth.

偶们使用BGP引导流量通过完全独立的由NTT和Level 3提供的网络。图中由Vyatta过滤之后到达A10负载均衡设备，这玩意偶们一月份才部署因为当时偶们达到了老负载均衡设备的SSL性能限制。目前偶们用1台AX 2500加上一个容灾设备就很淡定的处理了现存的流量，不过偶们准备在分级集群上测试一下他们的N+1配置以应对将来的扩容。

**Shards:** The core of the Evernote service is a farm of servers that we call “shards.”  Each shard handles all data and all traffic (web and API) for a cohort of 100,000 registered Evernote users.  Since we have more than 9 million users, this translates into around 90 shards.

**Shards：** 咱Evernote服务的核心是一组叫做shards的服务器群。每个shard处理以100,000注册用户为一组的所有数据和流量（web和API）。由于偶们有9百万注册用户，所以差不多有90个shards。

Physically, shards are deployed as a pair a SuperMicro boxes with two quad-core Intel processors, a ton of RAM and full chassis of Seagate enterprise drives in mirrored RAID configurations. On top of each box, we run a base Debian host that manages two Xen virtual machines. The primary VM on a box runs our core application stack:  Debian + Java 6 + Tomcat + Hibernate + Ehcache +  Stripes + GWT + MySQL (for metadata) + hierarchical local file systems (for file data).

物理上看，shards用成对的SuperMicro服务器部署，包括2个四核intel处理器、暴多的内存以及做了镜像RAID的满配的Seagate企业级硬盘。每个服务器上，偶们在Debian基础上开2个Xen虚拟机。第一个主虚拟机跑一堆核心应用：Debian + Java 6 + Tomcat + Hibernate + Ehcache + Stripes + GWT + MySQL (用于metadata) + 分层的本地文件系统 (用户文件数据).

All user data on the primary VM on one box is kept synchronously replicated to a secondary VM on a different box using [DRBD](http://www.drbd.org/). This means that each byte of user data is on at least four different enterprise drives across two different physical servers, plus nightly backups. If we have any problems with a server, we can fail the services from its primary VM over to the secondary on another box with minimal downtime via Heartbeat.

每台服务器上的主虚拟机上的所有用户数据用BRBD同步复制到另一台服务器上的第二个备虚拟机。也就是说每个字节的用户数据位于2台服务器上至少4个不同的企业级磁盘上，外加每天夜间备份。如果某个服务器有问题，偶们通过Heartbeat可以在最小的停机时间内把它的主虚拟机迁移到另一台服务器的备虚拟机上。

Since each users’ data is completely localized to one (virtual) shard host, we can run each shard as an independent island with virtually no crosstalk or dependencies. This means that issues on one shard don’t snowball to other shards.

由于每份用户数据都完全定位于一台（虚拟的）shard上，我们可以没有干扰没有依赖的独立运行每个shard“孤岛”。这表示任何一个shard上的问题，不会波及其他任何shards。

To connect users with their shard, we push most of the work into the load balancers, which have a cascade of rules to find the shard in the URL and/or cookies.

为了把用户连接到他们所在的shard，偶们折腾了很多经历在负载均衡设备上，弄了一堆级联的规则以根据URL和/或cookies来定位shard。

**UserStore:** While the vast majority of all data is stored within the single-tier NoteStore shards, they all share a single master “UserStore” account database (also MySQL) with a small amount of information about each account, such as: username, MD5 password, and user shard ID. This database is small enough to fit in RAM, but we maintain high redundancy with the same combination of RAID mirroring, DRBD replication to a secondary, and nightly backups.

**UserStore:** 虽然绝大多数用户数据存储在单级的NoteStore shard上，但是他们都共享一个单独的主UserStore账户数据库（也是MySQL）上面存了关于每个账户的少量信息，比如：用户名、MD5后的密码、以及用户的共享ID。这个数据库足够小到能放进内存里面，但是我们还是用上面提到的RAID镜像组合、虚拟机BRBD主从复制以及每天夜间备份等方式了保持了高度的冗余。

**AIR processors:** In order to allow you to search for words found within images in your notes, we maintain a pool of 28 servers that spend each day using their 8 cores to process new images. On a busy day, this translates into 1.3 or 1.4 million separate images. Currently, these use a mix of Linux and Windows, but we plan to convert them all to Debian by the end of the month now that we’ve removed some pesky legacy dependencies.

**AIR处理器：** 为了让你能搜索从你的notes里面图片上找到的词语，我们维护了一组28台服务器成天用他们的8核处理器处理新的图片。赶上忙的日子，差不多有1千3-4百万张独立的图片。目前，这里面极有linux也有windows，不过偶们现在计划这个月底把他们全换成Debian以摆脱这些讨厌的之前遗留下来的依赖问题。

These servers run a pipeline of “Advanced Imaging and Recognition” (AIR) software developed by our R&D team. This software cleans up each image, identifies word-shaped regions, and then attempts to compile a weighted list of possible matches for each word using a set of “recognition engines” that each contribute a set of guesses.  This includes engines developed by our own team which specialize in, for example, handwriting recognition, as well as licensed technologies from best-of-breed commercial partners.

这些服务器跑一套由我们研发组开发的“Advanced Imaging and Recognition” (AIR)软件。这软件先清理每张图片，识别文字型的区域，然后用一组“识别引擎”编制可能匹配的权重列表来全力“猜”出每个单词。这些引擎既包括了由偶们自己团队研发的专门的针对，例如手写识别的引擎，也包括其他牛逼的商业伙伴的授权技术。

**Other services:** All of these servers are racked in a pair of dedicated cages at our data center in Santa Clara, California. In addition to the hardware that provides our core service, we also have smaller groups of servers for lighter-weight tasks that only require one or two boxes or Xen virtual machines. For example, our “incoming email” SMTP gateway is a pair of Debian servers with Postfix and a custom Java mail processor built on top of Dwarf. Our @myen Twitter gateway is a simple in-house daemon using twitter4j.

**其他服务：** 所有的服务器都用成对的专用外壳上架在位于加州Santa Clara的数据中心。除了这些提供核心服务的设备之外，我们也有一些更小的服务器组，用来应对只需要1-2台服务器或者Xen虚拟机的轻量级任务。例如，我们“incoming email”的SMTP网关是一对装了Postfix和基于Dward定制的Java邮件处理程序的Debian服务器。偶们的@myen Twitter 网关是个简单的基于twitter4j的内部守护进程。

Our corporate web site is Apache, our blogs are WordPress, most of our fully redundant internal switching topology is from HP, we use Puppet for configuration management, and we monitor with [Zabbix](http://www.zabbix.com/), [Opsview](http://www.opsview.com/), and [AlertSite](http://www.alertsite.com/). We run nightly backups with a combination of different software that migrates data over a dedicated 1Gbps link to a secondary data center.

偶们的官网用的Apache，偶们的博客用的WordPress，大多数全冗余的内部交换拓扑网络来自HP，我们用Puppet做配置管理，我们用Zabbix、Opsview还有AlertSite来做监控。我们用一组不同软件组合来跑每日夜间备份，把数据通过一条专用的1Gbps链路迁移到另一个数据中心。

**Wait, but why?** I realize this post leaves lots of obvious questions about why we’ve chosen to do X instead of Y in a number of different places. Why run our own servers instead of using a cloud provider? Why such stuffy old software (Java, SQL, local storage, etc.) instead of hot new magic bullets? …

**稍等，为啥这样呢？** 偶知道这个帖子肯定会带来很多明显的问题，关于在各个地方为啥用X方案而不用Y方案。为啥偶们用自己的服务器而不是云服务提供商？为啥用这些老旧的软件(Java, SQL, 本地存储等等)而不是那些流行的新鲜玩意？

We’ll try to get into more details to answer these questions in the next few months.

偶们争取在未来几个月来更详尽的解答这些问题。

######EOF
