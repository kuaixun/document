# 热门搜索词
#### **实现说明**  

热门搜索词接口用来获取热门新闻搜索词列表，搜索词会自动更新，接口每次返回十条热词。

#### **请求说明**

* 请求方法 *getHotWordsList*

* 请求参数



* 返回参数

| 字段 | 说明 | 类型 | 级别 | 父节点
| -- | -- | -- |
|item | 热词列表 | 对象数组 |1 ||
|hotWords | 热词 | string | 2 | item | 

* 响应示例
```
{
    "item": [
        {
            "hotWords": "霍建华曝隆诗秘恋"
        }, 
        {
            "hotWords": "武汉公园卡通红军"
        }, 
        {
            "hotWords": "雷雨导致飞机延误"
        }, 
        {
            "hotWords": "河北民营钢企调查"
        }, 
        {
            "hotWords": "打麻将一年输4万"
        }, 
        {
            "hotWords": "少年突发缄默症"
        }, 
        {
            "hotWords": "金扫帚奖杨幂封后"
        }, 
        {
            "hotWords": "苹果发布会将至"
        }, 
        {
            "hotWords": "深圳母子洗澡身亡"
        }, 
        {
            "hotWords": "莱昂纳多写书法"
        }
    ], 
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }
}
```
