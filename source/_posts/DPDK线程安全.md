---
title: DPDK线程安全
date: 2025-05-07 01:32:10
tags: 
    - dpdk
categories: 
    - dpdk
---

*用人话总结一下dpdk文档中对[线程安全](https://doc.dpdk.org/guides/prog_guide/thread_safety.html)的描述。*

# Thread Safety

DPDK的rte一般都是per lcore的。无论从性能角度（锁、cache等），还是从开发角度，我们最好尽可能地避免跨线程共享数据结构。

## Fast-Path APIs

哈希、LPM和内存池**不是**线程安全的。

PMD的rx和tx也**不是**线程安全的（单个队列）。

## 性能不敏感的APIs

除了[Fast-Path APIs](#Fast-Path-APIs)，其他绝大多数库函数都**是**线程安全的。

但PMD的配置过程除外，它们**不是**线程安全的。

## 库初始化

DPDK会确保库只被初始化了一次。如果初始化了多次会返回错误。

## 信号安全

部分DPDK的回调函数可能不会在DPDK的处理线程中被调用，这些回调函数应该避免修改DPDK的数据结构。

