# macOS 重装前注意事项与配置备份清单（Intel `x86_64` 本机实测版）

> **整理日期**：2026-11-26  
> **适用环境**：**Intel 芯片 Mac（`x86_64`）** · `macOS 26.7.3`  
> **配合文档**：重装后的恢复与安装步骤见 [deploy_Sonoma/README.md](./README.md)

---

## 一、重装前必查事项（Checklist）

### 1. 准备“离线救急包”（最重要）
重装刚进桌面时没有代理工具，GitHub 与部分官网资源可能难以直接访问，建议提前拷贝到 **U 盘** 或手机：
- **FlClash 安装包（Intel `x86_64` `.dmg`）** + 节点订阅链接 / 配置文件导出包。
- **OneDrive 安装包**（装好就能立刻同步回 `SW Tools` 源码与全部配置文件）。
- **加密后的敏感配置包**（包含 `~/.ssh`、PicGo Token 等，见第四节脚本）。

### 2. 个人数据与未提交代码核对
- **用户常见数据目录排查**（当前本机重点目录）：
  - `~/Pictures`（当前约 `277 MB`）：确认照片、截图与素材已转存或同步。
  - `~/Documents`（含 `Obsidian Vault` 等笔记目录）、`~/Desktop`、`~/Downloads`、`~/Movies`、`~/Music`。
- **检查本地未提交 / 未推送的 Git 仓库**：
  ```zsh
  # 扫描常见目录中所有 git 仓库的未提交改动与未推送提交
  for d in ~ "$HOME/.gemini/antigravity/scratch"; do
    find "$d" -maxdepth 3 -name ".git" 2>/dev/null | while read g; do
      repo=$(dirname "$g")
      echo "=== $repo ==="
      git -C "$repo" status -s
      git -C "$repo" log --branches --not --remotes --oneline 2>/dev/null
    done
  done
  ```
- **确认 OneDrive 同步完全完成**：确保 `~/Library/CloudStorage/OneDrive-个人/quickbar config/`（含 `SWTools-Backup/`）均已是绿勾同步状态，无待上传文件。

### 3. 聊天记录与本地历史数据
- **WeChat / QQ**：电脑端记录不会随 App 自动保留，重装前务必在**手机端备份**或使用电脑端「迁移与备份」功能备份聊天记录。
- **SW Tools 剪贴板历史（按需保留）**：`snippets.json` / `websearches.json` / `settings.json` 会同步到 OneDrive，但本地剪贴板历史 `~/.config/quickbar/clipboard.json`、`clipboard_images/` 与使用频次 `usage.json` 仅存本地，需要留存则一并备份。

### 4. 凭证、密钥与授权核对
- **GitHub**：确认密码 + **2FA 恢复码** + **PicGo 图床用 Fine-grained PAT** + `gh auth` 登录状态。
- **SSH 私钥**：备份 `~/.ssh/id_ed25519`、`id_ed25519.pub` 与 `~/.ssh/config`。
- **Charles 根证书 / License**：如需抓包，导出 Charles 配置与 `ca/` 目录。
- **未同步到 iCloud 的密码**：检查「密码 / 钥匙串访问」及 Chrome 密码管理器，确认无仅存本机的登录凭据。

### 5. 系统与设备解绑（抹盘前必做）
- 在「系统设置 → Apple 账户 → iCloud」中关闭 **查找我的 Mac**，并退出 Apple ID。
- 若有按设备数量限制授权的软件，提前在旧系统内注销/解绑本机。
- 若计划使用 U 盘抹盘重装，提前插入 U 盘并验证安装器启动盘可被 `⌥` 启动菜单识别。

---

## 二、已安装软件配置备份与导出全景清单

### A 类：已自动云同步（重装登录账号即可恢复，基本无需导出）

| 软件 | 同步方式与保留内容 | 额外注意事项 |
| --- | --- | --- |
| **SW Tools** | **OneDrive** 自动同步 `quickbar config/`（`settings.json`、`snippets.json`、`websearches.json`）及源码 `SWTools-Backup/` | 本地 `clipboard.json`、`usage.json` 不上云，按需自备（见 B 类） |
| **Google Chrome** | Google 账号同步书签、密码、历史、扩展列表 | **Tampermonkey** 脚本与 **Infinity** 新标签页布局建议额外手动导出（见 B 类） |
| **Visual Studio Code** | GitHub / Microsoft 账号开启 Settings Sync | 建议同时导出本地 `Code/User/` 与扩展列表做双保险 |
| **Trae CN** | 登录账号自动同步基础设置 | 同时可备份本地 `Trae CN/User/` 兜底 |
| **Termius** | Termius 账号云同步 Hosts 与 Keys | 免费版如有本地 Hosts，建议软件内 `Export` 备份一份 |
| **OneDrive** / **aDrive** / **百度网盘** / **夸克网盘** | 账号云端存储 | 重装后重新登录并设置同步目录即可 |
| **OneNote** / **Outlook** / **WPS Office** | 微软 / WPS 账号云同步文档与邮件 | 检查本地未上传到云端的草稿/文档 |
| **Telegram** / **ChatGPT** / **Doubao** / **网易云音乐** | 账号云同步会话、歌单与设置 | 确认浏览器/软件本身登录凭据未丢失即可 |

---

### B 类：支持导出配置文件 / 需拷贝本地路径（强烈建议备份）

#### 1. 代理与抓包工具
| 软件 | 导出 / 备份方式 | 本机实测配置路径 | 含敏感信息 |
| --- | --- | --- | --- |
| **FlClash** | 软件内 **「备份与恢复」**（支持导出本地备份或 WebDAV） | `~/Library/Application Support/com.follow.clash/`（含 `config.yaml`、`profiles/`、`database.sqlite`） | ⚠️ **是**（含机场订阅链接与节点密钥） |
| **Charles** | 软件内 `Tools → Import/Export Settings` 导出 `.xml` | `~/Library/Preferences/com.xk72.charles.config`<br>`~/Library/Application Support/Charles/`（含 `ca/`、`profiles/`、`passwords.keystore`） | ⚠️ **是**（含 CA 私钥/密码） |

#### 2. 效率、图床与系统工具
| 软件 | 导出 / 备份方式 | 本机实测配置路径 | 含敏感信息 |
| --- | --- | --- | --- |
| **PicGo** | 直接拷贝 `data.json`（含 `picBed` 图床与 CDN 配置） | `~/Library/Application Support/picgo/data.json` | ⚠️ **是**（内含 GitHub PAT 明文） |
| **Snipaste** | 软件首选项内导出配置，或直接拷贝 `config.ini` | `~/.snipaste/config.ini` | 否 |
| **SW Tools（本地状态）** | 直接拷贝目录（可选，保留剪贴板历史与使用频次） | `~/.config/quickbar/`（`clipboard.json`、`clipboard_images/`、`usage.json`） | 🟡 **可能含复制过的敏感文本** |
| **Status Trio** | 直接拷贝应用支持目录（菜单栏监控排版配置） | `~/Library/Application Support/StatusTrio/` | 否 |
| **Mole** | 拷贝配置目录（清理白名单等） | `~/.config/mole/` | 否 |

#### 3. 终端、Shell、Git 与开发环境
| 工具 / 软件 | 导出 / 备份方式 | 本机实测配置路径 | 含敏感信息 |
| --- | --- | --- | --- |
| **Ghostty** | 拷贝配置文件 | `~/.config/ghostty/config` | 否 |
| **Zsh + Oh My Zsh + Vim** | 拷贝配置文件与自定义主题 | `~/.zshrc`、`~/.zprofile`、`~/.vimrc`<br>`~/.oh-my-zsh/custom/themes/myys.zsh-theme` | 否 |
| **字体** | 备份已安装的 Nerd Font（或重装再下载） | `~/Library/Fonts/FiraCodeNerdFont-*.ttf` | 否 |
| **Git** | 拷贝全局 Git 配置（多已记录在文档中） | `~/.gitconfig`、`~/.gitignore_global` | 否 |
| **GitHub CLI (`gh`)** | 建议重装后 `gh auth login` 重新授权；也可打包目录 | `~/.config/gh/` | ⚠️ **是**（含 OAuth 配置/状态） |
| **SSH 密钥** | 打包整个 `.ssh` 目录，恢复后需执行 `chmod 600 ~/.ssh/id_ed25519` | `~/.ssh/`（`id_ed25519`、`id_ed25519.pub`、`config`） | ⚠️ **高敏**（私钥） |

#### 4. IDE、AI 工具与浏览器扩展
| 软件 | 导出 / 备份方式 | 本机实测配置路径 | 含敏感信息 |
| --- | --- | --- | --- |
| **VS Code（本地兜底）** | 备份 `User/` + 命令行导出扩展列表：<br>`code --list-extensions > vscode-extensions.txt` | `~/Library/Application Support/Code/User/`（`settings.json`、`keybindings.json`、`snippets/`） | 否 |
| **Antigravity** | 备份自定义 Skills 与用户配置 | `~/.gemini/config/`、`~/Library/Application Support/Antigravity/User/` | 🟡 视 MCP/配置而定 |
| **Trae CN** | 备份 `User/` 目录 | `~/Library/Application Support/Trae CN/User/` | 否 |
| **HBuilderX** | 备份插件与用户配置 | `~/Library/Application Support/HBuilder X/` | 否 |
| **AI CLI 配置**（按需） | 若配有自定义 rules / skills / MCP，可拷出相关配置（不含登录缓存） | `~/.claude/`、`~/.codex/`、`~/.minimax/`、`~/.workbuddy/` | ⚠️ **含登录 Token/Key**，切勿公开上传 |
| **Chrome 扩展单独备份** | **Tampermonkey**：管理面板 → 实用工具 → 导出 `.zip/.txt`<br>**Infinity**：设置 → 备份与恢复 → 导出 `.infinity` | 历史已有备份参考：`~/OneDrive/Mac/infinityBackup-*.infinity`（`tampermonkey` 备份在 `SuvinLin/private/tools/`） | 🟡 部分油猴脚本可能含个人设置 |

---

### C 类：无需导出配置（重装后直接装包即可）

以下软件配置极简或完全无本地配置需求，重装后直接安装使用即可：
- **效率 / 实用工具**：`MacZip`、`Tiny Image`、`LocalSend`、`UURemote`、`小米互联服务`、`IINA`（若有自定义快捷键可在偏好内确认，默认开箱即用）。
- **影音 / 下载**：`Thunder`（迅雷）、`TunesKit Video Cutter`、`抖音`、`红果免费短剧`。
- **开发工具（账号态）**：`wechatwebdevtools`（微信开发者工具，扫码登录即可）、`MiniMax Code`、`WorkBuddy`（登录账号恢复）。

---

## 三、敏感配置 vs 非敏感配置分离原则

> [!CAUTION]
> **切勿将任何包含 Token、SSH 私钥或机场订阅的配置文件推送到公开 GitHub 仓库（如 `SuvinLin/private` 当前可见性为 public，`isPrivate: false`）！**

- **可放文档 / 普通云盘 / Git 仓库（非敏感）**：
  - `~/.zshrc`、`~/.zprofile`、`~/.vimrc`、`~/.gitconfig`、`~/.gitignore_global`、`~/.config/ghostty/config`、`~/.snipaste/config.ini`、`VS Code` 的 `settings.json` / `keybindings.json` / 扩展清单、`~/.oh-my-zsh/custom/themes/myys.zsh-theme`。
- **必须加密压缩或仅存离线 U 盘 / 私有保管箱（高敏感）**：
  - `~/.ssh/`（私钥）
  - `~/Library/Application Support/picgo/data.json`（GitHub PAT）
  - `~/Library/Application Support/com.follow.clash/`（FlClash 订阅配置）
  - `~/Library/Application Support/Charles/` 与 `com.xk72.charles.config`（CA 私钥与证书）
  - `~/.config/gh/`、`~/.claude/`、`~/.codex/`、`~/.gemini/` 中的凭据文件

---

## 四、一键整理本地备份包脚本（分开打包普通与加密配置）

重装前在终端执行下面脚本，会在桌面生成：
1. **`~/Desktop/mac-backup-public/`**：所有非敏感配置文件与列表（可直接放 OneDrive）。
2. **`~/Desktop/mac-backup-secrets.zip`**：**加密压缩包**（运行过程中会提示设置解压密码，**只放入 U 盘，不要直接明文传公网**）。

```zsh
#!/bin/zsh
set -euo pipefail

PUB="$HOME/Desktop/mac-backup-public"
SEC_TMP="$HOME/Desktop/mac-backup-secrets-tmp"
rm -rf "$PUB" "$SEC_TMP" "$HOME/Desktop/mac-backup-secrets.zip"
mkdir -p "$PUB"/{shell,ghostty,git,snipaste,vscode,antigravity,statustrio,mole,fonts}
mkdir -p "$SEC_TMP"/{ssh,picgo,flclash,charles,quickbar-local}

# 1) 非敏感配置
cp -a ~/.zshrc ~/.zprofile ~/.vimrc "$PUB/shell/" 2>/dev/null || true
cp -a ~/.oh-my-zsh/custom/themes/myys.zsh-theme "$PUB/shell/" 2>/dev/null || true
cp -a ~/.config/ghostty/config "$PUB/ghostty/" 2>/dev/null || true
cp -a ~/.gitconfig ~/.gitignore_global "$PUB/git/" 2>/dev/null || true
cp -a ~/.snipaste/config.ini "$PUB/snipaste/" 2>/dev/null || true
cp -a "$HOME/Library/Application Support/Code/User/"{settings.json,keybindings.json,snippets} "$PUB/vscode/" 2>/dev/null || true
command -v code >/dev/null 2>&1 && code --list-extensions > "$PUB/vscode/extensions.txt" 2>/dev/null || true
cp -a ~/.gemini/config "$PUB/antigravity/" 2>/dev/null || true
cp -a "$HOME/Library/Application Support/Antigravity/User/"{settings.json,keybindings.json} "$PUB/antigravity/" 2>/dev/null || true
cp -a "$HOME/Library/Application Support/StatusTrio/"* "$PUB/statustrio/" 2>/dev/null || true
cp -a ~/.config/mole/* "$PUB/mole/" 2>/dev/null || true
cp -a ~/Library/Fonts/FiraCodeNerdFont-*.ttf "$PUB/fonts/" 2>/dev/null || true

# 2) 敏感凭证与本地状态（将进行加密压缩）
cp -a ~/.ssh/* "$SEC_TMP/ssh/" 2>/dev/null || true
cp -a "$HOME/Library/Application Support/picgo/data.json" "$SEC_TMP/picgo/" 2>/dev/null || true
cp -a "$HOME/Library/Application Support/com.follow.clash/"{config.yaml,profiles,database.sqlite} "$SEC_TMP/flclash/" 2>/dev/null || true
cp -a "$HOME/Library/Preferences/com.xk72.charles.config" "$SEC_TMP/charles/" 2>/dev/null || true
cp -a "$HOME/Library/Application Support/Charles/"{ca,profiles,passwords.keystore} "$SEC_TMP/charles/" 2>/dev/null || true
cp -a ~/.config/quickbar/{clipboard.json,usage.json,clipboard_images} "$SEC_TMP/quickbar-local/" 2>/dev/null || true

# 加密打包敏感目录（会交互提示输入压缩密码）
(cd "$HOME/Desktop" && zip -er mac-backup-secrets.zip mac-backup-secrets-tmp)
rm -rf "$SEC_TMP"

echo "完成："
echo "  普通配置目录：$PUB"
echo "  加密敏感包：  $HOME/Desktop/mac-backup-secrets.zip"
```

### 恢复敏感包后的权限提醒
新系统解压 `mac-backup-secrets.zip` 并放回 `~/.ssh/` 后，务必修正私钥权限，否则 SSH 会拒绝连接：
```zsh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub ~/.ssh/config
```
