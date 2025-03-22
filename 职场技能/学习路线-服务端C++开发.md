包含 linux、windows。

# C++核心

目标：扎实掌握 C++ 语法、内存管理、面向对象与泛型编程。

看书顺序

- 《C++ Primer》（第 5 版），必读经典，全面覆盖 C++11/14 特性，适合打基础。
- 《Effective C++》、《More Effective C++》，学习高效编程的 55 条准则与陷阱规避。
- 《深度探索 C++对象模型》，理解虚函数、多重继承等底层实现机制。

视频教程：

- B 站侯捷 C++系列，《C++面向对象高级开发》《STL 源码剖析》深入浅出，适合进阶。

# 核心技术

目标：掌握多线程、网络编程、系统 API 与性能优化。

看书顺序

- 《Unix 环境高级编程》（APUE）  
  Linux 系统编程圣经，涵盖进程、线程、信号、I/O 多路复用。
- 《Linux 多线程服务端编程》- 陈硕  
  基于 muduo 网络库，深入讲解高并发架构设计。
- 《Windows 核心编程》  
  专攻 Windows 系统 API、进程通信、异步 I/O（IOCP）。
- 《TCP/IP 网络编程》- 尹圣雨  
  手写 Socket 程序，理解 TCP/UDP 协议栈。

视频教程：

- B 站《Linux C/C++网络编程》（施磊）：从 Socket 到 Epoll 的实战教学。
- 《C++高性能服务器开发》（腾讯课堂）：结合 Nginx/Redis 设计思想。

# 工具与框架

## 必学工具

调试工具：
Linux：GDB、Valgrind（内存检测）、strace
Windows：WinDbg、Visual Studio 调试器

性能分析：
perf（Linux）、VTune（Intel）、WPR（Windows）

构建工具：
CMake（跨平台编译）、vcpkg（依赖管理）

推荐框架/库：

网络库：
Boost.Asio（跨平台异步 I/O）、libevent（事件驱动）

协程库：
C++20 Coroutines、libco（腾讯）

RPC 框架：
gRPC（Google）、brpc（百度）

# 进阶方向与实战

## 高并发架构

Reactor/Proactor 模型
线程池、无锁队列（Lock-free）
分布式系统基础（CAP 理论、一致性协议）

## 性能优化

CPU 缓存友好设计、内存池、零拷贝技术
锁竞争优化、系统调用瓶颈分析

## 实战项目

HTTP 服务器：支持静态文件与 RESTful API  
即时通讯系统：基于 TCP 的多线程消息转发  
分布式缓存：模仿 Redis 实现 LRU 淘汰策略

# 优质博主与社区推荐

国内博主：
陈硕（《Linux 多线程服务端编程》作者）,博客：https://blog.csdn.net/Solstice,分享 muduo 网络库设计与并发编程经验。  
左耳朵耗子（陈皓）,专栏：酷壳 - CoolShell，深入系统底层与架构设计。  
B 站 UP 主：程序员柠檬，系列视频《C++服务端开发实战》，手写开源项目。

国外资源：
Scott Meyers（《Effective C++》作者），博客：https://www.aristeia.com/  
Bartek’s Coding Blog，地址：https://www.bfilipek.com/，专注 C++17/20 新特性与性能优化。

# 社区与开源

GitHub：关注  muduo、folly（Facebook）、C++标准库源码。  
知乎专栏：搜索“C++服务端开发”话题，跟踪行业动态。  
Stack Overflow：解决具体问题的黄金平台。

# 学习路线总结

C++基础 → 内存管理 → STL → 系统编程（Linux/Windows API）→ 网络编程 → 多线程/协程 → 高并发架构 → 性能优化 → 分布式系统

# 关键提示

跨平台开发：优先学习 Linux 生态（占主流），再补充 Windows IOCP 模型。
源码学习：研究 Nginx、Redis、Kafka 等开源项目的 C/C++ 实现。
持续实践：通过 GitHub 参与开源项目或自建项目积累经验。
