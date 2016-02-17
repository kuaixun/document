# 获取正文5

#### **实现说明**  

新版本获取正文接口*getContentService5*是为android客户端5.0之后的版本实现的接口，新版本正文统一采用html格式实现，新的正文接口返回正文的相关信息，包括正文地址、标题、时间、评论等。

#### **请求说明**

* 请求方法 *getContentService5*

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
|hot	|是否热门评论，1是0否 |int	|3	|hot_comment_list
|createtime	|评论时间 |long	|3	|hot_comment_list
|location	|位置 |string	|3	|hot_comment_list


* 响应示例

```