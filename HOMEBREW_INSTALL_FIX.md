# Homebrew 安装网络问题解决方案

## 问题：安装 Homebrew 时出现 Git 网络错误

```
error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly
fatal: early EOF
```

## 解决方案

### 方案 1：使用国内镜像源（推荐，最快）

**使用中科大镜像：**

```bash
# 设置 Git 使用 HTTP/1.1
git config --global http.version HTTP/1.1

# 使用中科大镜像安装 Homebrew
/bin/bash -c "$(curl -fsSL https://gitee.com/ineo6/homebrew-install/raw/master/install.sh)"
```

**或使用清华大学镜像：**

```bash
# 设置环境变量使用清华镜像
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"

# 安装
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 方案 2：配置 Git 后重试

```bash
# 增加 Git 缓冲区
git config --global http.postBuffer 524288000
git config --global http.maxRequestBuffer 100M
git config --global http.version HTTP/1.1

# 重新运行安装脚本
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 方案 3：手动安装（如果自动安装一直失败）

```bash
# 1. 创建 Homebrew 目录
sudo mkdir -p /opt/homebrew
sudo chown -R $(whoami) /opt/homebrew

# 2. 克隆 Homebrew（使用浅克隆）
git clone --depth 1 https://github.com/Homebrew/brew.git /opt/homebrew

# 3. 添加到 PATH
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
eval "$(/opt/homebrew/bin/brew shellenv)"

# 4. 安装 Homebrew Core
mkdir -p /opt/homebrew/Library/Taps/homebrew/homebrew-core
git clone --depth 1 https://github.com/Homebrew/homebrew-core.git /opt/homebrew/Library/Taps/homebrew/homebrew-core
```

### 方案 4：使用代理（如果有代理）

```bash
# 设置代理
export http_proxy=http://proxy.example.com:8080
export https_proxy=http://proxy.example.com:8080

# 安装
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 方案 5：跳过 Homebrew，直接下载软件（最简单）

**如果只是想运行 SuperSonic 项目，不需要安装 Homebrew：**

1. **Docker Desktop**：直接访问 https://www.docker.com/products/docker-desktop 下载
2. **Maven**：直接访问 https://maven.apache.org/download.cgi 下载
3. **Node.js**：直接访问 https://nodejs.org/ 下载

## 安装后配置（如果使用国内镜像）

如果使用国内镜像安装，建议配置 Homebrew 使用国内源加速：

```bash
# 替换 Homebrew 源为中科大镜像
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

# 更新
brew update
```

## 验证安装

```bash
# 检查 Homebrew 是否安装成功
brew --version

# 测试安装一个软件
brew install wget
```

## 推荐方案

**对于中国大陆用户：**
- 优先使用**方案 1（中科大镜像）**，速度最快且稳定

**如果网络环境较好：**
- 使用**方案 2（配置 Git 后重试）**

**如果只是想运行项目：**
- 使用**方案 5（跳过 Homebrew）**，直接下载需要的软件

