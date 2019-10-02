---
title: "体系结构论文清单[已读]"
date: 2018-07-31 21:30:23 +0800
category: Markdown
tags: [Markdown, Minimalism]
excerpt: 读研期间所读paper阅读总结汇总，包含领域：体系结构四大会（ASPLOS、ISCA、HPCA、MICRO），计算机安全四大顶会（NDSS、SP、Usenix Security、CCS）。
---

体系结构四大顶会：ISCA、HPCA、MICRO、ASPLOS

计算机安全四大顶会：NDSS、SP、Usenix Security、CCS

[2015.Usenix.Control-Flow Bending: On the Effectiveness of Control-Flow Integrity](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;根据SOK-Eternal War in Memory，Memory Corruption(内存错误)是所有软件安全漏洞的根本来源，也是产生控制流劫持攻击的原因。传统的控制流劫持攻击是修改控制转移类的跳转目标指令，指向注入的攻击代码，这种攻击方式被DEP（data execution prevention）解决——每个内存页可读和可写不能同时满足；后来攻击方式采用了重用代码片段——gadget，将控制转移类指令的目标地址指向libc或者可执行程序中的代码片段，如果被重用的代码规模达到一定程度，就能够从攻击程序中找到图灵完备的代码片段，比如：JOP、ROP、Return-To-Libc攻击。目前的解决方式是CFI防御机制(2005年)，其核心思想是真实程序运行trace中的控制转移，必须始终处于原有的控制流图所限定的范围内。本文提出一种新型的printf攻击方式，攻击者可以绕过shadow stack,不修改return address仅仅通过修改函数调用传入的参数，便可以完成攻击者的目的，包括执行任意代码、执行受限代码和信息泄露。通过证明CFI策略的不完备性，指出要想真正解决控制流劫持攻击，还是要从源头Buffer Overflow上解决。

[2019.SP.Using Safety Properties to Generate Vulnerability Patches](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;本文的研究背景是软件工程领域中的APR(自动程序修复,automated program repair)，生成源码级别的patch.主要工作是LLVM KLEE的扩展，KLEE是基于LLVM的符号执行引擎，能够自动生成测试样例检测软件缺陷，需要将C/C++源代码编译成LLVM bitcode，本文在KLEE的基础上对LLVM  bitcode做了一层pass，计算出循环体中指针的访问范围，用于后续patch综合中和分配的范围作比较。本文主要用到编译中的符号执行技术（使用符号值来表示程序的输入数据，并将程序的运算过程逐指令或逐语句地转换为数学表达式，在CFG的基础上生成符号执行树，并为每一条路径建立一系列以输入数据为变量的符号表达式），最终采用了11个真实应用程序中42个CVE漏洞进行测试，跑通了其中的32个，没跑通的10个程序的原因是：Senx无法将所有的符号变量统一到一个函数体内，否则需要显著更改应用程序的代码，这显然超出了Senx的能力范围。

[2019.HPCA-25.4AP1.Conditional Speculation An Effective Approach to Safeguard Out-of-Order Execution Against Spectre Attacks](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;中科院信工所的一篇paper.讲的是如何进行微体系结构设计解决幽灵问题，对于推测式执行我们全关闭会极大的硬性处理器的性能，所以我们对推测式的指令范围作折中.通过引入“安全相关”的概念，对要进行推测式的指令作有条件的推测式——只有满足安全相关且Cache Miss且符合S-Pattern的指令阻塞执行(re-issue)，因为这些指令很可能会造成幽灵攻击把敏感信息反映到cache侧信道上，而其他指令正常的进行推测式执行。最终性能开销6.8%，而且能解决已知的幽灵问题（spectre v1.v4.prime），功耗开销小.


[2019.ASPLOS-24.4AP1.Architectural Support for Containment-based Security](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;硬件安全的一篇paper，涉及到微体系结构的设计。为了解决硬件安全，一种传统朴素的做法是克隆一个处理器在后台作冗余执行，比较冗余执行的结果与处理器运行结果是否一致，但是这种方法的overhead>100%。我们知道I/O等外围设备对处理器的安全是一个重大的威胁，Thus，如果把关注的视野局限在保证处理器与外围设备交互的结果安全，不考虑处理器内部是否受到攻击的情境下，我们可以设计一个简单的安全检查单元Sentry，使得Sentry<<Processor，通过在Sentry里重执行指令，比较Sentry执行结果与处理器发送的结果是否一致，一致的话再发送到I/O，从而完成复杂系统与IO交互结果的验证工作。 

[2018.信息安全学报.Glibc 堆利用的若干方法](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;glibc是linux操作系统底层中标准C语言函数库，包括系统调用和以及各种IO的API，尤其是malloc、free的实现机制。由于glibc是开源，可以通过研究源码对glibc作攻击（用户程序千变万化，研究应用程序的攻击不具有普遍性）。本文主要内容有：（1）glibc的堆管理方式：chunck等数据结构、malloc、free、realloc的实现（2）glibc的攻击和防御：PIE（位置无关可执行）+ASLR 、 NX bit（不可执行位）+RELRO（重定位只读）。 读这篇文章主要是思考后期使用Minifat作glibc的加固。

[2018.ISCA-45.2BP2.A Hardware Accelerator for Tracing Garbage Collection](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;Java、GO、C#等大多主流的编程语言都带有Garbage Collection机制，而C/C++都是通过手动malloc、free分配和释放资源。paper通过在真实的Rocket SOC跑Java benchmark发现：多核体系结构下在CPU中运行GC的线程是不合适的，在存在多核互扰和存储一致性问题的同时，占有38%的时间开销和25%的功耗。所以，把GC的机制设计成硬件单元，通过内核的driver与JVM打交道，可以合理的管理堆区的内存。具体实现：chisel module+linux driver+libhwgc.so.

[2017.SOSP.Komodo: Using verification to disentangle secure-enclave hardware from software](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160; SOSP（symposium on operating system principles）操作系统原理大会的一篇paper。讲的是如何以ARM TrustZone为原型设计一套功能与Intel SGX相同的enclave执行环境，最最核心的是reference monitor的设计，就像科莫多龙保卫自己的领地一样，monitor管理和保护自己的enclave。区别于monitor在SGX中使用SGX指令实现，komodo采用的是一系列的Monitor call(SMC+SVC)进入ARM中的特权模式，实现OS-monitor、Monitor-Enclave之间的交互。简单理解，komodo最终实现了TrustZone中secure world+ SGX enclave的结合，在树莓派2上实现了该工作。 

[2018.CCS.A Robust and Efficient Defense against Use-after-Free Exploits via Concurrent Pointer Sweeping](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160; 组会PRE的一篇论文，主要内容是并发指针扫描算法解决Use-after-Free时间性安全问题。Use-after-Free指的是指针访问了已经释放的内存空间，这样的指针成为dangling pointer(悬垂指针or迷途指针)。实现机制具体来说，利用空闲的核运行指针扫描的线程，迭代地扫描所有的活性指针，如果遇到悬垂指针，对它进行使失效，是它变为合法指针或者指向NULL。由于并发指针扫描(CPW)线程是在另一个核运行的，所以涉及到存储一致性（consistence同步）问题，指针扫描不能在保证free()之后立刻完成，为此之间仍然存在攻击窗口可以被攻击者用来做控制流攻击，本文提出了一个很好的解决方法——延迟释放，在CPW回合结束之后再进行free()操作，由此又会带来迷途指针传播的问题，作者又通过在q=p之后加入check解决。最后，通过重构64位指针内容，用50位保留动态执行的对象的Index,这样在发现UAF之后可以把对应的object以及在第几次free()的动态信息打印出来，OOT（object origin tracking）是一个incremental的工作。

[2016.ISCA-43.5AP2.Bit-Plane Compression Transforming Data for Better Compression in Many-core Architectures](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;多核课上cache compression的一篇paper。为了增大cache的有效容量，在物理容量不变的情况下，需要对数据进行压缩。本文提出了一种Bit-Plane(位-平面)的cache压缩方法，能够达到整数集4.1:1的压缩比，压缩效果很明显。具体方法分为两步：第一步，Transform(转化)：以降低信息熵为目的，提出DBX（delta-bitplane-xor）转化方法，现将相邻的数据作差，然后转置，最后新相邻的数据作XOR操作，这样能够极大程度地开发数据的均匀性Homogeneity;第二步，Encode/Compression(压缩)：由于第一步转换开发了均匀性，产生了很多连续0的数据，降低了压缩端的压力，选用一种比较轻量级Lowweight的压缩器就可以获得理想的压缩比，本文采用的是以前诸多论文提到的ZBP-RLE+FPE的压缩方法.将Transform+Encode的方法结合起来，压缩效果非常明显。

[2018.HPCA-24.2BP1.KPart A Hybrid Cache Partitioning-Sharing Technique for Commodity Multicores](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;多核课LLC Partition的一篇论文.UCP划分策略是根据每一个core（或者应用）的Miss Cure曲线 miss ratio = f(cache capacity) 在总的cache容量一定下最小化总的核Miss Ratios的最优化策略。观察到这样一个现象：把2个对cache访问行为和需求相似的cores作为一个划分，发现：相同cache容量下总的Miss Ratios比单独划分的Miss Ratio1+Miss Ratio2之和要小，即Partition Miss Cure < Combined Miss Cure，其中Combined Miss Cure是两个core的Miss Cure简单相加。基于这样一个有趣的观察我们可以设计一套算法优化UCP——将n个cores分成K个簇Clusters，再以Cluster为基本单元对总的容量作Cache划分，这样既能提高LLC访问性能，又能解决路划分路数不够分的问题。具体来说，将n个Cores两两组合，分别计算每一对的适配度，这里适配度用Partition Miss Cure和Combined Miss Cure的差来度量，distance = Combined Miss Cure - Partition Miss Cure,distance越大，说明两者的适配度越好，越应该放在一个Cluster里面；第一次迭代能够得到n-1个优化方案，将适配度最好的对看成一个整体继续参与下一次迭代寻找新一轮的最适对，经过N-1次迭代可以找到k=[1..N]的所有cluster组合方案；采用最简单最有效的方法遍历一遍k=1...N便可以找到最适合的K Clusters划分方案，和相应的Miss Cure曲线。这样，后续模仿UCP的算法求解Cachhe路划分策略即可。


[2018.ISCA.Rethinking belady's algorithm to accommodate prefetching](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;多核课LLC prefetch的一篇论文.基本块实际上就是一种prefetch技术，当访问某个存储字节发生Miss时，会将64Bytes的基本块大小的内容全部放入cache中，以更好的发挥空间局部性。Hawkeye那篇文章讲述的只是如何设计cache 替换算法使得total miss最低，没有考虑prefetch的影响，实际上prefetch技术可以理解为额外、意料之外的Load操作，帮助CPU做了很多额外的替换操作，所以对后续的cache替换算法——不论是LRU还是OPT都会产生影响。本篇文章提出了OPT+prefetch的新算法Demand-MIN，每次cache Line替换时剔除掉prefetch最近的那个，如果cache中所有的基本块后续都没有prefetch，则按照OPT的算法做剔除。模仿Hawkeye的实现上，OPT扩充了重用区间的定义，节点也包括了每次petch X的时刻，同时对重用区间内活性区间相互覆盖的区间个数是否超过cache容量的评判标准也做了修改，而Predictor只是做了正常的扩充，RRIP没有修改，最终达到了很好的效果，与Hawkeye是一波人，18年的这一篇是在16年的基础上做的。

[2018.SP.Protecting the stack with metadata policies and tagged hardware](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;组会作PRE的一篇论文，讲的是如何利用tag memory的思想作stack overflow的保护。栈帧是在运行时刻用来维护函数调用信息的一种数据结构，信息包括：传入的参数、返回地址、frame pointer、上下文寄存器、局部变量等，我们采用二元组(frame_id,object_id)来标识栈区中某个对象，以提供一种细粒度的保护机制。tag memory的核心思想是对每一条指令和每一个stack memory word都打tag，在运行时刻拿指令的tag和栈区的tag作为输入，交给Security Monitor根据制定的安全策略作检查，比如：if instr-tag == STORE-RED-X and mem-tag == RED-X then allowed and result-tag = RED-X。实现上tag利用shadow memory的思想实现，映射到某个内存区域，而安全策略policy则在硬件上用cache作缓存以减少检查开销。本文还提出的一种lazy tagging的优化策略，避免了在栈帧分配和释放flush栈区EMPTY_STACK的操作，不做写的检查、只做读的检查，允许写溢出，在保证了数据流的完整性的基础上极大减小了开销，但牺牲了存储安全的属性，性能开销从12%减少到3-4%.

[2016.ISCA.Back to the Future Leveraging Belady’s Algorithm for Improved Cache Replacement](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;OPT算法是现实不可实现的，Back to the future这篇文章利用cache历史访问信息模拟最优LLC替换算法——OPT+Bypass，如果历史的行为是未来行为的很好的预测器的话，那么我们的算法就能够逼近OPT。作者首先将替换策略抽象成一个二元分类的问题，给定一个cache line X，设计一套机制能够判断出X是cache友好的还是cache不友好的，对于cache不友好的，我们可以直接Bypass掉，对于cache友好的且在cache miss要发生替换的情况下，优先选择cache中相对来说不那么友好的cache line剔除掉，并插入新的X。由此需要解决2个问题：1.二元分类算法如何设计，输入一个cache line，输出它是cache友好的还是cache不友好的；2.友好度如何量化，方便我们在cache替换时优先剔除友好度最低的那个。文章设计了OPTgen+Hawkeye用来解决第一个问题，3-bit的RRIP用来解决第二个问题。OPT+Bypass算法的核心思想是重用区间内活性区间相互覆盖的区间个数是否超过cache容量，如果超过会被Bypass，对cache也是不友好的。

[2017.SGXBOUNDS Memory Safety for Shielded Execution](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;SgxBound是为Intel SGX设计的一种BoundChecking策略，其设计思想是tag pointer + compact memory layout（类似于low-fat pointer），指针p的高32位存的是UpperBound，指针p指向的内存部分区域存的是LowerBound.

&#160; &#160; &#160; &#160;SgxBound源码地址[https://github.com/tudinfse/sgxbounds](https://github.com/tudinfse/sgxbounds)


[2009.soft bound Highly Compatibleand Complete Spatial Memory Safety for C](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
  
&#160; &#160; &#160; &#160;SoftBound是一种朴素的BoundChecking实现技术，只在编译过程中的中间表示中插入代码，而不修改源程序.插入的代码包括1.每个指针创建base和bound 2.每次使用指针进行访存操作之前做越界检查的语句，运行时刻的开销主要体现在访问查找表获取base和bound时间上。优点：能对所有的安全漏洞作全面检查，而且不改变源代码，兼容性好；缺点是：开发过程的压力都给了编译，运行时刻的开销也很大。

[2016.CC.Heap Bounds Protection with Low Fat Pointers](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)

&#160; &#160; &#160; &#160;Low-fat pointer的思想解决heap的boundchecking问题。内存结构包括：全局变量0区+M个堆区+1个栈区。编码方式：给定指针p对应的内存地址值，p的高32位决定所在哪个堆区，每个堆区只能动态分配固定大小的size，base同时也要求对齐.

[2018.ISCA-45.3AP3.SEESAW Using Superpages to Improve VIPT Caches](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 多核课第二周的paper2.改进VIPT的Index受限的问题，在4KB的base page引入2MB的超级页（大页），修改了TLB的Memory Hierarchy,TFT作为超级页映射表.

[2018.HPCA-24.2BP2.SIPT Speculatively Indexed, Physically Tagged Caches](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 多核课第二周的paper1.改进VIPT的Index受限的问题，提出增加额外1-3bit index与tag一起做地址翻译，采用感知器做方向预测，类似于BTB的结构做值预测.

[2018.ISCA-45.7BP2.Practical Memory Safety with REST](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- REST(Random Embedded Secret Token)通过软硬件协同的机制在缓冲区的上下插入了随机Token（令牌）.
- Cache Line增加了1 bit的Token标识位，在I1-D与Memory之间增加了一个Token比较器；ISA中通过修改X86指令增加了arm和disarm两条store指令.

[2002.CCured Type-Safe Retrofitting of Legacy Code](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- CCured表示对C语言的治愈，把所有指针分成Safe、Sequence、Dynamic三类，在静态编译和运行时刻对指针所覆盖的内存区域做类型检查.

[1998.StackGuard: Automatic Adaptive Detection and Prevention of Buffer-Overflow Attacks](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- Buffer Overflow Attack(缓冲区溢出攻击)：对栈帧中的返回地址return address作写覆盖，使得返回的地址指向插入的攻击代码attack code.
- StackGuard:1.插入Canary Word(金丝雀)在return时做检查2.MemGuard-对虚拟存储页写保护,禁止对返回地址写；前者是更高效，后者是更安全.

[CHERI(1).2014.ISCA-41.S7BP1.The CHERI capability model Revisiting RISC in an age of risk](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 剑桥大学的一套基于MIPS ISA+fat pointer存储安全系统，增加指令形成CHERI ISA，编译器使用LLVM，OS使用FreeBSD，处理器采用FPGA.
- CHERI ISA和指令级模拟器QEMU-CHERI已在2016年发布.

[CHERI(2).2015.SP.CHERI A Hybrid Capability-System Architecture for Compartmentalization](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 剑桥大学的一套基于MIPS ISA+fat pointer存储安全系统，增加指令形成CHERI ISA，编译器使用LLVM，OS使用FreeBSD，处理器采用FPGA.
- CHERI ISA和指令级模拟器QEMU-CHERI已在2016年发布.

[2012.Baggy Bounds Checking An Efficient and Backwards-Compatible Defense](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 一种高效的越界检查方法——baggy bound checking技术。通过buddy allocator给每个指针分配一个2的整数次幂的大小空间，使得只有1个字节就可以通过指针p索引到bound table并计算出base和size，大大降低了存储指针信息的overhead.

[2016.The Renewed Case for the Reduced Instruction Set Computer](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- RISCV的Macro-fusion技术：RVC + fusion。RISCV在cmp-branch指令上具有明显优势，但在index-load/store比不过x86甚至是ARM.
- 《Tue1130celio-fusion-finalV2》是这篇文章的slice.

[2018.ISCA-45.3AP4.A Case for Richer Cross-layer Abstractions Bridging the Semantic Gap to Enhance Memory Optimization](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 2018一篇ISCA,设计XMem系统(atom模型+Interface接口)实现跨层抽象，弥补应用程序和体系结构/系统之间的语义Gap,在存储优化上得到很大性能提升.

[1996.The Mips R10000 superscalar microprocessor](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 高体课的第1篇论文,MIPS R10000和Aplpha 21264堪称经典,后续RISC系列处理器都以此为原型.

[1992.Alternative implementations of two-level adaptive branch prediction](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 高体课的第2篇论文,两级自适应转移预测器{G,P}A{g,p/s},与当前主流的Gshare作比较.

[1995.Optimal 2-bit branch predictors](https://github.com/Hacker-vision/Tutorials/tree/master/1-paper)
- 高体课的第3篇论文,最优(0,2)转移预测器,5248种自动机方案的推导.
