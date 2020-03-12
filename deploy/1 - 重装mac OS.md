## 制作可引导的 macOS 安装器

1. 连接要用于可引导安装器的 U 盘
   - 确保这个驱动器 **至少** 有 12GB 可用储存空间
   - 格式化为 **Mac OS 扩展** 并将 U 盘更名为 **MyVolume**
2. 打开终端 
3. 在终端中键入或粘贴以下命令之一（根据你所下载的系统文件版本）
   - 这些命令默认安装器仍位于您的“**应用程序**”文件夹中
     - 通过 App Store 或者 系统升级下载的文件 都默认存放于此
     - 如果安装器不在此路径，为了方便，将其移动到应用程序文件夹中
   - 确认 **MyVolume** 是 U 盘 的名称。如果不是这个名称，请相应地替换为 **MyVolume** 替换为您的宗卷名称（当然不是非得叫这个名字，但是为了复制方便，So）

**Catalina** ：*

```shell
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```


**Mojave**：*

```shell
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```


**High Sierra**：*

```shell
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```


**Sierra**：

```shell
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Sierra.app
```


**El Capitan**：

```shell
sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app
```



4. 键入相应的命令后，回车
5. 出现提示时（Password：），输入管理员密码，回车
6. 出现提示时（If you wish to continue type ( Y ) then press return：），按 Y  回车
7. 接着会显示进度（10%， 20%）等待完成即可
8. 出现提示（Done.），完成制作

### 参考链接

[如何创建可引导的 mac OS 安装器](https://support.apple.com/zh-cn/HT201372)



## 使用可引导的 macOS 安装器重装

1. 插入U盘
2. 重启
3. 按住 `option` 键直到出现启动盘图标
4. 选择要重装的启动盘
5. 读条后出现 **macOS实用工具**
6. 选择 **磁盘工具**
7. 将内置抹除（名称 **Macintosh HD** 、抹除格式跟随默认，一般为 **APFS**，没有的话就选择**Mac OS 扩展( 日志式 )**）
   - 将内置中多余的磁盘删除
   - 内置中仅保留空间最大的磁盘 (Macintosh HD)
   - 最终仅剩余 内置磁盘(Macintosh HD) 和 磁盘映像 OS X Base System
8. 关闭磁盘工具
9. 回到 **macOS实用工具** 并选择 **重新安装 Mac OS**
10. 继续 →  同意  → 同意  → 选择磁盘（选 **Macintosh HD** ） → 等待即可
