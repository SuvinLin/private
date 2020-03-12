### 开启Mac充电提示音

````shell
defaults write com.apple.PowerChime ChimeOnAllHardware -bool true; open /System/Library/CoreServices/PowerChime.app &
````



### 关闭Mac充电提示音

````shell
defaults write com.apple.PowerChime ChimeOnAllHardware -bool false;killall PowerChime
````



### finder 显示/隐藏 路径

````
# 在 Finder 中
command + Option + P
````



### 调整launchpad图标排列

调整每一列显示图标数量 (*代表你要显示的列数)

````shell
defaults write com.apple.dock springboard-rows -int *
````

调整每一行显示图标数量（*代表你要显示的行数）

````shell
 defaults write com.apple.dock springboard-columns -int *
````

重启 Dock

````shell
killall Dock
````



### iOS设备连接Mac一直断开连接解决办法

````shell
sudo killall -STOP -c usbd
````



### 屏蔽系统更新图标的红点

````shell
defaults write com.apple.systempreferences AttentionPrefBundleIDs 0
````

重启系统即可消失