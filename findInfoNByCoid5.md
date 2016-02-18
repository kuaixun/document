# 新版本新闻列表

#### **实现说明**  

新版本新闻列表接口*findInfoNByCoid5*是为android客户端5.0之后的版本实现的接口，和[*findInfoNByCoidN*](findInfoNByCoidN.html)相比主要修改是规范了一些字段的命名，并且增加了一些满足新需求的字段。

#### **请求说明**

* 请求方法 *findInfoNByCoid5*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| coid | 频道ID | long | 是 |
| page | 请求页数，从1开始 | int | 是 |

* 返回参数

| 字段 | 说明 | 类型 | 级别 | 父节点
| -- | -- | -- |
| news | 新闻列表 | 对象数组 |1||
| newsId | 新闻ID | long | 2 | news | 
| channelId | 频道ID | long | 2 | news
| type | 新闻类型 | int | 2 | news
|open_type |打开方式|int	|2	|news
|title	|标题 |string	|2	|news
|desc	|描述 |string 	|2	|news
|isTop	|是否置顶	|int |2	|news
|time	|发布时间	|long |2	|news
|content	|正文内容，只针对段子频道返回	|string |2	|news
|upCount	|顶数量 |long	|2	|news
|downCount	|踩数量 |long	|2	|news
|shareCount	|分享数量 |long	|2	|news
|hot_comment_list	|热门评论 |对象数组	|2	|news
|commentId	|评论ID |long	|3	|hot_comment_list
|newsid	|新闻ID |long	|3	|hot_comment_list
|content	|评论内容 |string	|3	|hot_comment_list
|uid	|用户ID |string	|3	|hot_comment_list
|did	|用户设备号 |string	|3	|hot_comment_list
|headPic	|用户头像 |string	|3	|hot_comment_list
|nickname	|用户昵称 |string	|3	|hot_comment_list
|hot	|是否热门评论，1是0否 |int	|3	|hot_comment_list
|createtime	|评论时间 |long	|3	|hot_comment_list
|location	|位置 |string	|3	|hot_comment_list


* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "picNews": [ ], 
    "news": [
        {
            "title": "老婆：老公我去隆胸吧？老公：好啊...", 
            "desc": "老婆：老公我去隆胸吧？
                老公：好啊。
                老婆：哼！你果然嫌我胸小。
                老公：......
                老婆：老公我去隆胸吧？
                老公：别去了。
                老婆：哼！你就是舍不得我花钱。
                老公：......", 
            "time": 1455678900000, 
            "source": "手机冲浪", 
            "content_url": "http://go.10086.cn/surfnews/images/4648023/20160217/5341892/5341892.html", 
            "isTop": 0, 
            "type": 1, 
            "open_type": 0, 
            "ctype": 0, 
            "isComment": 1, 
            "showType": 1003, 
            "newsId": 5341892, 
            "channelId": 4648023, 
            "webView": 0, 
            "upCount": 1286, 
            "downCount": 207, 
            "shareCount": 120, 
            "content": "老婆：老公我去隆胸吧？
                老公：好啊。
                老婆：哼！你果然嫌我胸小。
                老公：......
                老婆：老公我去隆胸吧？
                老公：别去了。
                老婆：哼！你就是舍不得我花钱。
                老公：......"
        }
    ]
}
```

