# Common tool

!!! abstract
    It should  work in Unix-like system.


## tar

> From GPT-4o 

Q1: 我是一位学习《操作系统》课的新手。我现在有解压缩 “toybox-0.1.0.tar.bz2” 的任务，但是我毫无概念、无从下手。应该如何做？

A1: 
...
**解压缩文件**：使用 `tar` 命令。输入以下命令来解压缩 `.tar.bz2` 文件：

```sh 
    tar xvjf toybox-0.1.0.tar.bz2
```
   
其中：

- `x` 代表解压缩。
- `v` 代表详细输出。
- `j` 代表使用 bzip2 解压缩。
- `f` 代表文件。

解压缩后，你应该会看到一个 `toybox-0.1.0` 文件夹，其中包含了解压缩后的内容。

Q2: 文件扩展名为什么是 .tar.bz2 而不是 tar

A2: 文件扩展名 `.tar.bz2` 表示这个文件是经过两步处理的压缩文件：

1. **打包（归档）**：首先，将多个文件和目录打包成一个单一的 `.tar` 文件。这个过程不压缩数据，只是将它们组合在一起。`.tar` 是 "tape archive" 的缩写。
    
2. **压缩**：然后，使用 bzip2 压缩算法对 `.tar` 文件进行压缩，生成 `.bz2` 文件。`.bz2` 是 bzip2 压缩格式的扩展名。
    

因此，`.tar.bz2` 文件是先打包再压缩的文件，包含了多个文件和目录，并且通过 bzip2 算法进行了压缩。这种组合方式可以提高文件传输和存储的效率。

要解压缩这样的文件，需要首先解压缩 bzip2 压缩层，然后解开 tar 归档层，这就是为什么使用 `tar xvjf` 命令来一次性完成这两步操作。

Q3: 那么 tar 还支持其他的压缩算法吗？

A3: 
是的，`tar` 支持多种压缩算法。以下是一些常见的压缩算法及其对应的命令选项：

1. **gzip** (`.tar.gz` 或 `.tgz`)：

	- 打包并压缩：
        ```sh
		tar cvzf archive.tar.gz directory/
		```    
    - 解压缩并解包：
	    ```sh
		tar xvzf archive.tar.gz
		```     
2. **bzip2** (`.tar.bz2` 或 `.tbz` 或 `.tbz2`)：
    
    - 打包并压缩：  
		```sh
		tar cvjf archive.tar.bz2 directory/
		```
    - 解压缩并解包：
	    ```sh
	    tar xvjf archive.tar.bz2
		```
1. **xz** (`.tar.xz` 或 `.txz`)：
    
    - 打包并压缩：
        ```sh
	    tar cvJf archive.tar.xz directory/
		```  
    - 解压缩并解包：
        ```sh
        tar xvJf zrchive.tar.xz
		```
4. **compress** (`.tar.Z` 或 `.taz`)：
    
    - 打包并压缩：
		```sh
		tar cvZf archive.tar.Z directory/
		```  
    - 解压缩并解包：
        ```sh
	    tar xvZf archive.tar.Z
		```  
1. **lzma** (`.tar.lzma`):
    
    - 打包并压缩：
		```sh
		tar cv --lzma -f archive.tar.lzma directory/
		```
    - 解压缩并解包：
        ```sh
        tar xv --lzma -f archive.tar.lzma
		```

这些选项使 `tar` 能够与各种压缩算法一起使用，方便用户根据不同需求选择合适的压缩格式。

-----

## file 

The `file` command checks the type of file and compression format.

syntax: `file filename`

Example:
```sh
$: file archive.tar.lzma
// output
archive.tar.lzma: LZMA compressed data, streamed
```

## xxd

`xxd` is a command line utility that represents the contents of a file in hexadecimal. It can convert a file to a hexadecimal representation or convert the hexadecimal representation back to the original file.


## hexdump


## Other
> Linux, macOS may be not supported.

get memory page size for current Os: `getconf PAGESIZE` 