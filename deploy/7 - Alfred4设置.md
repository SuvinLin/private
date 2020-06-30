### General

- Startup → 勾选 Launch Alfred at login
- 更改快捷键 （command double tap）

<br />

### Features

#### Default results

​	Extras：除了 AppleScript 不勾选，其余选项全部勾选

<br />

#### 	Web Search

- MDN 搜索

  ````
  https://developer.mozilla.org/zh-CN/search?q={query}
  ````

- 百度百科搜索

  ````
  https://baike.baidu.com/search/none?word={query}&pn=0&rn=10&enc=utf8
  ````

- 百度搜索

  ````
  https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&ch=&tn=baiduerr&bar=&wd={query}
  ````

- bilibili 搜索

  ````
  https://search.bilibili.com/all?keyword={query}
  ````

- 爱奇艺搜索

  ````
  https://so.iqiyi.com/so/q_{query}
  ````

- 京东搜索

  ````
  https://search.jd.com/Search?keyword={query}&enc=utf-8)
  ````

- 豆瓣电影搜索

  ````
  https://movie.douban.com/subject_search?search_text={query}
  ````

- 什么值得买搜索

  ````
  https://search.smzdm.com/?s={query}
  ````

- 淘宝搜索

  ````
  https://s.taobao.com/search?oe=utf-8&f=8&q={query}
  ````

- 微博搜索

  ````
  https://s.weibo.com/weibo/{query}
  ````

- 优酷搜索

  ````
  https://www.soku.com/search_video/q_{query}
  ````

- 知乎搜索

  ````
  https://www.zhihu.com/search?q={query}
  ````
  
- Github 搜索仓库

  ````
  https://github.com/search?q={query}&amp;ref=opensearch
  ````


<br />

#### 	Web Bookmarks

**source**

​	勾选 Google Chrome Bookmarks

**show bookmarks**

​	via keyword bm

<br />

#### 	Terminal

​	Application → 选择 Custom → 并键入下面代码

````
on alfred_script(q)
    if application "iTerm2" is running or application "iTerm" is running then
        run script "
            on run {q}
                tell application \"iTerm\"
                    activate
                    try
                        select first window
                        set onlywindow to true
                    on error
                        create window with default profile
                        select first window
                        set onlywindow to true
                    end try
                    tell the first window
                        if onlywindow is false then
                            create tab with default profile
                        end if
                        tell current session to write text q
                    end tell
                end tell
            end run
        " with parameters {q}
    else
        run script "
            on run {q}
                tell application \"iTerm\"
                    activate
                    try
                        select first window
                    on error
                        create window with default profile
                        select first window
                    end try
                    tell the first window
                        tell current session to write text q
                    end tell
                end tell
            end run
        " with parameters {q}
    end if
end alfred_script
````

<br />

### Workflows

​	安装 workflow

<br />

### Appearance

​	选择  Alfred macOS Dark

<br />

### Update

​	取消勾选 Automatically check for  updates