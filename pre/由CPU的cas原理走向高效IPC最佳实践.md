## 由CPU的cas原理走向高效IPC最佳实践

### 背景
#### 业务开发过程中，常见的消息通讯，在体量大的情况下，会遭遇各种挑战，比如，如何保证生产的多条信息能够完全消费？如何保证线程不出现阻塞？在这方面有哪些最佳实践？涉及哪些底层知识？

### 关键字
<table>
    <th>名称</th>
    <th>所属模块</th>
    <th>解释</th>
    <tr>
        <td>cas</td>
        <td>CPU</td>
        <td>通过总线锁，将内存中某个地址锁住，不让其它CPU访问内存，用于实现CPU原子访问。</td>
    </tr>
    <tr>
        <td>总线锁</td>
        <td>CPU</td>
        <td>CPU提供了HLOCK pin引线，允许CPU在执行某一个指令时，通过拉低HLOCK pin引线的信号，直到指令执行完成才放开，从而锁住了总线，期间其它CPU无法访问内存。</td>
    </tr>
    <tr>
        <td>缓存锁</td>
        <td>CPU</td>
        <td>通过MESI协议实现</td>
    </tr>
    <tr>
        <td>MESI</td>
        <td>CPU</td>
        <td>缓存一致性协议，多核CPU处理器下，每一个核心都有自己的一级缓存，将缓存行上的数据用状态位进行表示，修改时，通过广播与监听，实现数据一致性。</td>
    </tr>
    <tr>
        <td>CMPXCHG</td>
        <td>汇编语言</td>
        <td>比较并交换，总线锁</td>
    </tr>
    <tr>
        <td>volitale</td>
        <td>Java语言</td>
        <td>内存可见性</td>
    </tr>
</table>

### 结论
#### 在单机的情况下，Distubor作为一个高性能队列，致力于解决n内存队列的延迟问题。

### 参考资料
#### <a href="https://tech.meituan.com/2016/11/18/disruptor.html">美团介绍高性能队列-Distubor</a>
#### <a href="https://ifeve.com/disruptor/">并发编程网对Disruptor的介绍-Distubor</a>
#### 现代操作系统原书的第4版的之第二章

### 写在后面
#### 本篇文章介绍了高性能队列的最佳时间-Distubor，涵盖《计算机组成原理》，《操作系统》，参考资料中，美团通过《计算机网络》，在Distubor的基础上进行扩展，从而走向分布式，难度再上一层，有空再深入研究。