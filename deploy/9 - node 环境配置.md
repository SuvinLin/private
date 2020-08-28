1. 安装 [nvm](https://github.com/nvm-sh/nvm)

2. 使用 nvm 安装 node 

   ````bash
   # 使用 nvm 安装 node 长期支持版， --lts 代表长期支持版，没有该参数则会下载最新版本的 node
   $ nvm install node --lts	
   ````

3. 检测 node 和 npm 是否安装成功

   ````bash
   # 输出版本号则为安装成功
   $ node -v 
   $ npm -v
   ````

4. 使用 npm 全局安装 nrm

   ````bash
   $ npm i nrm -g
   ````

5. 检测 nrm 是否安装成功

   ````bash
   # 输出版本号则为安装成功
   $ nrm --version
   ````

6. [nrm 使用教程](https://segmentfault.com/a/1190000017419993)

7. 使用 nrm 测试速度并切换到最快的镜像源

