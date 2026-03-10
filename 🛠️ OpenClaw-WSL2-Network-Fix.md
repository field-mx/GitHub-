# 🛠️ OpenClaw-WSL2-Network-Fix  
记录在 Windows + WSL2 (镜像网络) 环境下，解决 OpenClaw (AI Agent) 本地代理穿透与 SSRF (Server-Side Request Forgery) 安全拦截的完整 Debug 过程。  

## 📌 1. 环境背景与问题现象  
在搭建基于 Windows + WSL2 的自动化 AI 实验室时，为了让 OpenClaw 能够“自主浏览”外部学术网络并连接 WhatsApp 节点，配置了本地代理工具（端口 7891）。但在实际运行中遇到了两个致命的网络阻碍：  
* **报错 A (通信层)**:WhatsApp 节点持续输出 `WebSocket Error (Opening handshake has timed out)`，无法建立连接。
* **报错 B (工具层)**:Agent 在调用 Web 抓取工具时，触发 `Blocked: resolves to private/internal/special-use IP address`，网页读取功能彻底瘫痪。
 
## 🔍 2. 问题排查过程  
**1.基础网络排查**:在 WSL 终端内直接使用 `curl` 测试，证实基础网络畅通，排除系统级断网可能。  
   
**2.代理端口排查**:发现 Windows 代理软件监听的是 `7891` 端口，但通过 `Windows .bat` 脚本唤醒 WSL 时，拉起的是一个“纯净”的 Linux 会话，未能正确继承代理环境变量，导致流量“迷路”超时。  
  
**3.安全机制排查**:当尝试在终端手动指定代理 `127.0.0.1:7891` 后，触发了 `OpenClaw` 深层的` SSRF` 防御机制。系统将本地代理 IP 判定为“高危内网地址”并强行切断连接。  

