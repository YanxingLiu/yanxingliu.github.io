---
title: '实用命令行工具'
date: 2026-04-29
permalink: /posts/2026/04/useful-cli-tools/
tags:
  - tools
  - CLI
  - productivity
---

claude code 和 codex 等命令行工具的兴起使得我的工作日常也逐渐从vscode转移到了cmux等终端命令行工具，但单纯依靠命令行工具的文档来学习和查阅效率较低，因此我整理了以下自用的一些实用的现代命令行工具和 tmux 快捷键，方便日常使用时快速查阅。

## 安装（MacOS/Linux）
Linux安装需要先安装Linuxbrew，安装方法请参考 https://docs.brew.sh/Homebrew-on-Linux 。

```bash
brew install zoxide fd fzf uv tlrc lsd chezmoi jd jq ripgrep yazi neovim fastfetch bat hyperfine dust broot btop procs bandwhich parquet-cli
```

初始化 shell 集成（追加到 `~/.zshrc`）：

```bash
RC="${ZDOTDIR:-$HOME}/.zshrc"
touch "$RC"
grep -qxF 'export EDITOR="nvim"' "$RC" || echo 'export EDITOR="nvim"' >> "$RC"
grep -qxF 'export VISUAL="$EDITOR"' "$RC" || echo 'export VISUAL="$EDITOR"' >> "$RC"
grep -qxF 'command -v zoxide >/dev/null && eval "$(zoxide init zsh)"' "$RC" || \
  echo 'command -v zoxide >/dev/null && eval "$(zoxide init zsh)"' >> "$RC"
grep -qxF 'command -v fzf >/dev/null && source <(fzf --zsh)' "$RC" || \
  echo 'command -v fzf >/dev/null && source <(fzf --zsh)' >> "$RC"
exec zsh
```

---

## 工具速查

### zoxide — 智能 cd

学习访问频率，通过部分路径快速跳转。

| 命令 | 功能 |
|------|------|
| `z foo` | 跳转到访问频率最高的含 "foo" 的目录 |
| `zi` | 交互式选择目录 |
| `z -` | 回到上一个目录 |

---

### fd — 现代 find

比 `find` 更快、语法更直观，默认尊重 `.gitignore`。

```bash
fd foo                    # 搜索含 "foo" 的文件/目录
fd -t f foo               # 仅搜索文件
fd -t d foo               # 仅搜索目录
fd -e md                  # 搜索所有 .md 文件
fd -i foo                 # 不区分大小写
fd '^test'                # 正则：以 "test" 开头
fd -d 2 foo               # 最大深度 2
fd -H foo                 # 包含隐藏文件
fd foo /path/to/dir       # 在指定目录搜索
fd -e py -x python        # 对每个 .py 文件执行 python
```

---

### fzf — 模糊查找器

通用命令行模糊搜索，可与任何列表输入协作。

| 快捷键 | 功能 |
|--------|------|
| `Ctrl+R` | 历史命令模糊搜索 |
| `Ctrl+T` | 模糊搜索文件并插入命令行 |
| `Alt+C` | 模糊搜索目录并切换 |
| `Tab` / `Shift+Tab` | 多选 |

结合预览：

```bash
fzf --preview 'cat {}'
fd --type f | fzf
```

环境变量配置：

```bash
export FZF_DEFAULT_COMMAND='fd --type f'
export FZF_DEFAULT_OPTS='--height 40% --layout=reverse --border'
```

---

### uv — 极速 Python 包管理

Rust 编写，速度比 pip 快 10-100 倍，统一替代 pip / virtualenv / poetry。

```bash
uv venv                   # 创建虚拟环境
uv pip install requests   # 安装包
uv run python script.py   # 隔离环境运行脚本
uv sync                   # 同步虚拟环境
uv lock                   # 生成锁文件
```

---

### tldr — 简洁命令手册

社区维护的实用示例手册，比 `man` 更快速上手。

```bash
tldr tar          # 查看 tar 常用示例
tldr --update     # 更新本地缓存
tldr --list       # 列出所有可用页面
```

---

### lsd — 现代 ls

彩色图标输出，支持树状视图，开箱即用。

```bash
lsd           # 带图标和颜色列出文件
lsd --tree    # 树状视图
```

---

### chezmoi — Dotfiles 管理

安全地在多台机器之间同步 shell 配置，支持模板化与加密。

```bash
chezmoi init                       # 初始化
chezmoi add ~/.zshrc               # 添加文件到管理
chezmoi edit ~/.zshrc              # 编辑托管文件
chezmoi apply                      # 应用变更到家目录
chezmoi diff                       # 查看差异
chezmoi status                     # 查看变更状态
chezmoi update                     # 拉取远程并应用
chezmoi cd                         # 跳转到源目录
chezmoi init <git-repo-url>        # 新机器初始化
```

---

### jd — JSON Diff

清晰展示两个 JSON 文件的结构差异。

```bash
jd a.json b.json
```

---

### jq — JSON 处理器

命令行 JSON 解析、过滤与转换的瑞士军刀。

```bash
cat data.json | jq '.'            # 格式化输出
jq '.name' data.json              # 提取字段
jq '.items[]' data.json           # 遍历数组
```

---

### yazi — 终端文件管理器

Rust 编写，异步、支持实时预览。

| 操作 | 说明 |
|------|------|
| `yazi` | 启动 |
| `t` | 创建新标签页 |
| `/` | 搜索模式 |
| `Space` | 标记文件 |
| `yy` / `dd` / `dD` | 复制 / 移动 / 删除 |

---

### ripgrep (rg) — 极速 grep

尊重 `.gitignore`，默认跳过隐藏文件和二进制文件，速度极快。

```bash
rg foo                      # 递归搜索 "foo"
rg -t py 'import requests'  # 仅搜索 Python 文件
rg -i 'error'               # 不区分大小写
rg -C 3 'TODO'              # 显示前后 3 行上下文
rg -l 'TODO'                # 仅显示匹配文件名
rg -c 'error'               # 统计每文件匹配数
rg -S 'user'                # 智能大小写
rg '^def\s+\w+'             # 正则搜索
rg -l 'foo' | xargs sed -i 's/foo/bar/g'  # 配合 sed 批量替换
```

---

### dust — 磁盘用量可视化

实测比 `du` 快约 20 倍左右，直观展示目录磁盘占用。

```bash
dust                        # 当前目录
dust path/to/dir            # 指定目录
dust -d 3                   # 最多 3 层深度
dust --number-of-lines 30   # 显示 30 条
dust --reverse              # 从大到小排序
dust --no-percent-bars      # 不显示进度条
```

---

### hyperfine — 命令行 Benchmark

```bash
hyperfine 'make'                                # 基础 benchmark（至少 10 次）
hyperfine 'make target1' 'make target2'         # 对比 benchmark
hyperfine --min-runs 7 'make'                   # 指定最少运行次数
hyperfine --warmup 5 'make'                     # 预热
hyperfine --prepare 'make clean' 'make'         # 每次运行前执行准备命令
```

---

### bat — 带高亮的 cat

`cat` 替代品，支持语法高亮和 Git 集成。

```bash
bat file.py                          # 高亮显示文件
bat file1 file2 > output             # 合并文件
bat -p --pager never file            # 纯文本输出，不分页
bat --highlight-line 10:20 file      # 高亮第 10-20 行
bat --number file                    # 仅显示行号
bat --language json file.json        # 指定语言高亮
bat --list-languages                 # 列出所有支持语言
```

---

### bws — Bitwarden Secrets Manager CLI
> Bitwarden 是一款开源密码管理器，Secrets Manager 是其面向开发者的子产品，用于集中存储和分发 API Key、数据库密码等敏感凭证。`bws` 是其命令行客户端。
安全地管理和注入 Bitwarden的 API Key、密码等敏感凭证，避免明文出现在 shell 历史或代码中（非常适合于在不暴露密钥的情况下让AI使用这些凭证）。

**安装：**

```bash
# macOS
brew install bitwarden/tap/bws

# Linux
curl -LO https://github.com/bitwarden/sdk-sm/releases/download/bws-v2.0.0/bws-x86_64-unknown-linux-gnu-2.0.0.zip
unzip bws-*.zip -d /usr/local/bin/ && chmod +x /usr/local/bin/bws
```

**配置 Access Token**（在 Bitwarden Web Vault → Secrets Manager → Machine Accounts 创建）：

```bash
export BWS_ACCESS_TOKEN=""
# 或写入 ~/.zshrc 持久化
```

**常用命令：**

```bash
bws project list                          # 列出项目（验证连接）
bws secret list                           # 列出 secrets（会显示明文值，慎用）
bws secret get <secret-id>               # 获取指定 secret
bws secret create KEY VALUE PROJECT_ID   # 创建 secret
bws secret edit <secret-id> --value "x"  # 更新 secret
bws run -- 'python script.py'            # 注入所有 secrets 后执行命令
bws run --project-id <id> -- 'npm start' # 仅注入指定项目的 secrets
bws run --no-inherit-env -- './app'      # 使用干净环境执行
```

`bws run` 是核心用法：secrets 以环境变量形式注入子进程，调用方（包括 AI）无法看到实际值。

```bash
# ✅ 安全：应用内部读取环境变量
bws run -- 'node server.js'

# ❌ 危险：会将 secrets 打印到终端
bws run -- 'env'
bws run -- 'printenv'
```

**安全建议：** 不同环境（dev/staging/prod）使用独立 Machine Account；定期轮换 Access Token；遵循最小权限原则。

---

## Tmux 快捷键速查

> 前缀键默认为 `Ctrl+b`（下文简写 `C-b`），可通过 `set -g prefix <key>` 修改。

### 会话 / 窗口 / 窗格

| 快捷键 / 命令 | 功能 |
|---------------|------|
| `tmux new -s <name>` | 创建命名会话 |
| `tmux a` / `tmux a -t <name>` | 连接到（指定）会话 |
| `tmux ls` | 列出所有会话 |
| `C-b d` | 分离当前会话 |
| `C-b s` | 列出并切换会话 |
| `C-b c` | 新建窗口 |
| `C-b n` / `C-b p` | 下/上一个窗口 |
| `C-b ,` | 重命名当前窗口 |
| `C-b %` / `C-b "` | 垂直/水平分割窗格 |
| `C-b 方向键` | 切换窗格 |
| `C-b z` | 最大化/恢复窗格 |
| `C-b x` | 关闭当前窗格 |

### 复制模式（需先设置 `setw -g mode-keys vi`）

| 快捷键 | 功能 |
|--------|------|
| `C-b [` | 进入复制模式 |
| `v` / `y` | 开始选择 / 复制 |
| `C-b ]` | 粘贴 |
| `q` | 退出复制模式 |
> 按 `C-b ?` 可查看完整快捷键列表。
