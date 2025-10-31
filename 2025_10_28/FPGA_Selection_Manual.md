# **AMD-Xilinx FPGA/SoC 产品分类与选型手册**

**修订日期**: 2023年10月27日
**目的**: 本手册旨在系统梳理 AMD-Xilinx（原 Xilinx）的主要 FPGA 和 SoC 产品线，帮助用户理解不同产品家族、系列、型号的特点及适用场景，并提供开发板选型建议。

---

## **目录**

1.  **AMD-Xilinx 产品家族概述**
2.  **纯 FPGA 产品家族 (Programmable Logic Only)**
    1.  7 系列 FPGA (28nm)
        1.  Artix-7
        2.  Kintex-7
        3.  Virtex-7
    2.  UltraScale / UltraScale+ FPGA (20nm / 16nm FinFET)
        1.  Kintex UltraScale / UltraScale+
        2.  Virtex UltraScale / UltraScale+
    3.  Versal ACAP (7nm)[**错误:ACAP应该是Soc的超集,而不是纯FPGA系统,属于Soc下一代产品,片上集成Ps,Pl,AIE三个核心**]
        1.  Versal Prime 系列
        2.  Versal AI Core 系列
        3.  Versal AI Edge 系列
        4.  Versal Premium 系列
3.  **SoC 产品家族 (System-on-Chip)**
    1.  Zynq-7000 系列 SoCs (28nm)
        1.  XC7Z007S / XC7Z010
        2.  XC7Z020
        3.  XC7Z030 / XC7Z035 / XC7Z045 / XC7Z100
    2.  Zynq UltraScale+ MPSoC (16nm FinFET)
        1.  CG 系列 (Compact General-purpose)
        2.  EG 系列 (Embedded General-purpose)
        3.  EV 系列 (Embedded Vision)
    3.  Zynq UltraScale+ RFSoC (16nm FinFET)
4.  **Kria™ SOMs (System-on-Module) 平台**
5.  **神经网络加速器开发板选型建议 (总结)**

---

## **1. AMD-Xilinx 产品家族概述**

AMD-Xilinx 的可编程器件主要分为两大类：

*   **纯 FPGA (Field-Programmable Gate Array)**: 仅提供可编程逻辑资源，用于实现各种数字电路和硬件加速器。
*   **SoC (System-on-Chip)**: 将硬核处理器（通常是 ARM Cortex-A/R）与可编程逻辑（FPGA）集成在同一芯片上，实现异构计算。

**工艺节点演进**: 28nm (7系列/Zynq-7000) -> 20nm (UltraScale) -> 16nm FinFET (UltraScale+/Zynq UltraScale+) -> 7nm (Versal ACAP)。工艺越先进，性能越高，功耗越低，集成度越高。

---

## **2. 纯 FPGA 产品家族 (Programmable Logic Only)**

### **2.1 7 系列 FPGA (28nm 工艺)**
*   **概述**: Xilinx 在 2010 年代初推出的主流 FPGA 系列。成熟、稳定，在性能、功耗和成本之间取得良好平衡。
*   **特点**: 提供丰富的逻辑单元 (LUTs)、触发器 (FFs)、DSP Slice 和 Block RAM (BRAM) 资源。

#### **2.1.1 Artix-7**
*   **定位**: 成本优化、低功耗、性能适中。
*   **适用场景**: 成本敏感型应用、工业控制、消费电子、嵌入式视觉（非AI）、传感器融合。
*   **典型型号**:
    *   **XC7A35T**: 33,280 逻辑单元, 90 DSP Slice, 1.8Mb BRAM
    *   **XC7A100T**: 101,440 逻辑单元, 240 DSP Slice, 4.8Mb BRAM
    *   **XC7A200T**: 215,360 逻辑单元, 740 DSP Slice, 13.1Mb BRAM
*   **常见开发板**:
    *   **Basys 3 (XC7A35T)**: Digilent 入门级板卡，适合学习 FPGA 基础。
    *   **Nexys 4 DDR (XC7A100T)**: Digilent 中级板卡，资源更丰富，适合数字逻辑设计。
    *   **Arty A7 (XC7A35T/100T)**: Digilent 小型板卡，适合嵌入式 FPGA 应用。

#### **2.1.2 Kintex-7**
*   **定位**: 高性能、高带宽。
*   **适用场景**: 高速通信、DSP 密集型应用、数据中心、测试测量、医疗成像。
*   **典型型号**:
    *   **XC7K160T**: 162,240 逻辑单元, 600 DSP Slice, 11.7Mb BRAM
    *   **XC7K325T**: 326,080 逻辑单元, 840 DSP Slice, 16.9Mb BRAM
    *   **XC7K410T**: 412,160 逻辑单元, 1540 DSP Slice, 28.3Mb BRAM
*   **常见开发板**:
    *   **KC705 Evaluation Kit (XC7K325T)**: Xilinx 官方评估套件，功能全面。
    *   **Genesys 2 (XC7K325T)**: Digilent 高端板卡，资源丰富。

#### **2.1.3 Virtex-7**
*   **定位**: 最高性能、最高容量。
*   **特点**: 采用 SSI (Stacked Silicon Interconnect) 技术，突破单一硅片尺寸限制。
*   **适用场景**: 高端计算、航空航天、国防、ASIC 原型验证、超高速网络。
*   **典型型号**:
    *   **XC7VX330T**: 332,160 逻辑单元, 1040 DSP Slice, 20.6Mb BRAM
    *   **XC7VX485T**: 485,760 逻辑单元, 2016 DSP Slice, 33.6Mb BRAM
    *   **XC7VX690T**: 693,120 逻辑单元, 3600 DSP Slice, 47.5Mb BRAM
*   **常见开发板**:
    *   **VC707 Evaluation Kit (XC7VX485T)**: Xilinx 官方评估套件。
    *   **VC709 Evaluation Kit (XC7VX690T)**: Xilinx 官方最高端评估套件。

### **2.2 UltraScale / UltraScale+ FPGA 家族 (20nm / 16nm FinFET 工艺)**
*   **概述**: Xilinx 在 2014 年后推出的更先进 FPGA 系列，采用 20nm (UltraScale) 和 16nm FinFET (UltraScale+) 工艺。在 7 系列基础上显著提升了性能、容量、功耗效率。UltraScale+ 更是引入了 FinFET 技术，带来质的飞跃。
*   **特点**: 更高的逻辑密度、更快的时钟速度、更宽的内存带宽、更先进的收发器 (GTH/GTY)，继续沿用 SSI 技术。

#### **2.2.1 Kintex UltraScale / UltraScale+**
*   **定位**: 高性能、高带宽。
*   **适用场景**: 数据中心加速、网络、测试测量、高性能计算、5G 基础设施。
*   **典型型号 (UltraScale)**:
    *   **XCKU040**: 530,500 逻辑单元, 1920 DSP Slice, 21.1Mb BRAM
    *   **XCKU060**: 663,300 逻辑单元, 2880 DSP Slice, 27.0Mb BRAM
*   **典型型号 (UltraScale+)**:
    *   **XCKU035**: 466,000 逻辑单元, 1440 DSP Slice, 16.9Mb BRAM
    *   **XCKU060**: 663,300 逻辑单元, 2880 DSP Slice, 27.0Mb BRAM
    *   **XCKU115**: 1,143,000 逻辑单元, 5520 DSP Slice, 48.6Mb BRAM
*   **常见开发板**:
    *   **KCU105 Evaluation Kit (XCKU040)**: Xilinx 官方 UltraScale 评估套件。
    *   **KCU116 Evaluation Kit (XCKU060)**: Xilinx 官方 UltraScale+ 评估套件。

#### **2.2.2 Virtex UltraScale / UltraScale+**
*   **定位**: 最高性能、最高容量。
*   **特点**: 采用 SSI 技术，提供最大的逻辑容量和带宽。
*   **适用场景**: 最苛刻的计算和通信应用、ASIC 原型验证、仿真、数据中心加速。
*   **典型型号 (UltraScale)**:
    *   **XCVU065**: 819,000 逻辑单元, 3600 DSP Slice, 35.6Mb BRAM
    *   **XCVU095**: 1,186,000 逻辑单元, 6240 DSP Slice, 54.0Mb BRAM
*   **典型型号 (UltraScale+)**:
    *   **XCVU9P**: 1,182,000 逻辑单元, 6840 DSP Slice, 56.5Mb BRAM
    *   **XCVU13P**: 3,780,000 逻辑单元, 12288 DSP Slice, 150.0Mb BRAM (超大容量)
*   **常见开发板**:
    *   **VCU118 Evaluation Kit (XCVU9P)**: Xilinx 官方 UltraScale+ 评估套件。

### **2.3 Versal ACAP 家族 (7nm 工艺)**
*   **概述**: Xilinx 的下一代平台，不仅仅是 FPGA。Versal 是一个 **ACAP (Adaptive Compute Acceleration Platform)**，将处理器、可编程逻辑、AI 引擎、DSP 引擎、网络引擎等集成在一起的异构计算平台。
*   **特点**: 软件定义硬件，面向数据中心、5G、汽车、航空航天等需要极致性能和灵活性的应用。
*   **核心组件**:
    *   **PS (Processing System)**: ARM Cortex-A72 (APU) + ARM Cortex-R5F (RPU)
    *   **PL (Programmable Logic)**: 基于 UltraScale+ 架构的 FPGA 逻辑
    *   **AI Engine**: 矢量处理器阵列，专为 AI/ML 推理和信号处理优化
    *   **DSP Engine**: 高性能 DSP 模块
    *   **Network-on-Chip (NoC)**: 高效片上互联
    *   **高速收发器**: GTM 收发器

#### **2.3.1 Versal Prime 系列**
*   **定位**: 通用高性能计算和连接。
*   **适用场景**: 数据中心加速、网络、测试测量、工业。
*   **典型型号**: VM1802, VM1502
*   **常见开发板**:
    *   **VMK180 Evaluation Kit (VM1802)**: Xilinx 官方 Versal Prime 系列评估套件。

#### **2.3.2 Versal AI Core 系列**
*   **定位**: 专为 AI/ML 推理和信号处理优化，集成 AI 引擎。
*   **适用场景**: AI 推理、5G 通信、雷达、自动驾驶。
*   **典型型号**: VC1902, VC1802
*   **常见开发板**:
    *   **VCK190 Evaluation Kit (VC1902)**: Xilinx 官方 Versal AI Core 系列评估套件。

#### **2.3.3 Versal AI Edge 系列**
*   **定位**: 针对边缘 AI 应用，优化功耗和成本。
*   **适用场景**: 边缘 AI、机器人、ADAS、工业物联网。
*   **典型型号**: VE2302, VE2802
*   **常见开发板**:
    *   **VEK280 Evaluation Kit (VE2802)**: Xilinx 官方 Versal AI Edge 系列评估套件。

#### **2.3.4 Versal Premium 系列**
*   **定位**: 最高带宽和计算能力，集成多达 112 个 GTM 收发器。
*   **适用场景**: 下一代数据中心、云原生计算、超高速网络。
*   **典型型号**: VP1802, VP1502
*   **常见开发板**: 暂无面向大众的入门级开发板。

---

## **3. SoC 产品家族 (System-on-Chip)**

### **3.1 Zynq-7000 系列 SoCs (28nm 工艺)**
*   **概述**: Xilinx 最早的 SoC 系列，将双核 ARM Cortex-A9 处理器 (PS) 与 7 系列 FPGA 架构的可编程逻辑 (PL) 集成。
*   **特点**: 处理器软件开发与 FPGA 硬件加速的结合，性价比高，适合各种嵌入式系统。
*   **PS 核心**: 双核 ARM Cortex-A9

#### **3.1.1 XC7Z007S / XC7Z010**
*   **定位**: 入门级 Zynq，资源较少，成本更低。
*   **适用场景**: 简单的嵌入式控制、少量加速任务、教学实验。
*   **典型型号**:
    *   **XC7Z007S**: 23,000 逻辑单元, 60 DSP Slice, 1.3Mb BRAM (单核 A9)
    *   **XC7Z010**: 28,000 逻辑单元, 80 DSP Slice, 2.1Mb BRAM (双核 A9)
*   **常见开发板**:
    *   **Arty Z7-10 (XC7Z010)**: Digilent 入门级 Zynq 板卡。
    *   **MicroZed (XC7Z010)**: Avnet 小型 SOM，适合嵌入式。

#### **3.1.2 XC7Z020**
*   **定位**: 资源均衡，性价比高，Zynq-7000 系列中最受欢迎的型号。
*   **适用场景**: 中等规模的加速器设计、嵌入式视觉、工业自动化、机器人。
*   **典型型号**:
    *   **XC7Z020**: 85,000 逻辑单元, 220 DSP Slice, 4.9Mb BRAM (双核 A9)
*   **常见开发板**:
    *   **Zybo Z7-20 (XC7Z020)**: Digilent 经典入门级 Zynq 板卡，功能全面。
    *   **Pynq-Z2 (XC7Z020)**: TUL/Xilinx 合作，基于 Python 的 Zynq 开发板。
    *   **ZedBoard (XC7Z020)**: Avnet 经典 Zynq 开发板，资料丰富。
    *   **ZC702 Evaluation Kit (XC7Z020)**: Xilinx 官方评估套件。

#### **3.1.3 XC7Z030 / XC7Z035 / XC7Z045 / XC7Z100**
*   **定位**: 高性能 Zynq，资源更丰富。
*   **适用场景**: 更复杂的加速器、高性能嵌入式系统、多通道数据处理。
*   **典型型号**:
    *   **XC7Z045**: 350,000 逻辑单元, 900 DSP Slice, 19.2Mb BRAM (双核 A9)
*   **常见开发板**:
    *   **ZC706 Evaluation Kit (XC7Z045)**: Xilinx 官方评估套件，功能强大。

### **3.2 Zynq UltraScale+ MPSoC 家族 (16nm FinFET 工艺)**
*   **概述**: Zynq-7000 系列的下一代，基于 UltraScale+ FPGA 架构，集成了更强大的处理器系统 (PS) 和更先进的可编程逻辑 (PL)。
*   **特点**: 强大的异构处理能力，适用于高性能嵌入式计算、AI 推理、视频处理、工业自动化等。
*   **命名规则**: `XCZU<资源等级><功能集>`

#### **3.2.1 CG 系列 (Compact General-purpose)**
*   **定位**: 紧凑、低成本，简化版 MPSoC。
*   **PS 核心**: 双核 ARM Cortex-A53
*   **适用场景**: 工业物联网、小型控制器、传感器融合、对成本和功耗敏感的应用。
*   **典型型号**:
    *   **XCZU2CG**: 103,000 逻辑单元, 216 DSP Slice, 7.6Mb BRAM
    *   **XCZU3CG**: 154,000 逻辑单元, 360 DSP Slice, 11.4Mb BRAM
*   **常见开发板**:
    *   **MiniZed (XCZU3CG)**: Avnet 小型 MPSoC 开发板。

#### **3.2.2 EG 系列 (Embedded General-purpose)**
*   **定位**: 标准配置，通用高性能嵌入式。
*   **PS 核心**: 四核 ARM Cortex-A53 (APU) + 双核 ARM Cortex-R5F (RPU)
*   **适用场景**: 边缘 AI、机器人、高性能嵌入式计算、数据中心加速、通信。
*   **典型型号 (按 PL 资源递增)**:
    *   **XCZU2EG**: 103,000 逻辑单元, 216 DSP Slice, 7.6Mb BRAM
    *   **XCZU3EG**: 154,000 逻辑单元, 360 DSP Slice, 11.4Mb BRAM
    *   **XCZU4EG**: 192,000 逻辑单元, 480 DSP Slice, 14.2Mb BRAM
    *   **XCZU5EG**: 256,000 逻辑单元, 600 DSP Slice, 19.0Mb BRAM
    *   **XCZU7EG**: 343,000 逻辑单元, 780 DSP Slice, 25.7Mb BRAM
    *   **XCZU9EG**: 600,000 逻辑单元, 2520 DSP Slice, 38.0Mb BRAM
*   **常见开发板**:
    *   **Ultra96-V2 (XCZU3EG)**: Avnet 小型 MPSoC 开发板，**推荐初期测试板**。
    *   **ZCU102 Evaluation Kit (XCZU9EG)**: Xilinx 官方评估套件，**推荐后期纯算力板**。
    *   **Kria K26 SOM (XCZU2EG/XCZU3EG)**: 作为 KV260/KR260 的核心芯片。

#### **3.2.3 EV 系列 (Embedded Vision)**
*   **定位**: 在 EG 基础上集成硬件视频编解码单元 (VCU)。
*   **PS 核心**: 四核 ARM Cortex-A53 (APU) + 双核 ARM Cortex-R5F (RPU) + VCU
*   **适用场景**: 智能相机、视频分析、ADAS、视频监控、广播、高性能视频 AI。
*   **典型型号 (按 PL 资源递增)**:
    *   **XCZU4EV**: 192,000 逻辑单元, 480 DSP Slice, 14.2Mb BRAM, **带 VCU**
    *   **XCZU5EV**: 256,000 逻辑单元, 600 DSP Slice, 19.0Mb BRAM, **带 VCU**
    *   **XCZU7EV**: 343,000 逻辑单元, 780 DSP Slice, 25.7Mb BRAM, **带 VCU**
*   **常见开发板**:
    *   **ZCU104 Evaluation Kit (XCZU7EV)**: Xilinx 官方评估套件，**推荐后期视频 AI 算力板**。
    *   **Kria KR260 Robotics Starter Kit (XCZU4EV)**: 基于 Kria SOM 的机器人开发套件。

### **3.3 Zynq UltraScale+ RFSoC 家族 (16nm FinFET 工艺)**
*   **概述**: 在 Zynq UltraScale+ MPSoC 的基础上，集成了高性能的射频数据转换器（ADC/DAC）硬核。
*   **特点**: 将射频前端与数字处理集成到单个芯片，简化系统设计。
*   **PS 核心**: 四核 ARM Cortex-A53 (APU) + 双核 ARM Cortex-R5F (RPU)
*   **适用场景**: 5G 无线电、雷达、测试测量、卫星通信。
*   **典型型号**: XCZU28DR, XCZU48DR
*   **常见开发板**:
    *   **ZCU111 Evaluation Kit (XCZU28DR)**: Xilinx 官方 RFSoC 评估套件。

---

## **4. Kria™ SOMs (System-on-Module) 平台**

*   **概述**: Kria 是 AMD-Xilinx 推出的一个全新的产品线，它不是芯片系列，而是基于 **Zynq UltraScale+ MPSoC** 的预构建 **系统级模块 (SOM)**。Kria SOMs 将 MPSoC 芯片、内存、存储、电源管理和基本接口集成在一个小型板卡上。
*   **特点**: 旨在简化和加速边缘 AI 和嵌入式应用的开发，提供开箱即用的硬件和软件堆栈，降低开发门槛。
*   **核心芯片**: 主要基于 **XCZU2EG, XCZU3EG, XCZU4EV** 等 Zynq UltraScale+ MPSoC 芯片。
*   **常见产品**:
    *   **Kria KV260 Vision AI Starter Kit**:
        *   **核心芯片**: Kria K26 SOM (基于 XCZU2EG/XCZU3EG)，集成在载板上。
        *   **特点**: 针对视觉和 AI 应用，提供丰富的视觉接口。
    *   **Kria KR260 Robotics Starter Kit**:
        *   **核心芯片**: Kria K26 SOM (基于 XCZU4EV)，集成在载板上。
        *   **特点**: 针对机器人和工业自动化应用，提供丰富的机器人接口。
    *   **Kria K26 Production SOM**: 生产级 SOM，用于直接集成到最终产品中。

---

## **5. 神经网络加速器开发板选型建议 (总结)**

**研究方向**: 基于 FPGA 的神经网络加速器设计 (需要 SoC，PS 端对神经网络的抽象性兼容好，PL 实现具体的加速并行算法)。

### **阶段一：初期实验测试板 (验证流程和基础实验)**

*   **目标**: 跑通 PS-PL 协同、片内外通信、片内外存储访问的流程，验证神经网络加速器的基本架构和算法。
*   **推荐系列**: **Zynq UltraScale+ MPSoC EG 系列**
*   **推荐型号**: **XCZU3EG**
*   **推荐开发板**:
    *   **Ultra96-V2 (核心芯片: XCZU3EG)**:
        *   **理由**: 性价比高，资源均衡，足以验证 PS-PL 协同、AXI 接口、DDR 访问等核心概念。是学习和验证基础流程的理想选择。

### **阶段二：后期强大算力板 (高性能部署和深度优化)**

*   **目标**: 实现高性能的神经网络加速器，追求极致的吞吐量和低延迟，验证复杂的模型和大规模数据处理。
*   **推荐系列**: **Zynq UltraScale+ MPSoC EG 或 EV 系列**
*   **推荐型号**: **XCZU9EG** 或 **XCZU7EV**
*   **推荐开发板**:
    *   **ZCU102 Evaluation Kit (核心芯片: XCZU9EG)**:
        *   **理由**: **EG 系列中 PL 资源最丰富**，适合纯粹追求 PL 算力，实现最复杂、最大规模的神经网络加速器。
    *   **ZCU104 Evaluation Kit (核心芯片: XCZU7EV)**:
        *   **理由**: **EV 系列中 PL 资源丰富，且内置 VCU**。如果您的神经网络加速器涉及视频数据的预处理（如实时视频流的解码），VCU 可以显著提升系统效率。

---
