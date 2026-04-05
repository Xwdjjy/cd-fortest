# Python GitHub Drive | 派盘[^1] 🐍📦

利用 GitHub 仓库 + GitHub API 搭建 ~~实用~~ **免费** 的私人云盘服务，命令行搞定一切 ✨

## 主要功能

- 📂 列出仓库目录
- ⬇️ 下载 / ⬆️ 上传 / ❌ 删除文件
- ℹ️ 查看文件元信息

适合把**小文件（≤100 MB）**[^2] 存到 GitHub 仓库，并通过命令行管理（~~GitHub Desktop？不存在的~~）

---

## 快速开始

推荐在虚拟环境中以可编辑模式运行（开发友好）：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install -e .
```

验证安装：

```powershell
python -c "import pygitdrive; print(pygitdrive.__version__)"
```

搞定 🎉

---

## 配置

分两步走～ 👣

### 前提基础

1. 你电脑上得有 Python 并且配置好了（毕竟是 Python 项目嘛，bro～）
2. 在 GitHub 上新造一个仓库（当作派盘的“硬盘”），可见性随便选，想藏起来就选 `Private` 🤫
3. 搞一个 GitHub API token：`Settings` → `Developer settings` → `Personal access tokens` → `Tokens (classic)` → `Generate new token`，记得勾选 `repo` 权限，然后复制那一长串神奇数字 ✨

### 配置变量

推荐用环境变量存敏感信息（更安全 🔒）：

- `GITHUB_OWNER` — 仓库所有者（你的用户名或组织）
- `GITHUB_REPO` — 仓库名
- `GITHUB_TOKEN` — GitHub Personal Access Token（要有 `repo` 权限）
- `GITHUB_BRANCH` — 分支名（可选，默认 `main`）

示例（PowerShell，会话级别）：

```powershell
$env:GITHUB_OWNER = "yourname"
$env:GITHUB_REPO = "my-drive"
$env:GITHUB_TOKEN = "ghp_xxx..."
$env:GITHUB_BRANCH = "main"
```

或者用 CLI 初始化并保存到本地配置文件（明文，在 `~/.pygitdrive/config.json`）：

```powershell
pygitdrive init --owner <owner> --repo <repo> --token <token> [--branch <branch>]
```

如果既没有环境变量也没有本地配置，命令会提示配置错误并乖乖退出 🚪

---

## 常用命令

下面假设你已经配好了 `owner/repo/token`：

- 查看帮助菜单 🆘

```powershell
pygitdrive --help
pygitdrive -h
pygitdrive [list/download/upload/info/init/delete] -h
```

- 列出远程目录（根目录或子路径）📁

```powershell
pygitdrive list
pygitdrive list path/to/dir
```

- 下载文件（不指定本地路径就默认存到 `Downloads` 目录）⬇️

```powershell
pygitdrive download README.md
pygitdrive download docs/manual.pdf C:\Temp\manual.pdf
```

- 上传单个本地文件到仓库（不支持目录，暂时只传单文件）⬆️

```powershell
pygitdrive upload C:\path\local.txt
pygitdrive upload C:\path\local.txt remote/dir/remote.txt
```

- 删除远程文件 ❌

```powershell
pygitdrive delete remote/dir/remote.txt
```

- 查看文件元信息 ℹ️

```powershell
pygitdrive info path/to/file.txt
```

---

## 测试 🧪

项目包含在线测试（标记为 `live`，需要真的 `GITHUB_TOKEN`）和离线单元测试（用 `responses` 模拟 GitHub API）。运行测试：

```powershell
# 在已激活的虚拟环境中
pytest -q

# 只跑 live 测试（记得设置 GITHUB_TOKEN）
$env:GITHUB_TOKEN = "真实_token"
pytest -q -m live
```

---

## 限制与注意事项 ⚠️

- GitHub Contents API 对单个文件大小有限制，太大不行。真要存大文件可以考虑 Git LFS 或 Releases。
- 本地配置文件会明文保存 token，所以**强烈建议优先使用环境变量**或系统 keyring（后面可能会集成）。
- **千万不要**把你配置好的本地项目发给别人或传到公开仓库！因为配置文件里明晃晃地写着你的 token 🫣
- 当前上传不支持目录递归（一次只传一个文件）。如果你需要递归上传，可以喊我加～ 😉

---

## 其他

目前是纯命令行版，后面可能会搞个 GUI 界面……我加油，~~咕咕咕~~ 🐦

---

## 贡献 🤝

欢迎提 issue 或 pull request。开发建议：

1. Fork 并创建分支
2. 改代码 + 补测试
3. 提交 PR，说明改了什么

请保证测试通过（`pytest`），并在 PR 里注明是否有破坏性变更。

---

[^1]: 为啥叫“派盘”？因为 ~~带派不老弟~~ Python 发音带“派”嘛 🥧
[^2]: 为啥文件 ≤100 MB？GitHub 平台限制，API 单文件上传[上限 100 MB](https://docs.github.com/zh/repositories/working-with-files/managing-large-files/about-large-files-on-github)
