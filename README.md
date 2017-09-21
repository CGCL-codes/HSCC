# HSCC---Hardware/Software Cooperative Caching for Hybrid DRAM/NVM Memory Architectures

&#160; &#160; &#160; &#160;HSCC is implemented with zsim and NVMain simulators.  Zsim is a fast x86-64 multi-core simulator. It exploits Intel Pin toolkit to collect traces of memory accesses for processes, and replays the traces in the zsim simulator. NVMain is a cycle-accurate memory simulator, it models components of DRAM and NVMs, and memory hierarchy in detail. The integrated "zsim + NVMain" simulators can be forked from "https://github.com/AXLEproject/axle-zsim-nvmain". 

Based on the "zsim + NVMain" hybrid simulator, HSCC has added the following functions:

 * **Memory management simulations (such as MemoryNode, Zone, Buddy allocator etc.)**:   As the pin-based zsim only replays virtual address in the x86 system architecture, and does not support OS simulation, HSCC has added memory management modules into zsim, including memory node, zone and buddy allocator.


 * **TLB simulation:** The original "zsim + NVMain" simulator does not simulate the TLB. TLB simulation is implemented in zsim to accelerate address translations from virtual address to physical address.

 
 * **Implementation of HSCC, a hierarchical hybrid DRAM/NVM memory system that pushes DRAM caching management issues into the software level:** DRAM cache is managed by hardware in tranditional DRAM/NVM hierarchical hybrid systems. HSCC is a novel software-managed cache mechanism that organizes NVM and DRAM in a flat physical address space while logically supporting a hierarchical memory architecture. This design simplifies the hardware design by pushing the burden of DRAM cache management to the software layers. Besides, HSCC only caches hot NVM pages into the DRAM cache to mitigate potential cache thrashing and bandwidth waste between DRAM cache and NVM main memory.
 
 
 * **Multiple DRMA/NVM hybrid architecture supports:** HSCC supports both DRAM/NVM flat-addressable hybrid memory architecuture and DRAM/NVM hierarchical hybrid architecture. As shown in following figure, both DRAM and NVM are used as main memory and managed by OS in a single flat address space. In DRAM/NVM hierarchical hybrid memory architecture, DRAM is exploited as a cache to the NVM, and hardware-assisted hit-judgement mechanism is implemented to determine whether data is hit in DRAM cache. Besides, to reduce hardware overhead, DRAM cache is organized set-associative and usually uses demand-based caching policies.
![Image of Yaktocat](https://github.com/cyjseagull/SHMA/blob/master/images/DRAM-NVM_architectures.png)
 
 
 *  **Multiple DRAM/NVM hybrid system optimization policies:** We have implemented Row Buffer Locality Aware (RBLA) page caching policy and MultiQueue-based (MultiQueue) page migration policy in DRAM/NVM flat addressable hybrid memory system. RBLA caching policy is a simple implementation of hybrid memory system proposed in the paper "**Row Buffer Locality Aware Caching Policies for Hybrid Memories**", MultiQueue migration policy is a simple implementation of system proposed in the paper "**Page Placement in Hybrid Memory Systems**". RBLA caching policy is aimed at migrating NVM pages with poor row buffer locality to DRAM since row buffer miss of NVM pages incur higher overhead than that of DRAM pages. The MultiQueue migration policy places hot NVM pages into DRAM, and MQ algorithm is used to update the hotness of pages based on time locality and access frequency.


The architecture and modules of HSCC are shown in the following figure:
![Image of Yaktocat](https://github.com/cyjseagull/SHMA/blob/master/images/simulator_architecture.png)


Origianl License & Copyright of zsim
--------------------
zsim is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, version 2.

zsim was originally written by Daniel Sanchez at Stanford University, and per Stanford University policy, the copyright of this original code remains with Stanford (specifically, the Board of Trustees of Leland Stanford Junior University). Since then, zsim has been substantially modified and enhanced at MIT by Daniel Sanchez, Nathan Beckmann, and Harshad Kasture. zsim also incorporates contributions on main memory performance models from Krishna Malladi, Makoto Takami, and Kenta Yasufuku.

zsim was also modified and enhanced while Daniel Sanchez was an intern at Google. Google graciously agreed to share these modifications under a GPLv2 license. This code is (C) 2011 Google Inc. Files containing code developed at Google have a different license header with the correct copyright attribution.

Additionally, if you use this software in your research, we request that you reference the zsim paper ("ZSim: Fast and Accurate Microarchitectural Simulation of Thousand-Core Systems", Sanchez and Kozyrakis, ISCA-40, June 2013) as the source of the simulator in any publications that use this software, and that you send us a citation of your work.


License & Copyright of HSCC ([HUST SCTS & CGCL Lab](http://grid.hust.edu.cn/))
-------------------------
HSCC is implemented by Yujie Chen, Dong Liu and Haikun Liu at Cluster and Grid Computing Lab & Services Computing Technology and System Lab in Huazhong University of Science and Technology([HUST SCTS & CGCL Lab](http://grid.hust.edu.cn/)), the copyright of this HSCC remains with CGCL & SCTS Lab of Huazhong University of Science and Technology.

## Citing HSCC

If you use HSCC, please cite our reearch paper published at ICS 2017, included as doc/HSCC.pdf.

**Haikun Liu, Yujie Chen, Xiaofei Liao, Hai Jin, Bingsheng He, Long Zhen and Rentong Guo, Hardware/Software Cooperative Caching for Hybrid DRAM/NVM Memory Architectures, in: Proceedings of the 31st International Conference on Supercomputing (ICS'17), Chicago, IL, USA, June 14-16, 2017**
```javascript
@inproceedings{Liu:2017:HCC:3079079.3079089,
 author = {Liu, Haikun and Chen, Yujie and Liao, Xiaofei and Jin, Hai and He, Bingsheng and Zheng, Long and Guo, Rentong},
 title = {Hardware/Software Cooperative Caching for Hybrid DRAM/NVM Memory Architectures},
 booktitle = {Proceedings of the International Conference on Supercomputing},
 series = {ICS 2017},
 year = {2017},
 isbn = {978-1-4503-5020-4},
 location = {Chicago, Illinois},
 pages = {26:1--26:10},
 articleno = {26},
 numpages = {10},
 url = {http://doi.acm.org/10.1145/3079079.3079089},
 doi = {10.1145/3079079.3079089},
 acmid = {3079089},
 publisher = {ACM},
 address = {New York, NY, USA},
 keywords = {caching, hybird memory, non-volatile memory (NVM)},
}
```

Setup,Compiling and Configuration
------------
**1.External Dependencies**  
&#160; &#160; &#160; &#160;Before install hybrid simulator zsim-nvmain, it's essential that you have already install dependencies listing below.
* gcc(>=4.6)
* Intel Pin Toolkit(Already integrates in project, or you can download from [pin-2.13-61206-gcc.4.4.7-linux](http://software.intel.com/sites/landingpage/pintool/downloads/pin-2.13-61206-gcc.4.4.7-linux.tar.gz))
* [boost](http://www.boost.org/users/history/version_1_59_0.html) (version 1.59 is ok)
* [libconfig](http://www.hyperrealm.com/libconfig/libconfig-1.5.tar.gz)
* [hdf5](https://www.hdfgroup.org/ftp/HDF5/releases/)


**2.Compiling**
* Update environment script env.sh according to your machine configuration
```javascript
#!/bin/sh
PINPATH= path of pin_kit
NVMAINPATH= path of nvmain
ZSIMPATH= path of zsim-nvmain
BOOST= path of boost
LIBCONFIG= path of libconfig
HDF5=path of hdf5
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PINPATH/intel64/lib:$PINPATH/intel64/runtime:$PINPATH/intel64/lib:$PINPATH/intel64/lib-ext:$BOOST/lib:$HDF5/lib:$LIBCONFIG:/lib
INCLUDE=$INCLUDE:$HDF5/include:$LIBCONFIG:/include
LIBRARY_PATH=$LIBRARY_PATH:$HDF5/lib
CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:$HDF5/include
export ZSIMPATH PINPATH NVMAINPATH LD_LIBRARY_PATH BOOST CPLUS_INCLUDE_PATH LIBRARY_PATH
```
* Compiling and Installation
```javascript
[root @node1 HSCC]# cd zsim-nvmain
[root @node1 zsim-nvmain]# source env.sh  //init environmental values
[root @node1 zsim-nvmain]# scons -j16    //compiling, -j16 represents that compiling with 16 cores
```
If error "could not exec $PINPATH/intel64(ia32)/bin/pinbin" happens, it means that you are not authorized to execute pinbin, this can be solved with the following command:
```javascript
[root @node1 zsim-nvmain]# chmod a+x $PINPATH/intel64(ia32)/bin/pinbin 
```

* **Using a virtual machine**  
 If you use another OS, can't make system-wide configuration changes, or just want to test zsim without modifying your system, you can run zsim on a Linux VM. We have included a vagrant configuration file (http://vagrantup.com) that will provision an Ubuntu 12.04 VM to run zsim. You can also follow this Vagrantfile to figure out how to setup zsim on an Ubuntu system. Note that zsim will be much slower on a VM because it relies on fast context-switching, so we don't recommend this for purposes other than testing and development. Assuming you have vagrant installed (sudo apt-get install vagrant on Ubuntu or Debian), follow these steps:
Copy the Vagrant file to the zsim root folder, boot up and provision the base VM with all dependencies, then ssh into the VM.
```javascript
[root @node1 zsim-nvmain]# cp misc/Vagrantfile .
[root @node1 zsim-nvmain]# vagrant up
[root @node1 zsim-nvmain]# vagrant ssh
```
Vagrant automatically syncs the zsim root folder of your host machine to /vagrant/ on the guest machine. Now that you're in the VM, navigate to that synced folder, and simply build and use zsim (steps 5 and 6 above)
```javascript
[root @node1 zsim-nvmain]# cd cd /vagrant/
[root @node1 zsim-nvmain]# scons -j4
```


**3.zsim Configuration Keys** (example zsim configuration files is in zsim-nvmain/config directory)
* **Enable TLB、Page Table and Memory Management Simulation**  
**(1) sys.tlbs.tlb_type**: type of TLB, default is "CommonTlb","HotMonitorTlb" enables HSCC policy;  
**(2) sys.tlbs.itlb(dtlb): prefix for configuring instruction/data TLB**  
① **entry_num**: Number of TLB entries, default is 128;  
② **hit_lantency**: Latency(cycles) of TLB hit, default is 1cycle;  
③ **response_latency**: TLB response latency(cycles) to CPU, default is 1cycle;  
④ **evict_policy**: evict policy, default is "LRU";  
**(3) sys.pgt_walker( page table walker configuration)**  
① mode: paging mode configuration, HSCC supports seven paging modes, namely, **Legacy_Normal**(4GB address space, page size is 4KB), **Legacy_Huge**(4GB address space, page size is 4MB), **PAE_Normal**(64GB address space, page size is 4KB),**PAE_Huge**(64GB address space, page size is 2MB),**LongMode_Normal**(address length is 48 bits,page size is 4KB), **LongMode_Middle**(address length is 48 bits, page size is 2MB) and **LongMode_Huge**（address length is 48bits, page size is 1GB);  
② itlb: instruction TLB name corresponding to this page table walker;  
③ dtlb: name of data TLB corresponding to this page table walker;   
④ reversed_pgt:  true, enable  reversed  page table; false, disable  reversed page table; when simulating single process, default is false; while simulating multiple processes, default is true;

**(4) sys.mem.zone: memory management configuration** 
**zone_dma/zone_dma32/zone_normal/zone_highmem**: set OS zone size(MB)  
**(5) sys.enable_shared_memory**: true, enable shared memory simulation ( default is true )

* **Enable Simpoints**    
**(1) configuration key**  
startFastForwarded=true   
simPoints=directory of simpoints  
**(2) how to get simpoints**  
create .bb files with valgrind: cmd is execution command of the executable programs   
```javascript
 valgrind --tool=exp-bbv --interval-size=<instructions of a simpoint,example:1000000000> <cmd> 
```  
get simpoints with .bb files with SimPoint(get from https://github.com/southerngs/simpoint)  
```javascript
simpoint -k <simpoints num> -loadFVFile <path of .bb files> -saveSimpoints <file store generated simpoints> -saveSimpointWeights <file store weights of generated simpoints> -sampleSize <instructions of a simpoint, eg:1000000000>
```
**(3) format of simpoints**  
(the first simpoint period) 0  
(the second simpoint period) 1  
... ...  
(the ith simpoint period) i-1  
... ...  
example( simpoint file of msf with 31 simpoints):
```javascript
38 0
19 1
64 2
13 3
58 4
43 5
55 6
10 7
14 8
39 9
15 10
30 11
9 12
42 13
24 14
4 15
0 16
12 17
48 18
0 21
1 22
2 23
3 24
4 25
5 26
6 27
7 28
8 29
9 30
```   

* **HSCC Related Configuration**(example in zsim-nvmain/config/shma.cfg)  
**(1) sys.tlbs.tlb_type**: must be set to be "HotMonitorTlb";  
**(2) sys.init_access_threshold**: set initial value of fetching_threshold, default is 0;  
**(3) sys.adjust_interval**: period of adjusting fetching_threshold automatically, defalut is 10000000 cycles (1000cycles is basic units);
**(4) sys.mem_access_time**: cycles of per memory access caused by page table walking;
**4.nvmain Configuration Keys** (example nvmain configuration files is in zsim-nvmain/config/nvmain-config directory)
* **Enabling DRAM-NVM hierarchical hybrid architecture**( zsim-nvmain/config/nvmain-config/hierarchy)  
**(1) EventDriven**: true;  
**(2) CMemType**: set physical memory type, is **HierDRAMCache** in hardware managed DRAM Cache hybrid memory system;  
**(3) MM_Config**: configuration file of NVM main memory;  
**(4) DRC_CHANNEL**: configuration file of DRAM Cache;  


*  **Enabling HSCC policy in DRAM-NVM hierarchical hybrid architecture**(zsim-nvmain/config/nvmain-config/shma)  
**(1) EventDriven**:true;  
**(2) ReservedChannels**: number of DRAM cache channels;  
**(3) CONFIG_DRAM_CHANNEL**: configuration file of every DRAM cache channel;  
**(4) CONFIG_CHANNEL**: configuration files of every NVM main memory channel;  
**(5) CMemType**: physical memory type, is **FineNVMain** in HSCC;  
**(6) DRAMBufferDecoder**: DRAM cache decoder type, is **BufferDecoder** in HSCC;


*  **Enabling RBLA policy in DRAM-NVM hybrid architecture**(zsim-nvmain/config/nvmain-config/rbla)  
**(1) EventDriven**:true;  
**(2) Decoder**: physical decoder object, is **Migrator** in RBLA;  
**(3) PromotionChannel**: channel id of fast memory (DRAM);  
**(4) CMemType**: physical memory type, is **RBLANVMain** in RBLA;  
**(5) CONFIG_CHANNEL**: configuration file path of every main memory channel;  


*  **Enabling MultiQueue policy in DRAM-NVM hybrid architecture**(zsim-nvmain/config/nvmain-config/mq)  
**(1) EventDriven**: true;  
**(2) AddHook**: hook type, is "MultiQueueMigrator" in MultiQueue policy based hybrid memory system;  
**(3) Decoder**: decoder type, is "MQMigrator" in MultiQueue policy based hybrid memory system;  
**(4) PromotionChannel**: channel id of fast memory(DRAM);  
**(5) CONFIG_CHANNEL**: configuration file path of every main memory channel;  

* **Enabling Flat DRAM-NVM hybrid   architectures**(zsim-nvmain/config/nvmain-config/rbla)  
**(1) FAST_CONFIG**: configuration file of fast memory (eg DRAM)  
**(2) SLOW_CONFIG**: configuration file of slow memory (eg NVM)  
**(3) Decoder**: ***FlatDecoder**, decoder of flat DRAM-NVMA hybrid architecture  
**(4) CMemType**:**FlatRBLANVMain**, memory type  


TLB, Page Table and Memory Management Simulation Modules
-----------------------
&#160; &#160; &#160; &#160;As described above, original zsim doesn't support OS simulation, and HSCC has added TLB, page table and memory management simulation into zsim. The major modifications are shown in the following figure. The left side presents the major code of original zsim corresponding to system simulation, **the right side describes HSCC modifications to zsim for TLB, page table and memory management simulation support.**
![Image of Yaktocat](https://github.com/cyjseagull/SHMA/blob/master/images/zsim_modification.png)

Architecture of HSCC
---------------------------
&#160; &#160; &#160; &#160; HSCC has extended both page table and TLB to maintain both virtual-to-NVM and NVM-to-DRAM address mappings. HSCC also develops an utility-based DRAM caching policy that only fetching hot pages into DRAM cache when the DRAM is under high pressure to reduce DRAM cache pollution. HSCC also supports DRAM cache bypassing mechanism. The following figure shows the architecture of HSCC.![Image of Yaktocat](https://github.com/cyjseagull/SHMA/blob/master/images/SHMA_architecture.png)

Implementations of RBLA and MultiQueue Policies
----------------------------
* **Row Buffer Locality Aware Migrator (RBLA)**  
&#160; &#160; &#160; &#160;RBLA migrates NVM pages with poor row buffer locality to DRAM, and keeps pages with good row buffer locality in NVM to gain the benefit of row buffer hit. The implementation is shown in the following figure:![Image of Yaktocat](https://github.com/cyjseagull/SHMA/blob/master/images/RBLA.png)


* **hot page migrator based on MultiQueue Alogrithm (MultiQueue)**  
&#160; &#160; &#160; &#160;MultiQueue classifies NVM pages into hot pages and cold pages using multiqueue algorithm accroding to both page access frequency and time locality. Its implementation is shown in the following figure:![Image of Yaktocat](https://github.com/cyjseagull/SHMA/blob/master/images/MultiQueue.png)

* **Architecture of flat memory supporting different channel configurations of DRAM and NVM**  
&#160; &#160; &#160; &#160;Considering that DRAM and NVM with different channel configurations have the overlapping address space in the low end, we divide the continuous overlapped address space into {channel_nums} and mapping them to different address space interleavingly to make full use of channel parallization.

## Support or Contact
HSCC is developed at SCTS&CGCL Lab (http://grid.hust.edu.cn/) by Yujie Chen, Haikun Liu and Xiaofei Liao. For any questions, please contact Yujie Chen(yujiechen_hust@163.com), Haikun Liu (hkliu@hust.edu.cn) and Xiaofei Liao (xfliao@hust.edu.cn). 
 
