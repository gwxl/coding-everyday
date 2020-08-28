- [1. 进程管理之进程实体](#1-进程管理之进程实体)
  - [1.1. 进程](#11-进程)
  - [1.2. 进程的实体](#12-进程的实体)
  - [1.3. 进程 Process 与线程 Thread](#13-进程-process-与线程-thread)
  - [1.4. 进程和线程的对比](#14-进程和线程的对比)
- [2. 并行和并发的区别](#2-并行和并发的区别)
- [3. 什么是线程安全？](#3-什么是线程安全)
- [4. Python哪些操作是线程安全的？](#4-python哪些操作是线程安全的)
- [5. 进程管理之五状态模型](#5-进程管理之五状态模型)
  - [5.1. 就绪状态](#51-就绪状态)
  - [5.2. 执行状态](#52-执行状态)
  - [5.3. 阻塞状态](#53-阻塞状态)
  - [5.4. 创建状态](#54-创建状态)
  - [5.5. 终止状态](#55-终止状态)
- [6. 进程管理之进程同步](#6-进程管理之进程同步)
  - [6.1. 临界资源](#61-临界资源)
  - [6.2. 进程间同步的原则](#62-进程间同步的原则)
  - [6.3. 进程间同步的方法](#63-进程间同步的方法)
  - [Python中如何使用多进程？](#python中如何使用多进程)
    - [6.3.1. 进程同步之共享内存](#631-进程同步之共享内存)
    - [6.3.2. 进程同步之Unix域套接字](#632-进程同步之unix域套接字)
  - [6.4. 线程同步](#64-线程同步)
  - [6.5. 线程同步的方式](#65-线程同步的方式)
- [python中如何使用多线程？](#python中如何使用多线程)
    - [6.5.1. 线程同步之互斥量](#651-线程同步之互斥量)
    - [6.5.2. 线程同步之自旋锁](#652-线程同步之自旋锁)
    - [6.5.3. 线程同步之读写锁](#653-线程同步之读写锁)
    - [6.5.4. 线程同步之条件变量](#654-线程同步之条件变量)
- [7. Linux进程管理](#7-linux进程管理)
  - [7.1. 进程的类型](#71-进程的类型)
  - [7.2. 前台进程](#72-前台进程)
  - [7.3. 后台进程](#73-后台进程)
  - [7.4. 守护进程( daemon )](#74-守护进程-daemon-)
  - [7.5. 特殊进程](#75-特殊进程)
  - [7.6. 什么是线程池](#76-什么是线程池)
    - [7.6.1. 为什么使用线程池](#761-为什么使用线程池)
- [8. 作业管理之进程调度](#8-作业管理之进程调度)
  - [8.1. 进程调度](#81-进程调度)
  - [8.2. 三个机制](#82-三个机制)
  - [8.3. 分类](#83-分类)
  - [8.4. 非抢占式的调度](#84-非抢占式的调度)
  - [8.5. 抢占式的调度](#85-抢占式的调度)
  - [8.6. 进程调度算法](#86-进程调度算法)

### 1. 进程管理之进程实体

#### 1.1. 进程

1. 进程是系统进行资源分配和调度的基本单位
2. 进程作为程序独立运行的载体保障程序的正常运行
3. 进程使资源的利用率大幅提升

#### 1.2. 进程的实体

1. 主存中进程形态：标识符、状态、优先级、序计数器、内存指针、上下文数据、IO状态信息、记账信息
2. 可分为进程标识符、处理机状态、进程调度信息、进程控制信息等几类

```
标识符：
唯一标记一个进程（id）

状态：
标记进程状态，如运行态

程序计数器：
进程即将被执行的下一条指令的地址

内存指针：
程序代码、进程数据相关指针

上下文数据：
进程执行时处理器存储的数据

IO状态信息：
被进程IO操作所占用的文件列表（如磁盘、内存、文件等）

记账信息：
使用处理器时间、时钟数总和等

PCB进程控制块：
1. 用于描述和控制进程运行的通用数据结构
2. 经常被读取，常驻内存，存放在系统专门开辟的PCB区域内
```

#### 1.3. 进程 Process 与线程 Thread

- 线程是操作系统进行运行调度的最小单位
- 包含在进程之中，是进程中实际运行工作的单位
- 一个进程可以并发多个线程，每个线程执行不同的任务
- 进程的线程共享进程资源

#### 1.4. 进程和线程的对比

|          | 进程               | 线程                 |
| -------- | ------------------ | -------------------- |
| 资源     | 资源分配的基本单位 | 不拥有资源           |
| 调度     | 独立调度的基本单位 | 独立调度的最小单位   |
| 系统开销 | 进程系统开销大     | 线程系统开销小       |
| 通信     | 进程IPC            | 读写同一进程数据通信 |


### 2. 并行和并发的区别
1. 并行：**真正多核`cpu`去执行**（`python`不能同时利用多个`cpu`，只能说是并发）
2. 并发：看似是并行，其实通过`cpu`的时间片轮转来切换任务，**同一时刻还是只能执行一个线程**，对外界来说营造了一种同时执行的效果


### 3. 什么是线程安全？

1. 线程安全就是**多线程访问时**，采用了**加锁机制**，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染。
2. 线程不安全就是**不提供数据访问保护**，**有可能出现多个线程先后更改数据造成所得到的数据是脏数据**


### 4. Python哪些操作是线程安全的？
1. 一个操作可以在多线程环境中安全使用，获取正确结果
1. 线程安全的操作好比线程是顺序执行而不是并发执行的(`i += 1` 不是线程安全)
1. 一般如果涉及到写操作需要考虑如何让多个线程安全访问数据



### 5. 进程管理之五状态模型

就绪、阻塞、执行、创建、终止

#### 5.1. 就绪状态

- 当进程被分配到CPU以外所有其他的资源（只差CPU资源）
- 就绪队列：多个处于就绪状态的进程组成一个队列

#### 5.2. 执行状态

- 进程获得CPU，其程序正在执行
- 在单处理机（单核）中，某个时刻只能有一个进程处于执行状态

#### 5.3. 阻塞状态

- 进程因某种原因（如其他设备未就绪而无法继续执行）放弃CPU的状态
- 阻塞队列：多个阻塞状态的进程组成的队列

#### 5.4. 创建状态

- 进程创建过程：分配PCB => 插入就绪队列
- 创建进程时拥有PCB但其他资源尚未就绪的状态

#### 5.5. 终止状态

- 系统清理 =>  PCB归还
- 进程结束后由系统清理或归还PCB的状态称为终止状态


![进程管理之五状态模型](../../imgs/process_five_status.png)



### 6. 进程管理之进程同步

#### 6.1. 临界资源

一些作为共享资源却无法同时被多个线程共同访问的共享资源。


#### 6.2. 进程间同步的原则

- 空闲让进：资源无占用，允许使用
- 忙则等待：资源有占用，请求进程等待
- 有限等待：保证有限等待时间能够使用资源
- 让权等待：等待时，进程需要让出 CPU（执行变成阻塞状态）

#### 6.3. 进程间同步的方法

1. 管道/匿名管道/有名管道(`pipe`)
1. 信号(`Signal`)：比如用户使用`Ctrl+c`产生`SIGINT`程序终止信号
1. 消息队列（`Message`）
1. 共享内存（`share memory`）
1. 信号量（`Semphare`）
1. 套接字（`socket`）：最常用的方式，web应用都是这种方式


#### Python中如何使用多进程？
Python有GIL，可以用多进程实现cpu密集程序
1. `multiprocessing`多进程模块
1. ` multiprocessing.Process`类实现多进程
1. 一般在`CPU`密集程序里，避免`GIL`的影响


##### 6.3.1. 进程同步之共享内存

- 多进程共同使用物理内存
- 由于操作系统的进程管理，进程间的内存空间是独立的
- 进程默认 不能访问进程空间之外的内存空间

共享内存特点：

- 共享内存允许不相关的进程 访问同一片物理内存
- 共享内存是两个进程之间共享和传递数据 最快的方式（常用）
- 共享内存未提供同步机制，需要借助其他机制管理访问（自己设置can_read，即加锁/解锁）

步骤：

1. 申请共享内存 
2. 连接到进程空间 
3. 使用共享内存 
4. 脱离进程空间&删除

##### 6.3.2. 进程同步之Unix域套接字

- 域套接字是高级的进程间通信的方法
- 提供了单机 简单可靠的进程通信同步服务
- 只能在单机使用，不能跨机器使用
- Unix 域套接字提供了类似 网络套接字的功能

服务端使用：

- 创建套接字、绑定、监听套接字，接收并处理信息
  客户端使用：
- 创建套接字、连接套接字，发送信息



#### 6.4. 线程同步

- 因为进程的线程共享进程资源，所以也需要同步
- 方法：互斥量、读写锁、自旋锁、条件变量


#### 6.5. 线程同步的方式
1. 互斥量（锁）：通过互斥机制防止多个线程同时访问公共资源（缺点：同一时刻只有一个线程访问公共资源）
1. 信号量（`Semphare`）：控制同一时刻多个线程访问同一个资源的线程数
1. 事件（信号）：通过通知的方式保持多个线程同步


### python中如何使用多线程？
`threading`模块
1. `threading.Thread`类用来创建线程
1. `start()` 方法启动线程
1. `join()`方法等待线程结束



##### 6.5.1. 线程同步之互斥量

当一个线程操作的时候，阻止另一个线程访问这个临界资源（加锁、解锁）

- 生产者、消费者模型的根本：两个线程的指令交叉执行
- 互斥量可以保证先后执行

原子性定义：

- 一系列操作不可被中断的特性
- 这一系列操作要么全部执行完，要么全部没有执行
- 不存在部分执行部分未执行的情况


互斥量：

- 互斥量是最简单的线程同步的方法
- 互斥量（互斥锁），处于两态之一的变量：解锁和加锁
- 两个状态可以保证资源访问的串行



##### 6.5.2. 线程同步之自旋锁

使用临界资源之前加锁，使用再解锁，和互斥锁一样

与互斥锁不同点：

- 使用自旋锁的线程会反复检查锁变量是否可用
- 自旋锁不会让出CPU，是一种忙等待状态
- 死循环，等待锁被释放

特点：

- 自旋锁避免了进程或线程上下文切换的开销
- 操作系统内部很多地方使用的是自旋锁
- 自旋锁不适合在单核CPU使用

##### 6.5.3. 线程同步之读写锁

- 临界资源多读少写
- 读取时候并不会改变临界资源的值

读写锁特点：

- 特殊的自旋锁
- 允许多个读者同时访问资源以提高性能
- 对于写操作则是互斥的

读和读不互斥，读和写互斥，写和写互斥


##### 6.5.4. 线程同步之条件变量

- 条件变量是相对复杂的线程同步方法
- 条件变量允许线程睡眠，直到满足条件
- 当满足条件时，可以向该线程发送信号，通知唤醒
- 配合互斥量使用

具体：

1. 缓冲区小于等于0时，不允许消费者消费，消费者必须等待
2. 缓冲区满时，不允许生产者往缓冲区生产，生产者必须等待
3. 前者情况，当生产者生产一个产品时，唤醒可能等待的消费者
4. 后者情况，当消费者消费一个产品时，唤醒可能等待的生产者

### 7. Linux进程管理

#### 7.1. 进程的类型

1. 前台进程
2. 后台进程
3. 守护进程

#### 7.2. 前台进程

具有终端，可以和用户交互的进程（占用了终端shell）

#### 7.3. 后台进程

1. 没有占用终端（可能有打印，但不影响使用Shell，同时Ctrl+C也不会停止）
2. 不和用户交互，优先级比前台进程低
3. 将需要执行的命令以&符号结束

#### 7.4. 守护进程( daemon )

1. 特殊的后台进程
2. 很多在在系统引导的时候启动，一直运行直到系统关闭
3. 进程名字一般以d结尾，比如 crond、httpd、sshd、mysqld


#### 7.5. 特殊进程

- ID为 0 的进程是`idle`进程，是系统创建的第一个进程
- ID为 1 的进程是`init`进程，是 0 号进程的子进程，完成系统初始化
- init 进程是所有用户进程的祖先进程


#### 7.6. 什么是线程池

- 线程池是存放多个线程的容器
- CPU调度线程执行后不会销毁线程
- 将线程放回线程池 重复利用


##### 7.6.1. 为什么使用线程池

- 线程是稀缺资源，不应该频繁创建和销毁
- 架构解耦，线程创建和业务处理解耦，更加优雅
- 线程池是使用线程的最佳实践

### 8. 作业管理之进程调度

#### 8.1. 进程调度

- 计算机通过决策决定哪个就绪进程可以获得CPU使用权
- 前提是多道程序设计
- 保存 旧进程的运行信息，清除旧进程（收拾包袱）
- 选择新进程，准备运行环境并分配CPU（新进驻）

#### 8.2. 三个机制

- 就绪队列的排队机制
- 选择运行进程的委派机制（选择就绪进程，分配CPU给它）
- 新老进程的上下文切换机制（保存当前进程的上下文信息，装入被委派执行进程的运行上下文）

#### 8.3. 分类

1. 非抢占式的调度
2. 抢占式调度


#### 8.4. 非抢占式的调度

- 处理器一旦分配给某个进程，就让该进程一直使用下去
- 调度程序不以任何原因抢占正在使用的处理器
- 直到进程完成工作或者因为IO阻塞才会让出处理器

#### 8.5. 抢占式的调度

- 允许调度程序以一定的策略暂停当前运行的进程
- 保存旧进程的上下文信息，分配处理器给新进程


#### 8.6. 进程调度算法

1. 先来先服务调度算法
2. 短进程优先调度算法（不利于长作业进程的执行）
3. 高优先权优先调度算法（前台进程 优先级高于后台进程）
4. 时间片轮转调度算法（最公平）

只有最后一个是抢占式的调度