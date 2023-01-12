
本文介绍了7z，tar，zip命令的使用方法，其中zip操作最简单，tar操作最为复杂但扩展性高。

<!--more-->
{{< toc >}}

## 7z

### 添加文件

`7z a test.7z test_folder/`

### 移除文件

`7z d test.7z test_file`

`7z d -r test.7z *.png`

### 解压

`7z x test.7z [-o test_folder]`



## zip

### 压缩

`zip -r test.zip test_folder`

### 移除文件

`zip -d tset.zip test.png`

### 解压

`unzip test.zip`

## Tar

以 test.tar.gz 为例，tar为打包文件格式，gz为压缩文件格式

### 添加文件



### 移除文件



### 解压

<!-- ## 参考资料和推荐阅读 -->
