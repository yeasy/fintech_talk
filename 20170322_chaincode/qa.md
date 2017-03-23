## 答疑解惑

**问：Fabric 1.0中的系统 chaincode 可否简单介绍下？**

答：1.0中有五个系统chaincode，分别是lccc/cscc/escc/vscc/qscc，它们在peer启动或创建channel的时候就会部署，并且与peer运行在同一进程中，而不是Docker container中。
lccc是生命周期系统chaincode，用于管理用户chaincode的install、Instantiate等；cscc是配置系统chaincode，与系统配置有关，比如join channel的时候，就是通过cscc来进行的；escc和vscc分别是endorsement系统chaincode和verification系统chaincode，主要是endorser用于对用户chaincode进行相关验证和背书，它们可以在Instantiate chaincode的时候指定，也就是说用户可以根据自己的情况给某个endorser结点设置定制化的escc和vscc；qscc，不好意思我还不太了解，后面研究清楚之后再跟大家分享。


**问：Fabric 1.0 调用其他 CHAINCODE 现在支持了吗？**

答：1.0 里面是计划支持跨 chaincode 的读操作的。

**问：chaincode处理的数据来源可以自己获取吗，比如时间，或者其他服务器的一些数据？**

答：理论上 chaincode 就是一个独立运行的可执行程序，它会与远端的endorser 进行通信，所以 chaincode 程序是可以访问外部数据源的。

**问：fabric 中每个 block 都有 world state 的 hash，但是这个历史的 world state 存放在什么地方？如何读取指定 block height 的world state？**

答：fabric 1.0 中每个 peer 结点会维护四个 db，分别是 id store，存储 chainID；stateDB，存储 world state；versioned DB，存储 key 的版本变化；还有 blockdb，存储 block。

**问：请问在chaincode运行原理这块，CLI或App是把请求直接发给endoser节点还是发给install了这个chaincode的peer节点，再由这个peer去与endoser交互呢？**

答：实际上是发给了 endorser 结点，这个是在你的调用请求里指定的。

**问：chaincode 的 world state 何时被写入？？**

答：ordering之后，channel 上的所有 peer 都会执行 commiting 操作,即写入stateDB操作。

