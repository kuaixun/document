# 软件更新

#### **请求说明**

* 请求方法 *softUpdate*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| reqType | 请求类型，"0"："软件自动新""1"："手动检查更新" | int | 是 |
| vercode |冲浪桌面当前版本号 | int | 是 |

* 返回参数
 
| 字段名 | 说明 | 类型 | 级别 | 父节点 |
| -- | -- | -- | -- | -- |
| res | 响应 |  | 1 |  |
| resCode | 响应代码 | String | 2 | res |
| resMessage | 响应信息 | String | 2 | res |
| userid | 用户id | long | 1 |  |
| mob | 用户手机号（加密） | String | 1 |  |
| cityId | 城市id | String | 1 |  |
| city | 城市名称 | String | 1 |  |
| prov | 省份名称 | String | 1 |  |
| sid | 加密id | String | 1 |  |
| suid | 加密后的uid | String | 1 |  |
| hateOptions | 不喜欢选项标签信息,多个用逗号间隔 | String | 1 |  |
| navigateUrl | 导航地址 | String | 1 |  |
| notifyNewsCoids | 常驻通知新闻获取频道，多个用逗号间隔 | String | 1 |  |
| ih |  | String | 1 |  |
| newconfig |  | String | 1 |  |
| flowSyn | 流量信息 |  | 1 |  |
| dataTime | 时间 | String | 2 | flowSyn |
| flowData | 流量数据 | String | 2 | flowSyn |
| type | 流量类型 | String | 2 | flowSyn |
| skinPackage | 皮肤包 |  | 1 |  |
| downUrl | 文件下载路径 | String | 2 | skinPackage |
| file | 文件MD5校验，用于判断下载文件是否完整 | String | 2 | skinPackage |
| id | 皮肤包id | long | 2 | skinPackage |
| isEnable | 是否发布 | int | 2 | skinPackage |
| packageName | 皮肤包名称 | String | 2 | skinPackage |
| packageUrl | 文件存放路径 | String | 2 | skinPackage |
| updateTime | 更新时间，用于判断是否下载皮肤包 | long | 2 | skinPackage |

* 响应示例

```
{
    "city":"南京",
    "cityId":"101190101",
    "defaultEnergy":-10000,
    "flowSyn":[
        {
            "dataTime":"20160226 030110",
            "flowData":"0.0",
            "type":"0"
        },
        {
            "dataTime":"20160226 030110",
            "flowData":"62232.0",
            "type":"1"
        },
        {
            "dataTime":"20160226 030110",
            "flowData":"0.0",
            "type":"2"
        },
        {
            "dataTime":"20160226 030110",
            "flowData":"5824103.0",
            "type":"3"
        }
    ],
    "hateOptions":"重复,旧闻,质量差,其他",
    "navigateUrl":"https://www.baidu.com",
    "ih":"1",
    "mob":"95c03d669b2dc810d32e31b8b0940be5",
    "newconfig":"56:0:90",
    "prov":"江苏",
    "res":{
        "reCode":"1",
        "resMessage":"Operation is successful"
    },
    "sid":"1",
    "suid":"c73b32b8fbbab6d05aee1375ba6a56ab",
    "userid":12111545
    "skinPackage":{
        "downUrl":"http://192.168.10.170:8091/surfnews/usr/surf/package/20160226/new_year.zip",
        "file":"776b4ed897aa79f1ab025870d5e91503",
        "id":1,
        "isEnable":1,
        "packageName":"new year",
        "packageUrl":"/usr/surf/package/20160226/new_year.zip",
        "updateTime":1456458902141
    }
}
```