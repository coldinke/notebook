# 常用 Linux 文件相关工具

!!! abstract
    It should  work in Unix-like system.

## tar

The 'tar' command used to create and manipulate archive files. It stands for "tape archive" and is commonly used to combine multiple files and directories into a single archive file.

syntax: `tar [options] target_file`

Common options:
- `-c`: Create a new archive.
- `-x`: Extract files from an archive.
- `-v`: Verbose mode
- `-f`: Specify the archive file name

Example:
```sh
$: tar -xvf archiver.tar
# Extracting a tar archive.
```

## file 

The `file` command checks the type of file and compression format.

syntax: `file filename`

Example:
```sh
$: file archive.tar.lzma

archive.tar.lzma: LZMA compressed data, streamed
```

## xxd

`xxd` is a command line utility that represents the contents of a file in hexadecimal. It can convert a file to a hexadecimal representation or convert the hexadecimal representation back to the original file.

syntax: `xxd [options] filename`

Example:
```sh
$: xxd -l 32 file.bin

# Display first 32 bytes of file.bin in hex format.
```

## hexdump

The `heaxdump` command is a utility that displays the contents of a file in a hexadecimal format. 

syntax: `hexdump [options] file`

Example:
```sh
$: hexdump -C -n 64 file.bin

# Display first 64 bytes of file.bin in hex and ASCII format.
```

## strings

The `strings` command prints the printable character sequences that are at least 4 characters long in files.

syntax: `strings [options] file`

Example:
```sh
$: strings file.bin

# Print all printable strings in file.bin
```

## Other
> Linux, macOS may be not supported.

get memory page size for current Os: `getconf PAGESIZE` 