### 安装

1. ````shell
   brew cask install iterm2
   ````

2. 安装完毕后，输入命令 `chsh -s /bin/zsh` ， 切换终端为 zsh 并退出重开

3. 安装 [oh my zsh](https://github.com/ohmyzsh/ohmyzsh)

4. 安装插件

   - [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

   - [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

5. 下载颜色主题 [draculatheme](https://draculatheme.com/iterm/) ，将下载好的主题中的（Dracula.itermcolors）移入目录（~/.oh-my-zsh/custom/themes）

6. 安装 [nvm](https://github.com/nvm-sh/nvm)



### 设置

#### 改终端用户名

1. ````shell
   cd ~/.oh-my-zsh/themes
   open .
   cp ys.zsh-theme myys.zsh-theme
   vi myys.zsh-theme
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

4. ````
   cd ~
   vi .zshrc
   ````

5. 在 vim 打开的配置文件中修改主题为 myys，保存并退出

6. `source .zshrc`

### 参考链接

​	[oh-my-zsh终端用户名设置](https://www.jianshu.com/p/bf488bf22cba)



#### alias & setting

在 .zshrc 文件末尾键入代码

````
# homebrew alias
# brew
alias b="brew"
alias b-tree="brew deps --installed --tree"

# brew cask
alias bc="brew cask"

# brew tap
alias bt="brew tap"

# brew services
alias bs="brew services"

# git alias
alias ga="git add"
alias gcm="git commit -m"
alias gc="git clone"
alias gs="git status"

# npm alias
alias n="npm"
alias nr="npm run"

# Vue alias
alias cli2="vue init webpack"
alias cli3="vue create"

# permission alias
alias dis="sudo spctl --master-disable"
alias en="sudo spctl --master-enable"

# Apache alias
alias apo="sudo apachectl start"
alias apr="sudo apachectl restart"
alias aps="sudo apachectl stop"

# IOS alias
alias ios="sudo killall -STOP -c usbd"

# proxy alias
alias proxy='export http_proxy=http://127.0.0.1:1087 https_proxy=http://12    7.0.0.1:1087'
alias unproxy='unset http_proxy https_proxy'

# 关闭Homebrew 自动更新
export HOMEBREW_NO_AUTO_UPDATE=true
````



#### 首选项设置

General → Closing → 仅勾选（Quit when ...）



##### Appearance

- General → Theme → Minimal
- Windows → 取消勾选（Show window number ...）



##### Profiles

- 更换主题
- 更换字体
- Window → Transparency（半透明）
- keys → Key Mappings（设置alt ← / → 在单词间移动光标）
  - 找到`⌥ ←`  → 点击 → 将Action选择为（Send Escape ...）→ Esc+ （输入 b）
  - 找到`⌥ →`  → 点击 → 将Action选择为（Send Escape ...）→ Esc+ （输入 f）

##### Keys

- Hotkey →  勾选（Show/hide all ...）
- Keys → HotKey → HotKey改键 （option + i）



