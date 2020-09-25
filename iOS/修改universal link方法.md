### 修改universal link方法



1. 下载该文件，http://api.bwton.com/apple-app-site-association

2. 修改对应`bundleid`下的`path`

3. 修改`platformKeys.plist`中的`WXUniversalLink`地址

4. 修改微信开放平台的`Universal Links`地址

   注：3&4中的地址是相同的，如：`https://api.bwton.com/universal/`

   ​	3&4中地址的规则：`apple-app-site-association`文件的主域名 + `/`+ `apple-app-site-association`文件的中的`path`，以`/`结尾


https://czdt.czmetro.net.cn/apple-app-site-association