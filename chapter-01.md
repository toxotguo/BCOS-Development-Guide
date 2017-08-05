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



其他异常情况：

如果在编译过程因为内存等原因导致失败，如有这类输出：

```c++: internal compiler error: Killed (program cc1plus)```

请手工进入构建目录继续编译

```
cd bcos/build
make
```



## 2. 构建BCOS节点配置包



1. 更新配置文件（默认可用，可以不用更新）

   进入初始化目录

   ```
   cd bcos/init
   ```

   ​

目录下的node0.sample、node1.sample是已经配置好的两个默认配置。也可以根据需要来改动。

下面以node0.sample为例介绍：

    {
    "networkid":"123456",        #代表区块链网络ID 同一个网络内的所有节点都必须一样
    "nodedir":"/bcos-data/node0/",#代表节点的数据路径
    "ip":"127.0.0.1",			  #代表节点的IP
    "rpcport":8545,				  #代表节点的RPC端口，如果所有节点都在同一台机器，不能出现同样的端口配置
    "p2pport":30303				  #代表节点的P2P端口，如果所有节点都在同一台机器，不能出现同样的端口配置
    }


2. 执行初始化命令

命令使用方法：```Usage: node init.js node0.sample node1.sample node2.sample... ```

做好配置文件的更新之后，执行初始化:

```node init.js node0.sample node1.sample ```

如果初始化顺利，会有如下类似输出：

```
[root@iZwz90egdlk9bxr2pkezxzZ init]# node init.js node0.sample node1.sample 
开始检查配置...
配置检查成功!!!
生成管理员账户成功!!!即将拷贝备份到每个节点目录的admin.message文件中，请注意保管!!!
开始初始化节点配置...
节点0目录生成成功!!!
节点1目录生成成功!!!
创世文件生成成功!!!
记账节点列表生成成功!!!
节点0 genesis.json生成成功!!!
节点0 config.json生成成功!!!
节点1 genesis.json生成成功!!!
节点1 config.json生成成功!!!
恭喜！已全部构建成功！
执行 /bcos-data/node0/start0.sh 即可启动节点0
执行 /bcos-data/node1/start1.sh 即可启动节点1
```



## 3. 启动BCOS网络 



执行

```
chmod +x /bcos-data/node0/start0.sh
chmod +x /bcos-data/node1/start1.sh
```

启动节点0

```
/bcos-data/node0/start0.sh
```

启动节点1

```
/bcos-data/node1/start1.sh
```

查看节点日志：

```
tail -f /bcos-data/node0/log/* | grep Report
```

可以看到日志中blk在增长。



至此，两个节点组成的BCOS网络已经开始运行了！



## 4 总结

总的来说，搭建BCOS环境主要的步骤总共只有3步。

第一步，执行build脚本，构建bcoseth。

第二步，执行init.js脚本，初始化节点配置。

第三步，启动节点。网络开始运转。