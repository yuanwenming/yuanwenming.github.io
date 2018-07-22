### VIM常用命令

---

- 动态匹配检索对象
```
:set incsearch 或者 :set is
(is是incsearch的缩写形式)
```

- 关闭动态匹配检索对象
```
:set noincsearch 或者 :set nois
(nois是noincsearch的缩写形式)
```

- 是否显示行号
```
:set nu         //显示行号
:set nonu       //不显示行号
```

- 是否换行
```
:set wrap
:set nowrap
```

- 检索是否高亮显示
```
:set hlsearch
:set nohlsearch
```

- 是否自动缩排
```
:set autoindent
:set noautoindent
```

- 是否自动备份
```
:set backup         //开启后，修改时会自动生成filename~文件
:set nobackup
```

- 显示与系统默认值不同的值
```
:set
```

- 语法高亮
```
:syntax on
:syntax off
```

- 显示颜色色调
```
:set bg=dark
:set bg=light
```

- 查看所有可用设置
```
:set all
```
