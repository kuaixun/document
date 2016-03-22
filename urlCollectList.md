# 网址收藏列表

#### **实现说明**  

网址收藏列表接口用来获取用户网页收藏列表。

#### **请求说明**

* 请求方法 *urlCollectList*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| uid | 用户id | long | 是 |

* 返回参数

| 字段 | 说明 | 类型 | 级别 | 父节点
| -- | -- | -- |
|item |  收藏列表 | 对象数组 |1 ||
|url | 网址 | string | 2 | item | 
|title	|标题 |string	|2	|item
|icon |图标 |string |2 |item
|time | 收藏时间 | long | 2 | item

* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "item": [
        {
            "uid": 20, 
            "url": "http://www.baidu.com2", 
            "title": "baidu", 
            "icon": "http://www.baidu.com/favicon.ico", 
            "time": 1458552577424
        }, 
        {
            "uid": 20, 
            "url": "http://www.baidu.com1", 
            "title": "baidu", 
            "icon": "http://www.baidu.com/favicon.ico", 
            "time": 1458552563237
        }, 
        {
            "uid": 20, 
            "url": "http://www.baidu.com", 
            "title": "baidu", 
            "icon": "http://www.baidu.com/favicon.ico", 
            "time": 1458552529086
        }
    ]
}
```