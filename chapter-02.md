[TOC]



# 第二章 节点动态入网、退网

*注意：本指南所涉及脚本命令均是以在Centos7.2操作系统上为示例，其他操作系统不保证完全可用*

在第一章中，我们快速构建了BCOS区块链环境，并且启动了只有一个创世节点的区块链网络。

接下来，本章即将介绍，如何在把新节点加入到第一章所创建的区块链网络中，及如何将节点从区块链网络中退出。

## 1. 新节点加入

新节点加入的命令是

```node newnode.js 创世节点的初始化文件路径  新节点的初始化文件路径 
node newnode.js 创世节点的初始化文件路径  新节点的初始化文件路径 
```

其中，创世节点的初始化文件和新节点的初始化文件格式完全一样。

其中初始化项的含义请参看[第一章](https://github.com/toxotguo/BCOS-Development-Guide/blob/master/chapter-01.md) 中的说明。

在quickstart目录下已经默认有node1.sample初始化文件，可以根据需要来变更。

```
示例node1.sample
{
    "networkid":"123456",
    "nodedir":"/bcos-data/node1/",
    "ip":"127.0.0.1",
    "rpcport":8546,
    "p2pport":30304
}
```



在第一章中创世节点的初始化文件用的是node0.sample，所以可以把node1.sample当做新节点的初始化文件

输入如下命令

```
node newnode.js node0.sample node1.sample
```

正常情况下，会有类似以下输出：

```
始初始化节点配置...
新节点目录生成成功!!!
节点 config.json生成成功!!!
系统合约地址:0x4c0c6baf89cbc9e2b863b2d0474eb6283a47d2c9
新节点注册成功！！！
新节点启动成功！！
```



## 2. 查看节点互联情况

通过查看节点的连接情况，可以判断区块链网络是否在正常运行。

可以利用工具目录tool下的monitor脚本来查看:

```
cd ../tool
babel-node monitor.js
```

正常情况下，会有类似以下输出，输出可以看到这个节点已经连接上另外一个节点，且网络的块高在不断增长，既是说明区块链在正常运行中。

```
已连接节点数：1
...........Node 0.........
NodeId:c840d56a5cc2a7e4af2cf8f9b3b60aabbf98b3977df25faba679780f100fcacf934adff2c882d98da12c539310c3b81c597b6e88564f26867ac61942f01948ba
Host:127.0.0.1:30304
当前块高115
--------------------------------------------------------------
已连接节点数：1
...........Node 0.........
NodeId:c840d56a5cc2a7e4af2cf8f9b3b60aabbf98b3977df25faba679780f100fcacf934adff2c882d98da12c539310c3b81c597b6e88564f26867ac61942f01948ba
Host:127.0.0.1:30304
当前块高116
```



## 3. 节点退出

当需要将某个节点退出区块链网络时，可以利用系统合约目录systemcontractv2下面的tool工具。命令格式是：

```
babel-node tool.js NodeAction cancel 节点的信息文件
```

节点的信息文件既是每个节点目录下的node.json文件。以上面为例，路径是 /bcos-data/node1/node.json 

则执行以下命令可以将该节点退出网络：

```
cd ../systemcontractv2
babel-node tool.js NodeAction cancelNode /bcos-data/node1/node.json 
```

执行完命令之后，可以通过上面的[第二小节](##2. 查看节点互联情况) 来查看网络情况。



*第三章将介绍BCOS网络如何部署智能合约*

