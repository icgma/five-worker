# CF Worker 五协议节点

## 1. 部署与功能说明

### 核心设置
* **XHTTP 协议**：若绑定自定义域名，必须在 Cloudflare 域名下的 **左侧网络菜单** 中开启 **gRPC** 功能，否则无法连通。
* **节点导入说明**：
    * **Shadowsocks**：无法直接导入，需查看生成的图片后手动配置。
    * **Socks5**：导入后信息可能不完整，需手动补充配置。
* **初始密码**：初次部署后访问 Worker 地址，会自动弹出设置密码的界面。

### 路径功能
* `/Socks5`: 留空。
* `/REMOTE_CONFIG`: 远程参数配置路径，设置后可通过远程文件动态更新节点参数。

---

## 2. 配置优先级与容错机制

本程序支持三种方式设置参数，加载优先级如下：

1.  **KV (键值存储)**
2.  **远程配置 (Remote Config)**
3.  **环境变量 (Environment Variables)**

**运行逻辑**：
* **KV 限制**：免费版 Cloudflare Workers 的 KV 有读取次数限制。当 KV 无法读取时，系统会自动退回到 **远程配置**。
* **兜底策略**：如果远程配置也失效，将使用 **环境变量** 保证节点基本运行。
* **免部署更新**：使用 KV 或远程配置修改参数时，**无需重新部署代码**。

---

## 3. 配置文件模板 (config.json)

您可以将以下 JSON 内容保存为 `config.json` 并上传至 GitHub、Gist 或其他直链地址，然后在 Cloudflare 环境变量中设置 `REMOTE_CONFIG` 指向该文件地址。

```json
{
  "UUID": "",
  "KEY": "",
  
  "TIME": "99999",
  "UPTIME": "0",

  "PROXYIP": "",
  "DNS64": "64:ff9b::/96",
  "SOCKS5": "",
  "GO2SOCKS5": "",
  "BAN": "",

  "URL302": "",
  "URL": "",

  "SUBNAME": "MyWorkerSub",
  "ADD": "[www.visa.com](https://www.visa.com).tw:443#台湾Visa, [2606:4700::]:443#IPv6官方",
  "ADDAPI": "[https://raw.githubusercontent.com/username/repo/main/ips.txt](https://raw.githubusercontent.com/username/repo/main/ips.txt)",
  "ADDNOTLS": "[www.visa.com](https://www.visa.com).sg:80#新加坡非TLS",
  "ADDNOTLSAPI": "",
  "ADDCSV": "[https://raw.githubusercontent.com/username/repo/main/speed.csv](https://raw.githubusercontent.com/username/repo/main/speed.csv)",
  "LINK": "vless://..., vmess://...",
  
  "CFPORTS": "443, 8443, 2053, 2083, 2087, 2096",
  "DLS": "8",
  "CSVREMARK": "1",
  "EX": "false"
}
