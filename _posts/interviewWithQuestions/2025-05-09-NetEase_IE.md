---
layout: post
title: 图形开发面试题总结
date: 2025-05-09 10:50
category: interviewWithQuestions
author: "nothing丿zip"
header-img: " "
tags: 
    - 面试
---

### 1.什么是Unity, UE? 这些工具的基本情况

| **维度**               | **Unity**                                                                 | **Unreal Engine**                                                                 |  
|------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------|  
| **学习门槛**           | **低**，适合快速入门：<br>- 采用 **C# 语言**，语法简洁，接近现代编程语言，对新手友好<br>- 可视化工具链完善（如 Inspector 面板、预制体系统、Mono Behaviour 组件化开发）<br>- 官方文档和社区教程丰富，适合通过案例驱动学习（如官方《Roll a Ball》入门项目） | **中高**，需掌握底层逻辑：<br>- 核心模块使用 **C++ 开发**，需理解指针、内存管理、多线程等底层机制<br>- 蓝图（Blueprint）可视化脚本降低门槛，但复杂逻辑仍需结合 C++<br>- 引擎架构复杂（如模块系统、反射机制、垃圾回收），需学习《Unreal Engine 架构指南》等资料 |  
| **画面表现**           | **中端，平衡效率与画质**：<br>- 支持 **HDRP（高清渲染管线）** 和 **URP（通用渲染管线）**，移动端优化极佳（如动态批处理、纹理压缩）<br>- 适合卡通渲染、低多边形风格，近年通过 SRP（脚本化渲染管线）提升灵活性<br>- 代表案例：《Pokémon GO》（AR 实时渲染）、《原神》早期移动端版本 | **顶尖，次世代画质标杆**：<br>- 内置 **Nanite（几何体虚拟化）**、**Lumen（动态全局光照）**、**光线追踪（DXR）** 等技术，支持电影级写实渲染<br>- 物理材质系统（PBR）和动画引擎（Live Link Face）精度极高<br>- 代表案例：《赛博朋克 2077》（光追全局光照）、《曼达洛人》虚拟制片场景 |  
| **平台侧重**           | **移动端、中小项目、快速迭代**：<br>- 移动端市场占有率超 50%，支持 iOS/Android 深度优化（如 Metal/Vulkan 图形接口）<br>- 适合独立游戏、休闲手游、跨平台轻量级项目（如 WebGL 小游戏）<br>- 开发周期短，通过 Asset Store 资源快速搭建原型 | **主机/PC 端、大型项目、极致画质**：<br>- 原生支持 PS5/Xbox Series X 次世代主机特性（如 SSD 流式加载、硬件加速光线追踪）<br>- 主打 3A 大作、开放世界游戏（如《战争机器 5》《最终幻想 7 重制版》部分模块）<br>- 近年优化移动端性能（如《帕斯卡契约》手游），但复杂场景仍需深度调优 |  
| **开发语言**           | **C# 为主，可视化工具为辅**：<br>- 脚本逻辑基于 C#，支持面向对象编程，兼容 .NET 库（如异步编程、XML 解析）<br>- 可视化工具：可视化脚本插件 Bolt（非官方内置）、状态机编辑器 Playmaker | **C++ 核心，蓝图可视化脚本**：<br>- 引擎底层用 C++ 实现，支持高性能优化（如模板元编程、内存池）<br>- **蓝图**：节点式可视化编程，适合快速实现玩法逻辑，可与 C++ 代码双向交互（如蓝图调用 C++ 类，C++ 修改蓝图变量） |  

**总结选择逻辑**：  

- **选 Unity**：优先考虑 **移动端适配、快速开发、中小项目**，或团队以 C# 技术栈为主，追求低学习成本和资源生态便利性。  
- **选 Unreal Engine**：优先考虑 **极致画质、大型 3A 项目、主机/PC 端开发**，且团队能接受 C++ 开发门槛和复杂架构调试。

---

### 2.说说C++的共享指针

```std::shared_ptr``` 是 C++11 引入的智能指针，位于 ```<memory>``` 头文件中。它通过引用计数机制管理对象生命周期：

- 每个 ```shared_ptr``` 关联一个引用计数器，记录有多少个指针共享同一个对象。
- 当引用计数降为 0 时（即最后一个 ```shared_ptr``` 被销毁或重置），自动释放对象内存

示例代码：

```c++
#include <memory>
#include <iostream>

class MyClass {
public:
    MyClass() { std::cout << "MyClass created" << std::endl; }
    ~MyClass() { std::cout << "MyClass destroyed" << std::endl; }
};

int main() {
    // 创建 shared_ptr，引用计数为 1
    std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>();
    {
        // 拷贝构造，引用计数变为 2
        std::shared_ptr<MyClass> ptr2 = ptr1;
        std::cout << "Inside scope: ptr1 count = " << ptr1.use_count() << std::endl;
    } // ptr2 离开作用域，引用计数减为 1

    std::cout << "Outside scope: ptr1 count = " << ptr1.use_count() << std::endl;
    return 0; // ptr1 离开作用域，引用计数为 0，对象被销毁
}
```

```std::shared_ptr``` 是 C++ 中管理共享资源的首选工具，通过引用计数自动释放内存。但需注意避免循环引用，合理搭配 ```std::weak_ptr``` 使用。在性能敏感场景中，应权衡引用计数的开销（相比 ```unique_ptr```）。

---

### 3.weak_ptr是怎么工作的？

```std::weak_ptr``` 是一种不控制对象生命周期的智能指针，它指向由 ```std::shared_ptr``` 管理的对象。其特点：

- 不增加引用计数：```weak_ptr``` 不会影响所指对象的引用计数，即使存在多个 ```weak_ptr``` 指向同一对象，只要没有 ```shared_ptr``` 持有该对象，对象就会被释放。
- 不能直接访问对象：```weak_ptr``` 没有重载 ```*``` 和 ```->``` 操作符，需要先转换为 ```shared_ptr``` 才能使用对象。

---

### 4.shared_ptr是线程安全的吗？  

`std::shared_ptr` 的线程安全性需从 **引用计数的线程安全** 和 **对象访问的线程安全** 两个层面分析：  

#### 4.1 引用计数的线程安全  

`std::shared_ptr` 的**引用计数操作是线程安全的**，具体体现在：  

- 多个线程可同时对同一个 `shared_ptr` 进行拷贝、赋值或析构，不会导致数据竞争（data race）。  
- 引用计数的递增和递减操作通过原子操作（如 `std::atomic`）实现，保证线程安全。  

#### 4.2 对象访问的线程安全  

`std::shared_ptr` **不保证对象本身的线程安全**：  

- 若多个线程通过不同的 `shared_ptr` 实例访问同一个对象，且至少有一个线程修改对象，则需要外部同步（如互斥锁）。  

#### 4.3 总结  

| **操作类型**                | **线程安全性**                     | **说明**                          |  
|----------------------------|------------------------------------|-----------------------------------|  
| 引用计数操作（增减）        | 安全（原子操作保证）               | 无需额外同步，可并发执行拷贝/析构 |  
| 对象状态访问（尤其是写操作）| 不安全（需外部同步）               | 需通过互斥锁等机制保护共享对象    |

---

### 5.说说vector的扩容机制？

**Vector扩容机制关键点：**  

1.**动态增长**：当元素数量超过当前容量时，自动扩容。  
2.**扩容因子**：通常为**2倍**（常见实现，如GCC）或**1.5倍**（如VS），避免频繁扩容。
3.**操作步骤**：  

- 分配新内存（原大小的倍数）。  
- 拷贝/移动旧元素到新空间（C++11后支持移动语义）。  
- 释放旧内存。  
4.**时间复杂度**：均摊O(1)（分摊多次push_back的扩容开销）。  
5.**预留空间**：可用`reserve()`提前分配容量，避免多次扩容。  

---

### 6.说说list的实现？  

`std::list`是C++标准库中的一个双向链表实现，他的实现原理基于链表数据结构。  
每个节点包括两个指针，分别指向前一个节点和后一个节点。链表头和尾分别指向第一个和最后一个节点。  
链表节点的插入和删除操作都是O(1)的时间复杂度，但是链表在查找操作上性能较低，因为需要遍历整个链表。  

---

### 7.如何高效的查找有序的list里的元素？  

- 少量数据 → 线性搜索。
- 多次查找 → 转为 vector + 二分查找。
- 频繁增删+查找 → 考虑改用 std::set（红黑树，O(log n)）或跳跃表。

---

### 8.list二分查找的复杂度是多少？

**标准 `list` 二分查找的复杂度是 O(n)**，原因如下：  

1.无法随机访问：  

- `list` 的迭代器是**双向迭代器**（仅支持 `++`/`--`），不支持 `+n` 或 `[]`，导致无法直接跳到中间节点。  
- 每次“折半”需**线性遍历**到中间位置（单次中间定位需 O(n/2) ≈ O(n)）。  

2.总复杂度：  

- 二分需要 O(log n) 次“折半”，每次 O(n) → **总复杂度 O(n log n)**（比直接遍历 O(n) 更差）。  

对比其他数据结构：  

- `vector`/`array`：O(log n)（支持随机访问）。  
- `set`/`map`：O(log n)（红黑树实现）。

### 9.优先队列的实现

**优先队列（Priority Queue）的实现方式及特点：**  

优先队列通常基于以下数据结构，核心保证**快速取最值**（最大或最小）：  

#### **9.1 二叉堆（Binary Heap）**（最常用）  

- **存储结构**：数组（完全二叉树），空间紧凑。  
- **时间复杂度**：  
  - **插入（push）**：O(log n)（上浮调整）。  
  - **取最值（top）**：O(1)。  
  - **删除最值（pop）**：O(log n)（下沉调整）。  
- **操作示例**（大根堆）：  
  - **插入**：元素加到末尾，向上与父节点比较交换。  
  - **弹出**：根节点与末尾交换，删除末尾，根节点向下调整。  
- **缺点**：不支持随机查找/删除非堆顶元素（需扩展为索引堆）。  

#### **9.2 平衡二叉搜索树（如红黑树、AVL树）**  

- **时间复杂度**：插入、删除、查找均为 O(log n)。  
- **优点**：支持动态维护有序性，可查找/删除任意值。  
- **缺点**：代码复杂，常数开销大（适用于 `std::priority_queue` 的替代品如 `std::multiset`）。  

---

### 10.堆是怎么实现的？插入数据的时间复杂度是多少？

堆通过数组和完全二叉树实现，插入时通过上浮调整维持堆性质，时间复杂度为 O(log n)。

### 11.进程线程和协程

#### **11.1 垂直包含关系**

**进程（容器层） → 线程（执行层） → 协程（任务层）**  

- **进程** 是资源分配的顶层单位，为程序提供独立的内存、文件等隔离环境。  
  - 每个进程至少包含 **1个主线程**。  
  - 示例：浏览器中每个标签页是独立进程，崩溃互不影响。  

- **线程** 是CPU调度的基本单位，共享进程内的资源（内存、文件等）。  
  - 1个进程可包含 **多个线程**（如Chrome的渲染线程、网络线程）。  
  - 示例：视频编辑软件用多线程并行处理不同轨道的渲染。  

- **协程** 是用户态轻量级线程，寄生在线程中，由程序自行调度。  
  - 1个线程可调度 **成千上万个协程**（如Go的goroutine）。  
  - 示例：Web服务器用协程同时处理数万个HTTP连接。  

**类比**：  

- 进程 = 工厂（独立厂房和原料仓库）  
- 线程 = 生产线（共享厂房，多条线并行）  
- 协程 = 工人（在一条生产线上快速切换任务）  

---

#### **11.2 协作场景示例（Web服务器架构）**  

```plaintext
        [主进程]（守护进程）
           │
   ┌───────┴───────┐
[子进程A]       [子进程B]  ← 隔离用户A/B会话
   │               │
[线程池]         [线程池]  ← 每个进程4线程（4核CPU）
   │               │
[协程群]         [协程群]  ← 每线程处理1万个连接
```

- **进程级隔离**：防止单个用户请求崩溃导致服务全局瘫痪。  
- **线程级并行**：利用多核CPU执行压缩/加密等计算任务。  
- **协程级并发**：用极低开销管理海量I/O操作（如数据库查询）。  

---

#### **11.3 三者的本质区别**  

| 维度   | **进程**                  | **线程**                  | **协程**                  |
|------------|---------------------------|---------------------------|---------------------------|
| 资源   | 独立内存/文件             | 共享进程内存              | 共享线程栈                |
| 切换   | 需内核介入（开销大）      | 需内核介入（开销中）      | 用户态切换（开销极小）    |
| 崩溃   | 不影响其他进程            | 可能导致进程崩溃          | 仅影响当前协程            |

**关键结论**：  

- **包含性**：进程 ⊃ 线程 ⊃ 协程  
- **协作性**：三者共同实现 **安全隔离**（进程）、**硬件利用**（线程）、**高并发**（协程）。

### 12.为什么在代码中尽量少做那些复杂的数学计算？反三角函数、矩阵的逆等  

1.**性能差**：复杂数学计算（如矩阵求逆、反三角函数）消耗大量CPU/GPU资源，导致程序卡顿。  
2.**不稳定**：容易引发数值误差（如矩阵奇异、浮点精度问题），导致崩溃或错误结果。  
3.**难维护**：代码可读性降低，调试困难。  
4.**替代方案多**：预计算、查表法、近似计算或调用优化库（如Eigen）更高效。  

>**核心原则**：**能提前算的别实时算，能近似的不求精确，能用库就别手写。**

---

### 13.C++是如何实现多态的？

C++通过**虚函数机制**实现运行时多态，核心实现基于**虚函数表（vtable）**和**动态绑定**，具体分为以下三步：

#### **13.1 虚函数表（vtable）**

- 每个包含虚函数的类会隐式生成一个**虚函数表**（编译器实现），表中按序存储该类所有虚函数的地址。
- 对象内存布局首部隐藏一个**vptr指针**（虚表指针），指向对应的vtable。

**示例内存结构**：

```cpp
class Animal {
public:
    virtual void speak() { cout << "Animal sound"; } // 虚函数
};
class Dog : public Animal {
public:
    void speak() override { cout << "Bark"; } // 重写虚函数
};

// 内存布局（简化）：
Dog对象 -> [vptr] → Dog的vtable → [&Dog::speak]
```

---

#### **13.2 动态绑定（运行时决议）**

- 当通过**基类指针/引用**调用虚函数时：
  1.通过对象的`vptr`找到对应vtable。
  2.从vtable中查找函数地址并调用。
- **与静态绑定的区别**：
  - 普通函数：编译时确定地址（静态绑定）。
  - 虚函数：运行时根据对象实际类型确定地址（动态绑定）。

**代码示例**：

```cpp
Animal* animal = new Dog();
animal->speak(); // 输出"Bark"（调用Dog::speak，而非Animal::speak）
```

---

#### **13.3 多态条件**

- **必要条件**：
  1.基类声明虚函数（`virtual`）。
  2.派生类重写（`override`）该虚函数。
  3.通过基类指针或引用调用。

**不满足多态的情况**：

```cpp
Dog dog;
dog.speak();           // 直接调用Dog::speak（静态绑定）
Animal animal = dog;   // 对象切片，丢失多态性
```

**总结**：C++多态的核心是**虚函数表+动态绑定**，通过运行时查表实现“调用哪个函数由对象实际类型决定”。

---

### 14.**3D Gaussian Splatting 渲染原理与过程**  

#### **14.1 核心思想**  

3D Gaussian Splatting（3DGS）是一种**基于点云的实时渲染技术**，用3D高斯分布（椭球形）表示场景，通过**可微的光栅化**(Differentiable Rasterization)直接渲染，无需传统网格或体素。  

#### **14.2 关键技术点**  

- **3D高斯表示**：每个点用位置（μ）、协方差矩阵（Σ）、不透明度（α）和球谐系数（SH，控制颜色）描述。  
- **可微光栅化**：将3D高斯投影到2D屏幕，计算像素贡献（类似“泼溅”效果）。  
- **优化方法**：通过梯度下降动态调整高斯参数（位置、大小、颜色等）。  

#### **14.3 渲染流程**  

(1).**初始化**  

- 从多视角图像或点云（如COLMAP）初始化3D高斯分布。  

(2).**投影与光栅化**  

- **3D→2D投影**：将高斯椭球按摄像机参数投影到屏幕空间。  
- **像素混合**：对每个像素，按深度排序重叠的高斯，计算颜色和透明度（alpha blending）。  

(3).**可微优化**  

- **损失计算**：比较渲染结果与真实图像的差异（L1/L2损失）。  
- **梯度回传**：调整高斯参数（位置、旋转、缩放、透明度等）。  
- **自适应控制**：动态增删高斯（如分裂过大的高斯，删除冗余高斯）。  

(4).**实时渲染**  

- 最终优化后的高斯模型可直接渲染，支持动态视角和实时交互。  

#### **14.4 优势与挑战**  

| **优势** | **挑战** |  
|----------|----------|  
| 实时性能（>100 FPS） | 训练时间较长（小时级） |  
| 高质量细节（毛发、烟雾） | 显存占用高（百万级高斯） |  
| 无需网格/体素 | 动态场景处理较难 |  

#### **14.5 与传统方法对比**  

- **NeRF**：3DGS更快但依赖显存，NeRF更省显存但慢。  
- **点云渲染**：3DGS通过高斯分布实现平滑过渡，避免点云“空洞”。

**总结**：3DGS通过可微高斯泼溅实现高效渲染，平衡了速度与质量，是NeRF之后的重要突破。

---

### 15.渲染相关的问题
>
> **图形渲染管线**
> 从原始顶点数据开始，依次经过顶点处理（坐标变换）、图元组装（连接成三角形）、光栅化（转为像素）、片段处理（着色计算），最后经过深度测试、模板测试等输出到屏幕。现代 GPU 还支持曲面细分、几何着色器等可编程阶段。
>
> **向量点乘和叉乘**
>
> - 点乘结果为标量，可计算两个向量的夹角余弦值（a·b = |a||b|cosθ），常用于光照计算。
> - 叉乘得到垂直于原向量的新向量（右手定则），用于求平面法线、计算面积等。
>
> **欧拉角**
> 用三个旋转角度（俯仰Pitch、偏航Yaw、滚转Roll）表示 3D 旋转。
>
> - **优点**：参数少、直观易懂，适合美术调整。
> - **缺点**：
>   - **万向节死锁**：当某个轴旋转 90 度时，会丢失一个旋转角度的自由度（比如飞机俯仰 90°后，偏航和滚转重合）。
>   - 插值困难：直接对三个角度插值会产生不自然的旋转路径。
>
> **渲染空间变换**
>
> 1.**模型空间**：物体自身坐标系。
> 2.**世界空间**：所有物体统一坐标系。
> 3.**观察空间**：以摄像机为原点的坐标系。
> 4.**裁剪空间**：应用投影矩阵后的齐次坐标。
> 5.**屏幕空间**：映射到最终显示像素坐标。
>
> **模板测试进阶**
> **基本概念**
> 模板测试是渲染管线中的一种像素级过滤机制，通过一个称为模板缓冲区（Stencil Buffer）的额外通道，控制哪些像素可以写入颜色缓冲区和深度缓冲区。
>
> - **比较规则**：将当前像素的模板缓冲值与预设的参考值对比，通过则保留，否则丢弃。
> - **可编程操作**：可定义比较函数（如等于、大于）和测试通过后的操作（如替换、递增）。
>
> **工作原理**
>
> 1.**模板缓冲区初始化**：通常为 8 位整数（0-255），初始值全为 0。
> 2.**测试阶段**（在片段着色器之后，深度测试之前）：
>
> ```cpp
> if (StencilRef & StencilMask ◯ StencilBufferValue & StencilMask) {
>    保留像素，并执行指定操作（如替换、递增）;
> } else {
>    丢弃像素;
> }
> ```
>
> - `◯`：比较运算符（如`GL_EQUAL`、`GL_LESS`）。
> - `StencilMask`：位掩码控制参与比较的位数。
>
> 3.**操作类型**（通过`glStencilOp`设置）：
> - `GL_KEEP`：保持原值（默认）。
> - `GL_REPLACE`：用参考值替换。
> - `GL_INCR`：值 +1（饱和到最大值）。
> - `GL_DECR`：值 -1（饱和到 0）。
>
> 4.**总结**：模板测试是高效实现像素级遮罩、轮廓和空间标记的工具，尤其适合需硬件加速的固定逻辑筛选。
> 除了简单的开关控制，还可以：
> - 设置比较函数（如大于/等于时通过）。
> - 定义通过/失败时的操作（保持/递增/反转等）。
> - 典型应用：游戏中的镜子效果、技能范围指示圈。
>
> **阴影技术对比**
>
> - **Shadow Map**：从光源视角生成深度图，比较场景深度。
> - **PCSS**：添加滤波实现软阴影，更接近真实。
> - **光线追踪阴影**：精确但计算量大，适合离线渲染。
> - 优化方向：级联阴影（CSM）解决远近精度问题。
>
> **半透明渲染要点**
>
> 1.必须关闭深度写入（`glDepthMask(false)`）。
> 2.使用混合方程（如`glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA)`）。
> 3.常见问题：排序错误会导致穿帮（可改用 OIT 顺序无关透明）。
>
> **着色器优化技巧**
>
> - 将循环不变量提到循环外。
> - 用`step()`代替`if-else`分支。
> - 纹理采样时使用 mipmap。
> - 向量运算优先使用硬件指令（如`dot`、`cross`）。
>
> **光线求交优化**
> Möller-Trumbore 算法通过计算重心坐标快速判断相交，相比传统方法：
>
> - 避免预计算平面方程。
> - 直接得到 UV 坐标用于纹理采样。
> - 适合 GPU 并行计算。
>
> **法线贴图细节**
> 法线贴图（Normal Map）是一种用于模拟表面细节的纹理技术，通过存储表面法线方向（而非颜色）来影响光照计算，从而在不增加几何复杂度的情况下增强凹凸视觉效果。
>
> - 存储切线空间法线（RGB 对应 XYZ）。
> - 导入时注意从[0,1]映射到[-1,1]。
> - 压缩格式选择：BC5 保留法线精度。
> - 必须配合切线空间计算光照。
>
> **HDR 工作流**
>
> 1.用 16 位浮点纹理存储场景亮度。
> 2.亮度范围可以超过标准 0-1（如太阳光强度 10.0）。
> 3.最后通过色调映射适配显示器。
>
> **伽马校正原理**
> CRT 显示器响应曲线近似 2.2 次幂，因此需要：
>
> - 输入纹理做γ = 2.2 解码（sRGB→线性）。
> - 输出前做γ = 1/2.2 编码（线性→sRGB）。
> - 现代 GPU 支持自动 sRGB 纹理转换。
>
> **PBR 核心思想**
>
> 1.能量守恒：反射光总量不超过入射光。
> 2.微表面模型：粗糙度影响高光范围。
> 3.基于物理的参数：金属度/粗糙度代替传统高光强度。
> 4.使用 GGX 等更真实的 BRDF 模型。

### ▲ 16.一面: 面试问题 ▲

#### 1.实现**线程私有**变量

**Ⅰ.Go 语言**
Go 没有直接的线程局部存储，但可通过以下方式实现：

- **`context.Context` 传递**  
  通过上下文显式传递数据，适合请求级隔离：

  ```go
  func worker(ctx context.Context) {
      userID, ok := ctx.Value("userID").(int) // 从上下文获取数据
      if ok {
          fmt.Println("User ID:", userID)
      }
  }

  func main() {
      ctx := context.WithValue(context.Background(), "userID", 123)
      go worker(ctx) // 显式传递上下文
  }
  ```

- **`sync.Map` 自定义存储**  
  结合线程ID手动管理私有数据：

  ```go
  var (
      threadLocal = sync.Map{}  // 全局注册表
      idGen       = 0           // 线程ID生成器
      idMutex     sync.Mutex
  )

  func getThreadID() int {
      idMutex.Lock()
      defer idMutex.Unlock()
      idGen++
      return idGen
  }

  func worker() {
      threadID := getThreadID()
      threadLocal.Store(threadID, "private data") // 存储私有数据
  }
  ```

**Ⅱ.C++ 语言**
C++11 引入 `thread_local` 关键字，直接支持线程局部存储：

```cpp
#include <iostream>
#include <thread>

thread_local int counter = 0; // 线程局部静态变量

void worker() {
    counter++; // 每个线程独立操作自己的counter
    std::cout << "Thread " << std::this_thread::get_id() 
              << ": counter = " << counter << std::endl;
}

int main() {
    std::thread t1(worker);
    std::thread t2(worker);
    t1.join();
    t2.join();
}
```

**特点**：  

- 可修饰全局变量、函数内静态变量、类成员变量  
- 生命周期与线程绑定，自动初始化和销毁

**Ⅲ.Python 语言**
Python 的 `threading.local` 提供线程局部存储：

```python
import threading

local_data = threading.local()  # 创建线程局部存储对象

def worker(value):
    local_data.value = value  # 为当前线程设置私有值
    print(f"Thread {threading.current_thread().name}: {local_data.value}")

t1 = threading.Thread(target=worker, args=(1,), name="Thread-1")
t2 = threading.Thread(target=worker, args=(2,), name="Thread-2")

t1.start()
t2.start()
```

**特点**：  

- 自动管理线程关联，无需手动维护线程ID  
- 常用于 Flask 等框架的请求上下文管理

**对比总结**

| 语言   | 实现方式               | 核心机制                  | 数据传递方式 | 生命周期管理         |
|--------|------------------------|---------------------------|--------------|----------------------|
| Go     | `context` / `sync.Map` | 显式传递 / 全局注册表     | 手动传递     | 手动控制             |
| C++    | `thread_local`         | 语言原生关键字            | 隐式关联     | 与线程绑定           |
| Python | `threading.local`      | 线程自动关联存储对象      | 自动关联     | 线程结束时自动清理   |

#### 2.和多线程相关的C++标准库有哪些？

#### 3.100亿数据（Vector, list存不下），如何去除数据中的所有重复数据？

#### 4.正交投影和透视投影

#### 5.1080×1920的分辨率像素域，给定两个随机的点，如何点亮两个点之间的线段（0为不点亮，1为点亮）？

#### 6.相机照片和工业制图的区别

#### 7.齐次坐标的所有相关问题

#### 8.了解KDTree吗？讲下原理

#### 9.去噪：时域去噪和频域去噪

#### 10.http/https的请求指令和返回状态码
