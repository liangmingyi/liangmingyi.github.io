title: 如何追查诡异问题
date: 2017-09-10 22:01:07
tags: 项目总结
---

## 概述

这里的“问题“，主要指一个软件系统中，由于硬件故障、程序bug或人为失误而导致的系统异常。
“诡异问题”指的是不是那么容易追查的问题，有些问题一看就知道原因，这种问题不在本文讨论之内。
解决诡异问题一般会有四个步骤：
1. 分析问题
2. 复现问题
3. 定位问题原因
4. 寻找解决方案

## 一、分析问题

#### 信息收集

一般来说拿到一个问题，先收集与问题相关的信息。
* 是否有相关的业务日志、错误日志
* 是否有非业务日志（比如php-error, nginx error_log，系统dmsg等）
* 进程/线程状态是否正常（如ps、pstree、top、/proc/$pid、lsof）
* 网络状态是否正常（如netstat、ifconfig）
* 系统内存、磁盘状态是否正常（如free、df、iostat）
* 是否有core文件
* 查看程序的版本及changeLog（是否是某个版本的已知问题）
* 查看上下游模块是否有异常情况
* ……

#### 现象分析

从收集到的信息，可以看出一些不寻常的“现象”，接下来是对这些现象做一些初步分析。下面是一些比较常见的现象的分析思路。
* 错误日志：这是最常见的一种现象了，看看具体错误信息，看看报错的是哪一行，到代码里面看具体的逻辑，分析报错的原因
* 请求失败：server返回了明确的错误（比如http 500， error_no -1等），这种错误一般是有错误码的，查看错误码对应的错误原因。也可以看server日志，有没有相关的错误日志。
* 功能异常：请求并没有明显的失败，但从用户看到的功能来看不正常，比如乱码。这种一般是请求到的数据有异常，需要深入业务逻辑，查看系统各环节的数据，看看是从哪个环节开始出现异常，可以用二分法排查。
* 响应超时或长耗时：对比下client日志和server日志，看是否server真正长耗时，还是中间的网络延时。如果server真的长耗时，需要查看server各阶段日志，看看哪一阶段耗时长。
* 连接失败：可能是server端口异常，可能是网络异常。尝试手动连接端口：nc -vzz $ip $port,分别从client和server所在的机器连接，如果在server所在机器连接不上端口，说明端口不存在。如果server能连接，client不能，可能是中间的网络问题，可以尝试ping一下。也可能是server设置了一些防火墙策略，可以尝试server上面的其它端口看能否连接。
* 出core：首先用gdb查看core堆栈，如果是多线程程序，除了出core的那个线程，还可以看看其它线程都在做什么。
* hang死：这种情况进程是存活的，但是不工作了，可以gdb attach上去看看系统堆栈，看看hang在什么堆栈上，同样看看每个线程都在做什么。
* cpu打满：这种情况下进程是存活的而且一直在使用cpu，同样可以gdb attach上去看看在做什么，还可以在gdb里面输入continue让它继续执行，一会再Ctrl+C看看在做什么。
* 内存泄漏：这种情况下进程工作正常但是占用的内存一直在增长。先确定是否是持续增长，有些程序随着压力的增大内存占用会增大，这是正常的，如果在压力恒定情况下，内存还一直增长，那很有可能就是内存泄漏了。
* 机器无法登录：可尝试ping下，如果ping不通，说明网络不通，或者ip已经不存在。如果能ping通，但是ssh超时，那可能是机器假死，比如cpu打满、内存用满之类的。如果ssh立即返回错误，那有可能是密码错误或者没权限。
* ……

#### 因素分析

从现象初步分析后，还可以从因素角度分析，看看问题和哪些因素有关。
* 时间因素
    * 问题发生的时间有没有什么规律，比如每天高峰期容易出现，则可能与压力有关；比如每隔固定时间间隔出现，可能与定时任务有关。
    * 问题发生的时间点，有没有什么相关事件，比如上线、重启等。
* 空间因素
    * 问题发生的地点（机器）有什么规律，比如固定在某台机器出现，固定在某个网段出现，则可能和这些机器、机房、网段有关。
    * 比如说固定在某台机器出现，那么可以对比下这台机器和其它机器有什么不同，比如硬件、操作系统版本、环境变量、上面部署的程序、代码、配置有什么不同。
* 软件因素
    * 问题发生所在的软件有什么规律，比如只在某个模块出现，或者只在某个接口出现，或者只在某个参数传某个值的时候出现
    * 那么可以对比一下这个模块/接口和其它的模块/接口有什么不同，比如是否会走到不同的业务逻辑分支
    * 这里的软件因素可以是不同层面的，比如用户/客户端输入、配置/数据、业务代码、框架/基础库、语言/Runtime等都可以算软件层面的因素。


## 二、复现问题

按问题发生的条件，可以分为
* 无法复现：问题只发生过一次或几次，之后再也无法出现
* 不稳定复现：按照某个确定的步骤操作，问题有一定概率会出现
* 稳定复现：按照某个确定性的步骤操作，问题必然出现
从定位问题的难度来看，显然是 无法复现 > 不稳定复现 > 稳定复现。
从复现的场景分，又可分为线上复现和线下复现。从定位问题的难度来看，显然是 线上复现 > 线下复现。
由于线上是用户的真实流量，不能随意做各种实验，所以给定位问题会带来很大困难，因此我们第一步要想方设法实现线下复现，然后再想办法做到线下稳定复现。

#### 线下复现

一般来说线下复现就需要构造和线上尽量一致的条件。
* 硬件、网络一致，这点一般比较难做到，因为线下测试机和线上机器总有各种不一样。好在硬件问题一般是比较少见的。
* 软件环境一致，包括操作系统、环境变量、程序、配置、数据、依赖服务（这个比较难，可以考虑采用mock方式）。
* 输入请求一致，可以用tcpcopy从线上拷贝真实流量，到线下回放

#### 稳定复现

如何把不稳定复现变成稳定复现？
想办法增加问题发生的概率。
一些常规的方法如：
* 加大流量压力
* 增加请求的并发，包括调大客户端的请求并发数，以及服务端的并发数（进程/线程数）
* 减少服务端的并发数，这和上一条并不冲突，这是为了更快地制造请求达到处理瓶颈的条件，有些问题在请求达到瓶颈的时候更容易暴露出来
* 构造各种异常请求，如非法类型、超长请求、不存在的id等进行不断尝试
* 构造一些服务异常，如配置不存在ip、端口，故意杀死进程等
但是上述方法，缺乏针对性，实际操作的时候，需要根据具体问题发生的条件，来分析。比如经过前面的因素分析发现问题只出现在某个模块某个接口上，那么就专门针对这个模块这个接口进行复现，那么复现的概率会大很多。如果没能复现，那么估计是有某个因素我们尚未发现，没有去构造出这个因素。
当问题发生的概率够大时，比如在起压力的情况下每1分钟出一次问题，那就可以进行下一步的问题定位了。

#### 无法复现的情况

这是最麻烦的情况了，可能是线上一直没有再出现过，也可能是线上还出现但线下一直没能复现。而且，经过前面的现象分析和因素分析也没能得到更多有用的线索。
在这种情况下，可以尝试为程序增加更多详细的日志，然后等待线上再次出现，这些日志可以为我们提供进一步的线索。

## 三、定位问题原因

定位问题有多种方法，常见的方法是因素测试和调试。


#### 因素测试（控制变量法）

和前面的因素分析类似，因素测试就是人为地构造更多因素不一样的场景，来测试问题是否和某个因素有关。
比如说，修改程序版本，看问题是否发生。如果有的版本有问题，有的版本没问题，那就可以确定，是某些版本引入的问题。通过二分法，可以精确定位到是哪个版本引入的问题。
又比如说，调整server的线程数，如果线程数为1的时候没问题，线程数大于1的时候有问题，那说明是多线程带来的问题。


#### 调试

调试有两种方式，一种是通过打日志，把程序内部的一些信息打印出来。另一种是使用比如gdb这样的工具，设置断点然后单步进去查看各种变量的值。
打日志的优点是可以用于非稳定复现的情况（不是每个请求都必然出问题），日志可以一直打，积累足够多的请求到问题复现了，再回头来分析日志。缺点是需要改代码，如果是C模块这种还得重新编译，麻烦。
gdb这种工具调试的优点是不用改代码，缺点是一般只能用于稳定复现的情况。
工具分析

有很多工具可以用于分析问题，比如valgrind可用于检测内存非法使用和内存泄漏，gperftool可以用于检测cpu性能瓶颈和内存泄漏，strace可用于追踪系统调用的情况。关于工具的使用可以详见这里。


## 四、寻找解决方案

原因都定位出来了，解决方案就不成问题了：）
一般来说，应该解决根本原因（比如某个程序bug，那就修复这个bug）。

但是有时候，解决根本原因的成本较高（比如要修改第三方开源库），而线上问题又比较紧急，这时可以先考虑从上层去规避。前面通过因素分析、因素测试可以找出影响问题的一些因素，那么可以通过修改因素来规避。（比如发现在某些机器上有问题，其它机器没问题，可以先把有问题的机器摘除）