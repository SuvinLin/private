1. 安装代理软件

2. 打开终端

3. `vi .bash_profile` 粘贴下面代码

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

4. `:wq` 保存文件

5. `source .bash_profile` 使配置生效

6. 开启终端代理  `proxy`

7. 测试是否开启命令行代理  `curl ipinfo.io`

8. 安装 Xcode 命令行工具  `xcode-select --install`

9. 在 [Homebrew](https://brew.sh/index_zh-cn) 官网复制安装代码并在终端执行

10. 安装完后执行命令 `brew doctor`，检查 Homebrew 安装存在的问题

11. 执行命令 `brew cask doctor`，检查 brew cask

