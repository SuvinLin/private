## 安装与替换Git

macOS 系统中 Xcode 自带 Git ，但版本有些过时

若想使用新版本的 Git ，则需要通过安装新版本的 Git 来替代系统默认

<br />

### 通过 Homebrew 安装最新的 Git 版本

**查看 Xcode  Git 版本与指向**

````shell
$ git --version  # git version 2.21.1 (Apple Git-122.3)
$ which git  # /usr/bin/git（该 路径下的 git 是系统默认的 git）
````

**通过 Homebrew 升级到最新的 Git 版本**

````shell
$ brew install git  # 安装最新版本的 Git
$ brew link git  # 将 brew 安装的 git 连接上,并会输入下面两行内容
> Warning: Already linked: /usr/local/Cellar/git/x.xx.x
> To relink: brew unlink git && brew link git
````

**link 成功后，退出终端后，再次打开。然后查看 git 指向和版本信息**

````shell
$ git --version  # git version 2.25.1
$ which git  # /usr/local/bin/git
````

<br />

### 恢复系统默认 Git 的使用

````shell
$ brew unlink git		# 解除 brew 与 git 的连接
$ git --version			# git version 2.21.1 (Apple Git-122.3)
$ which git					# /usr/bin/git
````

<br />

## 初次运行 Git 前的配置

终端键入如下命令

````shell
$ git config --global user.name "yourname"
$ git config --global user.email your@email.com
$ git config --global core.quotepath false  # 解决 git status 中的乱码
$ git config --global i18n.commitEncoding utf-8  # 设置提交时, message 以什么编码保存
$ git config --global i18n.logOutputEncoding utf-8  # 设置查看时, message 以什么编码传递给查看器
````

配置完后，通过 `git config --global --list ` 检查配置

<br />

## SSH

### 生成 SSH 密钥

1. 打开终端粘贴下面的文本（替换为你的 GitHub 电子邮件地址）。

   ```shell
   $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   这将创建以所提供的电子邮件地址为标签的新 SSH 密钥

   ```shell
   > Generating public/private rsa key pair.
   ```

2. 提示“Enter a file in which to save the key”（输入要保存密钥的文件）时，按回车键， 这将接受默认文件位置

   ```shell
   > Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
   ```

3. 在提示时输入安全密码，不设置密码可以连续两个回车

   ```shell
   > Enter passphrase (empty for no passphrase): [Type a passphrase]
   > Enter same passphrase again: [Type passphrase again]
   ```

4. 以上操作完成后，会在第 2 步的文件位置中生成一个 .ssh 文件夹，里面包含两个文件（id_rsa，id_rsa.pub）

<br />

### 新增 SSH 密钥到 GitHub 帐户

1. ````shell
   $ pbcopy < ~/.ssh/id_rsa.pub
   # pbcopy 将公钥文件复制到剪贴板
   # 如果 pbcopy 不可用，可找到隐藏的 .ssh 文件夹
   # cat id_rsa.pub，复制输出的内容
   ````

2. 登录 [Github](https://github.com/)

3. 在任何页面的右上角，单击个人资料照片，然后单击 Settings

4. 在用户设置侧边栏中，单击 SSH and GPG keys

5. 单击 New SSH key

6. 填写 Title 字段并将密钥粘贴到 Key 字段

7. 点击 Add SSH key 按钮进行添加

<br />

### 测试 SSH 连接

1. ````shell
   $ ssh -T git@github.com  # 对 GitHub 尝试 ssh
   ````
   
2. 如果看到如下其中一种提示

   ````shell
   > The authenticity of host 'github.com (IP ADDRESS)' can't be established.
   > RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
   > Are you sure you want to continue connecting (yes/no)?
   ````

   ````shell
   > The authenticity of host 'github.com (IP ADDRESS)' can't be established.
   > RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
   > Are you sure you want to continue connecting (yes/no)?
   ````

3. 输入 **yes** 即可，输出下面信息

   ````shell
   > Warning: Permanently added the RSA host key for IP address 'xx.xx.xx.xx' to the list of known hosts.
   # 该警告会在你的IP首次使用或者变更时，让 git 帮助我们创建 known_hosts 文件并添加 IP 的映射关系，或者直接在 known_hosts 文件中添加IP 的映射关系，不用担心
   
   > Hi username! You've successfully authenticated, but GitHub does not provide shell access.
   ````

4. 看到上面输出的 Hi... 这一段便表示成功了

<br />

### 参考链接

[使用 SSH 连接到 GitHub](https://help.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh)

<br />



### SSH走代理

​	在 github 中使用 `ssh` 进行 `git clone` 到本地，速度慢的简直了。这个时候我们需要对 `ssh` 走代理

````bash
cd ~	
cd .ssh
vi config	# 该文件有则编辑，无则创建

# 在 config 文件中添加如下内容

# Host 必须是 github.com
# 若要走 HTTP 代理，添加下条命令
# ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=8080
# 走 socks5 代理（如 Shadowsocks），添加下条命令
# ProxyCommand nc -v -x 127.0.0.1:1080 %h %p

Host github.com
   HostName github.com
   User git
   ProxyCommand nc -v -x 127.0.0.1:51837 %h %p
````

<br />

## Git  其他设置

### 将 git 中提示信息显示中文

​	git 会默认根据系统的首选语言来设置提示信息

​	若要将其改回英文，在 item2 的 `.zshrc` 中添加配置 `export LANG=en_US.UTF-8`

<br />

### 设置全局忽略文件

>虽然每个项目配 .gitignore 文件可以成功，但是每个项目都需要配就很麻烦。所以可以在git的全局进行配置来忽略.DS_Store等文件

1. ````shell
   $ vi ~/.gitignore_global  # 新建并编辑一个git全局忽略文件
   ````
   
2. 在打开的 vim 中粘贴如下代码

3. ````
   .DS_Store
   node_modules
   .vscode
   .idea
   ````

4. `:wq`保存

5. 接着对 git 进行全局设置，让 git 忽略 .gitignore_global 中的所有文件就大功告成了

   ````shell
   $ git config --global core.excludesfile ~/.gitignore_global
   ````

   <br />

### 参考链接

[Mac中Git忽略.DS_Store文件](https://orianna-zzo.github.io/sci-tech/2018-01/mac%E4%B8%ADgit%E5%BF%BD%E7%95%A5.ds_store%E6%96%87%E4%BB%B6/)

[怎么让 git 忽略 .DS_Store](https://juejin.im/post/5cf674fa518825399965a3ba)



