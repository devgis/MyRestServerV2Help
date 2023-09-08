# MyRestServerV2帮助文档

> Help for MyRestServerV2 Docker 

## 说明

> 此版本与之前版本已经有了本质的区别，为全新架构设计，并且使用了.net 6技术，因此得以兼容Docker 和 Linux。 但秉承了一贯轻量，简单易用的原则。并且拥有比旧版更多的功能。目前更多的功能依然正在不断完善之中。

## 获取镜像

> docker pull devgis/myrestserverv2

## 运行镜像

> >  也是很简单的直接运行以下docker命令

> docker run  --net=host -v {localconfigpath}/appsettings.json:/app/appsettings.json:ro -v  {localconfigpath}/nlog.config:/app/nlog.config:ro -v  {localconfigpath}/server.pfx:/app/server.pfx:ro -v  {locallogpath}:/app/logs --privileged=true -e ASPNETCORE_ENVIRONMENT=Development -e ASPNETCORE_URLS="https://+:7011;http://+:7001" -e ASPNETCORE_Kestrel__Certificates__Default__Password=147258369 -e ASPNETCORE_Kestrel__Certificates__Default__Path=/app/server.pfx myrestserverv2

> > {localconfigpath} 是用于存放本地配置的目录，按自己实际情况修改即可。

> > {locallogpath} 是用于存放本地日志的目录 ，按自己实际情况修改即可。

> > appsettings.json主配置文件

> > nlog.config 是nlog配置文件

> > server.pfx是证书文件 可以自己生成，用于部署https,如果不需要https则是不需要的。

> docker-compose up -d

> > 默认http端口为 7001 https 端口为 7011，可以按照自己的实际情况进行修改。

> > 可以直接在浏览器中打开https:///{dockerip}:7011/ 查看相关信息

> > 可以在 https:///{dockerip}:7011/swagger/index.html中查看配置文件

## Docker compose 

> 本目录下已经存在相关配置文件docker-compose.yml：

```
version: '3.4'

services:
  webapp:
    image: myrestserverv2
    network_mode: "host"
    privileged: true
    restart: always
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=https://+:7011;http://+:7001
      - ASPNETCORE_Kestrel__Certificates__Default__Password=147258369
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/app/server.pfx
    volumes:
      - {localconfigpath}/appsettings.json:/app/appsettings.json:ro
      - {localconfigpath}/nlog.config:/app/nlog.config:ro
      - {localconfigpath}/server.pfx:/app/server.pfx:ro
      - {locallogpath}:/app/logs
```

> > {localconfigpath} 是用于存放本地配置的目录 

> > {locallogpath} 是用于存放本地日志的目录  

> > appsettings.json主配置文件

> > nlog.config 是nlog配置文件

> > server.pfx是证书文件 可以自己生成，用于部署https,如果不需要https则是不需要的。

> docker-compose up -d

> > 默认http端口为 7001 https 端口为 7011，可以按照自己的实际情况进行修改。

> > 可以直接在浏览器中打开https:///{dockerip}:7011/ 查看相关信息

> > 可以在 https:///{dockerip}:7011/swagger/index.html中查看配置文件

## 配置文件说明

> appsettings.json

> > 主要在以下部分

```
  "DEVGIS":{
    "DBType":"MYSQL", //数据库类型 Access "OLEDB","ACCESS"， SQLServer : "MSSQL","MSSQLSERVER","SQLSERVER" MYSQL:"MYSQL" Oracle:"ORACLE" Postgresql:"POSTGRESQL","POSTGRES" 如果不能识别则认为是Access数据库
    "Tables":["t_test","t_role"],//手动配置开放的表，如果此项为空则使用数据库检索
    "FilterTables":["t_user","t_sys"],//需要过滤的表
    "Views":["v_test","v_null"], //视图
    "Procedures":["sp_test1","sp_test2"], //视图
    "Addable":true, //允许添加数据
    "Updateable":true, //允许修改
    "DeleteAble":true, //允许删除
    "AllowExecuteProcedures":true //允许执行存储过程
  },
  "ConnectionStrings": {
    "WebApiDatabase": "server=localhost;port=33306;database=test_db;user=root;password=123456;SslMode=None" 为数据库连接字符串配置
  },

```

> nlog.config

> > 具体参考网络上关于nlog的配置

## 接口说明(数据表) GET 方式

> /api/v 查看系统版本信息

> > 示例 :https://localhost:7176/api/v

> api/info 查看程序详细信息

> > 示例 :https://localhost:7176/api/info

> api/tables 查看当前库表名列表信息

> > 示例 :https://localhost:7176/api/tables

> api/{tablename}/full 或 api/{tablename}/info查看完整表格信息

> > 示例 :https://localhost:7176/api/t_test/full 或 https://localhost:7176/api/t_test/info

> api/{tablename}/count 查看当前表行数

> > 示例 :https://localhost:7176/api/t_test/count 

> > 支持querystring :where

> api/{tablename}/all 查看表格中完整数据 Json形式

> > 示例 :https://localhost:7176/api/t_test/all

> > 支持querystring :cols,where

> api/{tablename}/top{count} 查看当前表格的列数据详细信息

> > 示例 :https://localhost:7176/api/t_test/top5 查询前库5条数据

> > 支持querystring :cols,where,ordercol,asc

> > https://localhost:7176/api/t_test/top1?cols=id,name&where=id%3C=2&ordercol=id&asc=true

> 分页查询 api/t_test/page/{PageSize}/{PageIndex}

> > https://localhost:7176/api/t_test/page/2/1 每页两条，第1页

> > 支持querystring :cols,where

> > https://localhost:7176/api/t_test/page/2/1?cols=name&where=id > 0

> > https://localhost:7176/api/t_test/page/5

> > 支持querystring :where

> > https://localhost:7176/api/t_test/page/5?where=1=1

## 增删改部分  全部为POST方式

> add

```
{
    "Parameters":[{"key":"id","value":99},{"key":"name","value":"testadd"},{"key":"remarks","value":"添加的"}]
}
```

> apit/{tablename}/update 

```
{
    "Parameter":{"name":"testadd991","remarks":"添加的991"},
    "Condition":{"id":991}
}
```

> apit/{tablename}/delete

```
content-type:application/json

{
    "Condition":{"id":10}
}
```
 
> api/execute/{procedurename}

```
{
    "Parameter":{"id":11,"name":"testadd22","remarks":"nn"}
}
```

> > 这一部分可以参考Tests目录下的测试脚本。

## 接口说明(视图) GET 方式

> api/views 查看当前库表名列表信息

> > 示例 :https://localhost:7176/api/v/views

> api/{viewname}/full 或 api/{viewname}/info查看完整表格信息

> > 示例 :https://localhost:7176/api/v/v_test/full 或 https://localhost:7176/api/v_test/info

> api/{viewname}/count 查看当前表行数

> > 示例 :https://localhost:7176/api/v/v_test/count

> > 支持querystring :where

> api/{viewname}/all 查看表格中完整数据 Json形式

> > 示例 :https://localhost:7176/api/v/v_test/all 

> > 支持querystring :cols,where

> api/{viewname}/top{count} 查看当前表格的列数据详细信息

> > 示例 :https://localhost:7176/api/v/v_test/top5 查询前库5条数据

> >  https://localhost:7176/api/v/v_test/top1?cols=name&where=Username like '%a%'

> > 支持querystring :cols,where

> 分页查询 api/v/v_test/page/{PageSize}/{PageIndex}

> > https://localhost:7176/api/v/v_test/page/10/1

> > 支持querystring :cols,where,ordercol,asc

> > https://localhost:7176/api/v/v_test/page/10/2?cols=UserName&where=1=1&ordercol=id&asc=true

> 分页页数查询 api/v/v_test/page/{PageSize}

> > https://localhost:7176/api/v/v_test/page/5

> > 支持querystring :where

> > https://localhost:7176/api/v/v_test/page/10?where=1=1

## 问题

> 目前功能还不完善，后期将不断完善更新，欢迎大家关注，并给予反馈。

> 可以通过邮箱 devgis@qq.com 或者 devgis@163.com 与我留言， 也可以直接在github上反馈。
