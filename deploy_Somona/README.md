# macOS 14 重装后配置清单 & 文档更新记录

> 来源：[SuvinLin/private/deploy](https://github.com/SuvinLin/private/tree/master/deploy)（共 9 篇文档，原基于 macOS 10.14 Mojave 编写）
> 当前环境：**macOS 14.8.9（Intel `x86_64`）**

## 执行进度总览

| 序号 | 原文档 | 状态 | 说明 |
| --- | --- | --- | --- |
| 1 | `1 - 重装mac OS.md` | ✅ 已完成 | 当前已重装为 macOS 14.8.9 |
| 2 | `2 - 系统设置.md` | ✅ 已完成 | 系统设置、DNS（腾讯/阿里）、`mac tips` 已完成，iCloud 登录已通 |
| 3 | `3 - vim设置.md` | ➡️ 已合并至第 5 步 | 已整合进终端配置环节，直接写入 `~/.vimrc` |
| 4 | `4 - 安装 Homebrew.md` | ✅ 已定稿（去包管理器方案） | 放弃 Homebrew 与 MacPorts；GUI 官网直装，现代 CLI 工具直接下载官方预编译二进制放 `~/.local/bin/` |
| 5 | `5 - iTerm安装与设置.md` | ✅ 已完成 | 含原第 3 步 `~/.vimrc`、Oh My Zsh + `myys`、插件、`zoxide v0.10.0` + `fzf v0.75.0`、`FiraCode Nerd Font`、Dracula 与 iTerm2 全部首选项 |
| 6 | `6 - Git的安装与设置.md` | ✅ 已完成 | `git v2.56.1` + `gh v2.105.0` + `delta 0.18.2`；`ssh -T git@github.com` 实测返回 `Hi SuvinLin!` |
| 7 | `7 - Alfred4设置.md` | ✅ 已完成 | 升级为 Alfred 5（含 Powerpack）；默认结果、5 条精简 Web Search、iTerm2 Terminal 脚本、快捷键、Dark 主题、自启与 Chrome 书签已全部配妥 |
| 8 | `8 - Typora & PicGo 设置.md` | ✅ 已完成 | PicGo → GitHub `SuvinLin/picBed` + fastly.jsDelivr；Typora 调用 PicGo.app 上传，已实测 |
| 9 | `9 - node 环境配置.md` | ✅ 已完成 | 已 `fnm install --lts`（Node v26.13.1 / npm 11.20.0）并设为默认； `nvm` 替换为 `fnm`（`v1.39.0` 已安装至 `~/.local/bin/fnm`，第 9 步直接 `fnm install --lts`） |

---

## 初步发现的文档过时 / 需调整项（待执行时逐一确认补充）

### 1. `1 - 重装mac OS.md`
- 仅包含 Catalina 及更早版本的 `createinstallmedia` 命令，缺少 macOS 14 (Sonoma) 及 Apple Silicon「抹掉所有内容和设置」方式。
- 文末链接仍指向旧用户名 `suvin7`。

### 2. `2 - 系统设置.md` & `tools/mac tips.md`
- 原文档基于 macOS 10.14 Mojave，macOS 14「系统设置」UI 与菜单路径变化很大（例如触控板三指拖移在「辅助功能 → 指针控制 → 触控板选项」、密码锁定在「锁定屏幕」、节能合并进「电池 / 锁定屏幕」等）。
- 依赖的 `Brooklyn.saver` 需确认是否兼容 macOS 14。
- **`tools/mac tips.md` 排查结果**（共 6 条命令）：
  - **Finder 顶部显示路径**（`defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES`）：仍有效，但原文档漏了 `killall Finder` 使之立即生效。
  - **调整 Launchpad 行列数**：原文档注释把 `springboard-rows`（行数）和 `springboard-columns`（列数）写反了。
  - **屏蔽系统更新红点**（`AttentionPrefBundleIDs 0`）：新版本 macOS 上基本已失效，且原文档未写 `killall Dock`。
  - **充电提示音**（`PowerChime`）：可按需选择开关。
  - **iOS 断连修复**（`sudo killall -STOP -c usbd`）：属于临时排障命令，重装时无需执行。

### 3. `3 - vim设置.md`
- 基本可用，暂无明显过时项。

### 4. `4 - 安装 Homebrew.md`
- 若为 Apple Silicon Mac，安装路径变为 `/opt/homebrew`，安装后需执行 `brew shellenv` 写入 `~/.zprofile`。

### 5. `5 - iTerm安装与设置.md`
- `brew cask install iterm2` 已废弃，应改为 `brew install --cask iterm2`。
- macOS 14 默认 Shell 已是 `zsh`，无需 `chsh -s /bin/zsh`。
- 字体 `FiraCode` 可直接用 `brew install --cask font-fira-code` 安装。
- `Dracula.itermcolors` 是 iTerm2 配色文件，放到 `~/.oh-my-zsh/custom/themes` 并不起作用，应直接导入 iTerm2。
- 第 5 步和第 9 步重复写了「安装 nvm」。
- `.zshrc` 中部分 alias 已过时（如 `vue init webpack`、`create-react-app`、`spctl --master-disable` 在新系统有变化），代理端口 `7890` 需与当前代理软件核对。

### 6. `6 - Git的安装与设置.md`
- Apple Silicon 下 Homebrew 的 Git 路径为 `/opt/homebrew/bin/git`（非 `/usr/local/bin/git`）。
- SSH 密钥 GitHub 现推荐使用 `ed25519`（`ssh-keygen -t ed25519 -C "..."`）替代 `rsa -b 4096`。
- `~/.ssh/config` 中代理端口为 `51837`，与第 5 步 `.zshrc` 的 `7890` 不一致，需统一。

### 7. `7 - Alfred4设置.md` → 升级为 `7 - Alfred 5 设置.md`
- Alfred 已升级为 **Alfred 5**（已含 Powerpack）；建议在 `Advanced → Syncing` 开启配置同步（如同步至 iCloud Drive），日后重装免重配。
- 精简清理过时的国内娱乐/购物搜索项，并修复 GitHub 搜索多余 `&amp;ref=opensearch`，补齐每条搜索触发关键词。

### 8. `8 - Typora & PicGo 设置.md`
- ✅ 已完成 – PicGo 使用 GitHub 图床 `SuvinLin/picBed`（`master`，Fine-grained token，仅 Contents 读写），CDN `https://fastly.jsdelivr.net/gh/SuvinLin/picBed@master`，代理 `127.0.0.1:7890`，时间戳重命名；Typora 偏好已设为 `image_uploader=picgo-app`、`allow_image_upload=1`。上传已实测成功。
- `cdn.jsdelivr.net` 在国内网络环境下稳定性较差，可考虑替代方案或确认是否仍使用。

### 9. `9 - node 环境配置.md`
- 原 `nvm install node --lts` 已替换为 Rust 实现的 **`fnm`**（秒级启动，兼容 `.nvmrc` / `.node-version`）：已从官方 GitHub Release 安装 `v1.39.0` 预编译二进制至 `~/.local/bin/fnm`，`.zshrc` 已预置 `eval "$(fnm env --use-on-cd --shell zsh)"`，第 9 步直接执行 `fnm install --lts`。

---

## 执行过程中的新增记录（随时补充）

### 第 2 步（系统设置 & `mac tips.md`）
- **可脚本化优化（建议补充进 GitHub 文档）**：以下系统设置已通过命令行直接写入：
  - 触控板轻点点按 + 三指拖移
  - 触发角左上角启动屏幕保护程序（`com.apple.dock wvous-tl-corner -int 5`）
  - 屏保闲置永不 + 锁屏立即输密码（`com.apple.screensaver`）
  - 键盘关闭自动纠错/大写/句点/智能引号/破折号（`NSGlobalDomain`）
  - 菜单栏电池显示百分比（`defaults -currentHost write com.apple.controlcenter BatteryShowPercentage -bool true`）
- **DNS 调整**：由原 `8.8.8.8` 改为腾讯 DNS（`119.29.29.29`）和阿里 DNS（`223.5.5.5`），可用 `sudo networksetup -setdnsservers "Wi-Fi" 119.29.29.29 223.5.5.5`。
- **Finder 顶部显示路径**：已执行并生效（文档需补充 `killall Finder`）。
- **Launchpad 排列**：已设为 **7 行 7 列** 并重启 Dock 生效（文档需修正 `rows`/`columns` 写反的注释）。
- **充电提示音（`PowerChime`）**：`defaults write com.apple.PowerChime ChimeOnAllHardware -bool true` 已写入（值为 `1`），但 macOS 14 上直接 `open /System/Library/CoreServices/PowerChime.app` 会报 `RBSRequestErrorDomain Code=5 / Launchd job spawn failed`，需重启或插拔电源验证是否已生效，文档需更新说明。
- **菜单栏时钟**：macOS 14 已不支持完全隐藏，文档需更新。
- **本机架构说明**：当前为 Intel（`x86_64`），后续文档可同时注明 Intel（`/usr/local`）与 Apple Silicon（`/opt/homebrew`）差异。
- **iCloud 登录踩坑**：系统开启 HTTP/SOCKS 全局代理且忽略列表未放行 `*.apple.com`/`*.icloud.com` 时，iCloud 登录极易卡住或失败；可临时关闭代理或先登录 App Store 后重试。
- **CommandLineTools 混装问题**：若此前残留旧版文件导致 `/Library/Developer/CommandLineTools/usr/include/swift/` 同时存在 `module.modulemap` 与 `bridging.modulemap`（触发 Swift 编译报错），可通过 `sudo rm -rf /Library/Developer/CommandLineTools && xcode-select --install` 彻底重装修复。

### 第 4 步（去 Homebrew / MacPorts 方案：预编译二进制 + 官网直装 + Mole）
- **最终方案**：在 macOS 14 Intel 上放弃 Homebrew 与 MacPorts，不再维护重型包管理器：
  1. **系统自带已够用**：`vim`、`/bin/zsh`（5.9）、`/usr/bin/git`（`2.39.5 Apple Git-154`）。
  2. **GUI 应用**：iTerm2、FlClash、Alfred、Typora 等直接官网/Release 下载安装。
  3. **现代单文件 CLI 工具**：统一将官方预编译 Release 二进制放入 `~/.local/bin/`（并在 `.zprofile` / `.zshrc` 加入 `PATH`），免 `sudo`、免编译、零依赖：
     - `zoxide v0.10.0`、`fzf v0.75.0`、`fnm v1.39.0`
     - `Mole CLI v1.73.0`（官方脚本 `install.sh --prefix "$HOME/.local/bin"`）+ 自建原生 macOS 图形端 `Mole.app`（`dist/Mole.app`）与 Antigravity Mole Skill（`~/.gemini/config/skills/mole/`）。
- **MacPorts 实测踩坑记录（为什么不建议装）**：
  - macOS 14 Intel 上安装 `zoxide` / `fnm` 时，因缺少预编译归档包直接触发本地从源码编译整个 `rust` + LLVM（耗时数小时），仅前置 C/C++ 编译依赖（`cmake`/`icu`/`autoconf` 等）就占了 `898 MB` `/opt/local` 空间。
  - 若以后确实需要 MacPorts，务必加 `-b` 参数（`sudo port install -b <pkg>`）强制只用二进制包；且 `/opt/local/etc/macports/sources.conf` 中添加清华 TUNA 源时必须注释原官方 `[default]` 行防止重复警告。

### 第 5 步（iTerm2 + Zsh + Vim 合并）
- **结构调整**：原第 3 步（`~/.vimrc`）直接合并进本步一起完成；原第 5 步中的「安装 nvm」与第 9 步重复，已从本步移除并由 `fnm` 替代归入第 9 步。
- **无需 `chsh`**：macOS 14 默认 Shell 已是 `/bin/zsh`（zsh 5.9），文档中 `chsh -s /bin/zsh` 可删去或改为提示性说明。
- **Oh My Zsh & `myys` 主题**：通过 `RUNZSH=no CHSH=no KEEP_ZSHRC=yes` 安装 Oh My Zsh，并将去掉 `@%m` 的 `myys.zsh-theme` 放入 `~/.oh-my-zsh/custom/themes/`（避免污染仓库 `themes/` 目录导致 `omz update` 冲突）。
- **替代 `autojump` 为 `zoxide` + `fzf`**：`autojump` 自 2016 年停更且依赖 Python，改用 Rust 预编译版 **`zoxide v0.10.0`** + **`fzf v0.75.0`**（支持 `z`/`zi` 跳转与 `Ctrl+R`/`Ctrl+T`/`Alt+C` 模糊检索），`.zshrc` 中通过 `eval "$(zoxide init zsh)"` 与 `source <(fzf --zsh)` 启用，并保留 `zsh-autosuggestions` 与 `zsh-syntax-highlighting`（后者置于 `plugins` 末尾）。
- **字体与配色修正**：
  - 字体升级为 **FiraCode Nerd Font**（从 Nerd Fonts Release 下载标准 `FiraCodeNerdFont-*.ttf` 至 `~/Library/Fonts/`），同时保留连字并补齐终端图标，替换原普通版 `FiraCode`。
  - `Dracula.itermcolors` 为 iTerm2 配色文件，放入 `~/.config/iterm2/Dracula.itermcolors` 并通过 `open` 导入（修正原文档误放入 `~/.oh-my-zsh/custom/themes` 的问题）。
- **iTerm2 首选项自动化**：已通过 `defaults import com.googlecode.iterm2` 写入全部偏好（`QuitWhenAllWindowsClosed=true`、`PromptOnQuit=false`、`OnlyWhenMoreTabs=false`、`TabStyleWithAutomaticOption=5 (Minimal)`、`WindowNumber=false`、`Default` Profile 应用 `Dracula` 配色、`FiraCodeNF-Reg 13` + `ASCII Ligatures`、`Transparency=0.12`、以及 `⌥←`/`⌥→` 映射到 `Esc+ b`/`Esc+ f`）。
- **`.zshrc` 更新**：保留 FlClash 代理配置（`7890` 端口、`proxy_on`/`proxy_off`）与 `~/.local/bin` PATH；预置 `fnm env --use-on-cd --shell zsh`（替代 `nvm`）；清理了原 Homebrew / MacPorts 及过时前端脚手架 alias。

### 第 6 步（Git v2.56.1 + `gh` + `delta` + SSH `ed25519`）
- **Git 最新版安装（免 `sudo`、去 Homebrew）**：拉取 Git 官方稳定版 `v2.56.1` 源码并指定 `NO_GETTEXT=1 NO_TCLTK=1 NO_RUST=1 APPLE_COMMON_CRYPTO=YesPlease RUNTIME_PREFIX=1` 编译安装至 `~/.local/bin/git`（注：`2.56+` 默认开启可选 Rust 组件，无 `cargo` 时必须加 `NO_RUST=1`；同时传入相同 flag 编译 `contrib/credential/osxkeychain` 凭据助手至 `~/.local/libexec/git-core/`），自动优先于系统自带 `/usr/bin/git`（`2.39.5`）。
- **新增现代 Git 生态工具（放入 `~/.local/bin/`）**：
  - `gh v2.105.0`（GitHub CLI，可用于命令行登录并自动上传 SSH 公钥、建仓库、提 PR）。
  - `delta 0.18.2`（`git-delta` 美化 diff 配色为 `Dracula` 并启用行号；注：`0.19.0+` 起官方已停止发布 Intel `x86_64-apple-darwin` 预编译包，Intel Mac 固定使用 `0.18.2`）。
- **`.gitconfig` 现代全局项**：除原文档 `user.name`/`user.email`/`core.quotepath`/`i18n.*` 外，新增 `init.defaultBranch main`、`push.autoSetupRemote true`、`fetch.prune true`、`core.autocrlf input`、`credential.helper osxkeychain`、`merge.conflictStyle zdiff3`、`diff.colorMoved default` 及 `delta` 集成。
- **`.gitignore_global` 精简**：移出原文档中的 `node_modules`（改由各项目自身 `.gitignore` 管理，避免掩盖漏配），保留 `.DS_Store`、`._*`、`.Spotlight-V100`、`.Trashes`、`.vscode/`、`.idea/`、`*.swp`、`*.swo`。
- **SSH 密钥与代理修正**：
  - 由 `rsa -b 4096` 升级为 **`ed25519`**（`~/.ssh/id_ed25519`）。
  - `~/.ssh/config` 加入 `AddKeysToAgent yes` + `UseKeychain yes`，并改为 `HostName ssh.github.com` + `Port 443` 即可稳定握手连通；实测在 FlClash 开启 TUN/代理时若再强加 `ProxyCommand nc` 走 `127.0.0.1:7890`（无论是 SOCKS5 还是 HTTP CONNECT）会触发 `kex_exchange_identification: Connection closed`，因此将 `ProxyCommand nc -X 5 -x 127.0.0.1:7890 %h %p`（已去掉冗余 `-v`）改为注释备用。

### 第 7 步（Alfred 5 设置）
- **快捷键与更新**：`hotkey/prefs.plist` 为 `⌘ 双击`（`double tap`），`SUEnableAutomaticChecks` 与 `SUAutomaticallyUpdate` 保持为 `False`。
- **Default Results**（`features/defaultresults/prefs.plist`）：已勾选 Folders / TextFiles / Archives / Documents / Images / Contacts / Preferences，取消勾选 `showAppleScripts`。
- **Web Search 优化（精简为 5 条，写入 `features/websearch/prefs.plist`）**：新增 `gg`、`npm`，移除 `bd`/`bk`/`zh`/`jd`/`tb`/`db`/`zdm`/`wb`/`优酷`/`爱奇艺`，修正 GitHub URL 并补全触发关键词：
  - `gg`（Google）、`gh`（GitHub 仓库）、`mdn`（MDN）、`npm`（npm 包）、`bili`（Bilibili）。
- **Terminal / Shell**（`features/terminal/prefs.plist`）：`application = 1 (Custom)`，写入 iTerm2 AppleScript 脚本（`on alfred_script(q)`）。
- **外观主题**：已通过 AppleScript `set theme "Alfred macOS Dark"` 切换为 `Alfred macOS Dark`。
- **界面收尾与建议**：`General → Launch Alfred at login` 与 `Features → Web Bookmarks`（勾选 `Google Chrome Bookmarks`、keyword `bm`）由用户在 Alfred Preferences 界面确认；顺便建议在 `Advanced → Syncing` 开启配置同步并开启 Powerpack 剪贴板历史。
