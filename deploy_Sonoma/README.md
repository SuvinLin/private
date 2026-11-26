# macOS 装机与开发环境配置指南（Intel `x86_64` · 2026 当前环境版）

> **整理日期**：2026-11-26  
> **硬件与系统**：**Intel 芯片 Mac（`x86_64`）** · `Intel(R) Core(TM) i5-1038NG7 CPU @ 2.00GHz` · `macOS 26.7.3`（Build `25G342`）  
> **参考来源**：[SuvinLin/private/deploy](https://github.com/SuvinLin/private/tree/master/deploy)（由原 9 篇基于 macOS 10.14 的文档按当前本机实际配置重写与合并）

> [!IMPORTANT]
> 本机为 **Intel 芯片（`x86_64`）架构**（非 Apple Silicon `arm64`）。下文涉及的所有 **GUI 软件安装包**与 **CLI 预编译二进制**，下载时请务必选择 **`Intel` / `x86_64` / `amd64` / `Universal`（通用版）**，切勿下载仅限 `Apple Silicon` / `arm64` 的版本。

---

## 步骤总览与架构演进

| 步骤 | 主题 | 相较旧版文档的核心变化 |
| --- | --- | --- |
| 第 1 步 | 重装 macOS 与基础初始化 | 补充抹盘重装方式、FlClash 代理下 iCloud 登录坑点、`CommandLineTools` 修复与 OneDrive 软链接 |
| 第 2 步 | 系统设置与常用 macOS Tips | 更新至新版「系统设置」路径与命令行一键写入；修正 Launchpad 行/列注释；DNS 改为腾讯/阿里 |
| 第 3 步 | 去包管理器方案（`~/.local/bin` + Mole） | **放弃 Homebrew 与 MacPorts**；GUI 官网直装，现代 CLI 用官方预编译二进制放 `~/.local/bin/` + `Mole` 清理 |
| 第 4 步 | 终端与编辑器配置（Ghostty + Zsh + Vim） | **iTerm2 → Ghostty 1.4.0**、**`autojump` → `zoxide` + `fzf`**、字体升级 **FiraCode Nerd Font**，合并 `~/.vimrc` |
| 第 5 步 | Git、GitHub CLI 与 SSH 配置 | Git `2.56.1` 源码免 `sudo` 编译；新增 `gh` + `delta`；密钥升级 **`ed25519`**（443 端口直连） |
| 第 6 步 | 效率启动器：**SW Tools** | **Alfred 5 + 系统聚焦 → 自研原生 SW Tools**（含剪贴板预览、文件/应用搜索、Snippet 自动展开、OneDrive 同步） |
| 第 7 步 | Node.js 环境配置（`fnm`） | **`nvm` → Rust 版 `fnm`**（自动读取 `.nvmrc` / `.node-version`，当前默认 Node `v26.13.1` / npm `11.20.0`） |
| 第 8 步 | 写作与图床配置（Typora + PicGo） | PicGo 接入 `SuvinLin/picBed` + `fastly.jsdelivr.net` + 本地 `127.0.0.1:7890` 代理 |
| 第 9 步 | 本机已安装软件总清单 | 按实际 `/Applications` 分类梳理全部 41 款主力应用与同步配置路径 |

---

## 第 1 步：重装 macOS 与基础初始化

### 1.1 系统重装方式（Intel `x86_64` Mac）
- **带 T2 安全芯片的 Intel Mac（推荐）**：系统设置 →「通用」→「传输或还原」→「抹掉所有内容和设置」，无需制作 U 盘即可快速恢复纯净系统。
- **内置恢复分区 / 网络恢复（Intel 快捷键）**：关机后开机立刻长按 `⌘ + R`（本地恢复）或 `⌥ + ⌘ + R`（联网安装最新兼容 macOS）。*(注意：不要用 Apple Silicon 的“长按电源键”方式)*
- **U 盘抹盘重装（通用兜底）**：下载对应版本安装器后制作启动盘，Intel Mac 开机长按 **`⌥`（Option）** 选择 U 盘启动安装：
  ```bash
  sudo /Applications/Install\ macOS\ <版本名>.app/Contents/Resources/createinstallmedia --volume /Volumes/<U盘卷名>
  ```

### 1.2 网络代理与 iCloud 登录（必看避坑）
1. 优先安装 **FlClash**（`mixed-port: 7890`），作为后续下载工具与终端代理基础。
2. **登录 Apple 账户 / iCloud 踩坑**：开启系统全局 HTTP/SOCKS 代理时，若忽略列表未放行 `*.apple.com` 与 `*.icloud.com`，登录 iCloud 极易转圈卡死；可临时关闭系统代理或先登录 App Store 后再试。
3. 装好 **OneDrive** 后，建立主目录软链接方便脚本与同步配置访问：
   ```bash
   ln -s "$HOME/Library/CloudStorage/OneDrive-个人" "$HOME/OneDrive"
   ```

### 1.3 安装 Xcode Command Line Tools
```bash
xcode-select --install
```
> [!TIP]
> 若曾跨大版本升级系统导致 `/Library/Developer/CommandLineTools/usr/include/swift/` 下同时残留 `module.modulemap` 与 `bridging.modulemap`（会引发 Swift/Clang 编译异常），执行以下彻底重装：
> ```bash
> sudo rm -rf /Library/Developer/CommandLineTools && xcode-select --install
> ```

---

## 第 2 步：系统设置与常用 macOS Tips

### 2.1 系统设置核对清单（新版「系统设置」）
- **辅助功能 → 指针控制 → 触控板选项**：开启「使用触控板进行拖移」，方式选「三指拖移」。
- **触控板**：开启「轻点来点按」。
- **桌面与程序坞 → 触发角**：左上角设为「启动屏幕保护程序」。
- **锁定屏幕**：不活动后不自动启动屏保（由触发角控制）；进入屏保或关闭显示器后「立即」要求输入密码。
- **键盘**：关闭自动纠错、自动大写、连按两下空格插入句点、智能引号和破折号（方便写代码）。
- **隐私与安全性 → 辅助功能**：后续需在此为 `SW Tools`、`Snipaste` 等效率工具勾选权限。

### 2.2 一键写入脚本与常用 Tips

以下配置均与当前本机生效状态一致：

```bash
# 1. 配置 Wi-Fi DNS（腾讯 DNS + 阿里 DNS）
sudo networksetup -setdnsservers "Wi-Fi" 119.29.29.29 223.5.5.5

# 2. Finder：关闭标题栏完整路径，改为开启底部路径栏（可点击/拖拽跳转）
defaults write com.apple.finder _FXShowPosixPathInTitle -bool NO
defaults write com.apple.finder ShowPathbar -bool true
killall Finder

# 3. 触发角：左上角启动屏幕保护程序（5 = Start Screen Saver）
defaults write com.apple.dock wvous-tl-corner -int 5
defaults write com.apple.dock wvous-tl-modifier -int 0

# 4. 调整启动台网格为 7 行 7 列（注意：rows 为行数，columns 为列数）
defaults write com.apple.dock springboard-rows -int 7
defaults write com.apple.dock springboard-columns -int 7
defaults write com.apple.dock ResetLaunchPad -bool TRUE
killall Dock

# 5. 接通电源提示音（写入后需重启或重新插拔电源生效）
defaults write com.apple.PowerChime ChimeOnAllHardware -bool true

# 6. （可选）菜单栏电池显示百分比（当前本机默认为关闭 0，按需开启）
# defaults -currentHost write com.apple.controlcenter BatteryShowPercentage -bool true
```

---

## 第 3 步：去包管理器方案（`~/.local/bin` + Mole）

本机为 **Intel `x86_64` 架构**，已**完全放弃 Homebrew 与 MacPorts**（随着生态向 Apple Silicon 倾斜，Intel 上 Homebrew/MacPorts 经常缺少预编译 Bottle/Archive，装一个工具容易触发长达数小时的 Rust/LLVM 源码本地编译且极占空间）：

1. **系统自带基础工具已足够**：`/bin/zsh`、`vim`、`/usr/bin/python3`（`3.9.6`）、`clang`（来自 Command Line Tools）。
2. **GUI 应用全部官网或 GitHub Release 直装**至 `/Applications/`（下载 `Intel` / `x64` / `Universal` 包）。
3. **现代 CLI 工具统一放入 `~/.local/bin/`**（零依赖、免 `sudo` 单文件二进制）：

| 命令 | 当前版本 | Intel 下载包架构标识 | 用途与来源 |
| --- | --- | --- | --- |
| `git` | `2.56.1` | 源码本机编译 | 第 5 步源码编译安装（优先于系统自带 Git） |
| `gh` | `2.105.0` | `macOS_amd64` | GitHub CLI（Release 二进制） |
| `delta` | `0.18.2` | `x86_64-apple-darwin` | Git Diff 语法高亮（`0.19.0+` 起停更 Intel 包，固定 **`0.18.2`**） |
| `fnm` | `1.39.0` | `fnm-macos.zip`（Universal） | Node.js 版本管理器（替代 `nvm`） |
| `fzf` | `0.75.0` | `darwin_amd64` | 终端模糊搜索（`Ctrl+R` / `Ctrl+T` / `Alt+C`） |
| `zoxide` | `0.10.0` | `x86_64-apple-darwin` | 智能目录跳转（替代 `autojump`） |
| `ghostty` | `1.4.0` | `Universal` `.dmg` | 软链接至 `/Applications/Ghostty.app/Contents/MacOS/ghostty` |
| `mo` / `mole` | `Mole CLI` | Universal / Intel 脚本安装 | 系统清理、空间分析与卸载工具（配合 `/Applications/Mole.app` 使用） |

初始化目录与清理工具安装：
```bash
mkdir -p ~/.local/bin
# 按官方脚本安装 Mole CLI（同时在 /Applications 安装图形端 Mole.app）
curl -fsSL https://raw.githubusercontent.com/tw93/Mole/main/install.sh | bash -s -- --prefix "$HOME/.local/bin"
```

---

## 第 4 步：终端与编辑器配置（Ghostty + Zsh + Vim）

### 4.1 安装 Ghostty 与 FiraCode Nerd Font
1. 从官网下载安装 `Ghostty.app`（当前 `1.4.0`），并创建 CLI 软链接：
   ```bash
   ln -sf "/Applications/Ghostty.app/Contents/MacOS/ghostty" ~/.local/bin/ghostty
   ```
2. 从 Nerd Fonts Release 下载 `FiraCode` 压缩包，将 6 个字重安装至 `~/Library/Fonts/`：
   - `FiraCodeNerdFont-Light.ttf` / `Regular.ttf` / `Retina.ttf` / `Medium.ttf` / `SemiBold.ttf` / `Bold.ttf`

### 4.2 Ghostty 配置文件（`~/.config/ghostty/config`）
> 注：macOS 上路径为 `~/.config/ghostty/config`（Ghostty 同时支持 `~/Library/Application Support/com.mitchellh.ghostty/config`，本机统一保存在 `~/.config/ghostty/config`）。

```ini
theme = Dracula
font-family = FiraCode Nerd Font
font-size = 13
background-opacity = 0.88
background-blur-radius = 20
macos-titlebar-style = tabs
quit-after-last-window-closed = true
confirm-close-surface = false
window-padding-x = 8
window-padding-y = 6
shell-integration = zsh
keybind = alt+left=esc:b
keybind = alt+right=esc:f
```

### 4.3 安装 Oh My Zsh、主题与插件
1. macOS 默认 Shell 已是 `/bin/zsh`，无需 `chsh`；无交互安装 Oh My Zsh：
   ```bash
   RUNZSH=no CHSH=no KEEP_ZSHRC=yes sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```
2. 复制自定义主题 `myys`（基于 `ys` 去掉 `@%m` 主机名）到 `~/.oh-my-zsh/custom/themes/myys.zsh-theme`。
3. 克隆常用插件：
   ```bash
   ZSH_CUSTOM="${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}"
   git clone https://github.com/zsh-users/zsh-autosuggestions "$ZSH_CUSTOM/plugins/zsh-autosuggestions"
   git clone https://github.com/zsh-users/zsh-syntax-highlighting "$ZSH_CUSTOM/plugins/zsh-syntax-highlighting"
   ```
4. 从 GitHub Release 下载 `zoxide` 与 `fzf` 预编译二进制放至 `~/.local/bin/` 并赋予 `chmod +x`。

### 4.4 配置 `~/.zprofile`、`~/.zshrc` 与 `~/.vimrc`

**`~/.zprofile`**：
```bash
# Load .zshrc if not already loaded in non-interactive login shells
if [ -z "${HTTP_PROXY}" ] && [ -f "$HOME/.zshrc" ]; then
  source "$HOME/.zshrc"
fi

# Local user binaries (e.g., Mole, zoxide, fzf, fnm)
case ":$PATH:" in
  *":$HOME/.local/bin:"*) ;;
  *) export PATH="$HOME/.local/bin:$PATH" ;;
esac
```

**`~/.zshrc`**：
```bash
# FlClash Proxy Configuration (mixed-port: 7890)
export FLCLASH_PORT=7890

proxy_on() {
  export http_proxy="http://127.0.0.1:${FLCLASH_PORT}"
  export https_proxy="http://127.0.0.1:${FLCLASH_PORT}"
  export all_proxy="socks5://127.0.0.1:${FLCLASH_PORT}"
  export HTTP_PROXY="http://127.0.0.1:${FLCLASH_PORT}"
  export HTTPS_PROXY="http://127.0.0.1:${FLCLASH_PORT}"
  export ALL_PROXY="socks5://127.0.0.1:${FLCLASH_PORT}"
  export no_proxy="localhost,127.0.0.1,::1,*.local"
  export NO_PROXY="localhost,127.0.0.1,::1,*.local"
}

proxy_off() {
  unset http_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY ALL_PROXY
}

# Enable proxy environment variables by default for Antigravity (language_server) & CLI tools
proxy_on

# Local user binaries (Mole, zoxide, fzf, fnm)
case ":$PATH:" in
  *":$HOME/.local/bin:"*) ;;
  *) export PATH="$HOME/.local/bin:$PATH" ;;
esac

# Path to your Oh My Zsh installation.
export ZSH="$HOME/.oh-my-zsh"

# Custom copy of ys theme without @hostname
ZSH_THEME="myys"

# Plugins (zsh-syntax-highlighting must stay last)
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)

source "$ZSH/oh-my-zsh.sh"

# fzf keybindings & completion (Ctrl+R history search, Ctrl+T file search, Alt+C cd)
if command -v fzf >/dev/null 2>&1; then
  source <(fzf --zsh)
fi

# zoxide (replaces autojump: `z <keyword>` or `zi` with fzf)
if command -v zoxide >/dev/null 2>&1; then
  eval "$(zoxide init zsh)"
fi

# fnm (Fast Node Manager, replaces nvm; auto-switches on .nvmrc / .node-version)
if command -v fnm >/dev/null 2>&1; then
  eval "$(fnm env --use-on-cd --shell zsh)"
fi

# Git & terminal locale
export LANG=en_US.UTF-8

# Reset DNS cache
alias dns="sudo killall -HUP mDNSResponder"

# Git aliases
alias gs="git status"
alias ga="git add"
alias gcm="git commit -m"
alias gc="git clone"
alias gl="git log"

# npm aliases
alias n="npm"
alias nr="npm run"
alias nl="npm ls --depth=0"
alias nl-g="npm ls -g --depth=0"

# Application permission & signing
alias dis="sudo spctl --master-disable"
alias en="sudo spctl --master-enable"
alias sign="codesign --force --deep --sign - "

# iOS USB connection workaround
alias ios="sudo killall -STOP -c usbd"
```

**`~/.vimrc`**（原第 3 步已合并至此）：
```vim
" 语法高亮
syntax on
" 显示行号
set nu
" 智能缩进
set smartindent
" 不需要备份
set nobackup
" 显示标尺，就是在右下角显示光标位置
set ruler
```

---

## 第 5 步：Git、GitHub CLI 与 SSH 配置

### 5.1 安装新版 Git、`gh` 与 `delta`
- **Git `2.56.1`（免 `sudo` 安装到 `~/.local/`）**：
  下载 Git 官方源码后编译，并一并编译 macOS 钥匙串凭据助手（`2.56+` 在无 Rust 工具链时必须指定 `NO_RUST=1`）：
  ```bash
  make prefix="$HOME/.local" NO_GETTEXT=1 NO_TCLTK=1 NO_RUST=1 APPLE_COMMON_CRYPTO=YesPlease RUNTIME_PREFIX=1 -j4 all install
  make -C contrib/credential/osxkeychain prefix="$HOME/.local" NO_GETTEXT=1 NO_TCLTK=1 NO_RUST=1 APPLE_COMMON_CRYPTO=YesPlease RUNTIME_PREFIX=1 install
  ```
- **`gh v2.105.0` & `delta 0.18.2`**：从 GitHub Release 下载二进制放入 `~/.local/bin/`。
  > [!NOTE]
  > `git-delta` 从 `0.19.0+` 起已停止提供 Intel `x86_64-apple-darwin` 预编译包，Intel Mac 固定使用 **`0.18.2`**；Apple Silicon Mac 可直接用新版 `aarch64-apple-darwin`。

### 5.2 配置 `~/.gitconfig` 与 `~/.gitignore_global`

**`~/.gitconfig`**：
```ini
[user]
	name = suvin
	email = linsuvin@gmail.com
[core]
	quotepath = false
	autocrlf = input
	excludesfile = /Users/suvin/.gitignore_global
	pager = delta
[i18n]
	commitEncoding = utf-8
	logOutputEncoding = utf-8
[init]
	defaultBranch = main
[push]
	autoSetupRemote = true
[fetch]
	prune = true
[credential]
	helper = osxkeychain
[interactive]
	diffFilter = delta --color-only
[delta]
	navigate = true
	dark = true
	syntax-theme = Dracula
	line-numbers = true
[merge]
	conflictStyle = zdiff3
[diff]
	colorMoved = default
```

**`~/.gitignore_global`**（不再全局忽略 `node_modules`，交给各项目自身 `.gitignore` 控制）：
```gitignore
# macOS
.DS_Store
._*
.Spotlight-V100
.Trashes

# Editors & IDEs
.vscode/
.idea/
*.swp
*.swo
```

### 5.3 生成 `ed25519` SSH 密钥与配置 `~/.ssh/config`
```bash
ssh-keygen -t ed25519 -C "linsuvin@gmail.com"
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**`~/.ssh/config`**（走 `ssh.github.com:443` 直连即可；开启 FlClash TUN 模式时若再套 `ProxyCommand nc` 反而容易握手中断，故仅作注释备用）：
```sshconfig
Host *
  AddKeysToAgent yes
  UseKeychain yes

Host github.com
  HostName ssh.github.com
  Port 443
  User git
  IdentityFile ~/.ssh/id_ed25519
  # 如未开启 TUN 且网络无法直连 GitHub 443 端口时再取消注释：
  # ProxyCommand nc -X 5 -x 127.0.0.1:7890 %h %p
```

登录 GitHub CLI 并验证 SSH：
```bash
gh auth login   # 或将 ~/.ssh/id_ed25519.pub 添加到 GitHub SSH Keys
ssh -T git@github.com
```

---

## 第 6 步：效率启动器 — SW Tools（替代 Alfred & 聚焦）

原 **Alfred 5 + 系统聚焦搜索** 已全面替换为自研轻量原生工具集 **SW Tools**（整合应用启动、剪贴板历史、文本片段展开、Chrome 书签与无痕打开、网页快搜、Finder 工具栏终端 `SW Terminal.app`、窗口分屏、菜单栏图标收纳与图片压缩；安装包约 844 KB，空载内存约 24 MB，采用常驻引擎 Worker + 懒加载面板）。

### 6.1 备份位置与重装后快速安装步骤

- **OneDrive 源码备份目录**：`~/Library/CloudStorage/OneDrive-个人/quickbar config/SWTools-Backup`（建立软链接后即 `~/OneDrive/quickbar config/SWTools-Backup`）
- **OneDrive 配置同步目录**：`~/OneDrive/quickbar config/`（包含 `settings.json`、`snippets.json`、`websearches.json`）
- **本机应用安装路径**：`/Applications/SW Tools.app`

1. **确认已安装 Xcode Command Line Tools**：
   ```zsh
   xcode-select --install
   ```

2. **从 OneDrive 备份目录编译并安装**：
   ```zsh
   cd ~/Library/CloudStorage/OneDrive-个人/"quickbar config"/SWTools-Backup
   ./build.sh
   open -a "/Applications/SW Tools.app"
   ```
   - 脚本会自动编译 `SW Tools.app` 与 `SW Terminal.app`，生成本地代码签名证书 `SW Tools Local Signer` 并安装到 `/Applications/SW Tools.app`。
   - 若首次签名时弹出钥匙串授权提示，点击 **「始终允许」**。

3. **首次启动配置**（按 `⌘,` 或点击顶部状态栏图标 → 偏好设置）：
   - **「通用」**：勾选 **开机自动启动**（原生 `SMAppService`），点击 **「打开辅助功能设置...」** 开启 `SW Tools` 权限（启用全局双击 `⌘`、自动粘贴、Snippets 展开与窗口分屏）。
   - **「别名与工具」**：点击 **「定位 Finder 工具栏图标」**，按住 `⌘` 将 `SW Terminal.app` 拖到 Finder 顶部工具栏。
   - **「窗口与菜单栏」**：按住 `⌘` 将需要隐藏的菜单栏图标拖到 `|` 分隔符左侧，点击折叠图标即可一键收起/展开。

4. **可选依赖**（如已放置 `~/.local/bin/pngquant` 可略过）：
   - 如需对透明 PNG 进行更高压缩比处理，可按需安装 `pngquant`（放入 `~/.local/bin/` 或 `brew install pngquant`）。

> [!IMPORTANT]
> 每次执行 `./build.sh` 本地重新编译签名后，若全局双击 `⌘` 暂时无响应，请在「系统设置 → 隐私与安全性 → 辅助功能」中移除并重新勾选/添加一次 `SW Tools`。

### 6.2 核心快捷键与内置指令

| 分类 | 触发方式 / 关键字 | 说明 |
| --- | --- | --- |
| **全局呼出 / 操作** | `双击 ⌘` / `⌥ + O` | 呼出面板 / 直达剪贴板历史；`⌘1~9` 速选、`⌥+Enter` 备选、`Esc` 退出并清空上次操作 |
| **应用与系统设置** | 直接输入名称/拼音/缩写 | 搜本机 App（支持别名如 `wx`/`vsc`/`zd`/`sz` 等）与系统设置面板，按使用频次智能加权 |
| **文件搜索** | `f 关键词` 或 `空格 + 关键词` | 极速搜索本机文件（回车打开、`⌥+Enter` 在 Finder 定位） |
| **剪贴板历史** | `cb [关键词]`、`cc` | 支持文本与图片双栏大图预览（`⌘P` 置顶、`⌘⌫` 单条删除）；`cc` 清空未置顶或 `⌥+Enter` 全清 |
| **Snippets 展开** | `sn [关键词]` / 全局触发词 | 全局任意输入框打字自动展开（默认含 `;mail`、`;gh`、`!date`、`!now`） |
| **Finder / 终端联动** | `nf`、`nfo`、`code`、`ft`、`tf` | 当前 Finder 目录新建文件并打开、VS Code 打开选中项、Ghostty ↔ Finder 目录互跳 |
| **开发与实用工具** | `enc`、`json`、`uuid`、`ts`、`color`、`tr`、`short`、`qr`、`kill`、`bm` | 编解码/哈希、JSON 格式化、时间戳、颜色转换、中英翻译、短链、弹窗内高清二维码、结束进程、Chrome 书签 |
| **网页搜索** | `gg` / `gh` / `mdn` / `npm` / `bili` + 词 | 与原 Alfred 精简版 5 条快捷搜索保持一致 |

### 6.3 配置与同步目录
- **本地运行缓存**：`~/.config/quickbar/`（`clipboard.json`、`clipboard_images/`、`usage.json`、`apps_cache.json`）
- **OneDrive 跨机同步配置**：`~/OneDrive/quickbar config/`
  - `snippets.json`（文本片段与全局触发词）
  - `websearches.json`（自定义网页搜索关键词）
  - `settings.json`（工具配置）

---

## 第 7 步：Node.js 环境配置（`fnm` 替代 `nvm`）

原 `nvm` 已被 Rust 编写的 **`fnm`**（`v1.39.0`）替代，`.zshrc` 中已配置 `eval "$(fnm env --use-on-cd --shell zsh)"`，切换进入含 `.nvmrc` 或 `.node-version` 的目录会自动切换版本。在 Intel Mac 上 `fnm` 会自动拉取 `darwin-x64` 架构的 Node 二进制包。

```bash
# 下载 fnm 预编译二进制至 ~/.local/bin/fnm 后执行：
fnm install --lts
fnm default lts-latest

# 核对版本与架构（当前本机输出）
node -v               # v26.13.1
node -p process.arch  # x64
npm -v                # 11.20.0
```
> 注：如需切换国内镜像，可按需临时或全局设置 `npm config set registry https://registry.npmmirror.com`。

---

## 第 8 步：写作与图床配置（Typora + PicGo）

1. **图床仓库**：GitHub 私有/公开图床仓库 `SuvinLin/picBed`（分支 `master`，Fine-grained PAT 仅授予 Contents 读写权限）。
2. **PicGo 配置**：
   - 图床类型：GitHub
   - 自定义域名（CDN）：`https://fastly.jsdelivr.net/gh/SuvinLin/picBed@master`
   - 代理：`http://127.0.0.1:7890`
   - 上传设置：开启「时间戳重命名」。
3. **Typora 偏好设置 → 图像**：
   - 插入图片时：**上传图片**
   - 上传服务：**PicGo.app**
   - 验证「验证图片上传选项」通过即可写作时自动贴图上传。

---

## 第 9 步：当前已安装软件总清单（按分类整理）

以下为当前 `/Applications`（及 `~/Applications`）中全部已装软件（重装下载安装包时均优先选 **Intel / `x64` / Universal** 版本）：

| 分类 | 已安装应用 | 备注 |
| --- | --- | --- |
| **开发与 AI 编程** | `Visual Studio Code`、`Antigravity`、`Trae CN`、`HBuilderX`、`MiniMax Code`、`WorkBuddy`、`wechatwebdevtools`、`Ghostty`、`Termius`、`Charles` | 主力代码、小程序调试、终端、SSH 与抓包 |
| **AI 助手** | `ChatGPT`、`Doubao`（含 `~/Applications/Doubao Apps`） | 桌面端 AI 工具 |
| **效率与系统工具** | `SW Tools`、`Mole`、`Snipaste`、`Status Trio`、`FlClash`、`MacZip`、`Tiny Image`、`LocalSend`、`UURemote`、`小米互联服务` | 启动器、清理、截图、状态栏监控、代理、解压、压缩图、局域网/远程互联 |
| **写作与办公** | `PicGo`、`wpsoffice`、`Microsoft OneNote`、`Microsoft Outlook` | 图床工具、文档套件、笔记与邮件 |
| **浏览器、通讯与网盘** | `Google Chrome`、`Safari`、`WeChat`、`QQ`、`Telegram`、`OneDrive`、`aDrive`（阿里云盘）、`BaiduNetdisk_mac`、`QuarkCloudDrive` | OneDrive 承担 `quickbar config` 等配置同步 |
| **影音与下载** | `IINA`、`NeteaseMusic`、`Thunder`（迅雷）、`TunesKit Video Cutter`、`抖音`、`红果免费短剧` | 本地视频播放、音乐与下载剪辑 |

---

## 重装速查 Checklist（新机恢复顺序）

1. 系统初始化 + 安装 **FlClash**（`7890`）+ 登录 Apple 账户 + 安装 **OneDrive** 并建 `~/OneDrive` 软链接。
2. `xcode-select --install`，执行第 2 步系统设置 `defaults` 脚本与腾讯/阿里 DNS。
3. 建立 `~/.local/bin`，放入 `fnm`、`fzf`、`zoxide`、`gh`、`delta`、`mole`，编译安装 `git 2.56.1`；写入 `~/.gitconfig`、`~/.gitignore_global` 与 `~/.ssh/config`。
4. 安装 **Ghostty** + **FiraCode Nerd Font** + **Oh My Zsh**（`myys` + 插件），还原 `~/.config/ghostty/config`、`~/.zprofile`、`~/.zshrc`、`~/.vimrc`。
5. 执行 `fnm install --lts` 配好 Node.js；进入 `~/OneDrive/quickbar config/SWTools-Backup` 执行 `./build.sh` 编译安装 **SW Tools**，并在 `⌘,` 中完成开机自启、辅助功能授权、Finder 工具栏终端与菜单栏收纳设置。
6. 装好开发 IDE、**PicGo**（如写作需要可配合 Markdown 工具）及第 9 步常用软件清单。
