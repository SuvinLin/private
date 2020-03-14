## 安装 Homebrew

1. 键入代码到对应的终端配置文件（.bashrc / .zshrc）

   ```shell
# proxy on/off
   function proxy() {
   	export http_proxy=http://127.0.0.1:1081
   	export https_proxy=http://127.0.0.1:1081
   	export all_proxy=socks5://127.0.0.1:1080
   	echo ' proxy on '
   }
   
   function unproxy() {
   	unset http_proxy
   	unset https_proxy
   	unset all_proxy
   	echo ' proxy off '
   }
   ```

2. 开启终端代理  `proxy`

3. 测试是否开启命令行代理  `curl ipinfo.io`

4. 安装 Xcode 命令行工具  `xcode-select --install`

5. 在 [Homebrew](https://brew.sh/index_zh-cn) 官网复制安装代码并在终端执行

6. 安装完后执行命令 `brew doctor`，检查 Homebrew 安装存在的问题

