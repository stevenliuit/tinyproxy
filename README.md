# Tinyproxy 多平台构建仓库

![构建状态](https://github.com/stevenliuit/tinyproxy/actions/workflows/build.yml/badge.svg)

本仓库提供 Tinyproxy 的多平台、多架构 CI 构建。

## 项目简介

[Tinyproxy](https://github.com/tinyproxy/tinyproxy) 是一个轻量级的 HTTP/SSL 代理守护进程，适用于小型网络环境。

## 支持的平台和架构

| 平台 | 架构 | 状态 |
|------|------|------|
| Linux | x86_64 | ✅ |
| Linux | ARM64 | ✅ |
| Linux | ARMv7l | ✅ |
| Windows | x86_64 | ✅ |
| macOS | ARM64 (Apple Silicon) | ✅ |
| macOS | x86_64 | ✅ |

## 构建产物

每次 push 或 pull request 都会自动构建，并生成以下压缩包：

- `tinyproxy-linux-x86_64.tar.gz`
- `tinyproxy-linux-arm64.tar.gz`
- `tinyproxy-linux-armv7l.tar.gz`
- `tinyproxy-windows-x86_64.zip`
- `tinyproxy-macos-arm64.tar.gz`
- `tinyproxy-macos-x86_64.tar.gz`

## 使用方法

### 下载并解压

```bash
# Linux/macOS
tar -xzf tinyproxy-{arch}-linux.tar.gz
tar -xzf tinyproxy-{arch}-macos.tar.gz

# Windows
Expand-Archive tinyproxy-windows-x86_64.zip
```

### 运行

```bash
# Linux/macOS
./tinyproxy-{version}/bin/tinyproxy

# Windows
.\tinyproxy.exe
```

### 配置

默认配置文件位置：
- Linux/macOS: `/etc/tinyproxy/tinyproxy.conf`
- Windows: `etc/tinyproxy/tinyproxy.conf`

## 手动触发构建

1. 进入 [Actions](https://github.com/stevenliuit/tinyproxy/actions) 页面
2. 选择 "构建 Tinyproxy" 工作流
3. 点击 "Run workflow"
4. 可选择指定版本标签

## 本地构建

### Linux/macOS

```bash
git clone https://github.com/stevenliuit/tinyproxy.git
cd tinyproxy
./autogen.sh
./configure
make
sudo make install
tinyproxy --version
```

### Windows

```bash
git clone https://github.com/stevenliuit/tinyproxy.git
cd tinyproxy
./autogen.sh
./configure --prefix=/c/tinyproxy
make
make install
./src/tinyproxy.exe --version
```

## 构建选项

Tinyproxy 支持以下可选编译参数：

| 参数 | 说明 |
|------|------|
| `--enable-debug` | 启用完整调试支持 |
| `--enable-xtinyproxy` | 编译 XTinyproxy 头支持 |
| `--enable-filter` | 启用域名/URL 过滤功能 |
| `--enable-upstream` | 启用上游代理支持 |
| `--enable-transparent` | 启用透明代理模式 |
| `--enable-reverse` | 启用反向代理模式 |
| `--with-stathost=HOST` | 设置统计主机名 |

## 许可证

Tinyproxy 使用 GNU GPLv2 许可证。

## 资源链接

- [上游仓库](https://github.com/tinyproxy/tinyproxy)
- [官方文档](https://tinyproxy.github.io/)
- [问题反馈](https://github.com/tinyproxy/tinyproxy/issues)
- IRC: `#tinyproxy` on libera.chat