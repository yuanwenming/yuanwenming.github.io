# git常用命令

---

### [完整git命令参考手册](https://git-scm.com/docs)

---

### Git配置

<br>
Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- /etc/gitconfig文件：系统中对所有用户都普遍适用的配置。若使用git config时用--system选项，读写的就是这个文件。
- ~/.gitconfig文件：用户目录下的配置文件只适用于该用户。若使用git config时用--global选项，读写的就是这个文件。
- 当前项目的Git目录中的配置文件（也就是工作目录中的.git/config文件）：这里的配置仅仅针对当前项目有效。<p>
每一个级别的配置都会覆盖上层的相同配置，所以.git/config里的配置会覆盖/etc/gitconfig中的同名变量。

例：

- 配置个人用户名以及电子邮箱
```bash
$ git config --global user.name "yourname"
$ git config --global user.email xxx@xxx.com
```

- 配置默认文本编辑器
```bash
$ git config --global core.editor vim
```

- 配置编码
```bash
$ git config --global i18n.commitencoding utf8
```

- 解决中文乱码
```bash
$ git config --global core.quotepath false
```

- 配置差异比较工具
```bash
$ git config --global merge.tool vimdiff
```

- 查看配置信息
```bash
$ git config --list
```

- 直接查看某个环境变量的设定，只需要把查看的名字写在后面即可
```bash
$ git config user.name
```

---

### 创建仓库


- 在当前目录生成仓库
```bash
$ git init
```
命令执行完后，会在当前目录生成.git目录

- 在指定目录初始化仓库
```bash
$ git init newrepo
```
命令执行完后，会在newrepo目录中生成.git目录

- 克隆仓库
```bash
$ git clone <repo url> <directory>
```
命令执行完后，将会把<repo url>所代指的仓库克隆到本地<directory>目录中去

### Git基本操作命令

- 将文件纳入版本控制
```bash
$ git add *.c                               //将所有的.c文件加入到缓存区
$ git add README
$ git commit -m "commit description"        //将文件保存到仓库
```
命令执行完后，将会把当前目录下所有的.c和README文件保存到仓库中。

- 显示自上次提交后仓库中是否有文件变动
```
$ git status
```

- 一次性把所有改动文件保存到仓库中
```bash
$ git commit -a
```

- 从缓存中撤销修改

    假设修改某个文件后，使用git add将修改添加到了缓存中，如果想撤销修改，可以使用如下命令：
    ```bash
    $ git reset HEAD -- xxx.file
    ```
    执行上述命令后，会从缓存区中把xxx.file的改动撤销

- 删除文件

    如果只是简单地从工作目录中手工删除文件，运行 git status 时就会在 Changes not staged for commit 的提示。

    要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除，然后提交。可以用以下命令完成此项工作
    ```bash
    $ git rm <file>
    ```

    如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
    ```bash
    $ git rm -f <file>
    ```

    如果把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可
    ```bash
    $ git rm --cached <file>
    ```

---
