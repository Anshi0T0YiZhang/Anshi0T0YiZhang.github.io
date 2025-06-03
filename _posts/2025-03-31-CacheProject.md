---
layout:     post  # 指定文章的布局（模板）。
title:      缓存算法项目  # 文章标题。
subtitle:   "基于LRU-k, LRU-Slice, ARC算法的缓存项目扩展"  # 子标题。
date:       2025-04-05 12:00:00  # 文章发布时间。
author:     "yZhang"  # 作者名称。
header-img: "img/selfTimer/background.jpg"
tags:       # 文章标签。
    - 项目
---

## 1.KamaCache

<div style="
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: 20px;
    margin-bottom: 20px;
">
    <!-- 第一行 -->
    <div style="width: 48%;">
        <img src="/img/Cache/projectIntroduction_4.jpg" alt="test config" style="width: 100%; border: 1px solid #eee; display: block;"/>
        <p style="text-align: center; font-weight: bold; margin-top: 5px;">项目配置</p>
    </div>
    <div style="width: 48%;">
        <img src="/img/Cache/projectIntroduction_3.jpg" alt="lru slice" style="width: 100%; border: 1px solid #eee; display: block;"/>
        <p style="text-align: center; font-weight: bold; margin-top: 5px;">LRU Slice 算法</p>
    </div>
    <!-- 第二行 -->
    <div style="width: 48%;">
        <img src="/img/Cache/projectIntroduction_2.jpg" alt="lru k" style="width: 100%; border: 1px solid #eee; display: block;"/>
        <p style="text-align: center; font-weight: bold; margin-top: 5px;">LRU-k 算法</p>
    </div>
    <div style="width: 48%;">
        <img src="/img/Cache/projectIntroduction_1.jpg" alt="arc" style="width: 100%; border: 1px solid #eee; display: block;"/>
        <p style="text-align: center; font-weight: bold; margin-top: 5px;">ARC 算法</p>
    </div>
</div>

## 2.Cache BackGround

1.**业界相关的关于 Cache 的工具和技术：Redis居多**

- Redis 是一个开源的基于内存的数据库，读写速度非常快，通常被用作缓存、消息队列、分布式锁和键值存储数据库。它支持多种数据结构，如字符串、哈希表、列表、集合、有序集合等，除此之外，Redis 还支持事务、持久化、Lua 脚本、多种集群方案（主从复制模式、哨兵模式、切片机群模式）、发布 / 订阅模式，内存淘汰机制、过期删除机制等。
- 具有以下特点：
  - **基于内存**：Redis 将数据存储在内存中，使得它具有快速的读写访问速度。这也使得 Redis 适用于需要高性能的应用场景，比如缓存。
  - **持久性**：虽然 Redis 主要是内存中的存储系统，但它可以通过将数据持久化到磁盘上的快照和日志文件来保证数据的持久性，防止数据丢失。
  - **多数据结构**：Redis 不仅支持简单的键值对存储，还提供了丰富的数据结构，如列表、哈希表、集合等，使得它更灵活地适应不同的应用场景。
  - **原⼦性操作**：Redis 支持原⼦性操作，这意味着它可以保证一个操作是原⼦的，要么执⾏成功，要么完全不执⾏，这对于并发环境下的数据⼀致性很重要。
  - **分布式**：Redis 提供了分布式特性，可以将数据分布在多个节点上，以提高可扩展性和可用性。

2.**Redis Cache Problems**
<img
  src="/img/Cache/imageCacheProblems.png"
  alt="缓存常遇到的问题"
  width="70%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">缓存常遇到的问题</p>

## 3.基于AndroidVideoCache的优化

### 3.1 项目地址

- **[AndroidVideoCache](https://github.com/danikula/AndroidVideoCache)**：基础的开源 Demo，本博客讨论的后续优化基本都是基于此框架完成的。

- **优化方向 1**：**[VideoPreload](https://github.com/zhuozp/VideoPreload)**：实现视频的预加载功能，防止在视频切换时出现卡顿现象。

- **优化方向 2**：**[SliceCache](https://github.com/JeffMony/MediaSDK)**：主要实现视频的分片缓存功能（没有细看其他功能），防止一直缓存整个视频导致 OOM。

  **考量**：从实现难度和理解讲解所需时长考虑，本文先探究优化方向 1 的内容，后续再探究优化方向 2 的内容。

### 3.2 项目初步运行

- **运行环境**：
  - 系统环境：Linux Ubuntu 18.04
  - IDE 编辑器：VSCode

- **安装依赖**：
  - VSCode 自动检测：Gradle for Java; Extension Pack for Java（未安装）

- **添加依赖**：
  - 首先，在项目的 build.gradle 文件中添加以下依赖：

    ```gradle
    dependencies {
        implementation 'com.danikula:videocache:2.7.1'
    }
    ```

- **初始化缓存服务器**：
  - 在你的 Application 类中初始化缓存服务器
  - 项目路径（如下）中有这个操作：

    ```java
    sample/src/main/java/com/danikula/videocache/sample/App.java
    ```

- **同步 Gradle**：
  - 在 Android Studio 中：
    - 点击顶部工具栏的 "Sync Now"。
    - 或在终端运行：

      ```bash
      ./gradlew build
      ```

    - 报错：

      ```bash
      (base) hx@hx-Z370-HD3:~/ZY/VideoCache/AndroidVideoCache$ ./gradlew build

      ERROR: JAVA_HOME is not set and no 'java' command could be found in your PATH.

      Please set the JAVA_HOME variable in your environment to match the
      location of your Java installation.
      ```

    - 解决方法：
      - 安装 `java-17`：`sudo apt install openjdk-17-jdk`。
      - 查找 Java 安装路径：

        ```bash
        sudo update-alternatives --config java
        ```

        会显示如下的输出：

        ```bash
        There is 1 choice for the alternative java (providing /usr/bin/java).

        Selection    Path                                            Priority   Status
        ------------------------------------------------------------
        * 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
        ```

      - 设置临时环境变量 + 验证设置：

        ```bash
        (base) hx@hx-Z370-HD3:~/ZY/VideoCache/AndroidVideoCache$ export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
        (base) hx@hx-Z370-HD3:~/ZY/VideoCache/AndroidVideoCache$ export PATH=$JAVA_HOME/bin:$PATH
        (base) hx@hx-Z370-HD3:~/ZY/VideoCache/AndroidVideoCache$ echo $JAVA_HOME
        /usr/lib/jvm/java-11-openjdk-amd64
        (base) hx@hx-Z370-HD3:~/ZY/VideoCache/AndroidVideoCache$ java -version
        openjdk version "11.0.26" 2025-01-21
        OpenJDK Runtime Environment (build 11.0.26+4-post-Ubuntu-1ubuntu120.04)
        OpenJDK 64-Bit Server VM (build 11.0.26+4-post-Ubuntu-1ubuntu120.04, mixed mode, sharing)
        ```

      - 重新运行 `./gradlew build`。
      - Java 版本不匹配的话，可尝试降低 java 版本（java 8）或更新 Gradle 版本，然后重新配置环境变量。

- **修改 AndroidManifest.xml**：
  - 在 AndroidManifest.xml 中注册你的 Application 类：

    ```xml
    test/src/main/AndroidManifest.xml
    ```

- **使用缓存服务器**：
  - 在播放视频时，使用缓存服务器提供的 URL。

### 3.3 在Window上运行项目

- Vscode 太麻烦，尝试了一段时间行不通。
- 然后在 Android Studio 中运行项目。

### 3.4 这个项目是个空包

倒也不全是，能在他的 github activity 中看到他的一些 commit 和提交记录。
