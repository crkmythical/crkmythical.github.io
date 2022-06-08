---
title: TAR
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2021-01-09 21:00:07
top:
tags:
layout:
---
编写进度
- [x] 
tar(tape archive) 命令可以为Linux的文件和目录创建档案。

## 归档
```zsh
tar -cf output.tar file1 file2 ... filen  # 创建归档文件 ,-c创建  -f 归档文件名
tar -tvf archive.tar  # 列出归档文件中的文件

tar -rf original.tar new_file # 向归档文件追加/替换文件
tar -uf archive.tar file1  # 更新归档中的指定文件
tar -df archive.tar    # 查看归档中文件与系统中文件的差异
tar -f archive.tar --delete file1 file2 ..  # 删除指定文件

tar -Af file1.tar file2.tar   # 合并多个tar文件
```

## 压缩
- `-j` : bunzip2格式
- `-z` : gzip格式
- `--lzma` : lzma格式

```zsh
tar -acvf  archive.tar.gz  *  --exclude "*.txt"   # -a根据压缩文件命令自动选择压缩算法,排除指定文件(也可使用-X exclue_file)
tar -acvf archive.tar.gz * --excluede-cvs  --totals     # 排除版本控制系统相关文件,显示压缩文件大小
```

## 解压
```zsh
tar -axvf archive.tar.gz  file1 file4  -C /path/to/extraction_directory   # 根据压缩后缀自动选择解压算法,提取指定文件到指定目录
tar -xzvkpf archive.tar    # 解压bunzip2格式文件，-k不覆盖本地已有文件 -p保留文件相关权限
```

## Tips
```zsh
tar -cvf - files/ --exclude-cvs | ssh user@server "tar xv -C /tmp" 
# 对file目录中的文件进行归档并将其输出到stdout(-代表stdout), 然后提取到远程系统中的/tmp目录
```

