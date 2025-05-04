---
title: 在Hyper-V中使用dpdk
date: 2025-05-04 13:19:33
tags: 
    - dpdk
categories: 
    - dpdk
---

# [Netvsc](https://doc.dpdk.org/guides/nics/netvsc.html)

> The Netvsc Poll Mode driver (PMD) provides support for the paravirtualized network device for Microsoft Hyper-V.

Netvsc为Hyper-V提供了半虚拟化网络设备支持。

> In this release, the hyper PMD provides the basic functionality of packet reception and transmission.

当前版本中，Netvsc提供了包收发的基本功能。

## 使用

`dpdk-devbind.py`只作用于pci设备，因此我们使用`driverctl`将指定网卡转为uio设备。

```bash
DEV_UUID=$(basename $(readlink /sys/class/net/eth1/device))
sudo driverctl -b vmbus set-override $DEV_UUID uio_hv_generic
```

在`driverctl`的文档中，


```
       set-override <DEVICE> <DRIVER>
           Set  a  driver  override for a device. By default the current driver is unbound from the de‐
           vice, the new driver is loaded into kernel, bound and the override is saved permanently.
```

这是说上述命令是永久的，但我发现至少在我的虚拟机中，重启后kernel的网络驱动仍然会接管eth1对应的网卡。

