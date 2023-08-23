# 集群操作手册 For YHC V1.0

*created by Han*

*Last Updated:2023/08/23*

## 基础教学



### 首先确定自己所处的网路环境  

集群是受特殊保护的计算资源，是属于学校的内网资源，严格上的来说只允许校内教育网络访问。我们在没有校内网络环境的时候可以通过VPN访问。登陆地址：10.130.2.222  端口：22

**综上所述：**

- 如果在校内需要登陆，需要登陆‘OUC-AUTO’或‘OUC-WIFI’，或者使用校园有线网络。

- 如果在校外（或无法登录校园网）登录，需要在信息门户申请集群的IP地址，再登陆atrust来连接集群。

  

### 登录集群（SSH登录）

| SSH信息        | 值                                                   |
| :------------- | :--------------------------------------------------- |
| 集群登陆节点IP | 10.130.2.222                                         |
| SSH 登录端口   | 22                                                   |
| 登陆软件       | Termianl，Putty，MobaXterm等等终端（终端模拟器）均可 |

- 一般情况下需要输入密码，如果本机配置了免密凭证可以免密登录。

- 密码输入时会不可见，但是已经输入上了。

  

### 操作

常规Linux操作，需要记得以下几点

- 我们登陆的是集群的登录节点，**不可以在你登陆的地方直接进行计算**。如需计算请参考‘集群计算’一章。
- 管理员说不允许在登陆节点上运行Python（悲
- 可以使用scp命令（[scp命令学习 ](https://www.runoob.com/linux/linux-comm-scp.html)）或其余命令进行文件传输。
- 传输多数量文件时可以使用xftp。
- 保证账户安全，减少信息泄露。
- 登陆节点有时候能联网，有时候不行（迷



## 集群计算

本章以Python计算为基础，在开始计算前一定要明白我们在集群上的计算都是

### 常规计算

#### mpirun(🍊多半看不懂，可以跳过的)

在程序中，不同的进程需要相互的数据交换，特别是在科学计算中，需要大规模的计算与数据交换，集群可以很好解决单节点计算力不足的问题，但在集群中大规模的数据交换是很耗费时间的，因此需要一种在多节点的情况下能快速进行数据交流的标准，这就是MPI。

1. intel mpi （Fortran&C）

   ```shell
   mpirun -genv I_MPI_FABRICS shm:tmi -machinefile /tmp/nodefile.$$ -n $NP $EXEC
   ```

   | 参数                           | 含义                                            |
   | :----------------------------- | :---------------------------------------------- |
   | -genv I_MPI_DEVICE  rdma       | 指定跑 ib 网络，rdma 可换成 rdssm               |
   | -machinefile  /tmp/nodefile.$$ | 指定跑哪几个节点，节点由调度器分配              |
   | -n $NP                         | 指定总共跑几个核，也是由调度器分配，默 认是均分 |
   | $EXEC                          | 用户的应用可执行文件，后面跟输入文件等          |

   Intel MPI编译命令及其对应关系

   | 编译命令 | 调用的默认编译器命令 | 支持的语言              |
   | :------- | :------------------- | :---------------------- |
   | mpicc    | gcc, cc              | C                       |
   | mpicxx   | g++                  | C/C++                   |
   | mpifc    | gfortran             | Fortran77* /Fortran 95* |
   | mpigcc   | gcc                  | C                       |
   | mpigxx   | g++                  | C/C++                   |
   | mpif77   | g77                  | Fortran 77              |
   | mpif90   | gfortran             | Fortran 95              |
   | mpiicc   | icc                  | C                       |
   | mpiicpc  | icpc                 | C++                     |
   | mpiifort | ifort                | Fortran77/Fortran 95    |

   更多Intel MPI请参考[开始使用适用于 Linux* 操作系统上的英特尔 oneAPI 的英特尔®® MPI 库 (intel.com)](https://www.intel.com/content/www/us/en/docs/mpi-library/get-started-guide-linux/2021-10/overview.html)

2. open mpi

   ```shell
   mpirun --mca btl openib,self -np $nprocs -hostfile $PBS_NODEFILE $EXEC <in.shock
   ```

   | 参数                     | 含义                                            |
   | ------------------------ | ----------------------------------------------- |
   | --mca btl openib,self    | 指定跑 ib 网络                                  |
   | -hostfile  $PBS_NODEFILE | 指定跑哪几个节点，节点由调度器分配              |
   | -np $nprocs              | 指定总共跑几个核，也是由调度器分配，默 认是均分 |
   | $EXEC                    | 你的应用可执行文件，后面跟输入文件等            |

   

### Han常用的Jupyter 云计算

