# 第三方依赖与再分发

本发行包只包含必要前端资源及对应许可证，不包含 Codex CLI、Tesseract Windows 二进制或个人账号配置。

| 组件 | 用途 | 上游许可/说明 |
| --- | --- | --- |
| PDF.js | PDF 显示与文字层 | Apache-2.0，见 vendor/pdfjs/package/LICENSE |
| KaTeX | 数学排版 | MIT，见 vendor/katex/package/LICENSE |
| Marked | Markdown 渲染 | MIT，见 vendor/marked/package/LICENSE.md |
| DOMPurify | HTML 清理 | Apache-2.0 或 MPL-2.0，见 vendor/dompurify/package/LICENSE |
| Tesseract | 本地 OCR | Apache-2.0；由使用者单独安装，Windows 构建及附带 DLL 另受各自许可约束 |
| FastAPI / Uvicorn / python-multipart | Python 服务 | 通过 pip 安装，请查阅相应上游许可 |
| PyMuPDF | PDF 提取与渲染 | AGPL-3.0 或商业许可，见 https://pymupdf.readthedocs.io/en/latest/about.html#license-and-copyright |

项目未擅自为原有代码补选开源许可证。源码公开和可运行不等于授予无限制再分发或商业授权；如需以正式开源项目发布，应由著作权人选定与依赖兼容的许可。商业或闭源再分发尤其需要核对 PyMuPDF 条款。
