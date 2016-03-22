# 网址收藏列表

#### **实现说明**  

网址收藏列表接口用来获取用户网页收藏列表。

#### **请求说明**

* 请求方法 *upDownOper*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| newsId | 新闻ID | long | 是 |
| type |操作类型，1顶，2踩，3分享 | int | 是 |
* 返回参数

* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }
}
```