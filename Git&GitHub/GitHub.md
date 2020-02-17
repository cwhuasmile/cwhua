## GitHub打不开或者图片加载不出来

修改 C:\Windows\System32\drivers\etc 中的hosts文件（PS：若没有修改权限，可以鼠标右键，属性，安全，修改权限。或者将hosts文件复制到桌面，修改之后，复制到原文件夹）

将下面一段话添加到hosts文件中：

```
# GitHub Start 
192.30.253.112 github.com 
192.30.253.119 gist.github.com 
151.101.100.133 assets-cdn.github.com 
151.101.100.133 raw.githubusercontent.com 
151.101.100.133 gist.githubusercontent.com 
151.101.100.133 cloud.githubusercontent.com 
151.101.100.133 camo.githubusercontent.com 
151.101.100.133 avatars0.githubusercontent.com 
151.101.100.133 avatars1.githubusercontent.com 
151.101.100.133 avatars2.githubusercontent.com 
151.101.100.133 avatars3.githubusercontent.com 
151.101.100.133 avatars4.githubusercontent.com 
151.101.100.133 avatars5.githubusercontent.com 
151.101.100.133 avatars6.githubusercontent.com 
151.101.100.133 avatars7.githubusercontent.com 
151.101.100.133 avatars8.githubusercontent.com 
# GitHub End
```

保存hosts文件，重新打开Github网站，一切正常。

如果还不能解决，请打开下面的网站

https://www.ipaddress.com/
到这个网站，查avatars0.githubusercontent.com ，就能得到最新的地址，替换掉原文的地址就行了

参考链接：https://zhuanlan.zhihu.com/p/36154464

> 转自： https://www.jianshu.com/p/3eacebfc55ab