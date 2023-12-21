# Toolbox Enterprise Demo

您可以按照以下步骤尝试搭建一个 Toolbox Enterprise 的演示环境([需要Docker Compose环境](https://dockerdocs.cn/desktop/))。

> Toolbox Enterprise 演示包不包括 IDE 许可证和 Code With Me Enterprise 组件。 如果您愿意尝试，请联系我们的支持团队。


## 安装环境的要求

* 服务器端需要安装 Docker 和 Docker Compose（有 [Docker Desktop](https://dockerdocs.cn/desktop) 即可）

* 客户端机器上安装 [Toolbox App](https://www.jetbrains.com/toolbox-app/) (版本 2.1 or later).

> 该演示安装包已经为您提供了 Toolbox Enterprise 需要的： 
> * PostgreSQL 数据库、
> * S3 兼容的对象存储
> * OAuth2 兼容的认证服务程序
> 
> **无需另外安装。**

## 安装及启动的步骤

1. 从[链接处](https://download.jetbrains.com/tbe/tbe-demo-1.0.13880.289.zip)下载压缩包。 将 ZIP 存档解压到新建目录。
> 这里推荐使用 JetBrains 的 IDE 产品打开解压缩后的目录，方便从用户界面来操作。

2. 启动 Toolbox Enterprise 之前，请阅读并接受许可协议。 您可以在安装目录中的 toolbox_user_eap.pdf 文件中找到协议文本。 
> 要接受协议，请将 application-demo.yaml 文件中 tbe.license.accept 属性的值更改为 eap.v1。

![licenseeapv1.png](licenseeapv1.png)

3. 使用命令行进入到安装目录。

（非必需）管理员邮箱，数据库账号密码，对象存储服务账户密码等可以自行修改。
![fixsomepws.png](fixsomepws.png)
（非必需）docker compose文件也需要按照上面的设置进行相应的修改。
![fixyaml.png](fixyaml.png)

4. 运行命令行 `docker-compose up` 部署 Docker Compose 配置。
(采用 JetBrains 产品打开目录的用户可以忽略命令行说明的部分，直接在IDE里按运行按钮)

![dockercomposeup.png](dockercomposeup.png)

> * 要查看应用程序的状态，请在此目录中运行 `docker compose ps`
> * 要停止应用程序，请在此目录中运行 `docker compose stop`
> * 要删除它并清理，请在此目录中运行 `docker compose down -v`

5. 在安装过程中，该脚本将在 ./server-ssl-key.p12 和 ./server-ssl-cert.pem 文件中生成临时的 HTTPS 证书。 将带有证书的相应文件添加到操作系统的已信任证书库中：
> **_Windows Server_** 
> 
> 启动 PowerShell 并执行: `certutil.exe -user -addstore "Root" C:\ <演示包解压缩后的目录> \server-ssl-cert.pem`。
> 或者，通过手动将 ./server-ssl-key.p12 文件用 Certificate Import Wizard 导入。通过工具导入的做法中, 你可能需要输入一个密码: `tbe-server`
> 
> **_Linux Server_** 
> 
> 执行下面的命令行，把 PEM 证书拷贝并转换成 CRT 格式: `sudo cp server-ssl-cert.pem /usr/local/share/ca-certificates/server-ssl-cert.crt`
> 然后，运行下面的命令行将证书添加到已信任的证书库: `sudo update-ca-certificates --fresh`。
> 关于证书库更多信息，请参考[这个文档](https://ubuntu.com/server/docs/security-trust-store)。
> 
> **_macOS_**
> 
> 按照[苹果官方文档](https://support.apple.com/zh-cn/guide/keychain-access/kyca2431/mac)的步骤将 ./server-ssl-cert.pem 文件添加至密钥管理。
> 确保将“Always Trust”指定为Secure Sockets Layer (SSL) 信任的设置。

Docker Compose完成上述配置的部署后应该是下面的状态：
![deploymentok.png](deploymentok.png)

通过浏览器访问Toolbox Enterprise服务的URL：
![done.png](done.png)

按Login to Toolbox Enterprise按钮，然后通过下图红框处进入管理员模式：
![enteradmin.png](enteradmin.png)

