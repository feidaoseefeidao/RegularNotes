## 进程和线程的调度
什么是进程的调度? 对于多道程序设计的程序，就会有多个进程或者线程在同时竞争CPU。对于单核系统，调度问题就是选择下一个要运行的进程或者线程。<br>

##### 考虑💭的因素
* 进程是CPU密集型还是IO密集型。IO密集型可能会需要尽快运行，并且很快就阻塞。CPU密集型可以会长期占用CPU
* 何时调度。创建进程、进程退出、进程阻塞以及IO中断发生
* 是否支持抢占。支持抢占的调度，可以不必等待当前进程运行完或阻塞，直接上CPU运行

#### 不同的环境需要不同的调度算法(调度算法分类)
* 批处理。批处理系统下，不需要特别快的响应速度，所以可以考虑非抢占以及每个进程都有长时间的抢占算法。指标主要是: 吞吐量、周转时间以及CPU利用率
* 交互式。交互式系统下，抢占是必须的，因为操作者会希望比较快的得到响应。考虑的指标主要是最小响应时间、均衡性
* 实时。实时系统下，最重要的是满足截止时间要求，即在一定的时间内完成任务

#### 批处理系统中的调度
* **先来先服务(FCFS)。** 最简单的方式，非抢占式的先来先服务，把任务作为一个队列，先来先服务。缺点是，无法体现任务的轻重缓急，达不到理想的指标性能
* **最短作业优先。**非抢占式下，即当一个任务完成时，从任务队列中挑选最短的一个作业执行。相对于先来先服务，提高了一些性能。运行时间必须提前掌握
* **最短剩余时间优先。**即最短作业优先的抢占版本。调度程序总是选择剩余运行时间最短的作业执行。每当一个新的作业到达，如果运行时间比当前进程的剩余运行时间短，就挂起当前进程并切换到新的进程

#### 交互式系统中的调度
* **轮转调度。**最简单且最公平的方法，给每个进程分配一个时间片。时间片耗尽时，进程会下CPU并加入到就绪队列的末尾。问题的关键是选择合适的时间片
* **优先级调度。**进程有轻重缓急，于是给进程设置不同的优先级，每次调度优先级最高的进程运行。当然，这样可能会导致低优先级进程饥饿。解决方案是，实行奖惩机制，高优先级进程耗尽时间片时降低它的优先级
* **多级队列。**给不同优先级的进程队列，设置不同单位的时间片。同时，也实行奖惩机制，耗尽时间片会改变进程的队列级别

#### 线程的调度
线程的调度，取决于支持的是内核级线程还是用户级线程<br>
对于用户级线程，内核不知道线程的存在，就给了进程很大的自主权。内核只是调度进程，进程中的调度程序选择哪个线程来运行。<br>
对于内核级线程，线程的调度就交给了系统完成<br>