# Git错误集锦

## 1.Git报错：OpenSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443

```shell
git config --global --list
查看以下配置
```

```shell
core.editor="D:\Microsoft VS Code\bin\code" --wait
user.name=hmisaka
user.email=1409640444@qq.com
credential.helper=manager
gui.recentrepo=D:/CODE/Web/web-learning
http.https://github.com.proxy=http://127.0.0.1:10809
```

使用http协议代理，window网络设置-->代理查看端口号为10209，解决

```shell 
git config --global http.https://github.com.proxy http://127.0.0.1:10809
```

