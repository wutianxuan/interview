# TCP

## TCP 三次握手

第一次握手：起初两端都处于CLOSED关闭状态，Client将标志位SYN置为1，随机产生一个值seq=x，并将该数据包发送给Server，Client进入SYN-SENT状态，等待Server确认；

第二次握手：Server收到数据包后由标志位SYN=1得知Client请求建立连接，Server将标志位SYN和ACK都置为1，ack=x+1，随机产生一个值seq=y，并将该数据包发送给Client以确认连接请求，Server进入SYN-RCVD状态，此时操作系统为该TCP连接分配TCP缓存和变量；

第三次握手：Client收到确认后，检查ack是否为x+1，ACK是否为1，如果正确则将标志位ACK置为1，ack=y+1，并且此时操作系统为该TCP连接分配TCP缓存和变量，并将该数据包发送给Server，Server检查ack是否为y+1，ACK是否为1，如果正确则连接建立成功，Client和Server进入ESTABLISHED状态，完成三次握手，随后Client和Server就可以开始传输数据。



## TCP和UDP的区别

### UDP

1. 无连接
2. 面向报文，只是报文的搬运工
3. 不可靠，没有拥塞控制
4. 高效，头部开销只有8字节
5. 支持一对一、一对多、多对多、多对一
6. 适合直播、视频、语音、会议等实时性要求高的

### TCP

1. 面向连接：传输前需要先连接
2. 可靠的传输
3. 流量控制：发送方不会发送速度过快，超过接收方的处理能力
4. 拥塞控制：当网络负载过多时能限制发送方的发送速率
5. 不提供时延保障
6. 不提供最小带宽保障



## 为什么三次握手四次挥手

- 四次挥手
  - 因为是双方彼此都建立了连接，因此双方都要释放自己的连接，A向B发出一个释放连接请求，他要释放链接表明不再向B发送数据了，此时B收到了A发送的释放链接请求之后，给A发送一个确认，A不能再向B发送数据了，它处于FIN-WAIT-2的状态，但是此时B还可以向A进行数据的传送。此时B向A 发送一个断开连接的请求，A收到之后给B发送一个确认。此时B关闭连接。A也关闭连接。
  - 为什么要有TIME-WAIT这个状态呢，这是因为有可能最后一次确认丢失，如果B此时继续向A发送一个我要断开连接的请求等待A发送确认，但此时A已经关闭连接了，那么B永远也关不掉了，所以我们要有TIME-WAIT这个状态。
  - 当然TCP也并不是100%可靠的。
- 三次握手
  - **为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误**