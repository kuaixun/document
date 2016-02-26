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
| res | 1:2 | 2:2 | 3:2 |  |
| resCode | 1:3 | 2:3 | 3:3 | res |
| resMessage | 1:4 | 2:4 | 3:4 | res |
| userid | 1:5 | 2:5 | 3:5 |  |
| mob | 1:6 | 2:6 | 3:6 |  |
| cityId | 1:7 | 2:7 | 3:7 |  |
| city | 1:8 | 2:8 | 3:8 |  |
| prov | 1:9 | 2:9 | 3:9 |  |
| sid | 1:10 | 2:10 | 3:10 |  |
| suid | 1:11 | 2:11 | 3:11 |  |
| hateOptions | 1:12 | 2:12 |  |  |
| ih | 1:13 | 2:13 | 3:13 |  |
| newconfig | 1:14 | 2:14 |  |  |
| flowSyn | 1:15 | 2:15 | 3:15 |  |
| dataTime | 1:15 | 2:15 | 3:15 | flowSyn |
| flowData | 1:15 | 2:15 | 3:15 | flowSyn |
| type | 1:15 | 2:15 | 3:15 | flowSyn |
| skinPackage | 1:15 | 2:15 | 3:15 |  |
| downUrl | 1:15 | 2:15 | 3:15 | skinPackage |
| file | 1:15 | 2:15 | 3:15 | skinPackage |
| id | 1:15 | 2:15 | 3:15 | skinPackage |
| isEnable | 1:15 | 2:15 | 3:15 | skinPackage |
| packageName | 1:15 | 2:15 | 3:15 | skinPackage |
| packageUrl | 1:15 | 2:15 | 3:15 | skinPackage |
| updateTime | 1:15 | 2:15 | 3:15 | skinPackage |


