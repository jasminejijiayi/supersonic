# Git 网络问题解决方案

## 问题：Git 克隆/拉取时出现网络错误

### 错误示例
```
error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

## 解决方案

### 方案 1：使用浅克隆（推荐，最快）

只克隆最新的代码，不包含完整历史：

```bash
git clone --depth 1 https://github.com/tencentmusic/supersonic.git
```

如果需要完整历史，克隆后可以：
```bash
cd supersonic
git fetch --unshallow
```

### 方案 2：禁用 HTTP/2

Git 使用 HTTP/1.1 而不是 HTTP/2：

```bash
git config --global http.version HTTP/1.1
git clone https://github.com/tencentmusic/supersonic.git
```

### 方案 3：增加缓冲区大小

```bash
git config --global http.postBuffer 524288000
git config --global http.maxRequestBuffer 100M
git config --global core.compression 0
```

然后重新克隆：
```bash
git clone https://github.com/tencentmusic/supersonic.git
```

### 方案 4：使用 SSH 协议（如果已配置 SSH 密钥）

```bash
git clone git@github.com:tencentmusic/supersonic.git
```

### 方案 5：分步克隆（如果网络非常不稳定）

```bash
# 先创建空仓库
git init supersonic
cd supersonic

# 添加远程仓库
git remote add origin https://github.com/tencentmusic/supersonic.git

# 只拉取主分支
git fetch origin main
git checkout main
```

### 方案 6：使用代理（如果有代理）

```bash
# 设置 HTTP 代理
git config --global http.proxy http://proxy.example.com:8080
git config --global https.proxy https://proxy.example.com:8080

# 克隆
git clone https://github.com/tencentmusic/supersonic.git

# 使用完后可以取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### 方案 7：直接下载 ZIP 文件（最简单）

如果 Git 一直有问题，可以直接下载源码：

1. 访问：https://github.com/tencentmusic/supersonic
2. 点击绿色的 "Code" 按钮
3. 选择 "Download ZIP"
4. 解压后即可使用

## 如果是在已有仓库中拉取代码

```bash
# 方法 1：增加缓冲区后重试
git config http.postBuffer 524288000
git pull

# 方法 2：使用浅拉取
git fetch --depth 1
git merge origin/main

# 方法 3：重置并重新拉取
git fetch origin
git reset --hard origin/main
```

## 检查网络连接

```bash
# 测试 GitHub 连接
ping github.com

# 测试 HTTPS 连接
curl -I https://github.com
```

## 临时解决方案

如果只是需要运行项目，不需要完整 Git 历史，建议：
1. **直接下载 ZIP 文件**（最简单）
2. 或使用 **浅克隆**（`git clone --depth 1`）

