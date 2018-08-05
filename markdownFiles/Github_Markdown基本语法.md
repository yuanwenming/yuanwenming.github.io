# GitHub Markdown基本语法

---

- [**标题**](#标题)
- [**样式文本**](#样式文本)
- [**引用文字**](#引用文字)
- [**引用代码**](#引用代码)
- [**链接**](#链接)
- [**列表**](#列表)
- [**任务列表**](#任务列表)

### 标题
可以使用1~6个#来创建标题，#越多，则标题文字越小
>\# 标题1
>\#\# 标题2
>\#\#\# 标题3
>\#\#\#\# 标题4
>\#\#\#\#\# 标题5
>\#\#\#\#\#\# 标题6

示例结果：
># 标题1
>## 标题2
>### 标题3
>#### 标题4
>##### 标题5
>###### 标题6

### 样式文本
可以使用粗体、斜体、删除线、粗斜体来格式文字

|样式|语法|示例|结果|
|:-----:|-----|-----|--|
|粗体  |\*\*xx\*\*或\_\_xx\_\_|\*\*This is a bold text\*\*|**This is a bold text**|
|斜体   |\*xx\*或\_xx\_|\*This text is italicized\*|*This text is italicized*|
|删除线|\~\~xx\~\~|\~\~This was mistaken text\~\~|~~This was mistaken text~~|
|粗斜体|** **和_ _|\*\*This text is \_extremely\_ important\*\*|**This text is _extremely_ important**|

### 引用文字
使用>引用文字

    In the words of Abraham Lincoln:
    > Pardon my French

<p>

>In the words of Abraham Lincoln:
>>Pardon my French

### 引用代码
可以使用反引号'\`'来强调句子中的代码或命令。反引号中的文本不会被格式化。

    Use `git status` to list all new or modified files that haven't yet been committed.
<p>

Use `git status` to list all new or modified files that haven't yet been committed.

使用三个反引号格式化代码块：

    ```c
    int main(int argc, char * argv[])
    {
        printf("Hello World!\n");
    }
    ```

结果：
```c
int main(int argc, char * argv[])
{
    printf("Hello World!\n");
}
```

### 链接
使用[]显示链接的文字，使用()指向链接地址

例：

    [点击跳转到百度](https://www.baidu.com)

结果：
[点击跳转到百度](https://www.baidu.com)


### 列表

##### 无序列表
可以使用'\-'或'\*'来表示无序列表

例：

    - Apple
    - Banana
    - Orange

结果：

- Apple
- Banana
- Orange

<p>

##### 有序列表
使用数字来表示有序列表

例：

    1. Apple
    2. Banana
    3. Orange

结果：

1. Apple
2. Banana
3. Orange

### 任务列表
要创建任务列表，请在前面列出具有常规空格字符的项目[ ]。要将任务标记为完成，请使用[x]。

例：

\- [x] Finish my changes
\- [ ] Push my commits to GitHub
\- [ ] Open a pull request

结果：

- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
