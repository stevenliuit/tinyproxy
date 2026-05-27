# Tinyproxy 多平台构建仓库

![构建状态](https://github.com/stevenliuit/tinyproxy/actions/workflows/build.yml/badge.svg)

本仓库提供 Tinyproxy 的多平台构建产物 CI 流水线。

## 项目介绍

[Tinyproxy](https://github.com/tinyproxy/tinyproxy) 是一个轻量级的 HTTP/SSL 代理服务器，适合小型网络使用。

## 支持的平台和架构

| 平台 | 架构 | 文件格式 |
|------|------|----------|
| Linux | x86_64 | .tar.gz |
| Linux | ARM64 | .tar.gz |
| Linux | ARMv7l | .tar.gz |
| macOS | ARM64 (Apple Silicon) | .tar.gz |

## 下载构建产物

请到 [Releases](https://github.com/stevenliuit/tinyproxy/releases) 页面下载对应平台的压缩包。

## 使用方法

### Linux/macOS

#### 1. 解压

```bash
tar -xzf tinyproxy-linux-x86_64-1.11.3.tar.gz
cd tinyproxy-bin
```

#### 2. 配置

编辑 `etc/tinyproxy/tinyproxy.conf` 配置文件：

```bash
# 修改监听端口 (默认: 8888)
Port 8888

# 修改允许访问的IP段 (默认只允许本机)
Allow 127.0.0.1
Allow 192.168.1.0/24

# 设置上游代理 (如果需要)
# upstream proxy.example.com:8080

# 配置日志级别
LogLevel Info
```

#### 3. 启动

```bash
# 前台运行 (调试模式)
./tinyproxy -d -c etc/tinyproxy/tinyproxy.conf

# 后台运行
nohup ./tinyproxy -d -c etc/tinyproxy/tinyproxy.conf &

# 检查运行状态
ps aux | grep tinyproxy
netstat -tlnp | grep 8888
```

#### 4. 停止

```bash
# 查找进程
ps aux | grep tinyproxy

# 杀掉进程
kill <PID>
```

## systemd 服务 (Linux)

创建服务文件 `/etc/systemd/system/tinyproxy.service`:

```ini
[Unit]
Description=Tinyproxy HTTP Proxy Server
After=network.target

[Service]
Type=forking
ExecStart=/path/to/tinyproxy-bin/tinyproxy -d -c /path/to/tinyproxy-bin/etc/tinyproxy/tinyproxy.conf
PIDFile=/var/run/tinyproxy.pid

[Install]
WantedBy=multi-user.target
```

管理服务:

```bash
# 重新加载systemd配置
sudo systemctl daemon-reload

# 启动服务
sudo systemctl start tinyproxy

# 开机自启
sudo systemctl enable tinyproxy

# 查看状态
sudo systemctl status tinyproxy

# 停止服务
sudo systemctl stop tinyproxy
```

## 常用配置选项

| 配置项 | 说明 | 默认值 |
|--------|------|--------|
| `Port` | 监听端口 | 8888 |
| `Listen` | 监听地址 | 0.0.0.0 |
| `Allow` | 允许访问的IP/段 | 127.0.0.1 |
| `LogLevel` | 日志级别 (Critical, Error, Warning, Notice, Connect, Info) | Notice |
| `LogFile` | 日志文件路径 | /var/log/tinyproxy/tinyproxy.log |
| `Timeout` | 请求超时秒数 | 60 |
| `Upstream` | 上游代理配置 | 无 |

## 配置示例

### 基本代理 (仅本机访问)

```
Port 8888
Listen 127.0.0.1
LogLevel Info
```

### 局域网访问

```
Port 8888
Listen 0.0.0.0
Allow 192.168.1.0/24
Allow 10.0.0.0/8
LogLevel Info
```

### 使用上游代理

```
Port 8888
Listen 0.0.0.0
Allow 0.0.0.0/0
LogLevel Info

Upstream proxy.example.com:8080
```

## 手动触发构建

1. 进入 [Actions](https://github.com/stevenliuit/tinyproxy/actions) 页面
2. 选择 "构建 Tinyproxy" 工作流
3. 点击 "Run workflow"
4. 输入上游 tag 版本（如 `1.11.3`，留空则使用最新版本）

## 命令行参数

| 参数 | 说明 |
|------|------|
| `-c file` | 指定配置文件路径 |
| `-d` | 守护进程模式 (后台运行) |
| `-f file` | 同 `-c` |
| `-h` | 显示帮助信息 |
| `-v` | 显示版本信息 |

## 验证代理是否正常工作

```bash
# 设置代理
export http_proxy="http://127.0.0.1:8888"
export https_proxy="http://127.0.0.1:8888"

# 测试访问
curl -I https://www.google.com

# 取消代理
unset http_proxy https_proxy
```

## 构建说明

### 从源码构建 (Linux)

```bash
git clone https://github.com/stevenliuit/tinyproxy.git
cd tinyproxy
./autogen.sh
./configure --prefix=/usr/local
make
sudo make install
tinyproxy --version
```

### 从源码构建 (macOS)

```bash
brew install autoconf automake libtool pkg-config glib autoconf-archive help2man
git clone https://github.com/stevenliuit/tinyproxy.git
cd tinyproxy
./autogen.sh
./configure --prefix=/usr/local
make
sudo make install
tinyproxy --version
```

## 许可证

Tinyproxy 使用 GNU GPLv2 许可证。

## 相关链接

- [上游源码仓库](https://github.com/tinyproxy/tinyproxy)
- [官方文档](https://tinyproxy.github.io/)
- [问题反馈](https://github.com/tinyproxy/tinyproxy/issues)