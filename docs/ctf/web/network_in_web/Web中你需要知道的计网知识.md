> Author: YoSheep （网络空间安全协会老狗）

### 01 IP 地址 — 是什么、为什么、怎么查

**定义**：IP 地址（Internet Protocol Address）是网络中设备的唯一标识，用于定位设备并实现通信，类似“门牌号”。

**常见类型**

- **IPv4**：32 位，点分十进制，如 192.168.0.1。
- **IPv6**：128 位，冒号分段，如 2001:0db8::1。

**公网IP vs 私网IP**

- **公网 IP**：可被互联网直接访问（ISP 分配），除了内网IP以外的其他IP段。
- **私有 IP**：局域网内部使用，无法在公网直接路由。常见范围：
  - A 类：10.0.0.0/8
  - B 类：172.16.0.0/12
  - C 类：192.168.0.0/16

**如何查看本机 IP**

- Windows: ipconfig
- macOS / Linux: ifconfig 或 ip addr show

### 02 端口（Port）

**定义**：端口用来区分同一台主机上的不同网络服务。IP + 端口 组合（IP:Port）唯一标识一个网络服务实例，例如 192.168.1.100:80。

**端口范围**：端口号范围为0到65535，其中：

- **知名端口（0-1023）**：用于常见协议和服务，较为固定。例如HTTP为80，HTTPS为443，FTP为21。
- **注册端口（1024-49151）**：为特定应用注册，通常由开发者使用。
- **动态/私有端口（49152-65535）**：为临时连接分配，通常由操作系统动态使用。

#### **常见端口和协议**

- **HTTP（80）和HTTPS（443）**：网页浏览服务，HTTP不加密，HTTPS加密。
- **FTP（21）**：文件传输协议，用于文件上传和下载。
- **SSH（22）**：安全外壳协议，远程安全登录。
- **SMTP（25）**：简单邮件传输协议，用于发送邮件。
- **DNS（53）**：域名系统协议，负责将域名解析为IP地址。

### 03 域名（Domain Name）与 DNS

**域名定义**：域名（Domain Name）是互联网中易于记忆的地址，代表一个IP地址，用户通过域名访问网络资源。域名是IP地址的别名，使人类无需记忆复杂的数字串。例如学校的域名：cauc.edu.cn

**结构**：域名由多个层次组成，分级由点号（.）分隔，如www.example.com。

- **顶级域名（TLD）**：域名的最右部分，代表域的类型或所属国家，如.com、.org、.cn、.edu等。
- **二级域名**：紧靠顶级域名左侧部分，通常为注册者的名称，例如example。
- **三级域名**：更靠左侧的部分，通常为服务名，如www(万维网)，也可以自定义其他名称，如blog.example.com。

**DNS（域名解析）**

- 工作流程：浏览器访问域名 → 本地/递归 DNS 服务器 → 根/顶级/权威 DNS → 返回 IP。
- 最终把域名解析成 IP，浏览器用这个 IP 建立连接。

### 04 IP、端口、域名之间的关系

用户访问 https://www.example.com:443 的过程：

1. DNS 将 www.example.com 解析为某个 IP（例如 93.184.216.34）。
2. 浏览器与 93.184.216.34 的端口 443 建立 TCP（并进行 TLS）连接。
3. 请求通过该 IP 与端口到达服务器，服务器处理并返回响应。

### 05 HTTP协议（基础）

**基础概念**：**HTTP（超文本传输协议）**是客户端与服务器之间进行通信的协议，基于请求-响应模型。

**特点**

- 基于请求—响应模型，通常运行在 TCP协议（HTTP/HTTPS）之上。
- **无状态**：每个请求独立，服务器默认不保存请求间状态（因此需要 Cookie / Session 等机制）。

**HTTP 请求结构（示例）**

```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

```http
POST /login HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
Accept: application/json
Content-Type: application/json
Content-Length: 40

{"username":"alice","password":"s3cr3t"}
```

- **请求行**：请求方法（GET/POST/PUT/DELETE 等） + 资源路径（此处为`index.html`） + HTTP 版本（此处为1.1）。

- **请求头**：传递元数据（Host、User-Agent、Cookie、Content-Type 等）。
- **请求体**：仅在需要时包含数据（如 POST 的表单或 JSON），例如上示例中的`{"username":"alice","password":"s3cr3t"}`。

**HTTP 响应结构（示例）**

```http
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1024
Set-Cookie: sessionId=abc123

<HTML>...页面内容...</HTML>
```

- **状态行**：版本 + 状态码（200/404/500 等）。
- **响应头**：元数据（Content-Type、Set-Cookie、Content-Length 等）。
- **响应体**：实际资源（HTML、JSON、图片等）。

**常见状态码分类**

- **2xx**：成功（200 OK）
- **3xx**：重定向（301/302）
- **4xx**：客户端错误（404/403）
- **5xx**：服务器错误（500）

### 06 在 Burp Suite 中分析 HTTP

Burp Suite 是一个常用的中间人代理，用来拦截、修改和重发 HTTP 请求。把浏览器的代理设置为 Burp 后，你可以在它里看到并操作请求/响应的各部分。

下面用访问百度（www.baidu.com）的过程做一个演示：

1. 配置好Burp后，在 Burp 中启用拦截：

   ![image-20251030014725823](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030014725823.png)

2. 在浏览器配置好代理后，访问百度

   ![image-20251030014917708](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030014917708.png)

3. 查看请求（不同版本burp显示可能不同）

   ![image-20251030015034917](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030015034917.png)

   在其中可以看到，我们发起了一个GET请求，访问了www.baidu.com这个域名的根目录

4. 查看返回

   在burp中有两种最常用的方法来查看响应包

   （1）在proxy模块的intercept中：鼠标右键->Do intercept->Response to this request

   ![image-20251030023015093](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030023015093.png)

   获得响应包，但这种方式只能单次查看，只要放掉这个包，无法再查看

   ![image-20251030023234326](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030023234326.png)

   （2）把请求包发送到repeater模块

   ![image-20251030024605849](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030024605849.png)

   然后在repeater模块中，可以多次改数据包查看不同的相应包

   ![image-20251030024645116](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20251030024645116.png)

### 07 小总结

> 本页内容只是 Web 安全中与计网相关知识的一小部分，帮助大家建立最基础的概念认知。在真实的渗透测试和 CTF 场景中，你会遇到更多协议细节、更复杂的网络结构，以及大量需要灵活判断的情况。比如抓包分析、代理、内网穿透、端口转发、协议特性漏洞利用等等。
>
> 后续大家遇到一些不懂的内容和知识，及时自己不断的查漏补缺，继续拓展知识体系，例如深入理解 TCP/IP 分层模型，HTTPS加密机制等等。
>
> 如果你想更轻松地入门网络原理，强烈推荐阅读 **崔鸿老师的《白话计算机网络》** ，非常适合作为学习配套材料。
>
> 对于本篇讲的内容，再多都只是纸上谈兵，自己打开burp，抓几次包，观察几次请求与响应的变化。

