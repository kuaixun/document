# 待添加站点列表

#### **实现说明**  

待添加站点列表接口*toAddSites*提供了导航版APP可添加的站点数据列表。

#### **请求说明**

* 请求方法 *toAddSites*

* 请求参数

* 返回参数

| 字段 | 说明 | 类型 | 级别 | 父节点
| -- | -- | -- |
| item | 新闻列表 | 对象数组 |1 |  |
| dataId | 主键 | long | 2 | item |
| name | 网站名称 | String | 2 | item |
| category | 网站分类 | String | 2 | item |
| img | 网站图标地址，当前版本不提供 | String | 2 | item |
| url | 网页地址 | String | 2 | item

* 响应示例
```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "item": [
        {
            "dataId": 1, 
            "name": "糗事百科", 
            "category": "笑话", 
            "url": "http://go.10086.cn"
        }, 
        {
            "dataId": 2, 
            "name": "快乐麻花", 
            "category": "笑话", 
            "url": "https://m.baidu.com"
        }, 
        {
            "dataId": 3, 
            "name": "捧腹", 
            "category": "笑话", 
            "url": "http://www.qq.com"
        }, 
        {
            "dataId": 4, 
            "name": "暴走漫画", 
            "category": "笑话", 
            "url": "http://www.people.com.cn"
        }, 
        {
            "dataId": 5, 
            "name": "儒豹段子", 
            "category": "笑话", 
            "url": "http://www.youku.com"
        }, 
        {
            "dataId": 6, 
            "name": "笑话之王", 
            "category": "笑话", 
            "url": "https://www.taobao.com"
        }, 
        {
            "dataId": 7, 
            "name": "内涵段子", 
            "category": "笑话", 
            "url": "http://www.jd.com"
        }, 
        {
            "dataId": 8, 
            "name": "凤凰军事", 
            "category": "军事", 
            "url": "http://www.ifeng.com"
        }, 
        {
            "dataId": 9, 
            "name": "抗美援朝", 
            "category": "军事", 
            "url": "http://go.10086.cn"
        }, 
        {
            "dataId": 10, 
            "name": "附近的人", 
            "category": "交友", 
            "url": "https://m.baidu.com"
        }, 
        {
            "dataId": 11, 
            "name": "陌陌", 
            "category": "交友", 
            "url": "http://www.qq.com"
        }, 
        {
            "dataId": 12, 
            "name": "土豆", 
            "category": "视频", 
            "url": "http://www.people.com.cn"
        }, 
        {
            "dataId": 13, 
            "name": "优酷", 
            "category": "视频", 
            "url": "http://www.youku.com"
        }, 
        {
            "dataId": 14, 
            "name": "起点", 
            "category": "小说", 
            "url": "https://www.taobao.com"
        }, 
        {
            "dataId": 15, 
            "name": "红袖添香", 
            "category": "小说", 
            "url": "http://www.jd.com"
        }
    ]
}
```