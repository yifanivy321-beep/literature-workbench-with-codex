# 文献阅读工作台

Windows 本地 PDF 文献阅读、检索、批注、OCR 与 Codex 问答工作台。文献、解析结果、会话和笔记默认保存在本机，网页服务只监听 `127.0.0.1`。

## 功能

- PDF 阅读、连续模式、关键词检索、阅读位置恢复、书签和 Markdown 笔记。
- 支持普通数字 PDF、混合 PDF，以及**扫描类 PDF**：没有可用文字层时调用本机 Tesseract OCR，保留页码、文字块、来源和解析版本。
- 页面、选段和检索结果上下文问答；Codex 请求由本机 CLI 发出。

扫描 PDF 的效果取决于分辨率、倾斜、语言包和版面。公式、表格、数字和低清页面必须对照原图；OCR 置信度不是答案正确率。

## Windows 安装与启动

1. 安装 Python 3.11 或更高版本。
2. 从 GitHub 下载 ZIP 并解压。
3. 在项目目录运行：

   ```powershell
   Set-ExecutionPolicy -Scope Process Bypass
   .\install-workbench.ps1
   ```

   脚本创建 `.venv`、安装依赖、检查前端资源和 Tesseract，并默认创建桌面快捷方式；跳过快捷方式使用 `-NoShortcut`。
4. 双击快捷方式，或运行 ` .\start-workbench.ps1`，访问 `http://127.0.0.1:4173/`。
5. 停止服务运行 ` .\stop-workbench.ps1`。

首次安装需要联网下载 Python 依赖；发行包不是独立 EXE/MSI。公开包不捆绑 Tesseract Windows 二进制：请按[OCR 安装说明](docs/安装与使用.md)安装引擎及中英文语言包。缺失引擎时仍可阅读 PDF 原图。

## Codex 登录

登录是可选的：不登录仍可阅读、OCR、搜索、批注和导出。需要问答时：

1. 打开“设置与诊断 → 查看登录步骤”。
2. 在 PowerShell 运行界面显示的同一个原生 CLI：`codex login`；无浏览器环境可尝试 `codex login --device-auth`。
3. 按官方页面完成浏览器或设备认证。不要复制桌面聊天令牌，也不要把令牌写入仓库。
4. 回到工作台点击“重新检查环境”，再“刷新模型目录”和“测试连接（远端请求）”。
5. 选择账号实际可用模型后发送问题。`AUTH_REQUIRED` 表示 CLI 尚未登录，`AUTH_EXPIRED` 表示需要重新登录。

问答会将选定文本、相关页面或用户勾选的图像发送给远端模型；普通阅读和 OCR 保持本地。模型权限、额度和图像能力由账号与模型决定。

## 界面截图

截图来自仓库内置的三页合成演示 PDF，不含个人文献或账号信息：

![阅读界面](docs/screenshots/01-reader.jpg)

![设置控制区（裁剪，未展示个人路径）](docs/screenshots/02-settings.jpg)

![选段与笔记界面](docs/screenshots/03-selection.jpg)

## 数据与隐私

个人数据位于 `.workbench-data`（PDF 副本、SQLite、解析记录、会话和日志），已被 `.gitignore` 忽略，发行 ZIP 不包含它。迁移前停止服务，再复制整个目录；不要提交数据库、日志、CLI 配置或访问令牌。

## 开发与验证

```powershell
python -m pip install -r requirements-dev.txt
python -m unittest discover -s tests -v
python tools/check_environment.py
python tools/package_workbench.py
```

打包脚本只收集源代码、必要前端 vendor、安装脚本、公开文档和合成演示 PDF，在 `dist/` 生成 SHA-256 文件，不收集个人数据、缓存或协议快照。

## 已知边界

多文献联合比较、标签体系、引用网络、Word/PDF 笔记导出尚未实现。OCR 不能替代人工校对，关键词检索不等同于语义理解。真实 Codex 问答必须由使用者完成登录并测试。

## 第三方资源

见 [第三方许可说明](THIRD_PARTY_NOTICES.md)。原项目未指定许可证，本次未擅自授权原有代码；公开源码不等于正式开源授权。PyMuPDF 涉及 AGPL/商业许可，商业或闭源再分发需核对条款。

## 详细文档

- [安装、扫描 PDF OCR 与完整使用方法](docs/安装与使用.md)
- [Codex 安装、登录、退出与故障处理](docs/登录Codex.md)
- [上传 GitHub 的文件与操作步骤](docs/GitHub发布指南.md)
- [本次发布验证与限制](docs/发布验证.md)
