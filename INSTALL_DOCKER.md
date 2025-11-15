# Docker 安装指南

## 问题：`brew install --cask docker` 很慢或卡住

如果通过 Homebrew 安装 Docker 很慢，**直接下载安装更快更稳定**。

## 推荐方法：直接下载 Docker Desktop

### 步骤 1：确定你的 Mac 芯片类型

```bash
# 检查芯片类型
uname -m
# 如果显示 arm64，是 Apple Silicon (M1/M2/M3)
# 如果显示 x86_64，是 Intel 芯片
```

### 步骤 2：下载 Docker Desktop

**方法 A：通过官网下载（推荐）**
1. 访问：https://www.docker.com/products/docker-desktop
2. 点击 "Download for Mac"
3. 网站会自动识别你的芯片类型

**方法 B：直接下载链接**
- **Apple Silicon (M1/M2/M3)**：
  https://desktop.docker.com/mac/main/arm64/Docker.dmg

- **Intel 芯片**：
  https://desktop.docker.com/mac/main/amd64/Docker.dmg

### 步骤 3：安装

1. 双击下载的 `Docker.dmg` 文件
2. 将 Docker 图标拖到 Applications 文件夹
3. 打开 Applications 文件夹
4. 双击 Docker 图标启动
5. 等待 Docker 启动完成（菜单栏会显示 Docker 图标）
   - 首次启动可能需要几分钟
   - 可能需要输入管理员密码

### 步骤 4：验证安装

打开终端，运行：

```bash
docker --version
docker compose version
```

如果显示版本号，说明安装成功！

### 步骤 5：运行 SuperSonic

```bash
cd docker
docker compose up -d
```

然后在浏览器访问：http://localhost:9080

## 如果 Homebrew 安装卡住

如果 `brew install --cask docker` 卡住了：

1. **按 Ctrl+C 取消**
2. **使用上面的直接下载方法**
3. 或者尝试：
   ```bash
   # 清理 Homebrew 缓存
   brew cleanup
   
   # 重新安装
   brew install --cask docker
   ```

## 常见问题

### Docker 启动很慢
- 首次启动需要初始化，可能需要 2-5 分钟
- 确保有足够的磁盘空间（至少 4GB）

### 菜单栏没有 Docker 图标
- 检查 Applications 文件夹中是否有 Docker
- 尝试重新启动 Docker Desktop

### 权限问题
- 安装和首次启动可能需要输入管理员密码
- 确保有管理员权限

## 卸载 Docker（如果需要）

```bash
# 如果通过 Homebrew 安装
brew uninstall --cask docker

# 如果直接下载安装
# 1. 退出 Docker Desktop
# 2. 删除 Applications/Docker.app
# 3. 删除 ~/.docker 目录（可选）
```

