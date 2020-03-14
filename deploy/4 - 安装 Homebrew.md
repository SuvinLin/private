## 安装 Homebrew

1. 开启代理全局模式，并键入代码到对应的终端配置文件（.bashrc / .zshrc）

   ````shell
   # trojan-qt5 on/off
   function tro() {
   	# 配置http访问的
   	export http_proxy=socks5://127.0.0.1:1080
   	# 配置https访问的
   	export https_proxy=socks5://127.0.0.1:1080
   	# 配置http和https访问
   	export all_proxy=socks5://127.0.0.1:1080
   	echo ' 开启当前终端代理 '
   }
   
   function untro() {
   	# 移除代理
   	unset http_proxy
   	unset https_proxy
   	unset all_proxy
   	echo ' 关闭当前终端代理 '
   }
   ````

2. 开启终端代理  `tro`

3. 测试是否开启命令行代理  `curl ipinfo.io`

4. 安装 Xcode 命令行工具  `xcode-select --install`

5. 在 [Homebrew](https://brew.sh/index_zh-cn) 官网复制安装代码并在终端执行

6. 安装完后执行命令 `brew doctor`，检查 Homebrew 安装存在的问题



