## Debian9安装Atom后无法启动之解决方法

---

在[Atom官网](https://atom.io/)下载了适合于Debian的安装包后，使用```$ sudo dpkg -i Atom.deb```进行了安装，然后点击Atom的图标启动时，发现加载半天后退出了。

打开命令行，执行命令```$ atom```，发现有错误提示，提示内容如下：
```bash
$ atom
$ atom 依赖于libgconf-2.4.so，单并没有发现该so文件，加载失败
$
```

原来是缺少依赖了，不多说，直接修复缺少的依赖：
```bash
$ sudo apt --fix-broken install
```

上述命令将会安装Atom缺少的依赖文件，修复完后，就可以正常启动Atom了。

---
