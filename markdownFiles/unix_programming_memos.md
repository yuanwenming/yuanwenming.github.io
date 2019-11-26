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
- 函数dup和dup2
    用来复制一个已经存在的文件描述符
    ```c
    #include <unistd.h>

    int dup(int fd);
    int dup2(int fd, int fd2);
    ```

    dup返回当前可用文件描述符中的最小值。
    dup2使用fd2指定新描述符，如果fd2已打开，则先关闭。如果fd等于fd2，则dup2不关闭fd2而直接返回。否则，fd2的FD_CLOEXEC文件描述符标志就被清楚，这样fd2在进程调用exec时是打开状态。
- 函数sync、fsync和fdatasync
    ```c
    #include <unistd.h>

    int fsync(int fd);
    int fdatasync(int fd);

    void sync(void);
    ```

    sync将所有修改过的块缓冲区排入写队列，然后就返回，不等待实际磁盘写操作结束。
    fsync函数只对文件描述符fd指定的一个文件起作用，并且等待写磁盘操作结束才返回。可用于数据库这样的程序，需要确保修改过的块立即写入到磁盘。
    fdatasync函数类似于fsync，但它之影响文件的数据部分，而除数据之外，fsync还会同步更新文件的属性。
- 函数fcntl
    fcntl函数可以更改已打开文件的属性。
    ```c
    #include <unistd.h>
    int fcntl(int fd, int cmd, ...);
    ```

    fcntl函数有以下5种功能：
    - 复制一个已有的描述符（cmd=F_DUPFD或F_DUPFD_CLOEXEC）
    - 获取/设置文件描述符标志（cmd=F_GETFD或F_SETFD）
    - 获取/设置文件状态标志（cmd=F_GETFL或F_SETFL）
    - 获取/设置异步I/O所有权（cmd=F_GETOWN或F_SETOWN）
    - 获取/设置记录锁（cmd=F_GETLK、F_SETLK或F_SETLKW）
- 函数ioctl
    ioctl函数是I/O操作的杂物箱，不能用其它函数表示的I/O操作通常都可以使用ioctl来操作。
    ```c
    #include <sys/ioctl.h>
    int ioctl(int fd, int request, ...);
    ```

# 第4章 文件和目录

- 函数stat、fstat、fstatat和lstat
    获取文件相关信息
    ```c
    #include <sys/stat.h>
    int stat(const char *pathname, struct stat *buf);
    int fstat(int fd, struct stat *buf);
    int lstat(const char *pathname, struct stat *buf);
    int fstatat(int fd, const char *pathname, struct stat *buf, int flag);
    ```
    
    stat返回pathname指向文件的信息结构。
    fstat返回fd指向文件的信息结构。
    lstat中pathname指向的文件如果是一个链接文件，则返回链接文件本身的信息结构，而不是链接指向文件的信息。
    fstatat函数为一个相对于当前打开目录（由fd参数指向）的路径名返回文件统计信息。flag参数控制着是否跟随一个符号链接。当为AT_SYMLINK_NOFOLLOW时不跟随，否则，默认情况下返回链接所指向文件的信息结构。如果fd是AT_FDCWD且pathname是一个相对路径名，fstatat会计算相对于当前目录的pathname参数。如果pathname是一个绝对路径，fd参数会被忽略。
    
    结构体stat的基本实现大致如下：
    ```c
    struct stat
    {
        mode_t          st_mode;
        ino_t           st_ino;
        dev_t           st_dev;
        dev_t           st_rdev;
        nlink_t         st_nlink;
        uid_t           st_uid;
        gid_t           st_gid;
        off_t           st_size;
        struct timespec st_atime;
        struct timespec st_mtime;
        struct timespec st_ctime;
        blksize_t       st_blksize;
        blkcnt_t        st_blocks;
    }
    ```
- 文件类型
    - 普通文件
    - 目录文件
    - 块特殊文件
    - 字符特殊文件
    - FIFO
    - 套接字
    - 符号链接
    
    文件类型信息包含在stat结构的st_mode成员中，可用以下宏来测试文件类型：
    |宏|文件类型|
    |--------|--------|
    |S_ISREG()|普通文件|
    |S_ISDIR()|目录文件|
    |S_ISBLK()|块文件|
    |S_ISCHR()|字符特殊文件|
    |S_ISFIFO()|管道或FIFO文件|
    |S_ISLNK()|符号链接文件|
    |S_ISSOCK()|套接字文件|
- 函数access和faccessat
    使用实际用户ID和实际组ID进行访问权限测试。
    ```c
    #include <unistd.h>
    int access(char *pathname, int mode);
    int faccessat(int fd, char *pathname, int mode, int flag);
    ```

    |mode|说明|
    |----|----|
    |F_OK|测试文件是否存在|
    |R_OK|测试读权限|
    |W_OK|测试写权限|
    |X_OK|测试执行权限|

    flag参数用于改变faccessat的行为，如果flag设置为AT_EACCESS，访问检查用的是调用进程的有效用户ID和有效组ID，而不是实际用户ID和实际组ID。
- 函数umask
    umask为进程设置文件模式创建屏蔽字，并返回之前的值。
    ```c
    #include <sys/stat.h>
    mode_t umask(mode_t cmask);
    ```

    cmask的值为下列一个或多个进行“或”运算：
    - S_IRUSR
    - S_IWUSR
    - S_IXUSR
    - S_IRGRP
    - S_IWGRP
    - S_IXGRP
    - S_IROTH
    - S_IWOTH
    - S_IXOTH
- 函数chmod、fchmod、fchmodat
    chmod、fchmod、fchmodat这3个函数可以更改现有文件的访问权限
    ```c
    #include <sys/stat.h>
    int chmod(char *pathname, mode_t mode);
    int fchmod(int fd, mode_t mode);
    int fchmodat(int fd, char *pathname, mode_t mode, int flag);
    ```

    当fchmodat函数的flag参数设置为AT_SYMLINK_NOFOLLOW标志时，fchmodat并不会跟随符号链接。
- 函数chown、fchown、fchownat和lchown
    用于更改文件的用户ID或组ID。如果owner或者group任意一个为-1，则对应ID不改变
    ```c
    #include <unistd.h>
    int chown(char *pathname, uid_t owner, gid_t group);
    int fchown(int fd, char *pathname, uid_t owner, gid_t group);
    int fchownat(int fd, char *pathname, uid_t owner, gid_t group, int flag);
    int lchown(char *pathname, uid_t owner, gid_t group);
    ```
- 函数truncate、ftruncate
    使用这两个函数将文件的长度进行截断
    ```c
    #include <unistd.h>
    int truncate(char *pathname, off_t length);
    int ftruncate(int fd, off_t length);
    ```
- 函数link、linkat、unlink、unlinkat和remove
    ```c
    #include <unistd.h>
    int link(char *existingpath, char *newpath);
    int linkat(int efd, char *existingpath, int nfd, char *newpath, int flag);
    int unlink(char *pathname);
    int unlinkat(int fd, char *pathname, int flag);
    int remove(char *pathname);
    ```
- 函数rename和renameat
    对文件或目录重命名
    ```c
    #include <stdio.h>
    int rename(char *oldname, char *newname);
    int renameat(int oldfd, char *oldname, int newfd, char *newname);
    ```
- 函数symlink和symlinkat、readlink、readlinkat
    创建一个符号链接或读取一个链接信息
    ```c
    #include <unistd.h>
    int symlink(char *actualpath, char *sympath);
    int symlinkat(char *actualpath, int fd, char *sympath);
    int readlink(char *pathname, char *buf, size_t bufsize);
    int readlinkat(int fd, char *pathname, char *buf, size_t bufsize);
    ```
- 函数futimens、utimesat和utimes
    访问或修改文件的时间
    ```c
    #include <sys/stat.h>
    int futimens(int fd, const struct timespec times[2]);
    int utimensat(int fd, const char *path, const struct timespec times[2], int flag);
    int utimens(char *pathname, const struct timeval times[2]);
    ```
- 函数mkdir、mkdirat和rmdir
    创建或者删除目录
    ```c
    #include <sys/stat.h>
    int mkdir(char *pathname, mode_t mode);
    int mkdirat(int fd, char *pathname, mode_t mode);

    #include <unistd.h>
    int rmdir(char *pathname);
    ```
- 读目录函数
    ```c
    #include <dirent.h>
    DIR *opendir(char *pathname);
    DIR *fdopendir(int fd);
    struct dirent *readdir(DIR *dp);
    void rewinddir(DIR *dp);
    int closedir(DIR *dp);
    long telldir(DIR *dp);
    void seekdir(DIR *dp, long loc);
    ```
- 函数chdir、fchdir和getcwd
    进程调用chdir、fchdir函数可以更改当前工作目录
    ```c
    #include <unistd.h>
    int chdir(char *pathname);
    int fchdir(int fd);
    ```

    进程获取getcwd函数获取绝对路径
    ```c
    #include <unistd.h>
    char *getcwd(char *buf, size_t bufsize);
    ```


# 第5章 标准I/O库

- 函数fwide
    fwide函数可用于设置流的定向
    ```c
    #include <wchar.h>
    int fwide(FILE *fp, int mode);
    ```
    
    根据mode的值不同，fwide函数执行不同的工作
    - 如果mode为负数，fwide将试图使指定的流是字节指向的
    - 如果mode为正数，fwide将试图使指定的流时宽定向的
    - 如果mode为0，fwide将不试图设置流的定向，但返回标识该流定向的值
- 函数setbuf、setvbuf
    设置流缓冲
    ```c
    #include <stdio.h>
    int setbuf(FILE *fp, char *buf);
    int setvbuf(FILE *fp, char *buf, int mode, size_t size);
    ```
    
    setbuf函数可以打开或关闭流的缓冲机制，若buf为NULL，则关闭缓冲
    setvbuf函数可以更精确的说明所需的缓冲类型，通过指定mode为：
    - _IOFBF    全缓冲
    - _IOLBF    行缓冲
    - _IONBF    不带缓冲
    如果指定一个不带缓冲的流，则buf和size会被忽略；如果该流是带缓冲的而buf为NULL，则系统自动地为该流分配适当的缓冲区。
- 函数fflush
    强制冲洗一个流
    ```c
    #include <stdio.h>
    int fflush(FILE *fp);
    ```
- 函数fopen、freopen、fdopen
    打开一个流
    ```c
    #include <stdio.h>
    FILE *fopen(char *pathname, char *type);
    FILE *freopen(char *pathname, char *type, FILE *fp);
    FILE *fdopen(int fd, char *type);
    ```
    
    fopen函数打开路径名为pathname的文件
    freopen函数在一个指定的流上打开一个文件，如过流已经打开，则先关闭该流。若该流已经定向，则使用freopen清除该定向。此函数一般用于将一个指定的文件打开为一个预定的流：标准输入、标准输出、标准错误
    fdopen函数取一个已有的文件描述符，并使一个标准的I/O流与该描述符相结合。常用于由创建管道和网络通信通道函数返回的描述符
    
    type参数如下：
    |type|说明|open标志|
    |----|----|----|
    |r或rb|为读而打开|O_RDONLY|
    |w或wb|把文件截断为0长，或为写而创建|O_WRONLY\|O_CREATE\|O_TRUNC|
    |a或ab|追加；为在文件尾写而打开，或为写而创建|O_WRONLY\|O_CREATE\|O_APPEND|
    |r+或r+b或rb+|为读和写而打开|O_RDWR|
    |w+或w+b或wb+|把文件截断至0长，或为读和写而打开|O_RDWR\|O_CREATE\|O_TRUNC|
    |a+或a+b或ab+|O_RDWR\|O_CREATE\|O_APPEND|
- 函数fclose
    关闭一个已经打开的流
    ```c
    #include <stdio.h>
    int fclose(FILE *fp);
    ```
- 函数getc、fgetc、getchar
    从流中一次读取一个字符
    ```c
    #include <stdio.h>
    int getc(FILE *fp);
    int fgetc(FILE *fp);
    int getchar(void);  // 等同于getc(stdin)
    ```
    
    不管是出错还是到达文件尾端，这三个函数都返回同样的值。为了区分这两种不同的情况，必须调用ferror或feof
    ```c
    #include <stdio.h>
    int ferror(FILE *fp);
    int feof(FILE *fp);
    
    void clearerr(FILE *fp);
    ```
    
    大多数实现中，为每个流在FILE对象中维护了两个标志：
    - 出错标志
    - 文件结束标志
    调用clearerr可以清除这两个标志
    
    从流中读取数据以后，可以调用ungetc将字符再压送回流中。
    ```c
    #include <stdio.h>
    int ungetc(int c, FILE *fp);
    ```
- 函数putc、fputc、putchar
    向流中输出一个字符
    ```c
    #include <stdio.h>
    int putc(int c, FILE *fp);
    int fputc(int c, FILE *fp);
    int putchar(int c);  //等同于putc(c, stdout);
    ```
- 函数gets、fgets
    从流中一次读取一行
    ```c
    #include <stdio.h>
    char *gets(char *buf);
    char *fgets(char *buf, int n, FILE *fp);
    ```
    gets不保存读取到的换行符；
    fgets保存读取到的换行符
- 函数puts、fputs
    向流每次输出一行
    ```c
    int puts(char *buf);
    int fputs(char *buf, FILE *fp);
    ```
    puts、fputs会将一个以null结尾的buf写入到流中，但puts还会额外写一个换行符
- 函数fread、fwrite
    可以执行二进制的读写操作
    ```c
    #include <stdio.h>
    size_t fread(void *ptr, size_t size, size_t nobj, FILE *fp);
    size_t fwrite(void *ptr, size_t size, size_t nobj, FILE *fp);
    ```
    fread从指定的流中一次读取nobj个size大小的数据到ptr中
    fwrite将ptr中的数据向指定的流中一次写入nobj个size大小的数据

# 第6章 系统数据文件和信息

- 获取口令文件项
```c
#include <pwd.h>
struct passwd *getpwnam(const char *name);
struct passwd *getpwuid(uid_t uid)
```

- 查看整个口令文件
```c
#include <pwd.h>
struct passwd *getpwent(void);
void setpwent(void);
void endpwent(void);
```
getpwent返回口令文件的下一个记录项，每次调用此函数都会重写passwd结构体。
setpwent反绕它所使用的文件，endpwent则关闭这些文件。
在使用getpwent查看完口令文件后，一定要调用endpwent关闭文件，因为getpwent知道何时打开它所使用的文件，但不知何时关闭文件。

# 第7章 进程环境

- 进程终止方式
    有8中终止方式，5种正常终止：
        - 从main返回
        - 调用exit
        - 调用_exit或_Exit
        - 最后一个线程从其启动例程返回
        - 从最后一个线程调用pthread_exit
    3中异常终止：
        - 调用abort
        - 接到一个信号
        - 最后一个线程对取消做出响应
- 退出函数
    ```c
    #include <stdlib.h>
    void exit(int status)
    void _Exit(int status)

    #include <unistd.h>
    void _exit(int status)
    ```
    exit函数总是执行一个标准I/O库的清理关闭操作，对于所有打开流调用fclose函数。_Exit和_exit立即进入内核
- 函数atexit
    ```c
    #include <stdlib.h>
    int atexit(void (*func)(void))
    ```
    atexit的参数是一个函数地址，当调用此函数时无需向它传递任何参数，也不期望它返回一个值。exit调用这些函数时与登记的顺序相反，同一函数如果登记多次，也会被调用多次。
- C程序启动及终止方式
    ![](../resources/pic/exit.png)
- C程序存储空间布局 
    - 正文段
        存储由CPU执行的机器指令部分，通常是只读的且可共享
    - 初始化数据段
        通常称此段为数据段，包含了程序中需要明确赋初值的变量，例如程序中的全局变量
    - 未初始化数据段
        未初始化数据段，通常称为bss段。在程序开始前，内核将此段中的数据初始化为0或空指针
    - 栈
        自动变量以及函数调用时所需保存的信息都放在此段中。
    - 堆
        动态分配存储

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



