# 📚 Z-Library to NotebookLM

> 一键将 Z-Library 书籍自动下载并上传到 Google NotebookLM

---

## ⚠️ 重要免责声明

**本项目仅供学习、研究和技术演示用途。请严格遵守当地法律法规及版权规定，仅用于：**

- ✅ 你拥有合法访问权限的资源
- ✅ 公共领域或开源许可的文档（如 arXiv、Project Gutenberg）
- ✅ 个人拥有版权或已获授权的内容

**作者不鼓励、不支持任何形式的版权侵权行为，不承担任何法律责任。使用风险自负。**

**请尊重知识产权，支持正版阅读！**

---

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

## ✨ 特性

- 🔐 **一次登录，永久使用** - 类似 `notebooklm login` 的体验
- 📥 **智能下载** - 优先 PDF（保留排版），自动降级 EPUB → TXT
- 🤖 **全自动化** - 一条命令完成整个流程
- 🎯 **格式自适应** - 自动检测并处理多种格式（PDF、EPUB、MOBI 等）
- 📊 **进度可视化** - 实时显示下载和转换进度

## 🎯 作为 Claude Skill 使用（推荐）

### Claude Skill 安装

```bash
# 1. 进入 Claude Skills 目录
cd ~/.claude/skills  # Windows: %APPDATA%\Claude\skills

# 2. 克隆仓库
git clone https://github.com/zstmfhy/zlibrary-to-notebooklm.git zlib-to-notebooklm

# 3. 完成首次登录
cd zlib-to-notebooklm
python3 scripts/login.py
```

### 使用方式

安装后，在 Claude Code 中直接说：

```text
用 zlib-to-notebooklm skill 处理这个 Z-Library 链接：
https://zh.zlib.li/book/25314781/aa05a1/钱的第四维
```

Claude 会自动：

- 下载书籍（优先 PDF）
- 创建 NotebookLM 笔记本
- 上传文件
- 返回笔记本 ID
- 建议后续问题

---

## 🛠️ 传统方式安装

### 1. 安装依赖

```bash
# 克隆仓库
git clone https://github.com/zstmfhy/zlibrary-to-notebooklm.git
cd zlibrary-to-notebooklm

# 安装 Python 依赖
pip install playwright ebooklib

# 安装 Playwright 浏览器
playwright install chromium
```

### 2. 登录 Z-Library（仅需一次）

```bash
python3 scripts/login.py
```

**操作步骤：**
1. 浏览器会自动打开并访问 Z-Library
2. 在浏览器中完成登录
3. 登录成功后，回到终端按 **ENTER**
4. 会话状态已保存！

### 3. 下载并上传书籍

```bash
python3 scripts/upload.py "https://zh.zlib.li/book/..."
```

**自动完成：**
- ✅ 使用已保存的会话登录
- ✅ 优先下载 PDF（保留排版）
- ✅ 自动降级 EPUB → TXT
- ✅ 创建 NotebookLM 笔记本
- ✅ 上传内容
- ✅ 返回笔记本 ID

## 📖 使用示例

### 基本用法

```bash
# 下载单本书籍
python3 scripts/upload.py "https://zh.zlib.li/book/12345/..."
```

### 批量处理

```bash
# 批量下载多本书
for url in "url1" "url2" "url3"; do
    python3 scripts/upload.py "$url"
done
```

### 使用 NotebookLM

```bash
# 上传完成后，使用笔记本
notebooklm use <返回的笔记本ID>

# 开始提问
notebooklm ask "这本书的核心观点是什么？"
notebooklm ask "总结第3章的内容"
```

## 🔄 工作流程

```
Z-Library URL
    ↓
1. 启动浏览器（使用已保存的会话）
    ↓
2. 访问书籍页面
    ↓
3. 智能选择格式：
   - 优先 PDF（保留排版）
   - 备选 EPUB（转换为纯文本）
   - 其他格式（自动转换）
    ↓
4. 下载文件到 ~/Downloads
    ↓
5. 格式处理：
   - PDF → 直接使用
   - EPUB → 转换为 TXT
    ↓
6. 创建 NotebookLM 笔记本
    ↓
7. 上传内容
    ↓
8. 返回笔记本 ID ✅
```

## 📁 项目结构

```
zlibrary-to-notebooklm/
├── SKILL.md              # Skill 核心定义（必需）
├── README.md             # 项目文档
├── LICENSE               # MIT 许可证
├── package.json          # npm 配置（用于 Claude Code skill）
├── skill.yaml            # Skill 定义
├── requirements.txt      # Python 依赖
├── scripts/              # 可执行脚本（官方标准）
│   ├── login.py         # 登录脚本
│   ├── upload.py        # 下载+上传脚本
│   └── convert_epub.py  # EPUB 转换工具
├── docs/                 # 文档
│   ├── WORKFLOW.md      # 工作流程详解
│   └── TROUBLESHOOTING.md # 故障排除
└── INSTALL.md            # 安装指南
```

## 🔧 配置文件

所有配置保存在 `~/.zlibrary/` 目录：

```
~/.zlibrary/
├── storage_state.json    # 登录会话（cookies）
├── browser_profile/      # 浏览器数据
└── config.json          # 账号配置（备用）
```

## 🛠️ 依赖项

- **Python 3.8+**
- **playwright** - 浏览器自动化
- **ebooklib** - EPUB 文件处理
- **NotebookLM CLI** - Google NotebookLM 命令行工具

## 📝 命令参考

### 登录

```bash
python3 scripts/login.py
```

### 上传

```bash
python3 scripts/upload.py <Z-Library URL>
```

### 查看会话状态

```bash
ls -lh ~/.zlibrary/storage_state.json
```

### 重新登录

```bash
rm ~/.zlibrary/storage_state.json
python3 scripts/login.py
```

## 🤝 贡献

欢迎贡献！请随时提交 Pull Request。

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 🙏 致谢

- [Z-Library](https://zh.zlib.li/) - 世界上最大的数字图书馆
- [Google NotebookLM](https://notebooklm.google.com/) - AI 驱动的笔记工具
- [Playwright](https://playwright.dev/) - 强大的浏览器自动化工具

## 📮 联系方式

- GitHub Issues: [提交问题](https://github.com/your-username/zlibrary-to-notebooklm/issues)
- 讨论区: [GitHub Discussions](https://github.com/your-username/zlibrary-to-notebooklm/discussions)

---

**⭐ 如果这个项目对你有帮助，请给个 Star！**
