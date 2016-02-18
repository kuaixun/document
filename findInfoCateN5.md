# 新版本频道列表

#### **实现说明**  

新版本频道列表接口*findInfoCateN5*是为android客户端5.0之后的版本实现的接口，和[*findInfoCateN*](findInfoCateN.html)相比主要修改是规范了一些字段的命名，并且增加了一些满足新需求的字段。

#### **请求说明**

* 请求方法 *findInfoCateN5*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| os | 客户端类型(android/iphone/androidTV/surfwap) | string | 是 |
| vercode | 客户端版本号 | long | 是 |

* 返回参数

| 字段 | 说明 | 类型 | 级别 | 父节点
| -- | -- | -- |
|channelList |频道列表 |对象数组 |1 ||
|channelId |频道ID |long |2 | channelList
|channelName |频道名称 |string |2 | channelList
|channelIndex |频道位置，值越小的越靠前 |long	|2	|channelList
|isBeauty	|请求类型，0传统方式，1美女频道，2webview打开，3T+，4订阅频道，5段子，6趣图|int	|2	|channelList
|isMixRecom	|是否混合推荐，1是0否 |int 	|2	|channelList
|isWidget	|是否在widget展示，1是0否	|int |2	|channelList
|isnew	|是否提示更新，0否，1point，2new，3hot	|int |2	|channelList
|type	|维护类型，0快讯维护，1冲浪维护，2图集，3微博源，4视频 |int |2	|channelList
|position |栏目位置，0顶部，1隐藏 |int |2 |channelList



* 响应示例

```
{
    "channelList": [
        {
            "channelId": 2186000, 
            "channelIndex": 55093, 
            "channelName": "时尚", 
            "isBeauty": 0, 
            "isMixRecom": 0, 
            "isWidget": 0, 
            "isnew": 0, 
            "openType": 0, 
            "parent_id": -1, 
            "position": 0, 
            "type": 0
        },
        {
            "channelId": 0, 
            "channelIndex": 4063, 
            "channelName": "南京", 
            "isBeauty": 0, 
            "isWidget": 0, 
            "parent_id": -1, 
            "type": 0
        }
    ], 
    "cityRssList": [
        {
            "cityId": "101010100", 
            "cityName": "北京", 
            "enName": "Beijing", 
            "parentId": "-1", 
            "parentName": "中国"
        }, 
        {
            "cityId": "101040100", 
            "cityName": "重庆", 
            "enName": "Chongqing", 
            "parentId": "-1", 
            "parentName": "中国"
        }
    ], 
    "cityRssTime": 1454034404000, 
    "rec": [
        {
            "recid": "59144189", 
            "recimg": "http://192.168.10.150:8090/hotpic/201304/23/source5914418920130423093651.png", 
            "recname": "百度军事", 
            "channelId": 438757
        }, 
        {
            "recid": "59145559", 
            "recimg": "http://192.168.10.150:8090/hotpic/201410/29/source5914555920141029154113.jpg", 
            "recname": "搜狐军事", 
            "channelId": 438757
        }
    ], 
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }
}
```