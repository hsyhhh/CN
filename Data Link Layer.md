## The Data Link Layer

[TOC]

### 0. 王道考研

#### 封装成帧和透明传输

<img src="Data%20Link%20Layer.assets/image-20201227111243233.png" alt="image-20201227111243233" style="zoom: 67%;" />

- 透明传输：不管所传数据是什么比特组合，都能够在链路上传送

- 字符计数法

  <img src="Data%20Link%20Layer.assets/image-20201227111531452.png" alt="image-20201227111531452" style="zoom:50%;" />

  - 帧首部用**第一个字节**（8位）标明帧内字符数（包括第一个字节）
  - 第一个字节出错，就会连续错

- 字节填充法

  <img src="Data%20Link%20Layer.assets/image-20201227111811911.png" alt="image-20201227111811911" style="zoom: 50%;" />

  <img src="Data%20Link%20Layer.assets/image-20201227111923672.png" alt="image-20201227111923672" style="zoom: 50%;" />

- 零比特填充法

  <img src="Data%20Link%20Layer.assets/image-20201227112038620.png" alt="image-20201227112038620" style="zoom:67%;" />

  - 首部和尾部的标识符都是01111110
  - 连续5个1，就立即填入1个0

- 违规编码法

  - 曼彻斯特编码只有高-低，低-高两种
  - 可以用高-高，低-低来定界帧的起始和终止
  - 局域网IEEE802标准采用

#### 检错编码

- 奇偶校验码：在最前面加

  - 奇校验：奇数个1
  - 偶校验：偶数个1
  - 奇数个错能够检查出来，偶数个错不能检查出来

- 循环冗余码CRC

  - 注意看可能出错的概率

  ![image-20201227115016648](Data%20Link%20Layer.assets/image-20201227115016648.png)

  ![image-20201227115155343](Data%20Link%20Layer.assets/image-20201227115155343.png)

#### 纠错编码

- 海明码

- **发现双比特错，纠正单比特错**

- <img src="Data%20Link%20Layer.assets/image-20201227125742707.png" alt="image-20201227125742707" style="zoom:50%;" />

- 确定校验码位数r

  <img src="Data%20Link%20Layer.assets/image-20201227125955394.png" alt="image-20201227125955394" style="zoom:67%;" />

- 确定校验码和数据的位置

  <img src="Data%20Link%20Layer.assets/image-20201227130122414.png" alt="image-20201227130122414" style="zoom: 67%;" />

- 求校验码的值

  - P1

#### 流量控制

- 发送速度和接收能力不匹配

- 在传输层也有

  - 链路层：点对点，相邻节点之间
  - 传输层：端对端

- 控制手段有区别

  - 链路层：接收方收不下就**不回复确认**
  - 传输层：接收端给发送端一个窗口公告

- 流量控制的方法

  - 停止-等待
    - 每发送完一个帧就停止发送，等待对方的确认，收到确认后再发下一个帧
    - 发送窗口 = 接收窗口 = 1
  - 滑动窗口
    - Go Back N
      - 发送窗口 > 1，接收窗口 = 1
    - SR 选择重传
      - 发送窗口 > 1，接收窗口 > 1
  - 发送窗口：发送方维护的一段允许发送的帧序号
  - 接收窗口

- 可靠传输：发送端发啥，接收端收啥

- 滑动窗口也可以解决可靠传输：发送方自动重传

- 信道利用率：发送方在一个发送周期内，有效的发送数据所需要的时间占整个发送周期的比例

- 信道吞吐率：信道利用率 * 发送方的发送速率

- Go back N

  - 接收窗口大小为1
  - **累积确认**：表明接收方已经收到了n号帧和它之前的全部帧
  - 如果超时，发送方重传所有已发送但未被确认的帧
  - 接收方非常专一，如果收到的不是期待的那个，**全部丢掉**，而后重新发送最近按序接收的帧的ACK，**无需缓存任何失序帧**
  - 窗口大小限制：有上限
  - 重传时必须把原来已经正确传输的帧重传

- SR 选择重传

  - **设置单个确认**，同时加大接收窗口，**设置接收缓存**，缓存乱序到达的帧

  - 超时后只重传一个帧

  - 接收方：窗口内的帧都接收

    <img src="Data%20Link%20Layer.assets/image-20210117171116113.png" alt="image-20210117171116113" style="zoom: 67%;" />

  - 窗口长度

    - 无法区分

    <img src="Data%20Link%20Layer.assets/image-20210117171511406.png" alt="image-20210117171511406" style="zoom:50%;" />

  - 发送窗口最好等于接收窗口

  - ![image-20210117171612340](Data%20Link%20Layer.assets/image-20210117171612340.png)

#### 链路层设备

- 网桥：根据MAC帧的目的地址对帧进行转发和过滤

  - 隔断冲突域
  - 可以互联不同物理层、不同MAC子层和不同速率的以太网

- 冲突域

  - 同一个冲突域内每一个节点都能收到所有被发送的帧

- 广播域

  - 网络中能接收任一设备发出的广播帧的所有设备的集合

- 网桥、交换机：每个端口就是一个冲突域

  <img src="Data%20Link%20Layer.assets/image-20210123213205734.png" alt="image-20210123213205734" style="zoom:50%;" />

#### PPP协议

- 通常用于广域网
- 链路层协议
- **全双工链路**
- 不需要可靠的传输：无需纠错，无需符合，无需流量控制
- 只需要帧定界符、差错检测
- 串行链路
- 同步传输：比特流，异步传输：字符或字节流

#### HDLC协议

- **ISO制定的协议，面向比特**
- **全双工**



![image-20210117203241802](Data%20Link%20Layer.assets/image-20210117203241802.png)

![image-20210117203247181](Data%20Link%20Layer.assets/image-20210117203247181.png)

![image-20210117204136796](Data%20Link%20Layer.assets/image-20210117204136796.png)

![image-20210117204126358](Data%20Link%20Layer.assets/image-20210117204126358.png)

![image-20210117211127825](Data%20Link%20Layer.assets/image-20210117211127825.png)

![image-20210117211139853](Data%20Link%20Layer.assets/image-20210117211139853.png)

![image-20210117211155107](Data%20Link%20Layer.assets/image-20210117211155107.png)

![image-20210117215109795](Data%20Link%20Layer.assets/image-20210117215109795.png)

![image-20210117215053702](Data%20Link%20Layer.assets/image-20210117215053702.png)

### 1. Issues

功能
- 向网络层提供一个well-defined service interface
- 处理transmission errors
- 调节data flow，确保慢速的接收方不会被快速的发送方淹没(swamped)

- 帧：帧头、有效载荷、帧尾
- Framing
- Error control
- Flow control

#### 提供给网络层的服务

- 数据链路层的3种服务（源机器 -> 目标机器）
  - 无确认的无连接服务 Unacknowledged connectionless
    - 发送独立的帧，但目标机器不检查
    - 实例：以太网（有线）
    - 事先不需要建立逻辑连接，事后也不用释放逻辑连接
    - 不会检查帧丢失，更不会试图恢复
    - 适用场合：
      - 错误率很低，差错恢复可以留给上层
      - 实时通信（比如语音传输）
  - 有确认的无连接服务 Acknowledged connectionless service
    - 没有逻辑连接，但是每一帧都需要单独确认
    - 发送方可以知道帧是否已经正确到达
    - 一个帧在指定时间内没到达，则发送方会再次发送
    - 适用场合：
      - 不可靠的信道，比如无线系统
      - 实例：802.11（WiFi）
  - 有确认的面向连接的服务 Acknowledged connection-oriented service
    - 发送数据之前要建立连接
    - 发送的每个frame都被numbered且被确认
    - 每个frame都只被接收一次，所有frame按正确顺序被接收
  

#### Framing

- 错误检测和错误恢复
  
  - 成帧，对每个帧计算checksum
- 四种成帧方法
  - 字节计数 Byte count
    - header里面说明这个帧里的字符数（包括byte count字段）
    - 传输出错，会导致后面也错误
    
    <img src="Data%20Link%20Layer.assets/image-20201103190413133.png" alt="image-20201103190413133" style="zoom: 33%;" />
    
  - 字节填充的标志字节法 Flag bytes with byte stuffing
    - A frame is **delimited（界定）** by **flag bytes**，用flag区分每个frame
    - 用转义字符ESC来表示Payload field里出现的与flag相同的字段
    
    <img src="Data%20Link%20Layer.assets/image-20201103190512031.png" alt="image-20201103190512031" style="zoom:50%;" />
  - 比特填充的标志比特法 Flag bits with bit stuffing
    - 比如flag为 01111110
    - 如果payload里有flag
      - sender：每看到5个连续的1就在后面加个0
      - receiver：根据01111110分出一个个帧，在帧内连续5个1后的0要删掉
    
    <img src="Data%20Link%20Layer.assets/image-20201103190546125.png" alt="image-20201103190546125" style="zoom:50%;" />
    
  - 物理层编码违禁法 Physical layer encoding violation
    - 比如以太网中，用曼彻斯特编码
      - High to Low代表1，Low to High代表0（这种方法方便定位比特的边界）
      - 实际上有两个signal没有用：h to h，l to l，可以用这两个signal区分两个帧
- 通常用byte count和其他方法中的一个
  - count field用来定位帧的结束
  - 如果在结束处有正确的delimiter，且checksum正确，则接收
  - Ex: preamble + byte count in Ethernet and 802.11
    - **preamble**：特殊的字符序列，作为delimiter（绪论，导言）

#### Error control

- 离不开三个机制
  - 给sender提供反馈
    - ACK: Positive acknowledgement
    - NAK: Negative acknowledgement 没收到才发，收到了不发
  - To provide timeout timers
    - 过了一定时间，sender没收到反馈，可能需要重传
  - To number frames
    - 为了在重传的时候分辨

#### Flow control

- 两种方式
  - Feedback-based flow control
    - receiver提供反馈，告知是否能传输更多的数据
  - Rate-based flow control
    - the protocol has a built-in mechanism that limits the rate at which senders may transmit data, **without using feedback**



### 2. Error detection and correction

- 无线传输，百分之十丢包率非常常见
- 错误种类：isolated errors, burst errors

#### Hamming distance

- m message bits => n-bit codeword (n = m + r) 码字

- **Hamming distance of 2 codewords**

  - 两个码字不一样的位的个数

- **Hamming distance of complete code (all valid codewords)**

  - **最小的**两个有效码字的hamming distance
  - 并不是每个码字都是valid的：$2^m -> 2^n$

- Example

  - 奇偶校验位：总共偶数个1
  - 对于parity bit，Hamming distance为1

  <img src="Data%20Link%20Layer.assets/image-20201103193026835.png" alt="image-20201103193026835" style="zoom: 67%;" />

- erro correction：find cloest valid codeword and accept

- 检测d位错误

  - 需要 d+1 hamming distance code
  - 因为d位错误不可能把一个有效码字转变成另一个有效码字

- 改正d位错误

  - 需要 2d+1 hamming distance code
  - 两个有效码字之间至少相距2d+1，即使出错，还是恢复成原来的，因为更近

- 

  <img src="Data%20Link%20Layer.assets/image-20201103194728087.png" alt="image-20201103194728087" style="zoom:67%;" />

  ![image-20201103194937040](Data%20Link%20Layer.assets/image-20201103194937040.png)

- 四种纠错码

  - Hamming Codes
  - Binary Convolutional Codes
  - Reed-Solomon Codes
  - Low-Density Parity Check Codes (LDPC)

#### Error correction

- Hamming code是重点

##### Hamming Codes

- 

- ![image-20201103200737658](Data%20Link%20Layer.assets/image-20201103200737658.png)

##### Convolutional Codes

- 有一个encoder；输出依赖于当前的输入比特和先前的输入比特；需要寄存器记录
- 先前比特的个数称为 constraint length
- <img src="Data%20Link%20Layer.assets/image-20201103201456102.png" alt="image-20201103201456102" style="zoom: 33%;" />

##### Reed Solomon Codes

- 也称RS codes

- 针对bursty error：出现一连串的error也有效，可以**纠正回来**

##### LDPC

- 由矩阵代表输出，这个矩阵的密度低（1的个数少），因此是low-density
- 纠错能力很不错，非常复杂

#### Error detection

- **CRC是重点**

##### Parity

- To detect single error by using parity bit

- To detect **a single burst of length k** by using parity bit

  - interleaving：不按照比特流传输的顺序计算parity bits
  - Interleaving of parity bits to detect a burst error

  <img src="Data%20Link%20Layer.assets/image-20201103203140252.png" alt="image-20201103203140252" style="zoom: 80%;" />

##### Checksum

- 把数据分成N-bit的一个一个word，然后把这些word加起来成为N check bits

  - Checksum treats data as N-bit words and adds N check bits that are the modulo $2^N$ sum of the words

- 例子：**Internet ip协议中 16-bit 1 complement checksum**

- error detection的能力有所提升；但对某些系统性错误检测不出

  - **增加了很多的0，checksum计算出来一样**

- 进位要加回去；计算出来后要取反

  <img src="Data%20Link%20Layer.assets/image-20201103203900597.png" alt="image-20201103203900597" style="zoom:67%;" />

##### CRC

- Polynomial code or Cyclic(循环的) Redundancy Chech
- 基本概念
  - 比特流表示系数为0或1的多项式
  - k-bit 帧认为是$x^{k-1}$至$x^0$
  - 110001 -> $x^5+x^4+x^0$
  - 没有加法的carry和减法的borrow，加减都是XOR，需要modulo 2



### 3. Protocols

- void wait_for_event(event_type *event)
  - 阻塞式函数，如果没有接收到事件就一直阻塞
  - 在event中返回事件类型

#### Protocol 1: A Utopian Simplex Protocol

- 单向；无损；传输和接收的网络层总是ready
- processing time可忽略；无限的buffer容量
- Simplex; Error-free channel; No flow control
  - damage vs. loses
- Simplex Communication 单工

#### Protocol 2: simplex, stop-and-wait

- 发送后等待收到确认帧后再发送
- 还是单工，还是error-free；但是有flow control：stop-and-wait
- 每时刻只有一个帧在外面
- <img src="Data%20Link%20Layer.assets/image-20201104100427177.png" alt="image-20201104100427177" style="zoom:50%;" />

#### Protocol 3: simplex, noisy channel, stop-and-wait

- 单工，有错，接收者不总是ready

- Protocol 2 + timer + to number the frame

  - stop-and-wait
  - timer: receiver没有回复ack，过一段时间需要重传
  - number the frame: sender没有接收到ack，但receiver其实接收到了，需要区分帧

- PAR or **ARQ** 

  - (Positive Acknowledgement Retransmission) or (Automatic Repeat reQuest)
  - 采用ack机制
  - 只有当前一个帧得到确认，即收到ack以后才发送下一个帧

- ARQ添加了error control

  - receiver正确收到帧后需要发送ack
  - sender没有在规定时间收到ack，重传
  - 帧和ack都需要编号，以分辨是否是重传
  - 对于stop-and-wait, 1位编号就足够（区分旧的还是新的）

- 数据丢失

  <img src="Data%20Link%20Layer.assets/image-20201104100503468.png" alt="image-20201104100503468" style="zoom:50%;" />

- ack丢失

  <img src="Data%20Link%20Layer.assets/image-20201104100529399.png" alt="image-20201104100529399" style="zoom:50%;" />

- 数据错误

  <img src="Data%20Link%20Layer.assets/image-20201104100604436.png" alt="image-20201104100604436" style="zoom:50%;" />

#### Protocol 4: duplex, stop-and-wait

- 双工；stop-and-wait
- 如何做到双工：
  - approach 1：在sender这开两个线程（sender和receiver），在receiver那也开两个线程
  - approach 2：希望用一个连接来实现两个方向
- Piggybacking
  - The technique of temporarily delaying outgoing acknowledgements so that they can **be hooked onto** the next outgoing data frame.

#### Utlization

<img src="Data%20Link%20Layer.assets/image-20201104195750343.png" alt="image-20201104195750343" style="zoom:50%;" />

- RTT：Round Trip Time
- Utilization也等于

<img src="Data%20Link%20Layer.assets/image-20201104195916604.png" alt="image-20201104195916604" style="zoom:50%;" />

- example:

  - 400/410
  - 400/420

  <img src="Data%20Link%20Layer.assets/image-20201104171227205.png" alt="image-20201104171227205" style="zoom:50%;" />

#### Sliding Window Protocol

- 在发送一个数据帧后，在得到这个确认帧的确认以前，**有一个window的帧都可以发送**

  - 假设能够发送N个数据帧

  - 得到Utilization (<= 1)

    <img src="Data%20Link%20Layer.assets/image-20201104172020761.png" alt="image-20201104172020761" style="zoom: 67%;" />

  <img src="Data%20Link%20Layer.assets/image-20201104172104218.png" alt="image-20201104172104218" style="zoom:50%;" />

- $BD$ ~ $T_{prop}/T_{frame} = (D / (Size/B))$
  - bandwidth * delay 与其成正比
  - 链路的利用率可以用 bandwidth delay product 也是可以计算的
- **Outstanding frames**
  - 发送了但还没acked的帧（在外面的帧）
  - 对stop-and-wait，值为1
  - 对滑动窗口，一般来说与BD成正比
- Window = Set of sequence numbers permitted to send/receive
- sender window / receiver window
  
  - 允许发送/接收的sequence number的集合
- **Unprocessed frames**
  - 接收了但是还没有发送ack的帧
  - frames that are received but not deliver to the network layer and their acks are not sent yet.
- Sender window
  
  - 当**outstanding pkts增加**时，sender window减小
- Receiver window
  
  - 当**unprocessed pkts增加**时，receiver window减小
- **cumulative acknowledgement**
  - 当ack为n，代表n以及n-1, n-2等等都被收到了
  - 另外一种形式：ack为n，说明
    - **期望下一个接收的是n**
    - n-1, n-2等等已经收到了
- **重点掌握滑动窗口的移动**
- Sliding Window Example
  - F0, F1, F2
    - 对A来说，outstanding pkts增加，window减小
    - 对B来说，0,1,2没有处理，window减小
  - ACK3：cumulative，期望接收3
    - 对B，可以接收更多未处理的帧
    - 对A，outstanding pkts减少
  - 接收F3并发送ACK4，使得B窗口大小还是7

<img src="Data%20Link%20Layer.assets/image-20201104182335050.png" alt="image-20201104182335050" style="zoom: 67%;" />

#### Protocol 5: go back N

- sender窗口大小为N，receiver只有一个buffer**（窗口大小为1）**
  - 2号包出错了，sender没有意识到，可以紧接着继续发送
  - receiver只希望收2号帧，后面的都discard掉
  - 直到sender这里发生timeout，重新传2号帧

<img src="Data%20Link%20Layer.assets/image-20201104175055306.png" alt="image-20201104175055306" style="zoom:50%;" />

- **Send Window Size <= MAX_SEQ (0...N)**
- send window size的最大值？
  - cumulative ack：ack5表示0到5已经被正确接收了
  - sender接收ack5后，就要发送新的数据包，seq_num为0，左图没有问题
  - 右图ack5丢失，sender以为receiver没有接收到，所以需要重传，seq_num为0
    - 在receiver这边语义分不清：递交给网络层还是discard？
      - 无法区分这个0号数据包是新的包还是重传的包
  - **如果发送窗口大小等于5：**只能发送0到4数据包
    - 新的数据包将会是5，而重传的是0，轻易地可以区分

<img src="Data%20Link%20Layer.assets/image-20201104180843521.png" alt="image-20201104180843521" style="zoom:50%;" />

#### Protocol 6: selective repeat

- 改进go back N：receive window大小不是1，而可以接收一系列的包
  - 发送Nak，不必等待timeout
  - 对带宽的利用更加高效

<img src="Data%20Link%20Layer.assets/image-20201104183616568.png" alt="image-20201104183616568" style="zoom:50%;" />

- 最大发送窗口大小
  - 假设sender窗口大小为7，接收方与发送方窗口大小相等
  - 发送7个数据包，接收方接收后移动窗口，回了ack
  - ack丢失
  - 发送方timeout，假如发送方发送的是012，接收方无法区分是新的还是老的012
  - 如果考虑了这个限制，sender窗口大小为4，就可以区分

<img src="Data%20Link%20Layer.assets/image-20201104184520134.png" alt="image-20201104184520134" style="zoom: 50%;" />



#### A go back N exercise

- send window有限制，max seq_num为7，因此最大的发送窗口大小为7
- t1时刻有两个帧在外面，因此只能再发5个帧（7-2）
- <img src="Data%20Link%20Layer.assets/image-20201104202304318.png" alt="image-20201104202304318" style="zoom: 33%;" />
- <img src="Data%20Link%20Layer.assets/image-20201104202425487.png" alt="image-20201104202425487" style="zoom: 33%;" />

### 4. Example Data Link Protocols

#### PPP

- Point-to-Point Protocol
- used to send packets over the links
  - including the SONET fiber optic links and ADSL links
- 

#### Packet over SONET (Synchronous Optical NETwork)



#### ADSL (Asymmmetric Digital Subscriber Loop)

- 非对称数字用户线路