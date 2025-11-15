# SuperSonic 快速启动指南

## 方式一：Docker 运行（推荐，最快）

### 安装 Docker（如果还没安装）

**macOS 安装方式（二选一）：**

**方式 A：直接下载安装（推荐，无需 Homebrew，最快）**

如果 `brew install --cask docker` 很慢或卡住，直接下载更快：

1. **访问 Docker 官网**：https://www.docker.com/products/docker-desktop
   - 或者直接下载链接：
     - **Apple Silicon (M1/M2/M3)**：https://desktop.docker.com/mac/main/arm64/Docker.dmg
     - **Intel 芯片**：https://desktop.docker.com/mac/main/amd64/Docker.dmg

2. **安装步骤**：
   - 双击下载的 `.dmg` 文件
   - 将 Docker 图标拖到 Applications 文件夹
   - 打开 Applications 文件夹，双击 Docker 图标启动
   - 等待 Docker 启动完成（菜单栏会显示 Docker 图标，可能需要几分钟）

3. **验证安装**：
   ```bash
   docker --version
   ```

**方式 B：使用 Homebrew 安装（需要先安装 Homebrew）**
```bash
# 如果还没有 Homebrew，先安装它：
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 然后安装 Docker
brew install --cask docker
```

安装完成后，确保 Docker Desktop 应用正在运行（菜单栏有 Docker 图标）。

### 运行项目

**注意**：新版本的 Docker 使用 `docker compose`（带空格），而不是 `docker-compose`（带横线）

```bash
cd docker

# 尝试使用新版本命令（推荐）
docker compose up -d

# 如果上面命令失败，尝试旧版本命令
docker-compose up -d
```

然后在浏览器访问：http://localhost:9080

停止服务：
```bash
cd docker
docker compose down
# 或
docker-compose down
```

## 方式二：使用预构建包（需要先下载）

1. 从 [GitHub Releases](https://github.com/tencentmusic/supersonic/releases) 下载最新的预构建包
2. 解压后运行：
```bash
cd assembly/bin
sh supersonic-daemon.sh start
```
3. 访问 http://localhost:9080

## 方式三：本地构建运行

### 前置要求
- Java 21+
- Maven 3.6+
- Node.js 18+ 和 pnpm（用于前端构建）

### 构建步骤

1. **构建后端和前端**
```bash
cd assembly/bin
sh supersonic-build.sh standalone
```

2. **启动服务**
```bash
cd assembly/bin
sh supersonic-daemon.sh start
```

3. **访问应用**
打开浏览器访问：http://localhost:9080

### 停止服务
```bash
cd assembly/bin
sh supersonic-daemon.sh stop
```

### 重启服务
```bash
cd assembly/bin
sh supersonic-daemon.sh restart
```

## 方式四：直接使用 Maven 运行（快速测试，无需完整构建）

如果你已经安装了 Maven，可以直接运行后端服务（使用内置的 H2 数据库）：

1. **安装 Maven**（如果还没安装）：

**方式 A：直接下载（无需 Homebrew）**
- 访问：https://maven.apache.org/download.cgi
- 下载 `apache-maven-*.tar.gz`
- 解压并配置环境变量

**方式 B：使用 Homebrew（需要先安装 Homebrew）**
```bash
# 如果还没有 Homebrew，先安装它：
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 然后安装 Maven
brew install maven
```

2. **直接运行后端服务**：
```bash
cd launchers/standalone
mvn spring-boot:run
```

服务启动后访问：http://localhost:9080

**注意**：这种方式只运行后端，前端需要单独构建或使用开发模式。

## 开发模式运行（前端热更新）

如果你想修改前端代码并实时看到效果：

1. **启动后端服务**（使用Maven直接运行）：
```bash
cd launchers/standalone
mvn spring-boot:run
```

2. **启动前端开发服务器**（新开一个终端）：
```bash
cd webapp
sh start-fe-dev.sh
```

前端开发服务器通常运行在 http://localhost:8000

## 常见问题

### 问题：找不到 supersonic-daemon.sh
**解决**：脚本在 `assembly/bin/` 目录下，需要使用完整路径：
```bash
sh assembly/bin/supersonic-daemon.sh start
```

### 问题：端口 9080 被占用
**解决**：修改 `launchers/standalone/src/main/resources/application.yaml` 中的端口号

### 问题：Java 版本不匹配
**解决**：项目需要 Java 21，检查版本：
```bash
java -version
```

### 问题：docker-compose 命令找不到
**解决**：新版本的 Docker Desktop 使用 `docker compose`（带空格）而不是 `docker-compose`（带横线）。尝试：
```bash
docker compose up -d
```

如果还是不行，可能需要安装 Docker Compose：
```bash
# macOS
brew install docker-compose
```

### 问题：Maven 命令找不到
**解决**：安装 Maven：

**方式 A：使用 Homebrew（如果已安装）**
```bash
brew install maven
```

**方式 B：手动安装（无需 Homebrew）**
1. 访问 https://maven.apache.org/download.cgi
2. 下载 `apache-maven-*.tar.gz`
3. 解压到 `/usr/local/` 或 `~/` 目录
4. 添加到 PATH：
```bash
export PATH=$PATH:/usr/local/apache-maven-*/bin
# 或
export PATH=$PATH:~/apache-maven-*/bin
```

验证安装：
```bash
mvn -version
```

### 问题：Homebrew 命令找不到
**解决**：Homebrew 是 macOS 的包管理器，可选安装。如果不想安装 Homebrew，可以：
- 直接下载 Docker Desktop：https://www.docker.com/products/docker-desktop
- 直接下载 Maven：https://maven.apache.org/download.cgi
- 或使用预构建包（方式二），无需安装任何工具

