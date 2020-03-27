## 制作可引导的 macOS 安装器

1. 连接要用于可引导安装器的 U 盘
   - 确保这个驱动器 **至少** 有 12GB 可用储存空间
   - 使用 Mac 下的磁盘工具 将 U 盘格式化为 **Mac OS 扩展（日志式）** 格式 并更名为 **MyVolume**
   
   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/抹掉U盘并更名.png)
   
   
   
2. 打开终端 

3. 在终端中键入或粘贴以下命令之一（根据你所下载的系统文件版本，复制以下对应的命令）
   - 这些命令默认安装器仍位于“**应用程序**”文件夹中
     - 通过 App Store 或者 系统升级下载的文件 都默认存放于此
     - 如果安装器不在此路径，为了方便，将其移动到应用程序文件夹中
   - 确认 **MyVolume** 是 U 盘 的名称。如果不是这个名称，请相应地替换为 **MyVolume** 替换为宗卷名称（当然不是非得叫这个名字，但是为了复制方便，So）

**Catalina** ：*

```shell
$ sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```



**Mojave**：*

```shell
$ sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```



**High Sierra**：*

```shell
$ sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```



**Sierra**：

```shell
$ sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Sierra.app
```



**El Capitan**：

```shell
$ sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app
```



4. 粘贴相应的命令到终端后，回车

5. 出现提示时（Password：），输入管理员密码，回车

6. 出现提示时（If you wish to continue type ( Y ) then press return：），按 y  回车

7. 接着会显示进度（10%， 20%）等待完成即可

8. 出现如下提示，表示制作成功

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/macOS 安装器制作完成.png)

   

### 参考链接

[如何创建可引导的 mac OS 安装器](https://support.apple.com/zh-cn/HT201372)



## 使用可引导的 macOS 安装器重装

1. 插入制作好的 macOS安装器 U 盘

2. 重启 mac

3. 按住 `option` 键直到出现启动管理器，选择固件（选名称为 Install macOS 你制作好的系统版本名称），这里选择右边那个固件

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/进入启动管理器.jpeg)

   

4. 读条后出现 **macOS实用工具**

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/mac OS 实用工具.jpeg)

   

5. 选择 **磁盘工具**

6. 将内置抹除（名称 **Macintosh HD** 、抹除格式跟随默认，一般为 **APFS**，没有的话就选择**Mac OS 扩展( 日志式 )**）

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/抹掉磁盘.jpeg)

   

   - 将内置中多余的磁盘删除

   - 内置中仅保留空间最大的磁盘 (Macintosh HD)

   - 最终仅剩余 内置磁盘(Macintosh HD) 和 磁盘映像 OS X Base System

     ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/剩余盘符.jpeg)

     

7. 点红色按钮关闭磁盘工具

8. 回到 **macOS实用工具** 并选择 **重新安装 Mac OS**

9. 选择继续 →  同意  → 同意  → 选择磁盘（选 **Macintosh HD** ）→ 安装 → 等待即可

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/step-1.jpeg)

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/step-2.jpeg)

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/step-3.jpeg)

   ![](https://cdn.jsdelivr.net/gh/suvin7/picBed/markdown/选择安装磁盘.jpeg)



## macOS 重装问题

[macOS重装问题汇总](https://github.com/suvin7/private/blob/master/tools/重装问题汇总.md)



