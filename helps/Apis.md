- [概述](#概述)
- [配置](#配置)
- [返回数据说明](#返回数据说明)
- [接口说明(数据表)](#接口说明数据表)
- [增删改部分](#增删改部分)
- [接口说明(视图)](#接口说明视图)
- [Swagger](#swagger)
- [Q\&A](#qa)

## 概述

> 支持数据库 MySQL(8.0以后，老版本未作测试),MS Sqlserver 2012以上，Oracle 11G以上,Access

## 配置

> 数据库支持

## 返回数据说明

> 返回对象主要包含以下三个字段

> > Success bool 类型，表示当前操作是否成功。

> > Data 具体的数据，如果Success为true 则该值才可能有值。

> > Message 详细消息信息，如果Success为true则该值为空，否则如果Success为false则该值提供参考错误消息。

## 接口说明(数据表)

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

## 增删改部分

> add 使用KeyValuePair结构

```
{
    "Parameters":[{"key":"id","value":99},{"key":"name","value":"testadd"},{"key":"remarks","value":"添加的"}]
}
```

> add2 使用Tuple结构 数据更小

```
{
    "Parameters":[{"id":99},{"name":"testadd"},{"remarks":"添加的"}]
}
```

## 接口说明(视图)

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

## Swagger 

> 可以在swagger中查询测试相关接口 swagger/index.html

> > https://localhost:7176/swagger/index.html

## Q&A