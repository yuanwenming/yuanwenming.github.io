### Linux进程间通讯

---

进程间通讯(IPC InterProcess Communication)是指在进程间交换信息，一般常见的IPC方法有管道pipe、消息队列、共享内存、信号、socket套接字等。
下面将就各种方法简单举例总结。

- 管道pipe

  - 普通管道
    (※本小节摘选自Linux C一站式编程j进程间通讯部分)
    <br>

        #include <uinstd.h>
        int pipe(int pipefd[2]);

    调用pipe函数时在内核中开辟一块缓冲区（称为管道）用于通信，它有一个读端一个写端，然后通过pipefd参数传出给用户程序两个文件描述符，pipefd[0]指向管道的读端，pipefd[1]指向管道的写端。所以管道在用户程序看起来就像一个打开的文件，通过read(pipefd[0]);或者write(pipefd[1]);向这个文件读写数据其实是在读写内核缓冲区。pipe函数调用成功返回0，调用失败返回-1。

    <br>

    ![](../resources/linux_c_programming/images/process.pipe.png)

    1. 父进程调用pipe开辟管道，得到两个文件描述符指向管道的两端。
    2. 父进程调用fork创建子进程，那么子进程也有两个文件描述符指向同一管道。
    3. 父进程关闭管道读端，子进程关闭管道写端。父进程可以往管道里写，子进程可以从管道里读，管道是用环形队列实现的，数据从写端流入从读端流出，这样就实现了进程间通信。

    示例：

    ```c
    #include <stdlib.h>
    #include <unistd.h>
    #include <stdio.h>
    #include <sys/types.h>
    #include <sys/wait.h>

    int main(int argc, char *argv[])
    {
        int n;
        int fd[2];
        pid_t pid;
        char line[80];

        // create a pipe
        if(pipe(fd) < 0)
        {
            printf("create pipe failed!\n");
            return -1;
        }

        if((pid = fork()) < 0)
        {
            printf("generate child process failed!\n");
            return -1;
        }

        // parent process
        if(pid > 0)
        {
            close(fd[0]);
            write(fd[1],"hello world!",12);
            wait(NULL);
        }
        // child
        else
        {
            close(fd[1]);
            n = read(fd[0], line, 80);
            write(STDOUT_FILENO,line, n);
        }
        return 0;
    }
    ```

  - 流管道
  - 命名管道

- 消息队列

- 共享内存

- 信号

- socket
