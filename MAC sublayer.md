## The Medium Access Control (MAC) Sublayer

[TOC]

- 介质访问子层
- 针对广播式的信道，比如无线发送
  - 问题：某个时刻，由哪个station来发送？
- MAC层是链路层的一个子层，**bottom part of data link layer**

### The channel allocation problem

- 从硬件来分网络：点对点、广播

- 广播网络：有竞争时，如何决定谁用这个信道

- 静态信道分配：TDM，FDM

  - traffic load多了之后会产生浪费

- FDM

  - sender的数目很大，经常发生变化
  - 或bursty（突发性的），就不有效
  - 根据排队论，当service rate和arrival rate都服从泊松分布，且只有1个人来服务
    - M/M/1 queue（最基本的一种队列），简单结论：
    - 一个人被服务到的平均时间T = 1/(**service rate** - **arrival rate**)
    - 考虑queue in delay，在队列的等待时间
    - n个station，在一个共享的broadcast channel上发送的情况，每个station发送一个frame的时间（还和FDM没有关系）

  <img src="MAC%20sublayer.assets/image-20201212152100005.png" alt="image-20201212152100005" style="zoom:50%;" />

  - 考虑FDM

    - 每个subchannel上能看到的到达的帧只有lamda/N了
    - 如果把channel分成N份，延迟是原来的N倍

    <img src="MAC%20sublayer.assets/image-20201212153028307.png" alt="image-20201212153028307" style="zoom:50%;" />

  - queueing theory：有用的工具

    <img src="MAC%20sublayer.assets/image-20201212153629592.png" alt="image-20201212153629592" style="zoom:50%;" />

    <img src="MAC%20sublayer.assets/image-20201212153923957.png" alt="image-20201212153923957" style="zoom:50%;" />

#### Five key assumptions

- Independent Traffic 独立传输
  - n个station互相独立：不会出现你发一个的时候我也发一个
- Single channel 单信道
- Collision assumption
  - 如果有两个station同时发送，就会产生冲突
- Time assumption
  - continuous / slotted
- Carrier assumption 载波假设
  - carrier sense / no carrier sense
  - 说话的时候是否先看看有没有别人正在说话

### Multiple access protocols

#### ALOHA

- 用户有数据时就发送

  - There will be collisions and the colliding frames will be destroyed
  - The sender just waits a random amount of time and sends it again if a frame is destroyed

- 假设有反馈机制，才能判断发送的帧有没有被destroyed

- Pure ALOHA / slotted ALOHA

- **不带载波侦听**

- A method for **doubling the capacity** of an ALOHA system

  - Time is slotted: 需要等到slot开头
  - The vulnerable period is halved for slotted ALOHA

- 具体分析

  - frame time：发送一个frame需要的时间
  - 服从泊松分布
  - 后面掉下来：冲突越来越多
  - Internet access over the cable / multiple RFID tags talk to the same RFID reader

  <img src="MAC%20sublayer.assets/image-20201212160209474.png" alt="image-20201212160209474" style="zoom:50%;" />

  <img src="MAC%20sublayer.assets/image-20201212160425773.png" alt="image-20201212160425773" style="zoom: 80%;" />

- Exercise

  ![image-20201212161152560](MAC%20sublayer.assets/image-20201212161152560.png)

  - <img src="MAC%20sublayer.assets/image-20201212161630985.png" alt="image-20201212161630985" style="zoom:50%;" />

#### CSMA

- 不听就说 / 先听再说
- Carrier Sense Multiple Access Protocols 载波侦听多路访问
- **CD: collision detection**
  - without CD: persistent CSMA / non-persistent CSMA / p-persistent CSMA
  - with CD

##### Persistent CSMA

- station在发送之前侦听信道
  - 如果信道是idle的，则发送一个帧
  - 如果信道是busy的，等待，直到信道变成idle，而后就发送一个帧
  - 如果发生冲突，station等待一个随机的时间，再重新开始
- 1-persistent
  - the station transmits with a probability of 1 whenever it finds the channel idle
- 当两个station都在等第三个station发完，则必然会冲突
- propagetion delay影响冲突发生可能
  - 有可能在一个station开始发送的时候，另一个station准备发送，并去侦听信道
  - 如果第一个station的信号还没有到达第二个station，此时第二个station会侦听到一个idle的信道，从而也开始发送，进而发生冲突
  - 发生的可能性依赖于
    - the number of frames that fit on the channel
    - or the **bandwidth-delay (BD) product of the channel**
    - Low BD product --> small chance of collision
      Large BD product --> high chance of collision

##### Non-persistent CSMA

- 不同：如果信道busy，station不会继续侦听，而等随机时间，然后重新开始
- 优缺点
  - less greedy: Better channel utilization than 1-persistent CSMA
  - longer delays than 1-persistent CSMA

##### p-persistent CSMA

- 用于slotted channels
- 在发送之前，station侦听信道
  - 如果信道idle，有p的概率会发送，有1-p的概率会延迟到下一个slot
  - 如果下一个slot的时候还是idle，则它或发送或再次延迟
    - 以概率p和q，q = 1 - p
  - repeated直到这个帧被发送了或另一个station开始发送了
  - 如果信道busy，则等待到下一个slot，再根据上述算法计算
- IEEE 802.11 uses a refinement of p-persistent CSMA

<img src="MAC%20sublayer.assets/image-20201212203724742.png" alt="image-20201212203724742" style="zoom: 80%;" />

- 关于p-persistent CSMA的delay

  <img src="MAC%20sublayer.assets/image-20201212203837438.png" alt="image-20201212203837438" style="zoom:50%;" />

  - t = q / p

##### CSMA with CD

- Collision Detection

- 带CD：**发送数据帧的同时还能监听信道**

  - 刚才的：类似半双工，要么在发送状态，要么在侦听状态
  - 硬件需要提供这样的能力
  - 一旦侦听到发送的和侦听的不一样，就立马结束发送

- 经典以太网的核心技术

- 有三种状态：contention, transmission, idle

  <img src="MAC%20sublayer.assets/image-20201212205401495.png" alt="image-20201212205401495" style="zoom: 80%;" />

- <img src="MAC%20sublayer.assets/image-20201212205449277.png" alt="image-20201212205449277" style="zoom:50%;" />

#### Collision free protocols

- CSMA/CD在contention阶段还是存在collision的

##### Bit-Map Protocol

![image-20201212211810930](MAC%20sublayer.assets/image-20201212211810930.png)

- Each contention period consists of exactly slots.
- In general, station inserts 1 in time slot in order to send data.
- After all slots have passed by, each station has complete knowledge of which stations wish to transmit. Then they begin transmitting in numerical order. Since everyone agrees on who goes next, there will never be any collisions.
- After the last ready station has transmitted its frame, another bit contention period is begun.

![image-20201212213947358](MAC%20sublayer.assets/image-20201212213947358.png)

- 与ALOHA不同，load is high时效率高

##### Token Passing

- 令牌是唯一的，有令牌才能发送

- Token Ring

  <img src="MAC%20sublayer.assets/image-20201212214627610.png" alt="image-20201212214627610" style="zoom: 67%;" />

  - 没有bias
  - 如果station有个等待传输的帧队列，当它接收到令牌就可以发送帧，然后再把令牌传递到下一站
  - 如果它没有排队的帧要传，则只是把令牌传递下去

##### Binary Countdown

- 对于bit-map来讲，load较低时为d / d+N

  - 如果N很大，效率很低

- 固定contention phase的长度

- 加是or

- 发送的是0，但看到的是1，就退出竞争

- 分析：

  <img src="MAC%20sublayer.assets/image-20201212220358136.png" alt="image-20201212220358136" style="zoom:50%;" />

#### Limited-contention protocols

<img src="MAC%20sublayer.assets/image-20201212220548792.png" alt="image-20201212220548792" style="zoom:67%;" />

- 

#### Wireless LAN Protocols

- **Hidden station problem / Exposed station problem**

  - 隐藏终端问题 / 暴露终端问题

  <img src="MAC%20sublayer.assets/image-20201216102349364.png" alt="image-20201216102349364" style="zoom:50%;" />

  - 无线传输是有距离的
  - 对a，这两个signal实质是不能同时发送的
  - 对b，这两个signal实质是可以同时发送的
  - 问题本质：希望感知的是接受者附近的signal，但传统上只能感知发送者的

- **MACA protocol**  Multiple Access with Collision Avoidance

  <img src="MAC%20sublayer.assets/image-20210125210610127.png" alt="image-20210125210610127" style="zoom:50%;" />
  
  - RTS: request to send, CTS: Clear transmiton to send
  - 发送数据包之前先进行一次握手
  - A发送RTS给B，B给A回复一个CTS
- RTS，CTS的发送都是广播的形式
  - **Collision can still occur, e.g. RTS**

  <img src="MAC%20sublayer.assets/image-20201216104049834.png" alt="image-20201216104049834"  />
  
  - D听到CTS后就会停止发送
- E听到了RTS和CTS，停止发送
  - C是可以继续发送的（exposed terminal），听到RTS，等待一小段时间（防止把B发送的CTS冲掉），而后可以继续发送

  <img src="MAC%20sublayer.assets/image-20201216104506862.png" alt="image-20201216104506862" style="zoom:67%;" />
  
  - A要向B发送数据：A先发送RTS，B再回CTS
  - 这个CTS：A和C都能听到，CTS中规定了只能A发送，C听到后就停止了
  - 后半部分：类似的
  - CTS起到比较关键的作用

### 802.3: Ethernet

- 802.3 standard describes a whole family of **1-persistent CSMA/CD system**, running at speeds from 1 to 10-Mbps over various media. (802.3 standard)

#### Classic Ethernet Physical Layer

- 10Base5

  - 10代表以太网速率是10Mbps，5代表500m
  - 打电话用的是双绞线

- 编码方式

  - 0 volts表示0 bit，5 volts表示1bit：无法得知是idle还是在发送
  - -1表示0 bit，1表示1 bit：时序很难分辨（发送100个1，接收方可能认为是99个1）

- Manchester编码：自带时序

  - Differential Manchester encoding
  - 有transition：0

  <img src="MAC%20sublayer.assets/image-20201216110308166.png" alt="image-20201216110308166" style="zoom:50%;" />

#### MAC Sublayer Protocol

- 帧格式有两种：先是DIX，IEEE后来推出了802.3
  - pad：补充成整数倍
  - 以太网的帧：有最小的payload
  - 判断是type还是length
  - 配置路由器：MTU，最大传输包的长度一般是1500
  - Data：最小要46个字节
  - Preamble：8字节，包含10101010，用来做同步
    - 区分边界；做同步
  - Checksum：32比特，CRC

<img src="MAC%20sublayer.assets/image-20201216110618542.png" alt="image-20201216110618542" style="zoom: 67%;" />

##### CSMA-CD

- **τ：相隔最远的两个station之间的propagation time**

- 一个station发送了数据包后至少要侦听2τ的时间

  - **检测出最长的collision需要2τ的时间**
  - 一定在2τ时间内检测出冲突

  <img src="MAC%20sublayer.assets/image-20201216154033498.png" alt="image-20201216154033498"  />

- **希望数据包持续的长度 >= 2τ**

  - 这样可以对每个包都进行判断它是否产生collision

- 细节：**最小的以太网的帧的payload长度** >= 46 **Bytes**

  <img src="MAC%20sublayer.assets/image-20201216154402081.png" alt="image-20201216154402081" style="zoom: 50%;" />

  - 这里为什么不减去Preamble的长度
    - Preamble不一定是decodable的数据，可能就是模拟信号，不需要对它解码

<img src="MAC%20sublayer.assets/image-20201216113317265.png" alt="image-20201216113317265" style="zoom:50%;" />

- Propagation time = 1/200000 = 5us
- Minimum frame size = 1Gbps*(2\*5us) = 10000bits

##### The Binary Exponential Backoff Algorithm

<img src="MAC%20sublayer.assets/image-20201216160544405.png" alt="image-20201216160544405" style="zoom: 50%;" />

- slot长度：2τ
- 802.3的MAC具体怎么做的
- 本质上是limited contention protocol
- 1-persistent CSMA/CD
- ![image-20201216160901220](MAC%20sublayer.assets/image-20201216160901220.png)
  - 如果只有少量站发生冲突，则它们可以确保较低的延迟
  - 如果许多站发生冲突，可以保证在一个相对合理的时间间隔内解决冲突
- 性能
  - frame长，包头比例小，效率会高（可以这么理解，但本质不是这样）
  - station数目多，冲突概率多，性能下降

<img src="MAC%20sublayer.assets/image-20201216161119205.png" alt="image-20201216161119205"  />

#### Switched Ethernet

![image-20201216161721703](MAC%20sublayer.assets/image-20201216161721703.png)

- Hub是广播式的信道
- Switch允许并发传输
  - Port 1 -> Port 2; Port 3 -> Port 4: no problem
  - Port 1 -> Port 2; Port 3 -> Port 2: **needs buffering at Port2**
- Switch: The heart of this system is a switch containing a **high-speed backplane** that connects all of the ports（高速面板）
- Switch把帧输出到目标端口，其他端口不知道这个帧的存在
- **可以同时发送，不需要MAC协议，相当于点对点网络**

#### Evolution of Ethernet

- **100Mbps**：Fast Ethernet 802.3u
- 1-Gbps：Gigabit Ethernet 802.3z
- 10-Gbps

### 802.11: Wireless LANs

#### The 802.11 Architecture and Protocol Stack

- 两种Architecture

  - Infrastructure mode 有架构模式
    - 每个客户端和一个AP关联（Access Point）
  - Ad-hoc mod 自组织网络

- 协议栈

  <img src="MAC%20sublayer.assets/image-20201216164300366.png" alt="image-20201216164300366" style="zoom:67%;" />

#### The 802.11 Physical Layer

- FHSS
  - 跳频技术
- HR-DSSS
  - High Rate Direct Sequence Spread Spectrum
  - 是将一位数据编码为多位序列，称为一个“码片”
  - 合理地选择码片，有助于提高处理增益，增强信道的抗干扰能力，以便应对嘈杂的无线网络环境
- OFDM：Orthogonal Frequency Division
  - 正交频分多路复用
  - 比频分多路复用好，因为相邻频段之间不需要留有空位，可以耦合在一起

#### The 802.11 MAC Sublayer Protocol

- **CSMA/CA（avoid）**

- **无线信道做CD很难：发送的同时要侦听，无线信号衰减很快，听到的几乎都是自己发送的，别人的都听不到了，所以CD只在以太网里**

- ![image-20201216182727553](MAC%20sublayer.assets/image-20201216182727553.png)

  ![image-20201216182708627](MAC%20sublayer.assets/image-20201216182708627.png)

  - 这种操作模式为**DCF**，分布式协调功能：每个站独立行事
  - 另一种为**PCF**，点协调功能：AP控制自己覆盖范围内的一切活动（实际少用）

- 

#### The 802.11 Frame Structure



#### Services

### Data link layer switching

