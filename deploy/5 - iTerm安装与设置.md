## 安装

1. ````shell
   $ brew cask install iterm2
   ````

2. 打开 iTerm

3. 输入命令 `chsh -s /bin/zsh` ， 切换终端为 zsh 并退出重开

4. 安装 [oh my zsh](https://github.com/ohmyzsh/ohmyzsh)

5. 安装插件

   - [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
   - [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
   - [autojump](https://github.com/wting/autojump)
     - 通过 `brew` 安装完成后，在 `.zshrc` 的 `Plugins` 之中添加 `autojump` 字符串

6. 安装字体 [FiraCode](https://github.com/tonsky/FiraCode)

7. 下载颜色主题 [draculatheme](https://draculatheme.com/iterm/) ，并将下载好的主题中（Dracula.itermcolors）文件移入目录（~/.oh-my-zsh/custom/themes）

8. 安装 [nvm](https://github.com/nvm-sh/nvm)

<br />

## 设置

### 改终端用户名（ys主题）

1. ````shell
   $ cd ~/.oh-my-zsh/themes
   $ open .
   $ cp ys.zsh-theme myys.zsh-theme  # 生成一个ys主题的副本
   $ vi myys.zsh-theme
   ````

2. 在 vim 打开的文件找到下面的代码段，**PROMPT**代表用户名配置

   ````
   PROMPT="
   %{$terminfo[bold]$fg[blue]%}#%{$reset_color%} \
   %(#,%{$bg[yellow]%}%{$fg[black]%}%n%{$reset_color%},%{$fg[cyan]%}%n) \
   %{$fg[white]%}@ \		# 删除该行
   %{$fg[green]%}%m \	 # 删除该行
   %{$fg[white]%}in \
   %{$terminfo[bold]$fg[yellow]%}%~%{$reset_color%}\
   ${hg_info}\
   ${git_info}\
   \
   %{$fg[white]%}[%*] $exit_code
   %{$terminfo[bold]$fg[red]%}$ %{$reset_color%}"
   ````

3. 删除不需要的配置并保存

4. ````shell
   $ cd ~
   $ vi .zshrc
   ````

5. 在 vim 打开的配置文件中修改主题为 myys，保存并退出

6. `source .zshrc`使配置生效

<br />

<br />

### 参考链接

​	[oh-my-zsh终端用户名设置](https://www.jianshu.com/p/bf488bf22cba)

<br />

### alias & setting

在 .zshrc 文件的 *# Example aliases* 处粘贴如下代码

````shell
# Git Language Setting
export LANG=en_US.UTF-8

# Terminal Proxy Setting 
# clashX
export ALL_PROXY=socks5://127.0.0.1:7890

# alias 
# reset the DNS cache
alias dns="sudo killall -HUP mDNSResponder"

# homebrew alias
# brew
alias b="brew"
alias b-tree="brew deps --installed --tree"

# brew services
alias bso="brew services start"
alias bsr="brew services restart"
alias bss="brew services stop"

# git alias
alias gs="git status"
alias ga="git add"
alias gcm="git commit -m"
alias gc="git clone"
alias gl="git log"

# npm alias
alias n="npm"
alias nr="npm run"
alias nl="npm ls --depth=0"
alias nl-g="npm ls -g --depth=0"

# Vue CLI alias
alias cli2="vue init webpack"
alias cli3="vue create"

# uni-app alias
alias uni="vue create -p dcloudio/uni-preset-vue"

# React alias
alias react="npx create-react-app"

# application permission alias
alias dis="sudo spctl --master-disable"
alias en="sudo spctl --master-enable"

# application Sign
alias sign="codesign --force --deep --sign - "

# IOS connect alias
alias ios="sudo killall -STOP -c usbd"
````

<br />

#### 首选项设置

##### General

​	Closing → 仅勾选（Quit when ...）

<br />

##### Appearance

- General → Theme → Minimal
- Windows → 取消勾选（Show window number ...）

<br />

##### Profiles

- 更换主题
- 更换字体
- Window → Transparency（半透明）
- keys → Key Mappings（设置alt ← / → 在单词间移动光标）
  - 找到`⌥ ←`  → 点击 → 将Action选择为（Send Escape ...）→ Esc+ （输入 b）
  - 找到`⌥ →`  → 点击 → 将Action选择为（Send Escape ...）→ Esc + （输入 f）
