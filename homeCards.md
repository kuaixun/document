# 主页卡片

#### **接口说明**  

主页卡片接口是导航版APP的首页数据接口。

#### **实现逻辑**
##### 关于卡片类型
卡片类型由产品定义，客户端通过接口返回的cardType字段来识别和实现相应的卡片布局；客户端内置支持一批卡片类型，不识别的卡片类型不显示，新增卡片类型需通过客户端升级才能支持。

目前客户端需支持的卡片类型如下：

| cardType | 卡片类型 | 需求说明 | 实现说明 |
| -- | -- | -- | -- | -- | -- |
| 1001 | 新闻导航 | 1、app主要模块导航位，图标、链接、排序后台均可配，固定展示，用户不可移除。<br>2、根据内容默认显示一行（5个+1个更多），点更多可下拉展示更多隐藏模块，最多展示2行11个；<br>3、如导航模块数量小于7，则不显示更多，反之才显示；<br>4、新闻-进入app底部资讯tab“热点”界面；视频-进入app底部视频tab首界面；其他-进入app底部导航对应栏目界面
 | |

   
#### **请求说明**

* 请求方法 *findInfoNByCoidN*

    客户端需要根据频道接口[*findInfoCateN5*](findInfoCateN5.html)返回的isMixRecom字段来控制刷新逻辑，isMixRecom为1时使用*findInfoNByCoidN*，为0时使用*findInfoNByCoid5*；
    
    客户端需要在本地保存刷新得到的广告除外的新闻，并且在每次刷新时根据newsId**去重**，广告根据是否有adid字段来判断，有的即为广告；

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| coid | 频道ID | long | 是 |
| gesture | 刷新手势，0下拉，1上滑 | int | 是 |
| interval | 分频道下拉刷新(仅针对gesture为0)的请求间隔分钟数<br>客户端第一次请求传0，后面请求每次至少传1 | int  | 是 | 
| ts | 分频道新闻刷新(包括下拉和上滑)时间戳<br>客户端第一次请求时传空串，后面每一次请求使用上一次请求返回的值 | string | 是

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