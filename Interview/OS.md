1、进程与线程  
进程是资源分配的最小单位，线程是CPU调度的最小单位；线程被包含在进程之中，是进程中的实际运作单位。  
在一个进程中创建多个线程，让它们在“同一时刻”分别去做不同的工作。这些线程共享同一块内存，线程之间可以共享对象、资源，如果有冲突或需要协同，还可以随时沟通以解决冲突或保持同步。  
在一个进程内，不管你创建了多少线程，它们总是被限定在一颗CPU内，或者多核CPU的一个核内。这意味着，多线程在宏观上是并行的，在微观上则是串行的，多线程编程无法充分发挥多核计算资源的优势。这也是使用多线程做任务并行处理时，线程数量超过一定数值后，线程越多速度反倒越慢的原因。  
多进程技术正好弥补了多线程编程的不足，我们可以在每一颗CPU上，或者多核CPU的每一个核上启动一个进程，如果有必要，还可以在每个进程内再创建适量的线程，最大限度地使用计算资源解决问题。因为不在同一块内存区域内，和线程相比，进程间的资源共享、通信、同步等，都要麻烦得多，受到的限制也更多。  
线程之间能够方便、快速地共享信息。只需将数据复制到共享（全局或堆）变量中即可。不过，要避免出现多个线程试图同时修改同一份信息的情况，这需要使用同步技术。  
创建线程比创建进程通常要快10倍甚至更多。（在 Linux 中，是通过系统调用 clone()来实现线程的）线程的创建之所以较快，是因为调用 fork()创建子进程时所需复制的诸多属性，在线程间本来就是共享的。特别是，既无需采用写时复制来复制内存页，也无需复制页表。  
  
2、进程间交流方式  
总公司和分公司，以及各个分公司之间，工具都是独立的，不能借用、混用----这叫进程间不能共享资源。  
各个分公司之间可以通过专线电话联系----这叫管道PIPE。  
各个分公司之间还可以通过公司公告栏交换信息----这叫进程间共享内存。  
  
(1)PIPE&FIFO:  
分为两类：匿名管道和命名管道  
有需求要将一个程序的输出交给另一个程序进行处理.半双工通信方式，数据只能单向流动，只能在相关进程之间使用,在命名管道中允许不相关的进程之间通信。  
Linux命令 "|" 其实就是匿名管道，表示把一个进程的输出传输到另外一个进程, 如 echo "Happyjava" | awk -F 'j' '{print $2}' # 输出 ava  
通过 mkfifo <pipename> 命令创建一个命名管道，mkfifo pipe ，一个进程往管道输入数据，则会阻塞等待别的进程从管道读取数据：左边的窗口（echo 'Happyjava' > pipe），右边窗口可以执行 cat < pipe 命令，来获得字符串，但是如果没有执行这个命令则左边窗口一直阻塞。  
  
(2)共享内存：  
让两个进程各自拿出一块虚拟地址空间来，然后映射到相同的物理内存中，这样，两个进程虽然有着独立的虚拟内存空间，但有一部分却是映射到相同的物理内存，这就完成了内存共享机制了。  
共享内存指可以被其他进程访问的一块内存。由一个进程创建，但可以由多个进程访问。最快的IPC模式，专门针对其他进程间通信模式效率低而设计的。通常与其他通信机制(如信号量)结合使用，以实现进程之间的同步和通信。
  
(3)信号量：  
  
(4)消息队列：  
消息队列是存储在内核中并由消息队列标识符标识的消息的链表，提供了一种从一个进程向另一个进程发送一个数据块的方法。 每个数据块都被认为含有一个类型，接收进程可以独立地接收含有不同类型的数据结构。消息队列与命名管道一样，每个数据块都有一个最大长度的限制。  
  
(5)socket:  
