# 新版本

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
| channelList | 频道列表 | 对象数组 |1||
| channelId | 频道ID | long | 2 | channelList
| channelName | 频道名称 | int | 2 | news
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