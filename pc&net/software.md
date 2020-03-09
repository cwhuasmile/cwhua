# Charles设置方法

1. 安装证书：

   依次选择Help--》SSL Proxying--》Install Charles Root Certificate
   打开证书安装对话框。点击“安装证书”，选择本地计算机按下一步。选择将所有的证书都放入下列存储，点击浏览，选择受信任的跟证书颁发机构，点击确定。

2. 设置代理端口：

   依次点击Proxy--》Proxy Settings
   勾选HTTP Proxy下面的Enable transparent HTTP proxying

3. 设置SSL代理规则：

   依次点击Proxy--》SSL Prxoying Settings
   勾选Enable SSL Proxying，然后点击“Add”，在Host中填入“*”，Port中填入“443”，点击OK



