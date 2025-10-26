---
tags: [学习报告]
title: 边沿FPGA的神经网络部署优化手段
created: '2025-10-23T08:38:29.602Z'
modified: '2025-10-23T10:23:10.406Z'
---

# 边沿FPGA的神经网络部署优化手段

## 1. 神经网络优化
神经网络优化是在算法层面减少模型的计算复杂度和参数量，使其更适合资源受限的边沿设备。这通常在不显著降低模型精度的前提下，通过改进网络结构或采用模型压缩技术，为后续的硬件部署（如FPGA）打下基础。

### 1.1轻量化网络架构设计
这是在模型训练 之前 就采取的优化手段，与知识蒸馏（训练后压缩）相辅相成。它专注于从零开始设计计算高效的网络结构，如使用深度可分离卷积 (MobileNet)、分组卷积 (ShuffleNet) 或 Fire 模块 (SqueezeNet)。这些结构在设计时就考虑了乘累加 (MAC) 操作的数量和参数量，天然适合资源受限的边沿FPGA部署。

* 参考文献：
[1]Sandler, M., Howard, A., Zhu, M., Zhmoginov, A., & Chen, L.-C. (2018). MobileNetV2: Inverted residuals and linear bottlenecks. 4510～4520. https://openaccess.thecvf.com/content_cvpr_2018/html/Sandler_MobileNetV2_Inverted_Residuals_CVPR_2018_paper.html


### 1.2 知识蒸馏模型学习教师模型的“知识”（如软标签），从而在保持较小模型体积的同时，获得接近教师模型的精度。
知识蒸馏是一种模型压缩方法，核心思想是利用一个复杂的“教师模型”来指导一个轻量级的“学生模型”训练。学生

* 参考文献：
[1]Mao, W.-L., Wang, C.-C., Chou, P.-H., Liu, K.-C., & Tsao, Y. (2024). MECKD: Deep learning-based fall detection in multilayer mobile edge computing with knowledge distillation. IEEE Sensors Journal, 24(24), 42195～42209. https://doi.org/10.1109/JSEN.2024.3456577



## 2. 针对硬件的软件优化

### 2.1 Quantification 量化
量化是将网络中高精度的浮点（如FP32）权重和激活值，转换为低位宽（如INT8, INT4）定点数或整数的过程。这能极大压缩模型大小，降低内存带宽需求。由于FPGA擅长处理低位宽整数运算，量化可以显著提升计算并行度和能效，是FPGA部署中最核心的优化之一。

* 参考文献：
[1]Li, J., Li, T., Chen, R., Shen, G., Zhao, D., Zhang, Q., & Zeng, Y. (2025). Hummingbird: A smaller and faster large language model accelerator on embedded FPGA (No. arXiv:2507.03308). arXiv. https://doi.org/10.48550/arXiv.2507.03308
[2]Li, J., Li, T., Shen, G., Zhao, D., Zhang, Q., & Zeng, Y. (2025). Pushing up to the limit of memory bandwidth and capacity utilization for efficient LLM decoding on embedded FPGA. 2025 Design, Automation & Test in Europe Conference (DATE), 1～7. https://doi.org/10.23919/DATE64628.2025.10993087


### 2.2 Sparse 稀疏
稀疏性是指网络模型（特别是权重）中存在大量零值。利用稀疏性，可以在推理时跳过所有与零相关的计算（如乘法），从而大幅减少实际的计算负载。在FPGA上，可以设计专门的硬件逻辑来高效处理稀疏数据，跳过无效运算，这比在通用CPU/GPU上利用稀疏性更具灵活性和效率优势。

* 参考文献
[1]He, X., Pal, S., Amarnath, A., Feng, S., Park, D.-H., Rovinski, A., Ye, H., Chen, Y., Dreslinski, R., & Mudge, T. (2020). Sparse-TPU: Adapting systolic arrays for sparse matrices. Proceedings of the 34th ACM International Conference on Supercomputing, 1～12. https://doi.org/10.1145/3392717.3392751

#### 2.2.1 Structured Sparsity（结构性稀疏）
与“非结构性稀疏”（Unstructured Sparsity）中随机、单独地移除任意位置的单个权重不同，结构性稀疏是按固定的、成组的模式来移除参数。这些“结构”通常是指硬件计算友好的单元，例如：
* 通道剪枝 (Channel Pruning)：移除卷积层中的整个滤波器（Filter）或通道（Channel）。
* 块稀疏 (Block Sparsity)：将权重矩阵划分为固定的块（Block），然后移除整个块。
* 向量/行稀疏 (Vector/Row Sparsity)：移除权重矩阵的整个行或列。
* N:M 稀疏：一种更细粒度的结构性稀疏，例如在每 $M$ 个连续的权重中，强制 $N$ 个为非零值（如 NVIDIA Ampere 架构支持的 2:4 稀疏）。
对FPGA而言:结构性稀疏的主要目的不是追求最高的压缩率，而是追求最高的硬件加速。非结构性稀疏虽然能移除更多参数，但会导致权重矩阵变得不规则，在FPGA或GPU上执行时，需要复杂的索引和不规则的内存访问，反而导致性能下降。而结构性稀疏（如通道剪枝）会产生更小、但仍然是“规整”、“稠密”的矩阵运算。这种规整性完美契合FPGA的并行硬件架构（如脉动阵列），能带来显著的、可预测的实际推理速度提升。

* 参考文献：
[1]Lym, S., & Erez, M. (2020). FlexSA: Flexible systolic array architecture for efficient pruned DNN model training (No. arXiv:2004.13027). arXiv. https://doi.org/10.48550/arXiv.2004.13027



### 2.3 Pruning 剪枝
剪枝是通过移除模型中冗余或不重要的参数（权重）来压缩模型的技术。它分为非结构化剪枝（单个权重设为0）和结构化剪枝（移除整个通道或滤波器）。结构化剪枝能更规整地减少模型大小和计算量，在FPGA上更容易实现硬件并行加速和存储优化。

#### 2.3.1 Column Packing（列组合）与Conflict Pruning（冲突性剪枝）
简述：合并权重矩阵的列，冲突时保留最大值。下面的论文展示了如何使用模拟退火算法来预测保留的值。

* 参考论文：
[1]Chen, X., Zhu, J., Jiang, J., & Tsui, C.-Y. (2020). Tight compression: Compressing CNN model tightly through unstructured pruning and simulated annealing based permutation. 2020 57th ACM/IEEE Design Automation Conference (DAC), 1～6. https://doi.org/10.1109/DAC18072.2020.9218701


### 2.4 低秩近似
低秩近似是一种模型压缩技术，其核心思想是：神经网络中的权重矩阵（尤其是全连接层或卷积层）往往是“过参数化”的，其本质的“秩” (Rank) 很低。因此，我们可以将一个庞大的m x n 权重矩阵W，分解为两个或多个更小的“瘦”矩阵的乘积，例如 W = U x V^T，其中 U 是 m x v，V^T 是 v x n，且 r 《 m，n。通过这种方式，参数量从 m x n 减少到 m x v +  v x n ，计算量也随之降低。这在FPGA上能显著节省BRAM（片上内存）资源，并减少所需的乘法器数量。

* 参考文献
[1]Kim, Y.-D., Park, E., Yoo, S., Choi, T., Yang, L., & Shin, D. (2015). Compression of deep convolutional neural networks for fast and low power mobile applications. CoRR. https://www.semanticscholar.org/paper/Compression-of-Deep-Convolutional-Neural-Networks-Kim-Park/4ca3b996d888d7178dbbf9855bb2ab253bdfa43d

### 2.5 算子融合

借助像ONNX，TVM之类的机器学习编译器，可以将训练好的神经网络转换为中间表示形态如图形态、多面体表示。然后将网络中连续的多个操作（如 Conv -> BatchNorm -> ReLU）合并成一个单一的、复合的硬件计算核（Kernel）。这样做的好处是显而易见的：中间结果（如Conv的输出）无需写回片外内存再读出，而是直接在FPGA片上的寄存器或BRAM中传递。这极大降低了对内存带宽的压力，是“带宽提升”的核心实现手段之一。

* 参考文献：
[1]黄思晓, 彭皓翔, 施旭, 苏志锋, 黄明强, & 余浩. (2025). 大模型FPGA推理实现技术综述与未来挑战. 集成电路与嵌入式系统, 25(6), 1～13. https://doi.org/10.20193/j.ices2097-4191.2025.0023


## 3. 硬件加速

### 3.1 脉动阵列

在HLS流程中发现，该流程对PL端的资源消耗巨大。一个5*5卷积核大概需要ZYNQ7020上PL端百分之二的lut，那么把完整的神经网络放进PL肯定是不行的，所以一般是放一些用的多的算子。但是算子之间亦有区别，虽然可以写成可配置的形式，但依然会消耗更多资源。有没有更加通用的方法呢？脉动阵列可以吗？

Systolic Array 就是一种规则的“处理单元网格”，每个小单元（Processing Element, PE）可以完成简单的计算（比如乘法、加法），并把结果传给相邻单元。

* 参考文献
[1]Xu, R., Ma, S., Guo, Y., & Li, D. (2023). A survey of design and optimization for systolic array-based DNN accelerators. ACM Comput. Surv., 56(1), 20:1-20:37. https://doi.org/10.1145/3604802
[2]Jouppi, N. P., Young, C., Patil, N., Patterson, D., Agrawal, G., Bajwa, R., Bates, S., Bhatia, S., Boden, N., Borchers, A., Boyle, R., Cantin, P., Chao, C., Clark, C., Coriell, J., Daley, M., Dau, M., Dean, J., Gelb, B., … Yoon, D. H. (2017). In-datacenter performance analysis of a tensor processing unit. 2017 ACM/IEEE 44th Annual International Symposium on Computer Architecture (ISCA), 1～12. https://doi.org/10.1145/3079856.3080246


### 3.2计算架构优化

#### 3.2.1 流水线优化

流水线是将一个复杂的计算任务（如一个完整的神经网络层）拆分成多个串行的子阶段（Stage）的硬件技术。在FPGA上，这些阶段可以同时处理不同的数据，就像工厂的装配线一样。这种技术能显著提高系统的吞吐率（Throughput），让硬件在每个时钟周期都保持忙碌，是实现高性能数据流处理的关键。

#### 3.2.2 片外片内带宽提升

神经网络计算是数据密集型的，性能瓶颈常在于片外内存（如DDR）的访问带宽，即计算单元等待数据。带宽提升旨在解决“内存墙”问题。在FPGA上，这包括优化数据在片上（BRAM/URAM）和片外存储间的传输，设计高效的数据复用策略（如权重缓存），确保计算单元能被持续“喂饱”数据。

采用带宽更大的硬件资源：DDR5/HBM等。

采用更高速的通信协议，如PCIE。





