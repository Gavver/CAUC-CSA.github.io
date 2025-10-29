# Docker超绝入门指南

---

最后更新于 2025-10-28 by ARC

!!! note "嘿！欢迎阅读本指南！"
    本指南旨在让你理解Docker！什么？你问我什么是docker？能吃吗？没关系，我会争取让你在阅读完这个指南之后就能手搓或者部署属于自己的容器的！

首先，请看这里，这是鲸鱼，它很可爱（docker的吉祥物）

![Docker喵](https://www.docker.com/app/uploads/external/logos/logo-docker-square.svg)

## Docker是什么？

Docker 是一个开源的应用容器引擎，你可以基于此 **构建、打包、运行和部署** 你的应用。
它就像一个轻量级的虚拟机，但比虚拟机更高效。Docker将应用程序及其所有依赖项（比如镜像、库、配置文件等）打包到一个独立的、可移植的单元中，这个单元我们称之为"容器"。这使得应用程序在不同的环境中运行起来都保持一致，无论是笔记本电脑、服务器还是生产环境，它都不会出问题的！（一般情况下来说）

!!! tip "简单来说"
    Docker 解决了"我的电脑上能跑起来，你的电脑就不行"的问题。

为了更好地使用 Docker，我们需要了解几个核心概念：

### 镜像 ( **Image** )

 **定义** ：一个 **只读** 的模板，包含了运行代码所需的所有文件、依赖、配置和指令。把它当做程序的"蓝图"或"模具"就好。

 **例子** ：一个包含 Ubuntu 操作系统、Python 解释器和你的 Python 应用程序代码的镜像。

### 容器 ( **Container** )

 **定义** ：镜像的一个运行实例。当镜像被执行时，它就变成了一个容器。你可以把容器想象成一个活动的、可交互的应用程序实例。

 **特性** ：容器是可写的，它在其镜像的基础上增加了一个读写层。容器之间互相隔离，拥有自己的文件系统、网络接口和进程空间。

 **例子** ：基于上面 Python 应用程序镜像运行起来的一个实际的 Python 应用进程。

### 仓库 ( **Registry** )

 **定义** ：用于存储和分发 Docker 镜像的地方，类似于 GitHub 的私有仓库

 **特性** ：国外最常用的公共仓库是 Docker Hub（超绝被墙中），也可以借助Github搭建私有仓库。

 **作用** ：方便镜像的共享和版本管理

 **三者关系** ：镜像好比一个程序的安装包，而容器就是这个安装包安装后运行起来的程序。仓库则是存放这些安装包的商店。

!!! quote "形象比喻"
    如果说GitHub的代码是一块汉堡肉，那么Image就是一个"加了调料生菜装到盒子里的预制堡"，Container就是烤好的汉堡，Registry就是西米(非diss，这里泛指卖预制菜的商家)

说完定义，我们该聊聊他的技术实现...了？

## Docker是如何工作的？（技术实现）

Docker 看起来很简单，但它的背后有强大且精巧的技术加持。主要有以下几个核心：

### 1. Namespace（命名空间）

Namespace 是 Linux 内核提供的一个特性，它是实现容器隔离的关键技术。想象一下，Namespace 给每个容器都分了一套"房子"，容器的进程、网络、文件系统等都在这套房子里，不同"房子"之间互不干扰，实现了进程隔离、网络隔离、文件系统隔离等等。好比你做梦梦到你在宿舍里和室友各住各的房间，谁也听不到谁半夜偷偷打游戏（理论上）。

### 2. Cgroups（控制组）

如果说 Namespace 负责隔离，那么 Cgroups 负责的就是 **资源限制** 。它可以限制和控制容器对CPU、内存、磁盘 I/O 等系统资源的使用。举个例子，就像你航工作日11点断电。这样可以避免某个容器"吃光"服务器的所有资源，导致其他容器"饿死"或是服务器被榨干导致延迟上天(?)。

### 3. Union File System（联合文件系统）

这是 Docker 实现分层镜像的关键技术。它可以将多个目录（通常是只读层和读写层）"叠加"在一起，形成一个看似单一的文件系统。这样镜像可以被分层存储，相同的基础层在第100个容器里也只存储一次，极大地节省了空间。就像一个带透明玻璃的展示柜，你可以看到底层、中层、顶层，但看起来就是一个整体。

### 4. Docker vs 虚拟机：性能之争

那你可能会问我了：Docker 和 VM（虚拟机）有什么区别？

| 特性 | Docker 容器 | 虚拟机 |
|------|------------|--------|
| 启动速度 | 秒级 | 分钟级 |
| 资源占用 | 极小（几MB甚至几十KB!） | 大（几GB起步） |
| 性能 | 接近原生 | 有损耗 |
| 隔离性 | 进程级 | 操作系统级 |

!!! note "简单来说"
    VM 是"房子套房子"（虚拟一套完整操作系统），而 Docker 是"房间隔房间"（共享同一个操作系统内核）。这就是为什么 Docker 轻量、快速，但隔离性相对较弱的原因（是了，没错，这里可以牵扯到一个及其强烈的攻击方式：容器逃逸，当然，这里不讲，有兴趣自己搜哈~）。

---

## 开始使用 Docker

好，理论知识讲完了（希望你已经吸收了！）。现在我们来实战一下！

### 第一步：安装 Docker

#### Windows 用户看这里

1. 打开浏览器，访问 [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/){target="_blank"}
2. 下载 Docker Desktop 安装程序
3. 一路"下一步"，安装完成后重启电脑（这是必须的！）
4. 双击桌面上的鲸鱼图标🐳，启动 Docker Desktop
5. 等待系统托盘中的 Docker 图标稳定（不要有一直在转动的小圈）

!!! warning "Docker无法启动？很可能是虚拟化功能没有开启！"
    如果启动 Docker Desktop 后一直卡住或报错，通常是因为 **Windows 的虚拟化功能未开启** 。解决方法如下：

??? note "方法一：开启 Hyper-V（适用于 Windows 10 Pro/Enterprise/Education 版本）"
    1. 打开"控制面板" → "程序" → "启用或关闭 Windows 功能"
    2. 勾选 **"Hyper-V"** 和 **"虚拟机平台"** 
    3. 点击确定，等待安装完成后 **重启电脑** 

??? note "方法二：开启 WSL 2（适用于 Windows 10 所有版本）"
    1. 以管理员身份打开 PowerShell（右键点击 PowerShell，选择"以管理员身份运行"）
    2. 输入以下命令：
    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```
    3. **重启电脑** 后，继续安装 WSL 2 内核：
       - 访问：[WSL2 Linux Kernel Update Package](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi){target="_blank"}
       - 下载并安装
    4. 设置 WSL 2 为默认版本：
    ```powershell
    wsl --set-default-version 2
    ```

??? note "方法三：在 BIOS 中开启虚拟化（如果上述方法无效）"
    1. **重启电脑** ，进入 BIOS 设置（通常是开机按 F2、F10、F12 或 Delete 键，具体看你的主板说明）
    2. 找到 "Virtualization Technology" 或 "Intel VT-x" / "AMD-V" 选项
    3. 将其设置为 **Enable** 
    4. 保存并退出 BIOS

完成后，再次启动 Docker Desktop 应该就能正常运行了！

#### 验证安装

打开 PowerShell 或 CMD，输入：
```bash
docker --version
```
如果显示出版本号，恭喜！安装成功！

---

### 第二步：配置国内镜像源（强烈推荐！）

#### 为什么要配置镜像源？

!!! note "配置镜像源的原因"
    由于某些众所周知的原因，Docker Hub 在国内访问可能非常慢，甚至直接超时。配置国内镜像源可以大幅提升拉取镜像的速度，让你告别"拉个镜像等到地老天荒"的痛苦！（虽然登录Docker以及上传镜像仍然难的要命就是了）（什么？你问我怎么上传？先研究一下科学上网吧！）

#### Docker Desktop 配置方法

1. **打开 Docker Desktop** 
   - 右键点击系统托盘中的 Docker 图标🐳
   - 选择 **Settings** （设置）（那个小齿轮）或 **Preferences** （首选项）（部分老版本是这样的）

2. **进入镜像源配置** 
   - 点击左侧菜单的 **Docker Engine** 
   - 你会看到一个 JSON 配置文件编辑器

3. **添加镜像源配置** 
   - 在 JSON 配置中添加 `registry-mirrors` 字段
   - 完整的配置应该类似这样：

   ```json
   {
     "builder": {
       "gc": {
         "defaultKeepStorage": "20GB",
         "enabled": true
       }
     },
     "experimental": false,
     "registry-mirrors": [
       "https://docker.mirrors.ustc.edu.cn",
       "https://hub-mirror.c.163.com",
       "https://mirror.ccs.tencentyun.com",
       "https://docker.xuanyuan.me"
     ]
   }
   ```
   
   !!! note "注意"
       如果配置文件中已经有其他字段，只需要添加或修改 `registry-mirrors` 部分即可。

4. **点击 Apply & Restart** 
   - 配置会自动保存，Docker Desktop 会自动重启以应用新配置

5. **验证配置是否生效** 
   ```bash
   docker info
   ```
   查看输出中的 `Registry Mirrors:` 部分，应该能看到你配置的镜像源地址。

#### 常用国内镜像源（以下为2024-2025年实测可用的镜像源）

=== "中科大镜像"
    `https://docker.mirrors.ustc.edu.cn`
    
    推荐！速度稳定，维护良好，教育网友好

=== "网易镜像"
    `https://hub-mirror.c.163.com`
    
    老牌镜像源，速度一般但稳定可用

=== "腾讯云镜像"
    `https://mirror.ccs.tencentyun.com`
    
    云服务商提供，速度快，推荐作为备选

=== "轩辕镜像"
    `https://docker.xuanyuan.me`
    
    轩辕镜像源，看运气可用

=== "阿里云镜像"
    需要登录获取专属加速地址
    
    - 访问 [阿里云容器镜像服务](https://cr.console.aliyun.com/){target="_blank"} → 镜像加速器
    - 登录后获取你的专属地址（格式类似：`https://xxxxxx.mirror.aliyuncs.com`）
    - 速度快但需要注册账号

!!! tip "使用建议"
    - 可以配置多个镜像源，Docker 会按顺序尝试（第一个失败自动尝试下一个）
    - 建议把稳定的源（中科大、腾讯云）放在前面，不太稳定的放后面
    - 如果某个镜像源经常超时，可以在配置中将其移除或调后
    - 不同地区对不同镜像源的速度可能不同，建议根据实际情况调整顺序

配置完成后，再拉取镜像就会快如闪电了！⚡

---

### 第三步：最常用的 Docker 命令（你必须掌握的！）

#### 基础命令

 **`docker pull <镜像名>`** - 拉取镜像
```bash
docker pull ubuntu:latest
```
就像从商店里买个标准模板回来。

 **`docker images`** - 查看本地所有镜像
```bash
docker images
```
看看你的"仓库"里都有什么。

 **`docker run <选项> <镜像名>`** - 创建并运行容器
```bash
docker run -it ubuntu /bin/bash
```
`-i` 表示交互式，`-t` 表示分配一个伪终端。这会让你进入容器的命令行！

 **`docker ps`** - 查看正在运行的容器
```bash
docker ps   #你看到的一大串字段中前面那一串带字母带数字的就是容器的ID
```
如果加上 `-a`，能看到所有容器（包括已停止的）：
```bash
docker ps -a
```

 **`docker stop <容器ID>`** - 停止容器
```bash
docker stop arc1337 
```

 **`docker start <容器ID>`** - 启动已停止的容器
```bash
docker start arc1337 
```

 **`docker rm <容器ID>`** - 删除容器
```bash
docker rm arc1337 
```
!!! warning "注意"
    删除前要先停止容器！（虽然说不停止也不会出什么大问题就是了）

 **`docker rmi <镜像ID>`** - 删除镜像
```bash
docker rmi ubuntu:latest  #这里的i指的是image
```

 **`docker exec -it <容器ID> <命令>`** - 在运行中的容器里执行命令
```bash
docker exec -it arc1337  /bin/bash
```
这是进入容器"调查"问题（擦屁股）的常用方法。

!!! warning "注意"
    你在容器里面做的修改不会同步到镜像！所以如果是代码层面的问题，老老实实去改源代码！（当然如果你急着用就当我没说）

 **`docker build -t <镜像名> <路径>`** - 构建镜像（从 Dockerfile）
```bash
docker build -t app:latest .
```
`.` 表示当前目录，`-t` 给镜像打个标签方便后续使用。

---

## 实战：创建你的第一个自定义镜像

光用命令不够，我们来创建一个属于自己的容器！

### Dockerfile 是什么？

Dockerfile 是一个文本文件，里面写着一系列指令，告诉 Docker 如何构建你的镜像。就像食谱一样，一步步教 Docker 怎么"做菜"。

 **常用指令：**

- **`FROM <基础镜像>`** - 基于哪个镜像构建（必须第一个写！）
- **`RUN <命令>`** - 在构建时执行命令（比如安装软件）
- **`COPY <源> <目标>`** - 复制文件到镜像里
- **`ADD <源> <目标>`** - 类似 COPY，但支持 URL 和自动解压
- **`WORKDIR <路径>`** - 设置工作目录
- **`EXPOSE <端口>`** - 声明容器监听的端口
- **`CMD ["命令"]`** - 容器启动时执行的默认命令

### 实战案例：部署一个 PHP Web 应用

假设你有一个 `index.php` 文件，你想用它做个容器，那么你的 Dockerfile 可能是这样：

```dockerfile
# 使用官方的 PHP-Apache 镜像作为基础
FROM php:8.2-apache

# 设置工作目录
WORKDIR /var/www/html

# 复制你的 PHP 文件到容器里
COPY index.php /var/www/html/

# 暴露 80 端口（Apache 默认端口）
EXPOSE 80

# 启动 Apache（基础镜像已经配置好了，直接运行）
CMD ["apache2-foreground"]
```

然后在 Dockerfile 所在目录运行：
```bash
docker build -t my-php-app .
docker run -d -p 8080:80 my-php-app
```
`-d` 表示后台运行，`-p 8080:80` 表示把容器的 80 端口映射到主机的 8080 端口。

现在打开浏览器访问 `http://localhost:8080`，你就会打开你创建的 web 容器的对应页面了！当然，容器的故事还在继续...

---

!!! note "以下是更加困难的部分，按需了解哦"

## Docker Compose：管理多个容器的小助手

单打独斗的时代已经过去了！现代应用往往需要多个容器协同工作（比如一个 Web 容器 + 一个数据库容器）。每次都要手动 `docker run` 几十条命令？太麻烦了！

这时候 **Docker Compose** 就登场了。它可以用一个 YAML 文件定义多个容器，一键启动/停止整个应用栈！

### 安装 Docker Compose

好消息：如果你安装了 Docker Desktop for Windows，Docker Compose 已经自动包含了！

验证安装：
```bash
docker-compose --version
```

### Compose 文件示例

创建一个 `docker-compose.yml` 文件：

```yaml
version: '3.8'

services:
  # Web 服务
  web:
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./src:/var/www/html
    depends_on:
      - db
    restart: always

  # 数据库服务
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: yourpassword123
      MYSQL_DATABASE: myapp
    volumes:
      - db_data:/var/lib/mysql
    restart: always

volumes:
  db_data:
```

 **关键字段：** 

- `services` - 定义所有服务（容器）
- `build` - 从 Dockerfile 构建镜像
- `image` - 直接使用镜像
- `ports` - 端口映射
- `volumes` - 数据卷挂载（下面会讲）
- `environment` - 环境变量
- `depends_on` - 依赖关系（web 依赖 db）
- `restart` - 重启策略

### 常用命令

 **`docker-compose up -d`** - 启动所有服务
```bash
docker-compose up -d
```

 **`docker-compose down`** - 停止并删除所有容器
```bash
docker-compose down
```

 **`docker-compose logs`** - 查看日志
```bash
docker-compose logs web
docker-compose logs -f  # -f 持续跟踪
```

 **`docker-compose ps`** - 查看运行状态
```bash
docker-compose ps
```

 **`docker-compose exec <服务名> <命令>`** - 在容器中执行命令
```bash
docker-compose exec web bash
```

---

## 数据卷（Volumes）：数据的持久化

容器一删除，里面的数据就全没了！这可不行，特别是数据库这种需要持久化数据的场景。

 **解决方案：Volumes（数据卷）**

数据卷可以把容器内的数据目录"映射"到主机的某个目录，这样即使容器删除，数据也还在！

### 三种挂载方式

=== "Bind Mount（绑定挂载）"
    直接把主机的目录挂载到容器：
    ```bash
    docker run -v /c/Users/yourname/data:/var/www/html ubuntu
    ```

=== "Volume（命名卷）"
    Docker 管理的卷，保存在 `/var/lib/docker/volumes/`：
    ```bash
    docker run -v my_data:/var/www/html ubuntu
    ```

=== "tmpfs（临时文件系统）"
    只在内存中，重启就消失：
    ```bash
    docker run --tmpfs /tmp ubuntu
    ```

 **在 Docker Compose 中：** 
```yaml
services:
  web:
    volumes:
      - ./src:/var/www/html           # 绑定挂载
      - db_backup:/backup             # 命名卷
```

---

## 网络配置：让容器互相"说话"

默认情况下，所有容器都在同一个默认网络上，可以通过容器名互相访问（感谢 Docker 的内置 DNS！）（虽然这个DNS有时候也会害了不少人就是了...）。

### 自定义网络

```bash
# 创建一个自定义网络
docker network create my_network

# 启动容器时指定网络
docker run -d --name web --network my_network nginx
docker run -d --name db --network my_network mysql
```

 **在 Compose 中：** 
```yaml
services:
  web:
    networks:
      - my_network

networks:
  my_network:
    driver: bridge
```

---

## 实用技巧与小窍门

### 1. 容器资源限制

防止某个容器吃掉所有资源：
```bash
docker run -m 512m --cpus="0.5" ubuntu
```
- `-m` 限制内存
- `--cpus` 限制 CPU 使用率

### 2. 环境变量

传递配置给容器：
```bash
docker run -e DATABASE_URL=mysql://localhost myapp # 这里是传递了一个数据库的地址
```

在 Dockerfile 中定义：
```dockerfile
ENV NODE_ENV=production
```

### 3. 多阶段构建（Multi-stage Build）

减小镜像体积的神器：
```dockerfile
# 第一阶段：构建
FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm run build

# 第二阶段：运行
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
CMD ["node", "dist/index.js"]
```

### 4. .dockerignore 文件

类似 `.gitignore`，构建时忽略某些文件：
```text
node_modules
.git
*.md
.env
```

---

## 常见问题排查

??? note "容器启动后立即退出"
     **原因** ：主进程结束了  
    
     **解决** ：
    ```bash
    docker logs <容器ID>  # 查看日志找原因
    docker run -it <镜像> bash  # 进入容器手动调试
    ```

??? note "端口被占用"
     **错误** ：`bind: address already in use`  
    
     **解决** ：
    ```bash
    # Windows
    netstat -ano | findstr :8080
    # 杀掉占用端口的进程或换个端口
    # 什么？你问我怎么杀？去看Linux或者是Windows命令教程去！
    ```

??? note "容器无法访问外网"
     **排查** ：
    ```bash
    docker exec -it <容器ID> ping alexyun.site # 这里随便引用个网站，可以换的哈
    # 检查 DNS 配置
    docker exec -it <容器ID> cat /etc/resolv.conf
    ```

??? note "数据丢失"
     **预防** ：使用 Volumes！  
    
     **恢复** ：如果你的 Volume 还在，重新挂载即可。

---

## 安全注意事项（重要！）

!!! warning "虽然 Docker 很方便，但安全问题不容忽视"

1. **不要以 root 用户运行** （如果可能）
   ```dockerfile
   RUN useradd -m myuser
   USER myuser
   ```

2. **定期更新基础镜像** 
   ```bash
   docker pull ubuntu:latest  # 拉取最新版本
   ```

3. **不要挂载敏感目录** 
   ```bash
   # 危险！
   docker run -v /:/host ubuntu
   ```

4. **限制容器权限** 
   ```bash
   docker run --read-only ubuntu  # 只读文件系统
   ```

---

## 总结

恭喜你！现在你已经掌握了 Docker 的核心知识：

- ✅ 理解了容器、镜像、仓库的概念
- ✅ 学会了基础命令
- ✅ 能够编写 Dockerfile
- ✅ 掌握了 Docker Compose
- ✅ 了解了数据卷和网络
- ✅ 知道如何排查问题

 **下一步该做什么？** 

1. **多实践！** 尝试手搓一个项目！然后把它扔到docker里面！
2. **学习 Kubernetes** （如果你想搞大规模部署）（当然，关于CTF_WEB的相关docker知识点你是一点都不缺了）
3. **阅读官方文档** [docs.docker.com](https://docs.docker.com){target="_blank"}

---

!!! quote "最后的话"
    Docker 只是一个工具，真正重要的是理解它背后的思想： **标准化、隔离、可移植** 。无论是开发、测试还是部署，容器化都能让你的工作更加高效。
    
    遇到问题？别慌，想一想提问的智慧，去GitHub翻一翻，网上搜一搜，总会找到答案的！
    
    祝你在 Docker 的海洋里乘风破浪！🐳

---

*编写日期：2025-10-28*  
*适用对象：Docker 零基础小白（应该也包括你）*  
*作者：Dawn_ARC*  
*如有疑问，请移步 **互联网** 或向你身边的大佬求救*