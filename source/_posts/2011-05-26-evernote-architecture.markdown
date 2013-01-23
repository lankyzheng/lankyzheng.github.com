---
layout: post
title: "[中英对照] Evernote Architecture – 9 Million Users and 150 Million Requests a Day : Evernote架构-9百万用户和每天1亿5千万请求"
date: 2011-05-26 07:53
comments: true
categories: tech
---
![banner](/images/pics/kO5Mi.png "banner")

<!--more-->

原文地址：[http://highscalability.com/blog/2011/5/23/evernote-architecture-9-million-users-and-150-million-reques.html](http://highscalability.com/blog/2011/5/23/evernote-architecture-9-million-users-and-150-million-reques.html)

The folks at Evernote were kind enough to write up an overview of their architecture in a post titled Architectural Digest. Dave Engberg describes their approach to networking, sharding, user storage, search, and some other custom services.

Evernote的那帮哥们挺不错的，写了篇文章叫Architectural Digest来讲他们的架构概况。Dave Engberg描述了他们在网络、shard、用户存储、搜索以及其他定制服务方面的实现。

Evernote is a cool application, partially realizing [Vannevar Bush](http://en.wikipedia.org/wiki/Vannevar_Bush)‘s amazing vision of a [memex](http://en.wikipedia.org/wiki/Memex). Wikipedia describes Evernote’s features succinctly:

Evernote是个很酷的应用，部分实现了Vannevar Bush关于memex的美好设想。维基百科上简洁的描述了Evernote的特征：（wiki的废话偶就不翻译了。）

>Evernote is a suite of software and services designed for notetaking and archiving. A “note” can be a piece of formattable text, a full webpage or webpage excerpt, a photograph, a voice memo, or a handwritten “ink” note. Notes can also have file attachments. Notes can then be sorted into folders, tagged, annotated, edited, given comments, and searched. Evernote supports a number of operating system platforms (including Android, Mac OS X, iOS, Microsoft Windows and WebOS), and also offers online synchronization and backup services.

Key here is that Evernote stores a lot of data, that must be searched, and synced through their cloud to any device you use.

关键在于Evernote存储了大量的数据，还要能被搜索，还要能在各种设备上同步使用。

Another key is the effect of Evernote’s business model and cost structure. Evernote is notable for their pioneering of the freemium model, based on the idea from their CEO: The easiest way to get 1 million people paying is to get 1 billion people using. Evernote is designed to become profitable at a 1% conversion rate. The free online service limits users to a hefty 60 MB/month while premium users pay $45 per year for 1,000 MB/month. To be profitable they most store a lot of data without spending a lot of money. There’s not a lot of room for extras, which accounts for the simple practicality of their architecture.

另一个关键在于Evernote的商业模式和成本结构的影响。Evernote因freemium模式的领先而著称，来源于他们CEO的点子：“让1百万人付费最简单的办法是让10亿人使用”。Evernote被设计为靠1%的转化率来盈利。免费的在线服务限制用户60MB/月，要是每年付45美刀则获得1,000MB/月。为了赢利他们尽可能省钱的存储尽可能多的数据。没啥多余花哨的东西，构成了他们简单使用的架构。

The article is short and succinct, so definitely read it for details. Some takeaways:

文章短而简洁，尽可详细阅读。要点如下：

- **Controlling costs.** Evernote runs out of a pair of dedicated cages in a data center in Santa Clara, California. Using a cloud wouldn’t provide enough processing power and storage at a cheap enough cost to make Evernote’s business model work. As their load doesn’t appear to be spiky, using their own colo site makes a lot of sense, especially given how they make use of VMs for reliability.

- **控制成本。** Evernote在加州成对部署独立服务器。要是用云计算的话，不一定能以足够低的成本提供足够的助理能力和存储来让他的商业模式奏效。由于他们的负载不猛，用他们自己的的组合就挺靠谱了，特别是用了虚拟机来的达到可靠性。

- **Architecture based on the nature of the data.** User notes are independent of each other, which makes it very practical for Evernote to shard their 9.5 million total users across 90 shards. Each shard is a pair of two quad-core Intel  SuperMicro boxes with lots RAM and a full chassis of Seagate enterprise drives in mirrored RAID configurations. All storage and API processing is handled by a shard. They’ve found using directly attached storage to have the best price/performance ratio. Using a remote storage tier, with the same level of redundancy, would cost substantially more. Adding drives to a server and replicating with DRDB is low both in overhead and costs.

- **基于数据本质设计架构。**用户的notes数据彼此独立，因此Evernote用90个shard来分担9百多万用户的办法就很实用了。每个shard有2个4核处理器，居多内存和做了镜像RAID的满配Seagate企业级硬盘。所有的储存和API处理都在shard内部处理。他们发现直接用硬盘存储性价比很好。要是用专门的存储设备达到相同的冗余级别，成本要高得多。在服务器上价格硬盘以及用DRDB做迁移，成本和日常费用都很低。

- **Application redundancy.** Each box runs two VMs. A primary VM runs the core stack: Debian + Java 6 + Tomcat + Hibernate + Ehcache +  Stripes + GWT + MySQL (for metadata) + hierarchical local file systems (for file data). DRDB is used to replication a primary VM to a secondary VM on another box. Heartbeat is used to fail over to a secondary VM is the primary VM dies. A smart way to use those powerful machines and make a reliable system with fewer resources.

- **应用冗余。**每个服务器跑2个虚拟机。主虚拟机跑核心应用。DRDB用来做主虚拟机和另一个服务器上的备虚拟机的复制。Heartbeat用来做fail over切换。确实是利用服务器能力以及用较少的资源建立可靠系统的聪明方式。

- **Data reliability.** User data is stored on four different enterprise drives across two different physical servers. Nightly backups  copies data over a dedicated 1Gbps link to a secondary data center.

- **数据可靠性。**用户数据存在2台物理机的至少4个磁盘上。每天夜间还有跨数据中心的数据复制。

- **Fast request routing.** User account information–username, MD5 password, and user shard ID–is stored in an in-memory MySQL database. Reliability comes from RAID mirroring, DRBD replication to a secondary, and nightly backups. This approach makes the routing of users to their data a simple and fast in-memory lookup, while still being highly available.

- **快速的请求路由。**用户账号信息存储在内存中的MySQL。RAID、DRBD迁移和夜间备份带来了可靠性。通过内存检索，用户可以快速到达他们的数据，同时保持了高可用性。

- A separate pool of 28 8-core servers process images for search, handwriting recognition, and other services. This is custom software and is a powerful value-add that is not easily replicated by anyone else.

- 单独的28台8和服务器处理图片搜索，手写识别等服务。定制软件以及强大的附加值不容易被他人山寨。

- Puppet is used for configuration management.

- Puppet用来做配置管理。

- Monitoring is done with Zabbix, Opsview, and AlertSite.

- 监控用了Zabbix, Opsview, 和AlertSite。

There’s a promise of future articles focusing more on individual subsystems. I look forward to these as you have to appreciate the elegance of the system they’ve created for their business model. A good example to learn from.

他们承诺继续写文章描述各个子系统。偶很期待，并膜拜对他们为自己的商业模式创造出如此优雅的系统。值得学习的范例！

######EOF
