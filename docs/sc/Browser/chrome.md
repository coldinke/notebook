# Chromium 和 V8 的编译初体验

## 前提

- 合理的网络环境
    - 使用全局的网络代理可以规避设置 Linux 下的命令行代理以及 Git 代理
- 下载 `depot_tools` 工具（核心）
    - 下载地址 `https://chromium.googlesource.com/chromium/tools/depot_tools.git` 

使用 Git 克隆完成后，将该工具的地址添加到环境变量 `PATH` 中，便于之后的使用。

## 下载 & 编译

### 下载

Chromium/V8 源码下载方式: 
```bash
mkdir analysis && cd analysis
fetch v8
```

如若在 `fetch v8` 下载时中断，之后可使用命令 `client sync` 重新开始。Chromium 的下载方式类似。

### 编译

> 如果想分析具体的 CVE，可以使用下面的方式调整到具体的漏洞版本。
> ```bash
> # 调整到包含具体漏洞的 commit 
> git reset --hard [commit hash with vulnerability]
> # 下拉依赖
> gclinet sync
> ```

编译 V8

```bash
gclient sync

tools/dev/v8gen.py x86.debug

ninja -C out.gn/x64.debug

./out.gn/x64.debug/d8
```

v8 中存在 Gdb 相关的辅助脚本，可以在 `~/.gdbinit` 或其他配置文件添加如下内容即可。
```bash
source /path/to/v8/tools/gdbinit
source /path/to/v8/tools/gdb-v8-support.py
```