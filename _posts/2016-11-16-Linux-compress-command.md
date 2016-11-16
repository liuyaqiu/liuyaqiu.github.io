---
layout: post
title: Linux下常见的压缩命令
categories: tutorial
---

Linux下有多种压缩工具，对应多种不同的压缩指令。

# **.tar**
.tar文件其实是打包文件，而不是压缩文件，相当于把许多文件捆在一起作为一个文件。因为压缩文件只能对单个文件进行操作，所以一般都需要先对多个文件进行打包，也就是tar一下。另外tar命令自带可以使用bz2, gzip, xz等压缩工具进行压缩。zip和rar不是Linux下的主流工具，不能被tar命令内置调用。

```
打包: tar cvf FileName.tar
解包: tar xvf FileName.tar
```

关于tar命令更加详细的说明可以参见`tar --help`和`man tar`。

# **.gz**
```
.gz
压缩: gzip FileName
解压: gzip -d FileName.gz, gunzip FileName.gz

.tar.gz
压缩: tar zcvf DirName.tar.gz DirName
解压: tar zxvf DirName.tar.gz
```

# **.xz**
```
.xz
压缩: xz -z FileName
解压: xz -d FileName.xz

.tar.xz
压缩: tar Jcvf DirName.tar.xz DirName
解压: tar Jxvf DirName.tar.xz
```

# **.bz2**
```
.bz2
压缩: bzip2 -z FileName
解压: bzip2 -d FileName.bz2, bunzip2 FileName.bz2

.tar.bz2
压缩: tar jcvf DirName.tar.bz2 DirName
解压: tar jxvf DirName.tar.bz2
```

# **.Z**
```
.Z
压缩: compress FileName
解压: uncompress FileName.Z

.tar.Z
压缩: tar Zcvf DirName.tar.Z DirName
解压: tar Zxvf DirName.tar.Z
```

# **.zip**
```
压缩: zip -r DirName.zip DirName
解压: unzip DirName.zip
```

# **.rar**
```
压缩: rar a DirName.rar DirName
解压: rar e DirName.rar
```

