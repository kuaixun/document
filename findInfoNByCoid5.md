# 新版本新闻列表

#### **实现说明**  

新版本新闻列表接口*findInfoNByCoid5*是为android客户端5.0之后的版本实现的接口，和[*findInfoNByCoidN*](findInfoNByCoidN.html)相比主要修改是规范了一些字段的命名，并且增加了一些满足新需求的字段。

#### **showType格式定义**
showType用来决定列表的展示格式，定义如下：

|取值 |说明 | 
| --  | -- | 
|1001 |单图
|1002 |多图
|1003 |无图
|1004 |趣图gif
|2001 |广告大图
|2002 |广告小图
|2003 |广告下载小图
|2004 |广告下载大图
|3001 |专题有图片
|3002 |专题无图片
|4001 |热推第一条的T+新闻推荐
|4002 |热推第一条的T+RSS推荐

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
|imgUrl |图片 |string |2 |news
|dm |图片分辨率，宽*高格式 |string |2 |news
|showType |展示格式，[参照格式定义](#showType格式定义) |int |2 |news
|isTop	|是否置顶	|int |2	|news
|time	|发布时间	|long |2	|news
|content	|正文内容，只针对段子频道返回	|string |2	|news
|upCount	|顶数量 |long	|2	|news
|downCount	|踩数量 |long	|2	|news
|shareCount	|分享数量 |long	|2	|news
|isComment |是否支持评论，1是0否 |int |2 |news
|comment_count |评论数量 |long |2 |news
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

* 一般频道响应示例
```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "picNews": [
        {
            "title": "壮观！数万只鹦鹉聚集饮水遮天蔽日", 
            "time": 1455775831928, 
            "source": "新华网", 
            "content_url": "http://go.10086.cn/surfnews/images/4061/20160218/5369992/5369992.html", 
            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160218/back/1454034002240.jpg", 
            "newsUrl": "http://news.xinhuanet.com/photo/2016-02/18/c_128730012.htm", 
            "isTop": 0, 
            "type": 2, 
            "bannerUrl": "http://go.10086.cn/surfnews/images/4061/20160218/back/1454034002240.jpg", 
            "open_type": 0, 
            "ctype": 0, 
            "is_energy": 1, 
            "positive_energy": 18412, 
            "negative_energy": 0, 
            "isComment": 0, 
            "comment_count": 0, 
            "updateTime": 1455775478000, 
            "rssId": 59322631, 
            "rssName": "网事联播", 
            "rssIcon": "http://go.10086.cn/hotpic/201506/01/source59322631.jpg", 
            "rssSource": "1", 
            "showType": 1003, 
            "newsId": 5369992, 
            "channelId": 4061, 
            "webView": 0, 
            "bigImg": "1", 
            "content_type": 1
        }
    ], 
    "news": [
        {
            "title": "微信提现收费动了谁的奶酪？", 
            "time": 1455762554000, 
            "source": "快讯", 
            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160218/back/1454034002186.jpg", 
            "dm":"600*800",
            "newsUrl": "http://go.10086.cn/surfnews/usr/surf/special/20160218/1323.html", 
            "isTop": 1, 
            "type": 1, 
            "open_type": 1, 
            "iconPath": "http://go.10086.cn/surfnews/usr/surf/icon/20151209174953.png", 
            "ctype": 0, 
            "multiImgUrl": [
                {
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160218/back/1454034002186.jpg"
                }, 
                {
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160218/back/1454034002188.jpg"
                }, 
                {
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160218/back/1454034002189.jpg"
                }
            ], 
            "is_energy": 0, 
            "isComment": 0, 
            "comment_count": 0, 
            "updateTime": 1455776734000, 
            "showType": 1002, 
            "newsId": 5363474, 
            "channelId": 4061, 
            "webView": 1, 
            "content_type": 1,
            "rssId": 59322631, 
            "rssName": "网事联播", 
            "rssIcon": "http://go.10086.cn/hotpic/201506/01/source59322631.jpg", 
            "rssSource": "1"
        }
    ]
}
```

* 段子频道响应示例

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

