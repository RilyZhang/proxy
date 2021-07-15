这是一个代理服务容器，当你拥有一个顶级域名和服务器，并且服务器中运行了多个服务，你可以使用该容器动态解析二级域名。

<img src="https://rilyzhang.github.io/picture-service/proxy.readme/introduce.png" alt="登录页" height="300">

使用步骤：

#### 第1步

拉取镜像：

```
docker push dyrily/proxy:latest
```

#### 第2步

创建```./data/.env```环境变量文件，文件内容如下：

```
# web端口
web.port=3366
# 登录用户名
web.username=admin
# 登录密码
web.password=123456
# 认证密钥
web.secret-key=fjC2Hy!*58usDRB^
# https web端口
web.ssl.port=8889
# ssl私钥
web.ssl.key=/data/cert/cert.key
# ssl证书
web.ssl.cert=/data/cert/cert.cer

# http代理端口
proxy.port=80
# https代理端口
proxy.ssl.port=443
# ssl代理私钥
proxy.ssl.key=/data/cert/cert.key
# ssl代理证书
proxy.ssl.cert=/data/cert/cert.cer
```

#### 第3步

启动容器：

```
docker run -d \
--name proxy-server \
-v <你的配置目录>:/proxy/data \
-v <你的ssl私钥目录>:<容器ssl私钥目录> \
-v <你的ssl证书目录>:<容器ssl证书目录> \
--net=host \
--restart=always \
dyrily/proxy:latest
```

**注意**：ssl是非必须选项，如果不想使用，可以在.env文件中将所有关于ssl的选项使用#注释。
