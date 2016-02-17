# 新版本

#### **实现说明**  

新版本新闻列表接口*findInfoNByCoid5*是为android客户端5.0之后的版本实现的接口，和[*findInfoNByCoidN*](findInfoNByCoidN.html)相比主要的修改是规范了一些字段的命名，并且增加了一些针对新需求的字段。

#### **请求说明**

* 请求方法 *findInfoNByCoid5*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| coid | 频道ID | long | 是 |
| page | 请求页数，从1开始 | int | 是 |

* 返回参数

| 字段 | 说明 | 类型 | 父节点
| -- | -- | -- |
| news | 新闻列表，详细字段参照接口[*findInfoNByCoid5*](findInfoNByCoid5.html) | 对象数组 ||
| ts | 新闻刷新时间戳,由客户端下一次请求时传回 | string | | |

* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "news": [
        {
            "title": "8旬老人含泪打官司要儿子一家搬走:不养老还啃老", 
            "desc": "杭州的王阿姨八十多岁了,去年她打了生平第一场官司,要求同住的儿子一家搬出去,最近又到法院申请强制执行。", 
            "time": 1453945320000, 
            "source": "新华炫闻", 
            "content_url": "http://192.168.10.170:8091/images/4061/20160128/3777717/3777717.html", 
            "newsUrl": "http://xw.feedss.com/show/index?newsid=235388", 
            "isTop": 0, 
            "type": 1, 
            "open_type": 0, 
            "ctype": 0, 
            "is_energy": 0, 
            "comment_count": 0, 
            "imageScale": 1, 
            "intimacyDegree": 2000, 
            "rssId": 59233315, 
            "rssName": "搜狐猛图", 
            "rssIcon": "http://192.168.10.150:8090/hotpic/201503/04/source5923331520150304174215.jpg", 
            "rssSource": "1", 
            "showType": 1003, 
            "newsId": 3777717, 
            "channelId": 4061, 
            "webView": 0, 
            "content_type": 1
        }
    ], 
    "ts": "1453949923000,1453945321000,0,0"
}
```

