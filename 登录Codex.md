# Codex 安装、登录与连接

工作台本身没有网站账号体系；这里的“登录”是给本机 Codex CLI 认证。PDF 阅读、OCR、搜索、笔记不需要登录，也不要求 Codex 订阅。远端问答需要账号具备对应模型访问权限和额度。

## 1. 安装 CLI

从 [OpenAI 官方 Codex CLI 安装页](https://learn.chatgpt.com/docs/codex/cli)选择 Windows 安装方式。安装后重开 PowerShell，执行 `codex --version`。不要把别人电脑上的用户目录或本仓库截图中的路径复制过来。

工作台需要原生 `codex.exe`；若命令行能运行但界面显示 CLI_MISSING，进入设置填写其完整路径。npm 安装可能提供 `codex.cmd` 包装器，工作台会尝试寻找包装器附近的原生 exe；自动发现失败时应填写原生文件，而不是 cmd 文件。路径留空表示自动发现。

## 2. 浏览器登录（推荐）

```powershell
codex login
codex login status
```

在 CLI 提示的官方网页中登录本人账号，完成认证后返回终端。工作台若配置了指定 exe，请使用它登录：

```powershell
& 'C:\你的安装目录\codex.exe' login
& 'C:\你的安装目录\codex.exe' login status
```

上面的路径是占位示例。最可靠的命令来自“设置 → 查看登录步骤”。不要仅凭桌面聊天可用，就认定工作台实际使用的 CLI 已完成认证。

## 3. 设备码登录

浏览器回调受阻或无图形环境时，可运行 `codex login --device-auth`。按照终端提供的地址和一次性代码完成登录；个人账号可能需要启用设备码登录，工作区账号可能受管理员权限限制。该方式不是绕过认证。

## 4. API Key（可选）

若你使用 OpenAI Platform API Key，按照官方认证说明通过 CLI 的标准输入登录；不要在工作台网页中填写密钥，不要把密钥写入命令文本、README 或提交记录。API Key 使用按 Platform 计费，不能认为它使用 ChatGPT 订阅额度。工作台未对所有认证方式和模型组合做真实请求验收。

## 5. 在工作台接通

1. 保存 CLI 路径，默认适配器保持 app-server。
2. 点击“重新检查环境”，查看是否已认证、协议是否握手成功。
3. 点击“刷新模型目录”，选择目录里账号实际可用模型。
4. 点击“测试连接（远端请求）”，此操作会请求模型并可能消耗额度。
5. 连接成功后打开演示 PDF，以“当前页面”问“这页讲了什么？”，检查回答与引用。

![设置控制区（裁剪，未展示个人路径）](screenshots/02-settings.jpg)

app-server 使用真实增量协议；exec 是完整答案降级模式。已登录、协议成功、模型可用是三个不同检查：任何一项成功都不保证其余成功。不要用登录成功替代连接测试。

## 6. 常见状态

| 状态 | 含义和处理 |
| --- | --- |
| CLI_MISSING | 未找到原生 CLI，安装或指定 exe |
| AUTH_REQUIRED | 对实际 CLI 执行 login |
| AUTH_EXPIRED | 重新认证 |
| TRANSPORT_FAILED | 检查网络、CLI 版本和错误中的 initialize/turn 阶段；先更新 CLI，再复测，不要删除文献库 |
| MODEL_UNAVAILABLE | 选择账号支持模型；模型名称不要凭截图猜测 |
| QUOTA_EXCEEDED | 检查账号额度、限流和计费 |

退出认证可运行 `codex logout`，可能影响使用同一凭据缓存的其他 CLI/IDE。认证资料由 CLI 存储在用户配置目录或系统凭据库，发行包不会包含它们。

官方依据（2026-10-10 查阅）：[认证](https://learn.chatgpt.com/docs/auth)、[CLI 安装](https://learn.chatgpt.com/docs/codex/cli)。安装方式、账号权限和模型目录可能随官方版本变化。
