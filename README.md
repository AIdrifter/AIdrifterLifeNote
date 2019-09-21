
# AIdrifter CS 浮生筆錄

- Self-introduction
    - Hi, I am AIdrifter, I have serviced Digital Right Management Team. Before I learned Computer Science(24), I studied Digital Arts at University. There is no doubt that I love painting. But the path that I have chosen,  I will not regret it. I clearly know to study computer science is too late for me. I will keep studying computer science until I died. Hope this note can help other guys like me, help you to find your way.
    - Hi, 我是AIdrifter，我曾經服務於DRM團隊，我大學時學習數位藝術，我熱愛繪畫，後來轉學習電腦科學領域，雖然比不上資工本科生，不過這是我曾經選擇的道路，我不曾後悔...現在會繼續研究電腦科學直到我死亡的一刻，希望在這份**浮生筆錄**上，可以幫助像我一樣的徬徨者找到追求的道路。
- My GitHub
    - [AIdrifter's GitHub](https://github.com/AIdrifter)
![](https://i.imgur.com/sgjjpvT.jpg)


-   In addition to painting, I have the another interests.
-  除了繪畫，我還有以下興趣 :
    - Trusted Execution Environment and Digital Right Management<br>(可信任執行環境與數位版權管理)
    - Program Language
    - Operation System - Linux (作業系統)
    - Debug Skill (GDB, KGDB, Coredump) (Debug 技能)
    - Linker, Loader & library (連結器， 載入器， 函示庫)- ELF的二三事
    - Algorithm And Data Structure
    - Linear Algebra and Discrete Mathematics
:::info
By the way, if you have any questions on this blog or notebook, please don't hesitate to let me know. `aidrifter@gmail.com`
:::

# Software Enginner Interview (程式面試考題)
## [C 初次試煉](https://hackmd.io/nk1U8st4TbWSFgs9H7WZFA?view#C)
- 介紹常見的面試考題，包含C語言，作業系統，資料結構等等。

## [Novatek Interview](https://hackmd.io/cqrAmIWmQ36qWUaVGixUDg?view)
- Novatek interview

## Google
### [Google Interviews Process](https://hackmd.io/0pxUeMlPTOS8tA7gFHm1XA?view)
### [Working at  Google in Taiwan:Software Engineering](https://hackmd.io/7nBDVLHNSA6CtvCa-1j6ew?view)
- gChips, Google Home, Chrome OS

## Coding Competition
### Competitive Programmer's Handbook
程式競賽入門書本，在開始歡樂練習前，先看看高手是怎麼做的。
- [Ch1 Intrdouction & Ch2 Time Complexity](https://hackmd.io/XwZN1IxxRkuclKG-V04aDA): 介紹需要處理的stdin, stdout與Time Complexiy。
- [Ch3 Sorting & Ch4 Data Structure](https://hackmd.io/sn6A_9mtTHCIlYbht-Qq5g): Sorting的概念與 C++ STL 常用的data structure。
    - vector, string, set, map ,iteraotr ...
    - 可以參考下面的 [C++ Standard libraries(STL)](https://hackmd.io/ww2WwMVDRZWo9CMm2FgtYA?view) 一起學習。
- [Ch5 Complete Search](https://hackmd.io/UGVQfwejSx27MkAnm9ATrQ) : 窮舉法
    - 介紹窮舉法的概念，如何把利用pruning the search把不要的分支去掉。
- [Ch6 Greedy Algorithms](https://hackmd.io/gesAEePtTvOEFyGSYG8aRA) : 貪婪演算法
- [Ch7 Dynamic Programming](https://hackmd.io/dstUYddbQAmJgqMwB7f5dA?view) : 動態規劃
- [Ch8 Amortized anaysis](https://hackmd.io/YhW2PUHDQRmfgEHS7Nqy9w) : 平攤分析
    - 平攤分析常用於分析**資料結構**（動態的資料結構），在使用平攤分析前須知道資料結構各種**操作所可能發生的時間**，並計算出最壞情況下的操作情況並加以平均。
- [Ch9 Range Queries ](https://hackmd.io/3w3X8BrFTB664DNYJ5hVcA) : Range Queries
    -  陣列中元素求Max, Sum, Min的各種演算法，Segment Tree在所有case皆可適用。
- [Ch10](https://hackmd.io/82U7wkL0QjG32ZQgYTvqdw) : Bit Manipulation
    - 先從基本的uint32_t 和int32_t 去介紹二進制的相關性質(overflow處理)與數學等式，經典的Hamming distance與Couting subgrids，然後帶到集合的相關操作。
    - 集合方面: 如何利用位元表示法去有效的處理集合相關問題，像是permutations, counting(combination) ，最後套用在DP上面，實際演練如何減少時間複雜度。
    - 其實很多DP等式，都很接近以前黃子嘉說的"小黑的故事"，將等式分成抓小黑，和不抓小黑兩種case，和排容原理有異曲同工之妙。

### [花花醬 Alog/DS 特輯](https://hackmd.io/cgUxxnrKTQi7vQb78i6llg?view)
### [Time Complexity](https://hackmd.io/hc-SpsfRQy-jF206dFJBjA)
- Define Big-O (Asymptotic Notation)
### [Sorting and Time Complexity](https://hackmd.io/tSqE-8_9QC-PyuR67q21dw)
- Basic
    - Bubble, Shell ,Insertion, Select
- Advanced
    - Heap, Merged, Quick

- Time Complexity : 各種sorting 的 insert ,lookup ,delete

### [C++ Standard libraries(STL)](https://hackmd.io/ww2WwMVDRZWo9CMm2FgtYA?view)
- 介紹STL的主角 **Container**, **Iterator**, **Algorithms** 並對Container分類
    - Sequence Container
    - Associative Container
    - Unorder Container
    - iterator and algorithms
    - Functor

Reference:
[Cracking the Coding Interview, 6th Edition 189 Programming Questions and Solutions](https://drive.google.com/open?id=1c1YbcHQlds9RfwiL9CmjFNQtmaoEFM6l)
    - 一本關於程式設計師面試的好書
[Introduction of STL #1: Overview](https://www.youtube.com/watch?v=ltBdTiRgSaw&t=1s)
作者：**Bo Qian** 說的是英文，但是簡單好懂，看得出其功力深厚。


## Program Language
### [RD Rule For Coding Style](https://hackmd.io/s/Hy1oNTpzQ#)
- What's different between student and engineer
- Good coding style
- Memory layout
### [Introduce C++11 from 良葛格(精簡版)](https://hackmd.io/YyK_6P6qQNmk0yNG2_VRUw#)
- [Deprecating Raw Pointers in C++20 ](https://www.bfilipek.com/2018/04/deprecating-pointers.html#)
- Practice Web : [hackerrank](https://www.hackerrank.com/domains/cpp)
### Design Pattern
- [阿洲的程式教學](http://monkeycoding.com/?page_id=899)
- Android DRM 採用
    - [簡單工廠模式(Simple Factory Pattern)](http://monkeycoding.com/?p=967)
    - [單例模式(Singleton Pattern)](http://monkeycoding.com/?p=969)
- Android Design pattern
    - Adapter
    - Registry of Singleton
    - Bridge
    - Observer
    - Factory Method



### 21st C
- 21st Century C: C Tips from the New School
- [2016 年，現代 C 語言的寫法](https://hackmd.io/s/B1w3d7nQQ)
- [Pointer, Address, and String](https://hackmd.io/s/rktthgCzQ#)

### [API Changed Rule](https://hackmd.io/s/B1YWw22QQ#)
- API changed rule

# Trusted Execution Environment (TEE)
- DRM(Digital Right Management)是一系列**存取控制Digital Content技術**的集合。Amazon、AT&T、AOL、Apple Inc.、Google、MicroSoft , Sony和Valve Corporation公司都使用了數位權利管理。
    - DRM主要兩大陣營Googloe(Widevine)，Micorsoft(PlayReady)
    - 都需要搭配**TEE** or **Secureted OS** 已保護其Digital Contnet，重要的資料都放在該OS的memory上:
        -  1.Key(AES content key or ECC private key or public key)
        -  2.Encrypted Digital Content

## [Trusted Execution Environment (TEE)](https://hackmd.io/s/SkxznuX-Q)
- 介紹TEE運作的方法

## [Op-tee](https://hackmd.io/qUbB_zlvSDqwYXNtsOawJg?view)
- 介紹optee 運作原理 還有如何trace optee source code

## [Android MediaDRM FrameWork](/1PbFLmLjR6eXwZ9F7tMEFw)
- Introduce Android MediaDRM Freame Work

# Linux
## Shell Script
- [Common Linux Command](https://hackmd.io/s/HJeWQwIrl7)
    - 推薦的常用shell commnad，還有技巧如何加快速度。
- **進階** : [How to debug and trace source code on bash](https://hackmd.io/s/BJ16jxN8Q#)

## [Kconfig](https://hackmd.io/s/r1jKDbyVm)
- 在核心配置 make menuconfig(或 xconfig 等)時，從 Kconfig 中讀出功能表，用戶選擇後保存到 .config的核心配置文件中。 在核心編譯時，主Makefile 調用這個 .config，就知道了用戶的選擇。
## [pthread](https://hackmd.io/nDBmYjfyR6y8h5ZzqKkrlg?view)
- 介紹POSIX pthread用法
## Profile
- [Linux 系統下常見的profile方法](https://hackmd.io/s/BklO1V1NIX)
    - 系統的bottleneck一直是開發者頭痛的問題，這邊介紹常見的profile方法

## Memory
- [Memory Alignment 記憶體對齊](http://opass.logdown.com/posts/743054-about-memory-alignment)
    - Reference: OPASS'S BLOG

## Multi-thread and Synchronization in Linux
- [Multi-thread Safe Problem](https://hackmd.io/McOLfJwIRXWnJLKmiEgfww)
    - What's difference between Process and Thread?
    - Create prcess and wait Process
    - Linux `fork()` vs Windows MFC
    - Multi thread
        - Thread safe problem
        - Double free problem
        - Volatile
        - Cache line problem

- [Thread Synchronization in Linux](https://hackmd.io/n69X1HA7TNeDS76c3HTK4w#)
    - Create proecess and wait process
    - Create thread and wait thread
    - Mutex
    - Shared Exclusive Lock
    - File Lock
    - Event
    - Semaphore

## [IPC in Linux](https://hackmd.io/eh8T4Z63T6WkWZSV9qGKhQ?view)
- 常見的IPC 有八種方法
- summary 彼此的差異


## Linux kernel
- [How to resolve Kernel CoreDump](https://hackmd.io/s/HJtq-qQbQ)
- [TW Linux Kernel Hackers](https://hackmd.io/c/Hkhqgrr1Z/https%3A%2F%2Fhackmd.io%2Fs%2FBkoD8EEWz)
    - **TW Linux Kernel Hacker**，FB這邊有社群可以加入喔 :) ，兩周大概會在台北有視訊。跟**新竹碼農**差不多。
    - [新竹碼農](https://hcsm.kktix.cc/)  [地點:TAB Tee and Beverage / 新竹市 大學路78號]

    - [也來寫Github好了 Github 裝屌指南](https://hackmd.io/p/ryuud0Jc#/)

- book list
    - Linux Kernel Hacks 改善效能、提昇開發效率及節能的技巧與工具
    - Binary Hacks－駭客秘傳技巧一百招

- [From Socket API to Kenrel](https://hackmd.io/s/H1ZBRbYTx#)

## Network Problem
- [常見網路的疑難雜症 for Linux](https://hackmd.io/FArCD2IjQdyNso-NxXUUvQ#)

# Android
## [Android O](https://hackmd.io/s/BkQWA_g7m)
- Android Trerble, extra `vendor.img`

## [Android MediaDRM FrameWork](/1PbFLmLjR6eXwZ9F7tMEFw)
- Introduce Android MediaDRM Freame Work

# Debug Skill
學會了 GDB，我有種山頂洞人學會用火的感動」 – 張至
- 只用 `printf()` 觀察的話，永遠只看到你設定的框架 (format string) 以內的資料，但很容易就忽略資料是否合法、範圍是否正確，以及是否看對地方

## GDB : Linux, Android, KGDB ...
- [How to use GDB on Android O](https://hackmd.io/s/Byx4jE5UZX#)
    - Android use `python` script and `adb` to debug
- [How to use Remote GDB on Linux](https://hackmd.io/s/SJUoLkL-m#)
    - Remote GDB for linux system
- [How to use GDB to adjust mulit-threads ](https://www.ibm.com/developerworks/cn/linux/l-cn-gdbmp/index.html)
![](https://i.imgur.com/7IQkkpM.png)
    - **follow-fork-mode方法**：方便易用，對系統內核和GDB版本有限制，適合於較為簡單的多進程系統
    - **attach**子進程方法：靈活強大，但需要**添加額外代碼**，適合於各種複雜情況，特別是守護進程
    - **GDB wrapper**方法：專用於fork+exec模式，不用添加額外代碼，但需要X環境支持（xterm/VNC）。

- KGDB
    - For kenerl debug (To be continued ...)

## 簡易CoreDump排除
- [Kernel CoreDump](https://hackmd.io/s/HJtq-qQbQ)
- [User Space CoreDump](https://hackmd.io/s/B1ViiB3Gm)

## 人民救星GDB COSCUP 2019
身為工程師，有一個好的debug環境非常重要，這是一篇適合初學者的入門，還在猶豫要不要學GDB嘛？
趕快來看看這篇吧：
- [How to debug using GDB](https://docs.google.com/presentation/d/1EeOp-jxZheS_8N1axdmcu9Nz3OEFNpl55mrhl4Uoy50/edit#slide=id.p)

### Debug Hacks 除錯駭客－極致除錯的技巧與工具
- [Ch1 暖身準備](https://hackmd.io/s/ry4fQjO4m)
- [Ch2 Debug前該知道的事](https://hackmd.io/s/HkpxVi_NX)
<!---
- [Ch3 Kenrel Debug的準備](https://hackmd.io/s/Hk8b8sO47)
- [Ch4 應用程式 Debug 實務](https://hackmd.io/s/Hk60XjOEX)
- [Ch5 Kernel Debug 實務](https://hackmd.io/s/BkwABid4X)
- [Ch6 致勝Debug技巧](https://hackmd.io/s/rkgXEsdNQ)
-->
# Linker, Loader, Libraries ELF的二三事
## [GNU Binary Utilities](https://hackmd.io/1Bjay1ChRNyFioo-aCRElA#)
- 利用`objdump`,`readelf`,`nm`,`objcopy`,`ld`, `lld` 搭配**cross compiler** 完成對elf的操作...

## [Makefile 常見語法](https://hackmd.io/@iST40ExoQtubds5LhuuaAw/BJqZPne1B)
- 字串處理相關語法

## [如何處理 undefined reference](https://hackmd.io/KVZ9oMhZThKMAO1-ZSIXQQ#)
- 基本上這邊已經是linking 的問題了。
- [Memory Errror Detection Using GCC](https://developers.redhat.com/blog/2017/02/22/memory-error-detection-using-gcc/)

## [GCC compiler option](https://hackmd.io/pqmAOr6SSDmc_WP3RveuRQ?view)

## booklist
- [The GNU Binary Utilities(book)](https://gcc-renesas.com/gnu-tools-manuals/gnu-binary-utilities/)

# Crypto
## [What's digital signature(數位簽名) and digital certificate（數位證書)](https://hackmd.io/aUKU8xkGSGyYDfHKqEM4iQ)
- Introduce digital signature(數位簽名) and digital certificate（數位證書）
## Symmetric vs Asymmetric
- 介紹對稱式(symmetric)與非對稱式(asymmetric)的差異
<!---
# Penetration attack
## Buffer Overflow Attack
- [ROBOT(Return Of Bleichenbacher's Oracle Threat)](https://hackmd.io/s/HywoOGQmQ#Abstract)
    - 針對 **TLS + RSA** 的攻擊
- [滲透測試工程師面試題大全](https://lonelysec.com/?p=782)


## Artificial Intelligence
### Introduce Artificial Intelligence
- AI/ML/DL overview
- Key Market Overview
- Usage case and Market Trend
- Deep learning Framework Introduction
- AI Software Architecture Introduction
- AI Hardware Architecture Introduction


# COSCUP
## 開源專案
- 我的linux  kernel 之旅
    - https://mp.weixin.qq.com/s/Dgq35AUIBy5nMUKPBwR5ag
- https://2018.coscup.org/tracks/misc/
- [Arch Linux Taiwan & Archers](https://2018.coscup.org/tracks/arch/)
- [Kernel & Coding Serfs & System](https://2018.coscup.org/tracks/kernel/)

## Let’s read the source code / 帶您讀源碼
- [Learn Bash - 批次檔除錯及排版工具](https://hackmd.io/c/coscup18-source/%2Fcoscup18-source-shell)
    - Bash alternative to Reflector for Ranking Mirrors
    - Author: Daniel Lin 林原志

- [用1500 行建構可自我編譯的 C 編譯器](https://hackmd.io/c/coscup18-source/%2Fcoscup18-source-c-compiler)
    - Author: 翁敏維

- [有效清理 Python Code 中的無用部份 - 利用 Python AST](https://hackmd.io/c/coscup18-source/%2Fcoscup18-source-python-ast)
    - Author: PCMan

- [Reade source cdoe : kubertnets apiserve](https://hackmd.io/c/coscup18-source/%2Fcoscup18-source-kubernetes-apiserver)
    - Need PowerPoint
    - Reference: David Chang


## opens source development and management
- [瀏覽器開發與開源經驗] (https://github.com/tigercosmos/COSCUP2018)
    - Reference: 微中子
- [neovim](https://2018.coscup.org/tracks/oss/)

--->
# Windows Tools
## EverythingPortable
- Everything 小巧實用而最大優點是搜尋速度與準確性!
        - Small installation file
        - leann and simple user interface
        - Quick file indexing
- Reference:[voidtools](http://www.voidtools.com/)
## XShell6
- windows上面最好用的吧 功能比putty強大 但是因為支援自身script
- 有時候uart亂碼會被判定schell script 導致開機出現一些問題
- Uart 請用 ==teraterm==

## Process Explorer
- show process information on windows
- set process priority and kill it => do anything you want

## Visualize source code
- support `C`, `C++`, `python`, `java`, `javascript`,... etc
- visualize your source code and trace
Reference: [Visualize source code](http://www.pythontutor.com/visualize.html#mode=edit)


