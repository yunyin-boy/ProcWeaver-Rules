# ProcWeaver-Rules

[ProcWeaver](https://github.com/yunyin-boy/ProcWeaver) 官方业务规则包仓库与分流规则发行中心。

---

## 仓库概述

本仓库专用于托管面向 [ProcWeaver](https://github.com/yunyin-boy/ProcWeaver) 客户端的**业务规则包（Business Rulesets）**预设模板与最新发行版本。

ProcWeaver 创新性地采用“以业务为中心”的精细化分流模式。通过规则包，用户可以将特定应用的主进程、深层伴生子进程树以及业务域名整体打包，为其指派专属的出站节点与独立 DNS。本仓库提供的规则包由官方维护，开箱即用，帮助用户快速构建隔离、纯净的业务网络环境。

---

## 官方预设业务规则包

当前仓库提供并维护两套面向本地开发与 IDE 场景的核心生产力业务规则包：

| 规则包名称 | 版本 | 适用业务场景 | 纳管核心进程 | 纳管核心域名 | 规则包下载 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **OpenAI / ChatGPT 生产力套件** | `v1.5` | ChatGPT 桌面客户端、IDE 扩展插件（Codex/Copilot）及本地开发环境 | `ChatGPT.exe`<br>`codex.exe`<br>`node.exe` | `openai.com`<br>`chatgpt.com`<br>`chat.com`<br>`sora.com`<br>`oaistatic.com`<br>`oaiusercontent.com`<br>`auth0.openai.com` | [下载规则包 (JSON)](https://raw.githubusercontent.com/yunyin-boy/ProcWeaver-Rules/main/Business-Rules/openai-chatgpt.pwpack.json) |
| **Google Antigravity 2.0 研发套件** | `v1.2` | Antigravity IDE、Gemini 客户端、语言推理模型工作进程 (LSP) 及 CLI 工具链 | `Antigravity.exe`<br>`language_server.exe`<br>`language_server_windows_x64.exe`<br>`Gemini.exe`<br>`agy.exe`<br>`antigravity-service.exe` | `antigravity.google`<br>`gemini.google.com`<br>`generativeai.google`<br>`aistudio.google.com`<br>`generativelanguage.googleapis.com`<br>`cloudaicompanion.googleapis.com`<br>`googleapis.com` | [下载规则包 (JSON)](https://raw.githubusercontent.com/yunyin-boy/ProcWeaver-Rules/main/Business-Rules/google-antigravity.pwpack.json) |

> 📁 规则包源文件存放于 [`Business-Rules/`](Business-Rules/) 目录。下载时如遇浏览器直接展示 JSON 文本，可直接右键“另存为”保存为 `.pwpack.json` 文件。

---

## 版本更新日志（Changelog）

### 2026-10-29
- **Google Antigravity 2.0 研发套件 (`v1.2`)**：
  - **安全自检透明接管**：策略配置默认开启 `disallowProxyArgs: true`，禁止注入 `--proxy-server` 命令行参数，完美兼容 Gemini 桌面端自检机制，自动配合 TUN / WinDivert / 用户态轻量 Hook 实现纯透明分流；
  - **纳管进程扩展**：新增 `Gemini.exe`、`language_server_windows_x64.exe` 及 `google gemini.exe` 变体；
  - **核心域名补全**：新增 `googleapis.com`，彻底打通 Gemini 账户鉴权与 API 路由。
- **OpenAI / ChatGPT 生产力套件 (`v1.5`)**：
  - **官方域名补充**：纳管域名新增 `sora.com`（OpenAI Sora 视频生成主站）；
  - **协议策略对齐**：显式对齐数据契约并设置 `disallowProxyArgs: false`，消除隐式 undefined。

---

## 规则包核心机制与网络安全策略

为了保证规则在跨设备、跨环境之间分发的安全性与灵活性，ProcWeaver 业务规则包遵循以下设计准则：

### 1. 规则定义与出口插槽解耦（Slot Architecture）
规则包仅描述**“匹配哪些应用进程”**、**“匹配哪些业务域名”**以及**“需要哪些出口用途”**，不包含任何具体用户的节点配置：
- **主业务出口（Main Slot）**：业务主进程与核心后台伴生进程访问外网所使用的代理节点。
- **DNS 解析出口（DNS Slot）**：该业务关联域名的专属解析出口，默认跟随主业务出口（`safe_pure` 纯净加密解析），彻底防止 DNS 投毒与污染。

### 2. 精准协议层防护策略（Protocol Policy）
- **代理 TCP 连接**：透明重定向 IDE 与本地客户端发起的 API 与模型流式请求。
- **阻断 UDP（防 QUIC 延迟与 WebRTC 泄露）**：消除 Electron/Chromium 发起 QUIC 握手失败造成的 1~2 秒卡顿，强制 0ms 秒切稳健的 TCP HTTP/2，并彻底防止 WebRTC 穿透泄露真实 IP。
- **阻断 IPv6 旁路**：过滤 AAAA 记录，杜绝开发机双栈网络下因直连 IPv6 导致的真实公网 IP 旁路泄露。
- **Safe-Pure 安全纯净 DNS**：通过 DoH 加密信道获取纯净解析，兼具高安全性与 100% 软件工具链兼容性。

### 3. 零隐私与安全便携性
- **无敏感凭据**：导出的规则包文件不包含任何本机的订阅链接、节点 IP、端口、密码或私有密钥。
- **独立绑定生效**：**每次导入新规则包后，用户都需要在 ProcWeaver 中为插槽重新选定本机当前订阅的节点**。未完成出口绑定的规则包不会静默启用。

---

## 快速使用指南

1. **获取客户端**：从 [ProcWeaver Releases](https://github.com/yunyin-boy/ProcWeaver/releases) 下载绿色便携包。
2. **下载规则包**：从本仓库 [`Business-Rules/`](Business-Rules/) 目录下载所需的 `.pwpack.json` 规则包文件。
3. **导入与配置**：
   - 打开 ProcWeaver 的“业务规则包”管理界面，点击“导入规则包”并选择下载的文件；
   - 在规则包配置面板中，为“主业务出口”绑定当前订阅中的可用节点（DNS 出口默认跟随主业务出口）；
   - 确认进程匹配范围（注：若存在同名程序如 `node.exe`，请核对实际路径是否符合业务预期）；
   - 点击“启用规则包”即可立即生效。

---

## 声明与开源许可

### 仅供学习声明

本仓库托管的所有规则包及分发内容**仅供计算机网络协议、分流路由机制及应用进程调度的个人学习、技术研究与学术交流**使用。

1. 本仓库仅提供分流匹配逻辑模板与客户端分发，**不提供任何网络代理节点、服务器资源或规避网络监管的服务**。
2. 规则包内所有条目均为开源的文本匹配规则，不包含任何商业机密、私有凭据或侵权内容。
3. 使用者应当严格遵守所在地区的法律法规。任何因将本仓库规则包用于技术研究之外的用途所引发的责任与后果，均由使用者自行承担，项目维护者不承担任何法律责任。

### 开源许可证

本仓库原创规则包遵循 **[GNU Affero General Public License v3.0 (AGPL-3.0-only)](LICENSE)** 协议开源。

### 商标与第三方说明

文中所涉及的 OpenAI、ChatGPT、Codex、Antigravity、Google 等公司、产品及服务名称，仅用于客观标明规则包适配的软件与域名范围。本仓库由 ProcWeaver 开源社区独立维护，不代表相关厂商参与、赞助或认可本项目。
