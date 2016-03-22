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
|news | 新闻列表 | 对象数组 |1 ||
|newsId | 新闻ID | long | 2 | news | 
|title	|标题 |string	|2	|news
|showTime |新闻时间 |long |2 |news
|type | 新闻类型 | int | 2 | news
|source |新闻来源 |string |2 |news
|contentUrl |正文地址 |string |2 |news
|newsUrl |新闻html地址 |string |2 |news
|coid | 频道ID | long | 2 | news
|open_type |打开方式|int	|2	|news
|imgUrl |图片地址 |string |2 |news
|dm |图片分辨率，宽*高格式 |string |2 |news
|showType |展示格式，[参照格式定义](findInfoNByCoid5.html#新闻展示格式定义 "格式定义") |int |2 |news
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