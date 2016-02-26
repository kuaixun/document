# 收藏列表

#### **实现说明**  

收藏列表接口*getCollectedList*用来获取用户收藏的新闻列表。

#### **请求说明**

* 请求方法 *getCollectedList*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| uid | 用户ID | long | 是 |

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
    "news": [
        {
            "newsId": 1056647, 
            "title": "国有银行离职潮再起 个人发展受阻成离职主因", 
            "showTime": 1440490870000, 
            "type": 0, 
            "source": "财经网", 
            "contentUrl": "/usr/surf/collect/4061/20150825/1056647/1056647.xml", 
            "newsUrl": "http://news.sohu.com/20150825/n419682916.shtml", 
            "coid": 4061, 
            "openType": 0
        }, 
        {
            "newsId": 60930, 
            "title": "江苏城市国际化专题咨询会举行", 
            "showTime": 1387496155000, 
            "type": 0, 
            "source": "要闻资讯", 
            "contentUrl": "/usr/surf/collect/320000/20131220/60930/60930.xml", 
            "newsUrl": "http://3g.jssjb.net/content.php?id=7468559", 
            "coid": 0, 
            "openType": 0
        }
    ]
}
```