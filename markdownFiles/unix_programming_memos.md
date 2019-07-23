# 第1章 UNIX基础知识

# 第2章 UNIX标准及实现

# 第3章 文件I/O

# 第4章 文件和目录

# 第5章 标准I/O库

# 第6章 系统数据文件和信息

# 第7章 进程环境

# 第8章 进程控制

# 第9章 进程关系

# 第10章 信号

# 第11章 线程

# Linux多线程

### 1.基本函数
- int pthread_create(pthread_t *thread, pthread_attr_t *attr, void *(*start_routine)(void *), void *arg)

    创建一个新线程，新线程ID通过thread返回，attr是创建线程时指定的线程属性，start_routine是线程要执行的函数，arg是要传给待执行函数的参数

    创建成功时返回0;失败时返回非0，且thread时未定义状态

- int pthread_equal(pthread_t id1, pthread_t id2)

    比较两个线程是否相等，相等返回非0，不等返回0

- pthread_t pthread_self(void)

    获取线程自身的ID
    
- void pthread_exit(void *rtnval)

    退出当前线程，并通过rtnval返回线程的返回值
    
- int pthread_join(pthread_t thread, void **retval)

    等着thread指定的线程终止，并通过retval获取thread线程的返回值
    
- int pthread_cancel(pthread_t thread)

    向线程发送一个取消请求，仅提出请求，不等待线程终止
    
- void pthread_cleanup_push(void (*routine)(void *),void *arg)

    入栈一个线程清理函数，并在线程调用pthread_exit退出、pthread_cancel取消及pthread_cleanup_pop的参数为非0时，执行该清理函数；pthread_cleanup_pop的参数为0时，将不再执行清理函数
    
- void pthread_cleanup_pop(int execute)

    从栈中清除最近一个使用pthread_cleanup_push设置的清理函数
    execute为0时，不执行清理函数但仍会从栈中删除该函数
    execute不为0时，执行清理函数并将函数从栈中清除
    
### 2.线程同步
#### 2.1 互斥量
- pthread_mutex_init
- pthread_mutex_destory
- pthread_mutex_lock
- pthread_mutex_trylock
- pthread_mutex_unlock
- pthread_mutex_timedlock
#### 2.2 读写锁/共享互斥锁
- pthread_rwlock_init
- pthread_rwlock_destory
- pthread_rwlock_rdlock
- pthread_rwlock_wrlock
- pthread_rwlock_unlock
- pthread_rwlock_tryrdlock
- pthread_rwlock_trywrlock
- pthread_rwlock_timedrdlock
- pthread_rwlock_timedwrlock

# 第12章 线程控制

# 第13章 守护进程

# 第14章 高级I/O

# 第15章 进程间通讯

- 管道(匿名管道)

    管道是半双工（即数据只能在一个方向上流动）的，且只能在具有公共祖先的两个进程间使用。
    
    通常一个管道由一个进程创建，在进程调用fork后，该管道就可以在父进程和子进程之间使用了。
    
    函数原型：
    ```c
    #include <unistd.h>
    int pipe(int fd[2])
    ```
    fd返回两个文件描述符：fd[0]为读而打开、fd[1]为写而打开。fd[1]的输出是fd[0]的输入。
    
    单个进程中的管道几乎没有任何用处。通常，进程会先调用pipe，接着调用fork，从而创建从父进程到子进程的IPC通道，反之亦然。
    
    fork之后的半双工管道：
    
    ![半双工管道](../resources/pic/pipe_001.png)
    
    从父进程到子进程的管道：
    
    ![从父进程到子进程的管道](../resources/pic/pipe_002.png)
    
    其它创建管道的方式：
    
    ```c
    #include <unistd.h>
    FILE *popen(const char *cmdstring, const char *type);
    int pclose(FILE *fp);
    ```
    
    函数popen先执行fork，然后调用exec执行cmdstring，并且返回一个标准I/O文件指针。如果type是“r”，则文件指针连接到cmdstring的标准输出；如果type是“w“，则文件指针连接到cmdstring的标准输入
    
    ![](../resources/pic/pipe_003.png)
    
    协同进程(coprocess)

- FIFO（命名管道）

    管道（匿名管道）只能在两个相关的进程之间使用，而且这两个进程还要有一个共同的创建了他们的祖先进程。
    但是FIFO（命名管道）在不相干的进程间也能交换数据。
    
    相关函数原型：
    ```c
    #include <sys/stat.h>
    int mkfifo(const char *path, mode_t mode);
    int mkfifoat(int fd, const char *path, mode_t mode);
    ```
    
    
- 消息队列
- 信号量
- 共享存储
- 信号
- unix domain socket

# 第16章 网络IPC：套接字

# 第17章 高级进程间通信

# 第18章 终端I/O

# 第19章 伪终端

# 第20章 数据库函数库

# 第21章 与网络打印机通信



