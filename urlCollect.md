# 网址收藏

#### **实现说明**  

网址收藏接口是提供给客户端用来收藏网址，只有登陆用户才可以使用。

#### **请求说明**

* 请求方法 *urlCollect*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| oper | 操作类型，1添加或更新，2删除 | int | 是 |
| uid | 用户id | long | 是 |
| url | 网址 | string | 是 |
| title | 网址标题 | string | oper为1时必须 |
| icon | 网站图标 | string | oper为1时必须 |

* 返回参数

* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }
}
```