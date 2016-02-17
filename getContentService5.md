# 获取正文5

#### **实现说明**  

新版本获取正文接口*getContentService5*是为android客户端5.0之后的版本实现的接口，新版本正文统一使用html格式，新的正文接口返回正文的相关信息，包括正文地址、标题、时间、评论等。

#### **请求说明**

* 请求方法 *getContentService5*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| id | 新闻ID | long | 是 |
| type |新闻类型，0是普通新闻，1是RSS新闻 | int | 是 |

* 返回参数

| 字段 | 说明 | 类型 | 级别 | 父节点
| -- | -- | -- |
| news | 新闻 | 对象 |1||
|title	|标题 |string	|2	|news
|source	|来源 |string 	|2	|news
|comment_count	|评论数量	|long |2	|news
|content_url	|正文地址	|string |2	|news
|newsUrl	|新闻源网址	|string |2	|news
|is_collected |是否已经收藏,1是0否 |int | 2 |news
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
|hot	|是否热门评论，1是0否 |string	|3	|hot_comment_list
|createtime	|评论时间 |long	|3	|hot_comment_list
|location	|位置 |string	|3	|hot_comment_list


* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "news": {
        "title": "郭美美开设赌场被判5年", 
        "source": "新华社",
        "comment_count": 26, 
        "content_url": "http://go.10086.cn/surfnews/images/4061/20150910/3094871/3094871.html", 
        "newsUrl": "http://sinanews.sina.cn/sharenews.shtml?id=fxhuyha2136212-comos-zx-cms&wm=3200_00031", 
        "is_collected": "0",
        "hot_comment_list": [
            {
                "id": 96150, 
                "commentId": 96150, 
                "newsid": 3094871, 
                "content": "富的有几个不违法！", 
                "uid": "c14fbdcbf037faad5aee1375ba6a56ab", 
                "did": "ebd62283-f198-3f45-bdeb-70a5e88a8694", 
                "status": 1, 
                "hot": 0, 
                "createtime": 1441935168890, 
                "location": "湖南省永州"
            }, 
            {
                "id": 95705, 
                "commentId": 95705, 
                "newsid": 3094871, 
                "content": "人家还是小孩，有错就改吗？", 
                "did": "f7270686-ca73-3d8a-8f8d-16c760f52a55", 
                "status": 1, 
                "hot": 0, 
                "createtime": 1441923875171, 
                "location": ""
            }, 
            {
                "id": 95627, 
                "commentId": 95627, 
                "newsid": 3094871, 
                "content": "还可以", 
                "uid": "72e57cd74ef9b1ff", 
                "did": "79A7BACB-DF19-47B7-8EFF-75703F11C13F", 
                "status": 1, 
                "hot": 0, 
                "createtime": 1441904152849, 
                "location": "上海上海"
            }
        ]
    }
}
```