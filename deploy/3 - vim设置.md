1. 打开终端

2. 在系统的用户目录下,输入 `vi .vimrc`

3. 添加如下代码

   ````
   "语法高亮
   syntax on
   
   "显示行号
   set nu
   
   "智能缩进
   set smartindent
   
   "不需要备份
   set nobackup
   
   "显示标尺，就是在右下角显示光标位置
   set ruler
   ````


4. `:wq` 保存后会立即生效，不用source .vimrc



