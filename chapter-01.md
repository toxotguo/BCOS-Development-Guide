[TOC]



# 第一章 快速搭建BCOS区块链

*注意：本指南所涉及脚本命令均是以在Centos7.2操作系统上为示例，其他操作系统不保证完全可用*

## 1. 环境初始化与编译

进入期望安装的目录下，执行

```curl -fsSL "https://raw.githubusercontent.com/toxotguo/bcos/master/build.sh" | /bin/sh```

由于构建过程需要安装依赖包、环境初始化和bcoseth的编译。因此执行时间视网络状况和机器配置而定。一般情况在10分钟左右。请耐心等待。

直到出现以下输出：

```bcoseth构建成功!!!```



且执行bcoseth 命令，如果有正常输出，既是已经构建成功！

目录下会看到有bcos目录。



*其他异常情况：*

- *如果在编译过程因为内存等原因导致失败，如有这类输出：*

```c++: internal compiler error: Killed (program cc1plus)```

*请手工进入构建目录继续编译*

```
cd bcos/build
make
```

- *如果由于网络原因导致依赖库无法下载成功，可以从其他渠道下载依赖库包或者直接拷贝到编译目标路径下*

  ```bcos/deps/src```

  ​

## 2. 启动区块链网络



1. 更新创世节点初始化文件（**默认可用，可以不用更新**）

   进入快速启动目录

   ```
   cd bcos/quickstart
   ```

   ​

目录下的node0.sample是已经设置好的初始化文件。也可以根据需要来改动。

下面以node0.sample为例介绍初始化项含义：

    {
    "networkid":"123456",        #代表区块链网络ID 同一个网络内的所有节点都必须一样
    "nodedir":"/bcos-data/node0/",#代表节点的数据路径
    "ip":"127.0.0.1",			  #代表节点的IP
    "rpcport":8545,				  #代表节点的RPC端口，如果所有节点都在同一台机器，不能出现同样的端口配置
    "p2pport":30303				  #代表节点的P2P端口，如果所有节点都在同一台机器，不能出现同样的端口配置
    }


2. 启动创世节点命令

做好创世节点初始化文件的更新之后，执行初始化:

```sh quickstart.sh node0.sample ```



如果初始化顺利，会有如下类似输出：

```
[root@iZwz90egdlk9bxr2pkezxzZ init]# sh quickstart.sh node0.sample
...
系统合约部署成功!!!合约地址:0x6d394b2deec03451759df6e5d624069f96c76161
开始更新节点配置...
更新节点配置成功!!!
节点0注册成功！！！
节点0重启成功！！！
网络启用系统合约成功！！！
BCOS区块链网络成功启动！！！
```

至此，一个节点组成的BCOS网络已经开始运行了！



## 3. 查看节点日志 



查看节点日志：

```
tail -f /bcos-data/node0/log/* | grep Report
```

可以看到日志中blk在增长。

```
INFO|2017-08-05 22:39:00|^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Report: blk=40,hash=5229a852…,idx=0, Next: blk=41
INFO|2017-08-05 22:39:00|^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Report: blk=40,hash=5229a852…,idx=0, Next: blk=41
INFO|2017-08-05 22:39:01|^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Report: blk=41,hash=33e369b4…,idx=1, Next: blk=42
INFO|2017-08-05 22:39:01|^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Report: blk=41,hash=33e369b4…,idx=1, Next: blk=42
```



## 4 总结

总的来说，搭建BCOS环境主要的步骤总共只有2步。

第一步，执行build脚本，构建bcoseth。

第二步，执行quickstart脚本，快速启动bcos区块链网络。



*[第二章](https://github.com/toxotguo/BCOS-Development-Guide/blob/master/chapter-02.md) 将介绍新节点动态加入区块链网络*