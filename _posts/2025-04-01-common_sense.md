---
layout:     post
title:      "常识记录"
subtitle:   "记录基本知识"
date:       2025-04-05 12:00:00
author:     "nothing丿zip"
header-img: " "
tags:
    - 常识
---

### 1.常识记录

#### 人力资源相关

- **HC**
  - **全称**：Head Count（人员编制）
  - **含义**：指公司或部门在一定时期内可招聘的岗位名额，即“有多少个职位空缺”。

- **OC**
  - **全称**：Offer Call（录用通知电话）
  - **含义**：指HR通过电话或邮件向候选人正式发放录用通知的环节。

- **JD**
  - **全称**：Job Description（职位描述）
  - **含义**：是对一个职位的工作内容、职责、要求等进行详细描述的文件，是求职者了解岗位情况的重要依据。

- **OA**
  - **全称**：Online Assessment（在线测评）
  - **含义**：是企业招聘过程中常用的一种筛选方式，通过在线测试来评估候选人的各种能力，如智力、性格、专业知识等。

### 2.其他常识

#### 2.1 面对批评时你如何应对?
>
>- 把批评转化为"免费的专业咨询"，用解决问题替代自我辩护。真正需要防御的不是批评本身，而是拒绝成长的可能性。

#### 2.2 为什么要学Go语言？他和AI有什么关系/应用吗？
>
>- Go语言是Google开发的一种静态强类型、编译型、并发的编程语言，它可以轻松地构建高性能的网络服务和应用。

>- 可以使用 Go 编写服务器端代码，接收客户端的请求，将请求数据传递给已训练好的 AI 模型进行推理，并将推理结果返回给客户端。常见做法是借助 TensorFlow Serving、ONNX Runtime 等工具，通过 Go 调用其 API 实现模型推理。
>- AI 训练和推理过程中需要处理大量的数据，Go 的高效性能使其在数据处理方面表现出色。可以使用 Go 编写数据处理脚本，对原始数据进行清洗、转换和特征提取等操作。此外，Go 还可以与数据库（如 MySQL、Redis 等）进行交互，实现数据的存储和读取。

#### 2.3 计算机考研中，计算机学科专业基础综合（408）考试中的专业课程有哪些？

- **数据结构**：是计算机科学与技术的基础课程，主要研究数据的存储和组织方式。包括线性表、栈和队列、树和图等基本数据结构，以及查找、排序和遍历等基本操作。在考研408中占45分，题型有选择题、算法设计题和问答题等。
- **计算机组成原理**：主要涉及计算机硬件系统的结构与工作原理，包括数字逻辑、指令系统、总线技术、存储器等方面。考生需要深入理解计算机硬件系统的工作原理，掌握计算机组成原理的基本概念和技术。在考研408中占45分，有选择题和大题等题型。
- **操作系统**：是计算机系统中的核心组件，负责管理计算机的资源和提供各种服务，包括进程管理、内存管理、文件系统等内容。考生需要理解操作系统的基本原理和设计方法，掌握操作系统的各种功能和技术。在考研408中占35分，题型有选择题和大题，其中PV操作是常考的大题知识点。
- **计算机网络**：是连接不同计算机之间的通信网络，是现代信息技术的重要组成部分，包括网络基础、传输层、网络层、应用层等内容。考生需要了解计算机网络的基本概念和技术，掌握网络安全和管理的相关知识和技能。在考研408中占25分，题型有选择题和大题。

试卷满分为150分，考试时间为180分钟，答题方式为闭卷、笔试。

#### 2.4 LRU和LFU

LRU 和 LFU 是两种常见的缓存淘汰策略，它们的中文名称分别是：

- **LRU**（Least Recently Used）：**最近最少使用**  
  淘汰最久未被访问的数据，核心思想是“最近使用过的数据更可能被再次使用”。

- **LFU**（Least Frequently Used）：**最不经常使用**  
  淘汰访问频率最低的数据，核心思想是“频繁使用的数据更可能被再次使用”。

| 策略   | 淘汰依据               | 优点               | 缺点               |
|--------|------------------------|--------------------|--------------------|
| **LRU** | 时间维度（最久未使用） | 实现简单，适合热点数据 | 无法应对偶发的高频访问 |
| **LFU** | 频率维度（访问次数少） | 对历史统计更敏感     | 需要维护计数器，实现复杂 |

在实际应用中，LRU 更常用（如 Redis 默认淘汰策略），而 LFU 适合需要长期统计的场景。
