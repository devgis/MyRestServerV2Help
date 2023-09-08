- [安装组件](#安装组件)
- [集成JWT认证](#集成jwt认证)
- [集成SWAGGER](#集成swagger)
- [EF MySQL](#ef-mysql)
- [集成MVC](#集成mvc)
- [集成Blozer](#集成blozer)
- [Docker](#docker)
- [问题](#问题)

## 安装组件

> dotnet add package Swashbuckle.AspNetCore --swagger 用
 
> dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer -v 6.0.1

> dotnet add package MySql.Data

>  ---暂时不需要 不确定与不带DATA的区别 dotnet add package MySql.Data.EntityFrameworkCore
>  dotnet add package MySql.EntityFrameworkCore
>  dotnet add package Microsoft.EntityFrameworkCore

>  dotnet add package Microsoft.EntityFrameworkCore.Design 暂时没有导入

> dotnet tool install -g dotnet-ef 安装 eftools

>  dotnet ef migrations add InitialCreate 创建 不能在程序启动的时候执行

>  dotnet ef database update 更新

> > No use

> dotnet remove package XXXX 移除package

>  dotnet add package Masuit.Tools.Core

## 集成JWT认证

> 基于TOKEN

> /api/Account/GetToken 进行获取token 后期改成登录界面

> 具体其他API 接口可以查询 /swagger/index.html

## 集成SWAGGER

> 在线显示可用接口

> 访问地址 /swagger/index.html

## EF MySQL

> EF 中进行数据迁移相关命令

···
 启动ef core数据迁移

1.先安装Microsoft.EntityFrameworkCore.Tools

安装包时注意版本一致！并不是所有包都要是最新

tools包含的全部命令
Add-Migration
Bundle-Migration
Drop-Database
Get-DbContext
Get-Migration
Optimize-DbContext
Remove-Migration
Scaffold-DbContext
Script-Migration
Update-Database

2.Add-Migration xx //添加迁移，xx代表迁移名称
3.Update-Database //更新到数据库


（代码迁移）如果要删除某迁移类文件，最好先将数据库回滚到此迁移的上一版本，再删除
update-database -TargetMigration: [name] 命令，其中 [name] 要使用带时间戳的全名 
···

## 集成MVC

## 集成Blozer 

> @page "/counter" 后面的标记不可以重复，不然会全局出错

> > An unhandled exception occurred while processing the request.

> > NullReferenceException: Object reference not set tootnet dev-certs https --trust an instance of an object.

> > Microsoft.AspNetCore.Components.Routing.Router.Refresh(bool isNavigationIntercepted)

> 将Blozer 组件集成到MVCView 中 https://localhost:7176/Home/Index

## Docker 

> > 进入容器办法

> sudo docker exec -it 65d919b2b351  /bin/bash

> > Docker build

> 进入Source 目录下运行 docker build  -t myrestserverv2 .

> docker run  --net=host -v /home/liyafei/DockerVolumes/MyRestServerV2//appsettings.json:/app/appsettings.json:ro -v /home/liyafei/DockerVolumes/MyRestServerV2//nlog.config:/app/nlog.config:ro myrestserverv2 

 dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\aspnetapp.pfx -p devgis147258369


> > docker 日志卷

> > 外部日志目录需要权限 ：chown -R 1000:1000 logs/

docker run -d -P --name web4 -v vol1:/volume training/webapp python app.p

> docker run  --net=host -v /home/liyafei/DockerVolumes/MyRestServerV2/appsettings.json:/app/appsettings.json:ro -v  /home/liyafei/DockerVolumes/MyRestServerV2/nlog.config:/app/nlog.config:ro -v  /home/liyafei/DockerVolumes/MyRestServerV2/server.pfx:/app/server.pfx:ro -v /home/liyafei/Logs/Dotnet/MyRestServerV2:/app/logs --privileged=true -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_URLS="https://+:7011;http://+:7001" -e ASPNETCORE_Kestrel__Certificates__Default__Password=147258369 -e ASPNETCORE_Kestrel__Certificates__Default__Path=/app/server.pfx myrestserverv2

> > 生成 pfx证书 生成时会有个导出密码 就是 这里的ASPNETCORE_Kestrel__Certificates__Default__Password=147258369

> > .pfx倒入到docker中启动就可以了。

> 支持DockerCompose 

> > 生成镜像后 在DockerCompose目录下执行命令就可以查看

> > docker-compose up -d 不显示日志

> > docker-compose up 显示日志
 
> > host 网络模式不能映射端口 -p 333:3333

> docker https 需要证书 所以只部署 Http

> > http://localhost:7001;https://localhost:7011

> DOcker 镜像发布到Docker hub

> > docker login

> > docker tag 镜像ID 用户名称/镜像源名(repository name):新的标签名(tag)来更改

> > 更改 docker latest myrestserverv2 devgis/myrestserverv2:0.1

> > docker commit containerid user/reponame

> > docker commit defb945eabd5 devgis/myrestserverv2

> > docker push <hub-user>/<repo-name>:<tag>

> > docker push devgis/myrestserverv2:latest

## 问题

> need start with code . in source code path

> ubuntu 下 vscode 找不到 dotnet 安装

> > 报错The terminal process failed to launch: Path to shell executable "dotnet" does not exist. 

> task.json 中修改 "command": "/dotnet", 为  "command": "/usr/dotnet/dotnet",  //默认情况下改回dotnet /usr/dotnet/dotnet是我在linux下安装路径

> Unhandled exception. System.InvalidOperationException: Unable to configure HTTPS endpoint. No server certificate was specified, and the default developer certificate could not be found or is out of date.
To generate a developer certificate run 'dotnet dev-certs https'. To trust the certificate (Windows and macOS only) run 'dotnet dev-certs https --trust'.

> dotnet dev-certs https

> dotnet dev-certs https --trust

> Minimal api序列化引擎问题

> > .netcore Minimal api中使用的是System.Text.Json 的是 不支持 Datable的序列化，但是minimalapi中似乎不能把序列化引擎替换为Newtonsoft.Json。 

> > 使用builder.Services.AddControllers().AddNewtonsoftJson(options => {})；在Minimal api中是不起作用的。


