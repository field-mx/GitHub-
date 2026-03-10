# 🛠️ OpenClaw-WSL2-Network-Fix  
记录在 Windows + WSL2 (镜像网络) 环境下，解决 OpenClaw (AI Agent) 本地代理穿透与 SSRF (Server-Side Request Forgery) 安全拦截的完整 Debug 过程。  

## 📌 1. 环境背景与问题现象  
在搭建基于 Windows + WSL2 的自动化 AI 实验室时，为了让 OpenClaw 能够“自主浏览”外部学术网络并连接 WhatsApp 节点，配置了本地代理工具（端口 7891）。但在实际运行中遇到了两个致命的网络阻碍：  
* **报错 A (通信层)**:WhatsApp 节点持续输出 `WebSocket Error (Opening handshake has timed out)`，无法建立连接。
*  **报错 B (工具层)**:Agent 在调用 Web 抓取工具时，触发 `Blocked: resolves to private/internal/special-use IP address`，网页读取功能彻底瘫痪。  
