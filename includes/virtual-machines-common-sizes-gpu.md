---
title: include 文件
description: include 文件
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/23/2018
ms.author: danlep;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 4fde34338d5606a1f431ff4b7f7074d9cd472e90
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "50035273"
---
GPU 优化 VM 大小是具有单个或多个 NVIDIA GPU 的专用虚拟机。 这些大小是针对计算密集型、图形密集型和可视化工作负荷设计的。 本文介绍有关 GPU、vCPU、数据磁盘和 NIC 的数量和类型的信息。 此分组中的每个大小还包括存储吞吐量及网络带宽。 

* NC、NCv2、NCv3 和 ND 的大小针对计算密集型和网络密集型应用程序和算法进行了优化。 一些示例包括基于 CUDA 和 OpenCL 的应用程序和模拟、AI 和深度学习。 NCv3 系列专用于采用 NVIDIA Tesla V100 GPU 的高性能计算工作负载。  ND 系列专用于进行深度学习的培训和推理方案。 该系列使用 NVIDIA Tesla P40 GPU。
* **NV 和 NVv2** 大小已使用 OpenGL 和 DirectX 之类的框架针对远程可视化效果、流式处理、游戏、编码和 VDI 方案进行了优化和设计。  这些 VM 由 NVIDIA Tesla M60 GPU 提供支持。


## <a name="nc-series"></a>NC 系列

高级存储：不支持

高级存储缓存：不支持

NC 系列 VM 采用 [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) 卡。 通过将 CUDA 用于能源勘探应用、碰撞模拟、光纤跟踪渲染、深度学习等领域，用户可以更快地分析数据。 NC24r 配置提供了针对紧密耦合的并行计算工作负荷优化的低延迟、高吞吐量网络接口。


| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 8 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 | 64 | 4 |

1 GPU = 半块 K80 卡。

*支持 RDMA

## <a name="ncv2-series"></a>NCv2 系列

高级存储：支持

高级存储缓存：支持

NCv2 系列 VM 采用 [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU。 这些 GPU 可提供比 NC 系列高 2 倍以上的计算性能。 客户可将这些更新的 GPU 用于传统的 HPC 工作负荷，例如油藏模拟、DNA 测序、蛋白质分析、Monte Carlo 模拟和其他工作负荷。 NC24rs v2 配置提供了针对紧密耦合的并行计算工作负荷优化的低延迟、高吞吐量网络接口。

> [!IMPORTANT]
> 对于此大小系列，订阅中的 vCPU（核心）配额在每个区域中最初都设置为 0。 可以请求在某个[可用区域](https://azure.microsoft.com/regions/services/)中[提高此系列的 vCPU 配额](../articles/azure-supportability/resource-manager-core-quotas-request.md)。
>

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | ---  | --- |
| Standard_NC6s_v2 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 2948 | 4 | 64 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = 一个 P100 卡。

*支持 RDMA

## <a name="ncv3-series"></a>NCv3 系列

高级存储：支持

高级存储缓存：支持

NCv3 系列 VM 采用 [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU。 这些 GPU 可提供 NCv2 系列的 1.5 倍计算性能。 客户可将这些更新的 GPU 用于传统的 HPC 工作负荷，例如油藏模拟、DNA 测序、蛋白质分析、Monte Carlo 模拟和其他工作负荷。 NC24rs v3 配置提供了针对紧密耦合的并行计算工作负荷优化的低延迟、高吞吐量网络接口。

> [!IMPORTANT]
> 对于此大小系列，订阅中的 vCPU（核心）配额在每个区域中最初都设置为 0。 可以请求在某个[可用区域](https://azure.microsoft.com/regions/services/)中[提高此系列的 vCPU 配额](../articles/azure-supportability/resource-manager-core-quotas-request.md)。
>

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 64 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = 一个 V100 卡。

*支持 RDMA

## <a name="nd-series"></a>ND 系列

高级存储：支持

高级存储缓存：支持

ND 系列虚拟机是针对 AI 和深度学习工作负荷设计的 GPU 家族的新成员。 它们在训练和推理方面性能卓越。 ND 实例采用 [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU。 这些实例可以针对单精度浮点运算和利用 Microsoft 认知工具包、TensorFlow、Caffe 及其他框架的 AI 工作负荷提供卓越的性能。 ND 系列还提供了更大的 GPU 内存大小（24 GB），能够适应更大的神经网络模型。 与 NC 系列一样，ND 系列可通过 RDMA 和 InfiniBand 连接提供含辅助型低延迟、高吞吐量网络的配置，以便可运行跨多个 GPU 的大规模训练作业。

> [!IMPORTANT]
> 对于此大小系列，订阅中每个区域的 vCPU（核心）配额最初都设置为 0。 可以请求在某个[可用区域](https://azure.microsoft.com/regions/services/)中[提高此系列的 vCPU 配额](../articles/azure-supportability/resource-manager-core-quotas-request.md)。
>

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 736 | 1 | 24 | 12 | 4 |
| Standard_ND12s |12 |224 | 1474 | 2 | 48 | 24 | 8 | 
| Standard_ND24s |24 |448 | 2948 | 4 | 96 | 32 | 8 |
| Standard_ND24rs* |24 |448 | 2948 | 4 | 96 | 32 | 8 |

1 GPU = 一个 P40 卡。

*支持 RDMA

## <a name="nv-series"></a>NV 系列

高级存储：不支持

高级存储缓存：不支持

NV 系列虚拟机采用 [NVIDIA Tesla M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU 和 NVIDIA GRID 技术，适用于桌面加速型应用程序和虚拟桌面，方便客户将其数据或模拟可视化。 用户可以在 NV 实例上直观显示其图形密集型工作流以获取高级图形功能，并可额外运行单精度工作负荷，例如编码和渲染。 

NV 实例中的每个 GPU 都带有 GRID 许可证。 使用此许可证，可以灵活地将 NV 实例用作单个用户的虚拟工作站，或将 25 个并发用户都连接到用于虚拟应用程序方案的 VM。

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 | 虚拟工作站 | 虚拟应用程序 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV6 |6 |56 |340 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = 半块 M60 卡。

## <a name="nvv2-series-preview"></a>NVv2 系列（预览版）

高级存储：支持

高级存储缓存：支持

NVv2 系列虚拟机采用 Intel Broadwell CPU，由 [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU 和 NVIDIA GRID 技术提供支持。 此类虚拟机面向 GPU 加速图形应用程序和虚拟桌面，客户希望利用这些应用和桌面直观呈现数据、模拟要查看的结果、处理 CAD 或渲染和流式处理内容。 此外，这些虚拟机还能运行编码和渲染等单精度工作负荷。 NVv2 虚拟机支持高级存储，且与之前的 NV 系列相比，系统内存 (RAM) 翻了一番。  

NVv2 实例中的每个 GPU 都带有 GRID 许可证。 使用此许可证，可以灵活地将 NV 实例用作单个用户的虚拟工作站，或将 25 个并发用户都连接到用于虚拟应用程序方案的 VM。

| 大小 | vCPU | 内存：GiB | 临时存储 (SSD) GiB | GPU | GPU 内存：GiB | 最大数据磁盘数 | 最大 NIC 数 | 虚拟工作站 | 虚拟应用程序 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1 | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

1 GPU = 半块 M60 卡。

 
