---
layout: post
title: Web实时音视频
subtitle: "WebRTC实现视频会议"
date: 2025-04-18 17:37
author: "nothing丿zip"
header-img: " "
tags: 
    - WebRTC
---

### 1.项目背景

在线视频会议是现代互联网应用中不可或缺的一部分。在线视频会议可以让多个参与者在线一起进行视频会议，并共享自己的屏幕、摄像头、麦克风等设备。
流媒体是一种**将视频、音频等连续媒体以流的形式，通过网络实时传输到客户端进行播放的技术**。  
PTSP协议介绍：  

>+ RTSP（Real Time Streaming Protocol），RFC2326，实时流传输协议，是TCP/IP协议体系中的一个应用层协议。该协议定义了**一对多应用程序如何有效地通过IP网络传送多媒体数据。RTSP在体系结构上位于RTP和RTCP之上，它使用TCP或UDP完成数据传输。**  
>+ **RTSP本身并不传送数据**，而仅仅是是媒体播放器能控制多媒体流的传送，暂停播放，快进快退等。**实际数据传输依赖 RTP/RTCP（视频/音频）和 RTCP（质量控制）。**  
>+ RTSP 是实时流控制的经典协议，在监控、广电等领域仍不可替代，但**在Web端逐渐被WebRTC和基于HTTP的协议（如WHIP/WHEP）补充**。选择时需权衡延迟、控制需求和环境兼容性。

<img
  src="/img/WebVideo/applicationProtocol.png"
  alt="现代的流媒体通信协议栈"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 20px; /* 下边距设为20px */
  "
/>

WHIP/WHEP协议介绍：

>+ WHIP（WebRTC HTTP Ingestion Protocol）和 WHEP（WebRTC HTTP Egress Protocol）是基于 HTTP 的轻量级 WebRTC 信令协议，旨在简化实时音视频流的 推流（Ingestion） 和 拉流（Egress） 流程。它们**通过标准化接口解决传统 WebRTC 信令复杂的问题**，成为现代低延迟流媒体的重要技术。
>+ WHIP核心功能  
用途：将音视频流从客户端（如浏览器、摄像头）**推送**到媒体服务器。  
类比：类似 RTMP 推流，但基于 WebRTC 技术栈，延迟更低（<1秒）。
>+ WHEP核心功能  
用途：从媒体服务器**拉取音视频流**到播放端（如浏览器、APP）。  
类比：类似 HLS/DASH 拉流，但延迟低至 500ms。

### 2.常用术语

>+ **WebRTC: Web Real-Time Communication; 网页实时通信**

>+ WHIP: WebRTC-HTTP Ingestion Protocol; WebRTC-HTTP 推流协议
>+ WHEP: WebRTC-HTTP Egress Protocol; WebRTC-HTTP 拉流协议
>+ **SDP: Session Description Protocol; 会话描述协议**
>+ TCP: Transmission Control Protocol; 传输控制协议
>+ RTSP: Real Time Streaming Protocol; 实时流协议
>+ RTP: Real-time Transport Protocol; 实时传输协议
>+ RTCP: RTP Control Protocol; RTP 控制协议
>+ RTMP: Real Time Messaging Protocol; 实时消息传输协议

>+ HLS: HTTP Live Streaming; HTTP 直播流媒体
>+ MMS: Microsoft Media Server; 微软媒体服务器
>+ RSVP: Resource Reservation Protocol; 资源预留协议

>+ **ICE: Interactive Connectivity Establishment; 交互式连接建立**
>+ STUN: Session Traversal Utilities for NAT; 会话遍历实用程序
>+ TURN: Traversal Using Relays around NAT; 穿越 NAT 的中继
>+ **SFU: Selective Forwarding Unit; 选择性转发单元**

### 3.主要技术

#### 3.1 WebRTC

WebRTC，是一个支持网页浏览器进行实时音视频通信的开源项目，允许网页应用或站点在不借助中间媒介的情况下，**建立浏览器之间的点对点（P2P）连接，实现音视频流或其他任意数据的传输。**

技术组件：  
>
>+ RTCPeerConnection：用于建立和管理点对点的实时通信连接，是WebRTC的核心组件，负责音视频的采集、编解码、网络传输和显示等功能。  
>+ MediaStream：表示一个媒体数据流，可以包含音频、视频或其他类型的数据，开发者可通过MediaStream API获取和操作媒体流。  
>+ RTCDataChannel：用于在点对点连接中传输任意类型的数据，如文本、图片、文件等。  
>+ 信令服务器：用于交换SDP和ICE候选信息，不处理实际的音视频数据，仅负责协调通信的初期协商。
>+ SDP（会话描述协议）：提供媒体类型、编解码器、带宽要求等信息，描述会话的具体配置，使两个客户端能够相互了解对方的通信能力并进行匹配。  
>+ ICE（交互式连接建立）：帮助客户端穿越NAT，通过收集不同网络路径的候选者，找到可以建立P2P连接的最佳路径。

WebRTC主要让浏览器具备三个作用:
>
>+ 获取音频和视频
>+ 进行音频和视频通信
>+ 进行任意数据的通信

#### 3.2 Websocket

 WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

   WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据（这是HTTP协议无法做到的）。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

#### 3.3 Live777

Live777 是一个高性能的边缘 WebRTC SFU服务器，主要用于实时视频流处理。它支持 WHIP/WHEP 协议，能够与其他 WebRTC 应用模块进行互操作，而无需进行自定义适配。

<img
  src="/img/WebVideo/sfu.png"
  alt="媒体服务器"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 0; /* 下边距设为0px */
  "
/>
<p style="text-align: center; font-weight: bold;">媒体服务器架构</p>

Live777 的主要特点包括：

+ 支持 WHIP/WHEP 协议：提高了与其他 WebRTC 应用模块的互操作性。  
+ P2P/SFU 混合架构：仅负责转发，不进行混流、转码等资源消耗较大的媒体处理工作。
+ 多平台支持：支持 Linux、MacOS、Windows、Android 和 ARM、x86 等多平台。

该项目的主要编程语言是 Rust，同时也使用了 TypeScript 和 JavaScript 进行部分前端和脚本开发。

#### 3.4 项目逻辑和结构
>
> 1.ICE机制建立网络连接, 通过一系列的技术（如 STUN、TURN 服务器）帮助通信双方发现和协商可用的公共网络地址，从而实现 NAT 穿越;
   实现用户间的元数据(例如用户名,用户状态等)交换, 并将其存储在数据库中。
> 2.通过 WebRTC 提供的 API 获取各端的媒体信息 SDP 以及网络信息 candidate ，并通过信令服务器交换，进而建立了两端的连接通道完成实时视频语音通话。

##### 3.4.1 项目流程

###### 媒体协商（SDP 交换） 

目的：协商双方的媒体能力（如支持的编解码器、分辨率等）。

<img
  src="/img/WebVideo/SDP.png"
  alt="信息交换"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 0; /* 下边距设为0px */
  "
/>
<p style="text-align: center; font-weight: bold;">SDP信息交换</p>

> + **创建 RTCPeerConnection 对象**  
  · 双方（A 和 B）分别创建 `RTCPeerConnection` 实例。  
  · 配置 STUN/TURN 服务器信息以支持 NAT 穿透。  

> + **采集本地媒体流**  
  · 通过 `getUserMedia()` 获取摄像头和麦克风的媒体流。  
  · 使用 `addTrack()` 将媒体流添加到 `RTCPeerConnection`。  

> + **生成 Offer（由发起方 A）**  
  · A 调用 `createOffer()` 生成 **SDP Offer**，描述本端的媒体能力和参数（如音视频编码格式、SSRC 等）。  
  · A 调用 `setLocalDescription(offer)` 将 Offer 设为本地描述。  

> + **发送 Offer 至对端 B**  
  · 通过信令服务器（如 WebSocket）将 Offer 传输给 B。  

> + **处理 Offer（由响应方 B）**  
  · B 调用 `setRemoteDescription(offer)` 设置 A 的远端描述。  
  · B 调用 `createAnswer()` 生成 **SDP Answer**，描述自身支持的媒体参数。  
  · B 调用 `setLocalDescription(answer)` 设置本地描述。  

> + **发送 Answer 至发起方 A**  
  · B 通过信令服务器将 Answer 返回给 A。  

> + **完成 SDP 交换**  
  · A 收到 Answer 后调用 `setRemoteDescription(answer)`。  
  · 双方完成媒体能力协商。

###### 网络穿透（ICE Candidate 交换）

目的：发现两端间的可用网络路径，建立 P2P 连接；使用 STUN和 TURN服务器，解决 NAT 和防火墙问题。

> + **收集 ICE Candidate**  
  · 每当 `RTCPeerConnection` 发现新候选时，触发 `onicecandidate` 事件。  
  · 候选信息包含 IP、端口、协议和优先级。  

> + **发送 Candidate 至对端**  
  · 通过信令服务器将候选实时传输给对方。  
  · 使用 **Trickle ICE 机制**（逐步发送候选，而非等待全部收集完成）。  

> + **添加远端 Candidate**  
  · 双方通过 `addIceCandidate()` 将收到的候选添加到连接中。  
  · ICE 框架按优先级尝试进行连通性检查。

###### 连接建立与媒体传输  

目的：通过 ICE 协议选择最优路径，建立安全的传输通道。
>
> + **ICE 连通性检查**  
  · ICE 代理按优先级配对本地和远端候选，发送 STUN 请求测试连通性。  
  · 成功响应后确认可用路径（可能为直连或中继）。  

> + **安全传输**  
  · 使用 **DTLS** 协商加密密钥，建立安全连接。  
  · 音视频数据通过 **SRTP** 加密传输。  

> + **状态监控**  
  · 监听 `onconnectionstatechange`（如转为 `connected` 表示成功）。  
  · 处理 `oniceconnectionstatechange`（如 `failed` 时尝试回退到 TURN 中继）。

##### 3.4.2 <span style="color:rgb(235, 84, 15);">项目结构</span>

###### 信令服务器

在你的项目中，信令服务器通过 api.go 文件中的路由配置和反向代理实现。具体步骤如下：

+ 路由配置：定义多个路由处理函数，用于房间管理和用户管理。
+ 反向代理：将 WHIP 和 WHEP 请求转发到 SFU 服务器（live777Url），SFU 服务器负责处理这些请求并管理媒体流。
+ 中间件：使用 JWT 认证中间件确保请求的安全性。
+ 静态文件服务器：处理所有其他请求，提供前端资源。

###### 媒体服务器

分为两个关键的类：Client和Context;

| 特性/功能 | 客户端（Client） | 上下文（Context） |
|---------|----------------|----------------|
| 定义 | 具体的工具类或库，用于与远程服务器交互 | 封装本地逻辑的管理器，协调本地与远程的交互 |
| 功能 | 处理信令交换和流传输 | 管理本地设备、用户状态、流生命周期等 |
| 职责 | 专注于完成流的发布或接收任务 | 专注于管理本地复杂逻辑并调用客户端完成任务 |
| 使用场景 | 直接与远程服务器通信 | 协调本地资源并与远程服务器交互 |

### 4.进阶优化
>
>优化目标: 重点关注下如何能让音视频通信稳定、流畅、可靠，也就是关乎视频会议的质量体验问题。  
>基础条件: WebRTC中已经具备了一些保障QoS的策略，比如ARQ，FEC，Jitter Buffer(抖动缓冲)，Congestion Control(拥塞控制)等。

#### 4.1 典型SFU传输链路

在典型的SFU传输链路中，媒体流(RTP数据包)由Sender发送到Receiver，媒体控制流(RTCP包)由Receiver反馈给Sender。控制流中包括了NACK, PLI, REMB, Receiver Report等反馈信息。这些反馈信息是配合QoS策略的辅助手段。有了这些QoS策略的加持，WebRTC的视频通话能够对抗一定程度的网络状况，正常情况下的通话质量可以保障。  
但是，这种默认的策略也存在明显的改进空间，比如：

+ QoS的策略是在端到端之间生效的，接收端发现丢包后，才会向发送端发送NACK请求重传，全链路的路径(rtt)过长，影响数据重传和恢复的效率。  
+ 接收端在发现无法恢复视频帧后，才会发送PLI(Picture Lost Indicator)向源端请求关键帧，直到下一个关键帧到达前，所有链路上的视频帧都无法正常解码，影响接收端的视频帧率，较大概率造成卡顿。

<img
  src="/img/WebVideo/traditionalpassing.png"
  alt="典型的SFU传输链路"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">典型的SFU传输链路</p>

#### 4.2 优化策略

1.如下图所示，在改进的SFU传输架构中，重传请求不再是全链路反馈，而是在客户端和服务器之间进行。一方面，服务器具备了NACK请求的能力，及时发现上行链路的丢包，及时向发送到请求重传。另一方面，服务器能够及时响应接收端的NACK请求。丢包重传的效率提升，有助于减少端到端延时，改善音视频体验。

<img
  src="/img/WebVideo/sfuImprove.png"
  alt="改进的SFU传输架构"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">改进的SFU传输架构</p>

2.对于弱网下视频帧率较低的问题，除了优化传输过程中的FEC+NACK策略之外，还可以从源端编码器入手调整。  
常规的RTC系统中的编码GOP是IPPP…P，每一个P帧都作为参考帧，一旦某一帧有数据包缺失，其后的所有P帧都无法正常解码，抗误码扰动的能力比较差。
一种改进的思路是，改变编码器的参考帧选择，采用长参考帧Long-Term Reference (LTR) frames机制。
引入LTR后，P帧不再是单一的前向参考，而是会有选择性的参考一些固定的帧，只要这部分固定的参考帧能够完整被接收，就能确保其他的完整帧能够正常解码，提高接收端的视频帧率，保障流畅。这种编码方式是比较适合于RTC系统的，能够对抗更大的网络抖动。如图：

<img
  src="/img/WebVideo/LTR.png"
  alt="Long-Term Reference (LTR) frames"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">Long-Term Reference (LTR) frames</p>

3.在多人会议情况下，各个接收端的带宽能力是不相同的，每条链路的带宽估计值都会反馈到发送端，由发送端来统一决策，控制编码和发送码率。这会带来两个潜在的问题：

+ 多条链路的带宽反馈导致发送端的决策困难，编码/发送码率容易抖动。

+ 某一个接收端的网络带宽不足(如图中的300k下行)，发送端就会降低码率以适配当前带宽，导致每个接收端的体验都会下降，这显然是不合理的。
<img
  src="/img/WebVideo/multiMeeting.png"
  alt="Multi-meeting"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>

<p style="text-align: center; font-weight: bold;">Multi-meeting</p>

这里要借助于两种源端编码策略 - Simulcast和SVC。

>simulcast（同时发送不同分辨率的视频流，适用于高带宽用户）  
原理：发送端同时生成多档码率的独立视频流（如高清、标清、低清），接收端根据自身带宽选择订阅对应流。  
优势：避免全局码率耦合，高带宽用户不受低带宽用户影响。  
缺点：占用更多上行带宽和编码资源。

<img
  src="/img/WebVideo/simulcast.png"
  alt="simulcast"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">simulcast</p>

> SVC（可伸缩视频编码，Scalable Video Coding）  
原理：将视频分层编码（基础层+增强层），接收端按需接收部分层。
基础层：保证最低可看画质（如300kbps）。  
增强层：叠加提升画质（如2Mbps）。  
优势：节省上行带宽，动态适配更灵活。  
缺点：编码复杂度高，兼容性要求高。  

<img
  src="/img/WebVideo/SVC.png"
  alt="SVC"
  width="50%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 0px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">SVC</p>

Simulcast和SVC各自的优点:
<img
  src="/img/WebVideo/SimulcastAndSVC.png"
  alt="两种技术的优劣总结"
  width="60%"
  style="
    border: 1px solid #eee;
    display: block;
    margin: 0 auto 10px; /* 下边距设为10px */
  "
/>
<p style="text-align: center; font-weight: bold;">Simulcast和SVC的优劣总结</p>

### 5.通俗理解 (仅供参考)

#### 信令服务器与媒体服务器的业务和交互

##### 1.信令服务器：通话的“协调员”

###### 业务逻辑

+ **职责**：处理所有**非音视频数据**的沟通任务。

+ **核心工作**：
  + 协调通话双方（例如：“有人加入房间！”）。
  + 交换网络地址和媒体格式（如IP、端口、视频编码格式）。
  + 管理房间状态（如人员进出、静音、屏幕共享通知）。

###### 通俗示例

+ 你创建一个房间（房间号123），朋友加入时：
  1.信令服务器通知你：“朋友来了！”
  2.双方通过信令服务器交换“联系方式”（SDP Offer/Answer）。
  3.确认后，音视频通话开始，信令服务器退居幕后。

##### 2.媒体服务器（SFU）：音视频的“快递站”

###### 业务逻辑

+ **职责**：直接传输**音视频数据流**，不处理内容。

+ **核心工作**：
  + 接收并转发音视频流（如你的摄像头画面）。
  + 优化传输效率（如网络差时降低分辨率）。
  + 不关心通话内容（只管“搬运”数据）。

###### 通俗示例

+ 你的视频流发送到SFU后：
  1.SFU将画面转发给朋友。
  2.朋友看到的是SFU实时转发的视频，但SFU不知道你们聊的是工作还是晚饭。

##### 3.信令服务器和SFU如何配合？

**创建房间**  

+ 你点击“创建会议”，信令服务器生成房间号，并通知SFU：“房间123已开，准备接活！”

**加入房间**  

+ 朋友输入房间号123，信令服务器：
  + 检查权限并回复：“可以加入！”
  + 通知SFU：“房间123有新成员，准备接收他的视频流。”

**交换连接信息**  

+ 你发送SDP Offer（网络地址和视频格式）→ 信令服务器 → 朋友。
+ 朋友回复SDP Answer → 信令服务器 → 你。

**建立传输通道**  

+ 双方直接与SFU建立连接，开始传输音视频流。

**实时控制**  

+ 静音操作：信令服务器通知SFU：“别转发他的麦克风声音！”
+ 网络优化：SFU自动降低视频质量，无需信令服务器介入。

##### 4.为什么要把它们分开？

+ **分工明确**  
  + 信令服务器：专注协调（轻量级文本）。
  + SFU：专注搬运（高负载音视频数据）。

+ **扩展性**  
  + 1万人会议时：部署多个SFU分担流量，信令服务器只需管理房间状态。

+ **安全性**  
  + 信令服务器加强权限控制，SFU专注传输优化。

##### 5.现实中的类比

+ **信令服务器** ≈ 客服中心  
  + 协调物流信息，不关心包裹内容。

+ **SFU** ≈ 物流中转站  
  + 只负责分拣和派送包裹，不管为什么寄送。

### 6.WebRTC八股

#### 6.1 WebRTC中GCC和BBR的区别

1.1 GCC

GCC（Google Congestion Control）：主要基于丢包和延迟来估计网络拥塞情况。
应用场景：视频通话、实时直播。

1.2 BBR

BBR（Bottleneck Bandwidth and Round-Trip Time）：是一种基于TCP的拥塞控制算法，主要基于带宽和往返时间（RTT）来估计网络拥塞情况。
应用场景：文件传输、在线游戏。
