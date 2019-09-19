# 第1章 UNIX基础知识


# 第2章 UNIX标准及实现

# 第3章 文件I/O

- 函数open和openat

    调用open或openat函数可以打开或创建一个文件。
    ```c
    #include <fcntl.h>
    
    int open(const char *pathname, int flags);
    int open(const char *pathname, int flags, mode_t mode);
    
    int openat(int dirfd, const char *pathname, int flags);
    int openat(int dirfd, const char *pathname, int flags, mode_t mode);
    ```
    
    - pathname参数是要打开或创建文件的名字。
    - flags参数用来说明此函数的多个选项，可用下面一个或多个进行“或”运算
        - 必选（五选一）：
            - O_RDONLY
                只读打开
            - O_WRONLY
                只写打开
            - O_RDWR
                读、写打开
            - O_EXEC
                只执行打开
            - O_SEARCH
                只搜索打开（应用于目录）
        - 可选（可多选）：
            - O_APPEND
                每次写时都追加到文件的尾端
            - O_CLOEXEC
                把FD_CLOEXEC常量设置为文件描述符标志
            - O_CREAT
                若此文件不存在则创建它。使用此选项时，open函数必须同时说明第3个参数mode（openat函数需说明第4个参数mode）
            - O_DIRECTORY
                如果pathname引用的不是目录则出错
            - O_EXCL
                如果同时指定了O_CREAT，而文件已存在，则出错。用此可以测试一个文件是否存在，如果不存在则创建，这使测试和创建文件成为一个原子操作。
            - O_NOCTTY
                如果pathname引用的时终端，则不将该设备作为此进程的控制终端
            - O_NOFOLLOW
                如果pathname引用的是一个符号链接，则出错
            - O_NONBLOCK
                如果pathname引用的是一个FIFO、块特殊文件或字符特殊文件，则此选项为文件本次的打开操作和后续的I/O操作设置非阻塞方式
            - O_SYNC
                使每次write等待物理I/O操作完成，包括由该write操作引起的文件属性更新所需的I/O
            - O_TRUNC
                如果此文件存在，而且为只写或读-写方式成功打开，则将其长度截为0
            - O_TTY_INIT
                如果打开一个还未打开的终端设备，设置非标准termios参数值，使其符合Single UNIX Specification
            
            下面两个标志也是可选的，他们是Single UNIX Specification（以及POSIX.1）中同步输入和输出选项的一部分
            
            - O_DSYNC
                使每次write要等待物理I/O操作完成，但是r如果该写操作并不影响读取刚刚写入的数据，则不许等待文件属性被更新
            - O_RSYNC
                使每一个以文件描述符作为参数进行的read操作等待，直至所有对文件同一部分挂起的写操作都完成。
    - mode参数指定了flags为O_CREATE时操作文件的其它模式
        - S_IRWXU
            文件所有者拥有读、写、执行权限
        - S_IRUSR
            文件所有者拥有读权限
        - S_IWUSR
            文件所有者拥有写权限
        - S_IXUSR
            文件所有者拥有执行权限
        - S_IRWXG
            同组用户拥有读、写、执行权限
        - S_IRGRP
            同组用户拥有读权限
        - S_IWGRP
            同组用户拥有写权限
        - S_IXGRP
            同组用户拥有执行权限
        - S_RWXO
            其他用户拥有读、写、执行权限
        - S_IROTH
            其它用户拥有读权限
        - S_IWOTH
            其他用户拥有写权限
        - S_IXOTH
            其他用户拥有执行权限
        - S_ISUID
            设置用户ID
        - S_ISGID
            设置组ID
        - S_ISVTX
            设置sticky bit
    
    fd参数把open和openat函数区分开，共有3种可能性：
    1. pathname参数指定的路径为绝对路径时，fd被忽略，openat函数相当于open函数
    2. pathname参数指定的是相对路径，fd参数指出了相对路径在文件系统中的开始地址。fd参数是通过打开相对路径所在目录来获取
    3. pathname参数指定了相对路径，fd参数具有特殊值AT_FDCWD,在这种情况下，路径名在当前工作目录中获取，openat函数在操作上与open函数类似
- 函数create
    创建一个新文件
    ```c
    #include <fcntl.h>
    
    int create(const char *path, mode_t mode);
    ```
    
    此函数等效于：
    ```c
    open(path, O_WRONLY|O_CREATE|O_TRUNC, mode);
    ```
    
    
- 函数close
    关闭一个已打开的文件
    ```c
    #include <fcntl.h>
    
    int close(int fd)
    ```
    
    关闭一个文件时还会释放该进程加在该文件上的所有记录锁。
    当一个进程终止时，内核自动关闭它打开的所有文件。
- 函数lseek
    为一个已打开的文件设置偏移量
    ```c
    #include <fcntl.h>
    
    off_t lseek(int fd, off_t offset, int whence);
    ```
    
    - 若whence为SEEK_SET,则将该文件的偏移量设置为距文件开始出offset个字节
    - 若whence为SEEK_CUR，则将该文件的偏移量设置为其当前值加offset，offset可为正或负
    - 若whence为SEEK_END，则将该文件的偏移量设置为文件长度加offset，offset可为正或负
- 函数read
    从打开的文件中读取数据
    ```c
    #include <fcntl.h>
    int read(int fd, char *buf, size_t nbytes);
    ```
    
    读取成功返回实际读取的字节数，如到达文件末尾则返回0
- 函数write
    向已打开的文件写数据
    ```c
    #include <fcntl.h>
    int write(int fd, const void *buf, size_t nbytes);
    ```
    写成功时返回值与 nbytes值相同，否则为失败。
- 文件共享
- 原子操作
- 函数dup和dup2
- 函数sync、fsync和fdatasync
- 函数fcntl
- 函数ioctl


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



