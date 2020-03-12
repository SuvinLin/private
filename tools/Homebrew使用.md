## Homebrew

**Homebrew** 由开发者 Max Howell 开发，并基于 BSD 开源，是一个非常方便的formula管理器工具。在早期， **Homebrew** 仅有 **macOS** 的版本，后续随着用户的增多，**Homebrew** 还提供了 **Linux** 的版本，帮助开发者在 **Linux** 同样使用 **Homebrew** 来配置环境。



### Homebrew 的几个核心概念

| 词汇        | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| formula (e) | 安装formula的描述文件，formulae 为复数                       |
| cellar      | 安装好后所在的目录                                           |
| keg         | 具体某个formula所在的目录，keg 是 cellar 的子目录            |
| bottle      | 预先编译好的formula，不需要现场下载编译源码，速度会快很多；官方库中的formula大多都是通过 bottle 方式安装 |
| tap         | 下载源，可以类比于 Linux 下的formula管理器 repository        |
| cask        | 安装 macOS native 应用的扩展，你也可以理解为有图形化界面的应用。 |
| bundle      | 描述 Homebrew 依赖的扩展                                     |



### 使用 Brewfile 完成环境迁移（更换设备需要）

设备用久了，我们的电脑中会有大量的软件，如果你需要迁移环境，重新安装会是一个大麻烦，好在 **Homebrew** 本身为我们提供了一个非常好用的环境迁移的工具 —— **Homebrew Bundle**

你首先需要在之前的电脑中执行 `brew bundle dump` 来完成当前环境的导出,导出完成后，你会得到一个 **Brewfile**

然后将 **Brewfile** 复制到新的电脑中，并执行 `brew bundle` 来开始安装的过程



### Homebrew 常用命令

#### 检查 Hombrew 环境

`brew doctor`	开启 Homebrew 自带的检查，从而确认有哪些问题，并进行修复



#### 更新 Homebrew

`brew update`	想要获取最新的formula / application，首先得更新 Homebrew 本身



#### 搜索

`brew search 关键词 / 正则表达式`	会显示 brew 和 brew cask 与关键词关联的 formula  /  application



#### 安装

brew 和 brew cask 支持多个formula / application安装， 多个formula / application在application名后加空格即可

- `brew install formula`

- `brew cask install application`



#### 卸载

- `brew uninstall formula`
- `brew uninstall formula --force`   彻底卸载某个formula

- `brew cask uninstall application`



#### 查看  formula / application 的信息

- `brew info`		显示安装的包的数量、文件个数、占用内存
- `brew info formula / application`       显示某个 formula  / application 的信息
- `brew cask  info application`            显示某个 application 的信息



#### 检查本地是否有更新 formula / application

- `brew outdeted`
- `brew cask outdated`



#### 更新 formula/application

- `brew upgrade formula`	更新某个formula
- `brew upgrade`		更新全部formula
- `brew cask upgrade application`	更新某个application
- `brew cask upgrade`      更新全部application



#### 锁定某个不想更新的formula

- `brew pin formula`       锁定指定formula
- `brew unpin formula`     取消锁定指定formula 



#### 列出已安装的 formula / application

- `brew list`
- `brew cask list`



#### 查看已安装 formula 的依赖

- `brew deps  formula`    查看某个formula的依赖
- `brew deps --installed --tree`    查看已安装formula的依赖，以树形结构显示



#### 查看 Homebrew 下载的formula存放路径	

`brew --cache`



#### 清理旧版本的 formula

- `brew cleanup`            清理所有旧版本的formula
- `brew cleanup formula / application`    清理指定的旧版本formula
- `brew cleanup -n`         查看可清理的旧版本formula 



#### 获取帮助

- `brew help`
- `brew cask help`



#### 添加一个新的 tap

Homebrew 官方在安装的时候会有一些 tap 但是在使用时，依然会需要安装一些特殊的 tap ，这个时候，我们就要用到 tap 的命令来添加新的 tap

在添加 tap 时，输入命令 `brew tap [user/repo]` ，就可以完成添加 tap 了



##### 常用 tap

在使用 Homebrew 时，我们一般会添加几个常用的 tap，来确保我们有足够的软件来安装



**Caskroom**

​	Caskroom 是 Homebrew 下一个非常出名的 tap ，有了 caskroom，我们就可以安装一些有图形化界面的软件了

​	使用起来也非常简单，最新版 Homebrew 中，你可以直接使用 `brew cask install [软件名]` 来安装特定的软件，Homebrew 会自动安装 Caskroom



**homebrew-cask-fonts**

​	程序员难免要安装一些代码字体，这样才能更好的写代码，Homebrew 也提供了方便我们安装字体的 tap

​	在使用时，你需要先添加对应的 tap ，然后执行安装即可了，比如我们要安装 FiraCode ，只需要执行如下命令

```bash
brew tap homebrew/cask-fonts
brew cask install font-fira-code
```



#### 管理后台软件

诸如 Nginx、MySQL 等软件，都是有一些服务端软件在后台运行，如果你希望对这些软件进行管理，可以使用 `brew services` 命令来进行管理

- `brew services list`    查看所有服务
- `brew services run [服务名]`     单次运行某个服务
- `brew services start [服务名]`      运行某个服务，并设置开机自动运行
- `brew services stop [服务名]`     停止某个服务
- `brew services restart`         重启某个服务



