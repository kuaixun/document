# 主页卡片

### **接口说明**  

主页卡片接口是导航版APP的首页数据获取接口。

### **实现逻辑**
#### 主页属性
主页拥有一系列属性，主要包括配置项字段和一些控制字段：

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| cardSortTime | 卡片排序更新时间，更新时间变化时以服务端排序为准 | long时间戳 |
| dhSearchUrl | 导航搜索风云榜页面地址 | string |
| dhNovelUrl | 小说卡片更多链接地址 | string |
| moreFlowUrl | 查询更多流量套餐跳转地址 | string |
| mobBusiUrl | 掌上营业厅跳转地址 | string |
| controls | 控制开关字段，由多个具体的控制项组成，每项占一个字节，目前从低位到高位分别表示是否显示添加网站按钮，后续可扩展； | int |

#### 卡片属性
每个卡片都拥有一系列属性，用来控制卡片的展示和实现逻辑：

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| cardId | 主键 | long |
| name | 卡片名称 | String |
| desc | 卡片描述 | String |
| type | 卡片类型，用于唯一标识某种卡片实现，具体定义见卡片类型一节 | String |
| moreLink | 更多跳转，用于唯一标识某种卡片实现，具体定义见卡片类型一节 | String |
| controls | 控制开关字段，由多个具体的控制项组成，每项占一个字节，目前从低位到高位分别表示是否可排序、是否可关闭显示，后续可扩展；举例如返回数字2，对应二级制10，则表示可关闭显示、不可排序 | int |
| sort | 排序字段 | long |
| newsList | 新闻类数据项列表，不同的卡片对应的数据项列表字段可能不一样，详细数据项属性见卡片类型一节 | Object Array |
| dataList | 其他类数据项列表，不同的卡片对应的数据项列表字段可能不一样，详细数据项属性见卡片类型一节 | Object Array |

#### 卡片数据更新
由于首页接口数据量大，针对特定卡片客户端可以指定卡片类型，服务端会只查询特定的卡片数据，同时扩展支持查询所有新闻类卡片数据，请求参数：

| 参数 | 说明 | 类型 |
| -- | -- | -- |
| type | 卡片类型，默认返回所有卡片数据，具体的type指定返回特定的卡片数据，扩展kuaixun查询所有快讯新闻类卡片数据 | string |

#### 卡片类型
卡片类型由产品定义，客户端通过接口返回的type字段来识别和实现相应的卡片布局；客户端内置支持一批卡片类型，不识别的卡片类型不显示，新增卡片类型或修改现有卡片布局需通过客户端升级完成。

目前客户端需支持的卡片类型如下：
##### 模块导航
* type定义：*nav*
* 需求说明  
  1.app主要模块导航位，图标、链接、排序后台均可配，固定展示，管理员控制用户不可移除；  
  2.导航模块至少配1行6个（考虑视觉效果），如超过6组则点更多可下拉展示更多隐藏模块，最多展示2行11个；  
  3.导航模块数量1~6个，则单行显示，且不显示更多；数量7~11个，分2行显示，首行最后1个显示为“更多/收起”；  
  4.根据内容分2种跳转方式（app内部和url方式跳转）：新闻-进入app底部资讯tab“推荐”界面；视频-进入app底部视频tab首界面；其他-进入app底部导航对应栏目界面
* 实现说明  
  1.更多逻辑由客户端实现；  
  2.跳转TAB当前定义为1新闻TAB，2视频TAB，3导航TAB；后面扩展的新TAB标识客户端应该判断并屏蔽；
* 字段定义

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| dataId | 主键 | long |
| name | 显示名称 | String |
| img | 显示图标http地址 | String |
| jumpType | 跳转类型,1APP内部TAB频道,2外部URL | int |
| tab | 跳转TAB当前定义为1新闻TAB，2视频TAB，3导航TAB | int |
| coid | 跳转频道ID,如为空则默认跳转TAB第一个频道 | long |
| url | 跳转http地址 | String | 

##### 每日快讯  
* type定义：*news*
* 需求说明  
  1.固定展示4条新闻（第1条固定为左文右图最多2行标题+来源（icon根据配置显示）形式，2-4条固定为纯文字标题单行（icon根据配置显示），点击新闻区块进入新闻正文；  
  2.点击换一换卡片内每次更换4条（从本地缓存里轮播更换），点击更多快讯，进入资讯tab的“推荐”频道；  
  3.新闻为推荐频道编辑推荐内容；  
  4.可设置每隔xx分钟自动取推荐频道最新16条（4条的倍数）存本地（广告除外），按时间倒序显示；  
  5.接口返回数量或内容类型异常时的处理机制；
* 实现说明  
  1.更多快讯和换一换逻辑由客户端实现；  
  2.服务端控制只返回带缩略图的新闻；  
  3.数据更新可以指定请求type；
* 字段定义  
参照接口返回[*findInfoNByCoid5*](findInfoNByCoid5.html)

##### 搞笑视频
* type定义：*funny*
* 需求说明  
  1.固定展示4条视频新闻，均以缩略图+标题形式（标题最多2行）展示，点击视频缩略图+标题区块进入对应页面（不在主页直接播放）；  
  2.点击换一换卡片内每次更换4条（从本地缓存里轮播更换），点击更多视频，进入视频tab的首界面；  
  3.内容为视频内容类型；  
  4.可设置每隔xx分钟自动取最新16条（4条的倍数）存本地（景点直播频道除外），按时间倒序显示；  
  5.接口返回数量或内容类型异常时的处理机制；
* 实现说明  
  1.更多视频和换一换逻辑由客户端实现；  
  2.服务端控制只返回带缩略图的视频新闻；  
  3.数据更新可以指定请求type；
* 字段定义  
参照接口返回[*findInfoNByCoid5*](findInfoNByCoid5.html)

##### 轻松一刻
* type定义：*joke*
* 需求说明  
  1.每1小时自动获取段子频道最新16条入缓存，按时间先后显示1条，点击换一换按顺序循环显示，每次1条；  
  2.均以纯文字形式展示，最多显示400字符（字数超过...），点击内容区域进入对应正文页；  
  3.点击更多笑料，进入资讯tab的段子频道列表；
* 实现说明  
  1.更多笑料和换一换逻辑由客户端实现；  
  2.数据更新可以指定请求type；
* 字段定义  
参照接口返回[*findInfoNByCoid5*](findInfoNByCoid5.html)

##### 美女
* type定义：*beauty*
* 需求说明  
  1.每1小时自动获取美女频道最新16张图入缓存，按时间先后显示2张，点击换一换按顺序循环显示，每次2条；  
  2.2张图片可点，点击后进入资讯tab的美女频道对应图片的幻灯模式，点back返回美女列表；  
  3.点击更多美图，进入资讯tab的美女频道列表；
* 实现说明  
  1.更多美图、幻灯片和换一换逻辑由客户端实现；  
  2.数据更新可以指定请求type；  
* 字段定义  
参照接口返回[*findInfoNByCoid5*](findInfoNByCoid5.html)

##### 推荐站点
* type定义：*site*
* 需求说明  
  展示数量：  
  1.分为默认内容和用户自定义添加内容；  
  2.默认内容预设显示4行15组网站（第16个为添加入口），默认内容只能由管理员控制修改；  
  3.用户可在添加界面新增或取消新增内容（添加界面的网站和分类也由管理员管理），新增的网站在默认网站后排序展示；  
  4.总网站显示数量不做限制；
* 实现说明  
  1.添加的网页保存在客户端，由客户端控制；  
* 字段定义  

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| dataId | 主键 | long |
| name | 网站名称 | String |
| img | 网站图标http地址 | String |
| url | 网页地址 | String |

##### 最常访问
* type定义：*latest*
* 需求说明    
  1.无数据时显示“暂无访问记录”；  
  2.有数据时最多可显示2行8个（每行4组，每组最多显示8个字符）；  
  3.客户端根据用户访问时间显示最近访问的8个常用网址，点击webview方式进入对应网址；  
  4.卡片内显示名称为网页title，排序根据用户最近访问（在app内）的时间由近及远显示；
* 实现说明  
  1.最常访问逻辑由客户端实现；  
* 字段定义  
无

##### 实时热点
* type定义：*hot*
* 需求说明  
  1.每1小时自动获取快讯抓取百度热词接口最新18条入缓存，按时间先后显示1-6条，点击换一换按顺序循环显示，每次6条；  
  2.均以纯文字形式展示，每组至少能显示16个字符（超过字符...），单行显示，，点击单条热词进入对应热词百度搜索页面；   
  3.点击更多热点，进入导航搜索风云榜页面；  
* 实现说明  
  1.换一换由客户端实现；  
  2.导航搜索风云榜页面地址通过配置对象字段返回；
* 字段定义  

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| dataId | 主键 | long |
| name | 热词名称 | String |

##### 小说
* type定义：*novel*
* 需求说明  
  1.推荐3本含封面小说（单行标题）+2条文字小说（单行标题，含xx次阅读的备注）均通过后台人工配置更新；  
  2.点击换一换，更新上方5条后台配置的内容（后台可多配1-2倍对应数据，图片和文字类对应更换）；  
  3.点击更多小说进入导航小说页面；
* 实现说明  
  1.换一换由客户端实现；  
  2.更多链接地址通过配置对象字段返回；  
* 字段定义  

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| dataId | 主键 | long |
| name | 小说主题 | String |
| showType | 展示类型，1图片小说，2文字小说 | int |
| img | 小说封面地址 | String |
| url | 小说跳转地址 | String |
| readCount | 阅读数量 | long |

##### 广告
* type定义：*ad*
* 需求说明  
  1.该卡片不在卡片管理内显示，由管理员控制是否发布和排序（在app内显示）；  
  2.广告卡片内容对接直投平台广告位，纯图片，支持单张（单张时不显示锚点）或多张（最多5张），用户可关闭（app缓存清除后重现）
* 实现说明  
  1.广告不可排序；  
  2.广告显示类型支持扩展；
* 字段定义  

| 属性 | 说明 | 类型 |
| -- | -- | -- |
| dataId | 主键 | long |
| showType | 广告展示类型，1单张大图，其他待扩展 | int |
| name | 广告主题 | String |
| img | 广告图片地址 | String |
| url | 广告跳转地址 | String |

##### 话费流量查询
* type定义：*flow*
* 需求说明  
  1.调流量查询接口定期获取最新话费余额和流量余额数据；  
  2.点击中间区块和更多查询进入个人中心话费流量查询详情页；  
  3.点击刷新在卡片内刷新数据（如有更新再更换），同时显示卡片更新时间；  
  4.如果用户没有流量套餐，则话费居中显示;
* 实现说明  
  1.话费流量刷新由客户端调用接口获取数据；
* 字段定义  
  待定
   
#### **请求说明**

* 请求方法 *homeCards*

* 请求参数

| 字段 | 说明 | 类型 | 必须 |
| -- | -- | -- | -- | -- | -- |
| type | 卡片类型，默认返回所有卡片数据，具体的type指定返回特定的卡片数据，扩展kuaixun查询所有快讯新闻类卡片数据 | string | 否 |

* 返回参数
返回参数参照上面实现说明

* 响应示例

```
{
    "res": {
        "reCode": "1", 
        "resMessage": "Operation is successful"
    }, 
    "config": {
        "cardSortTime": 1468918387354, 
        "dhSearchUrl": "http://go.10086.cn/hao/dwz/0000rT", 
        "dhNovelUrl": "http://go.10086.cn/hao/dwz/0000rR", 
        "moreFlowUrl": "https://m.baidu.com/", 
        "mobBusiUrl": "https://m.baidu.com/",
        "controls": 1
    }, 
    "cardList": [
        {
            "cardId": 1, 
            "name": "模块导航", 
            "desc": "冲浪快讯，从这里开始", 
            "type": "nav", 
            "controls": 3, 
            "sort": 1, 
            "dataList": [
                {
                    "dataId": 101, 
                    "name": "新闻", 
                    "img": "", 
                    "jumpType": 1, 
                    "tab": 1, 
                    "coid": 4061
                }, 
                {
                    "dataId": 102, 
                    "name": "视频", 
                    "img": "", 
                    "jumpType": 1, 
                    "tab": 2
                }, 
                {
                    "dataId": 103, 
                    "name": "美女", 
                    "img": "", 
                    "jumpType": 1, 
                    "tab": 1, 
                    "coid": 1024229
                }, 
                {
                    "dataId": 104, 
                    "name": "趣图", 
                    "img": "", 
                    "jumpType": 1, 
                    "tab": 1, 
                    "coid": 5753911
                }, 
                {
                    "dataId": 105, 
                    "name": "游戏", 
                    "img": "", 
                    "jumpType": 2, 
                    "url": "http://go.10086.cn/hao/dwz/0000rU"
                }, 
                {
                    "dataId": 106, 
                    "name": "小说", 
                    "img": "", 
                    "jumpType": 2, 
                    "url": "http://go.10086.cn/hao/dwz/0000rR"
                }, 
                {
                    "dataId": 107, 
                    "name": "生活", 
                    "img": "", 
                    "jumpType": 2, 
                    "url": "http://go.10086.cn/hao/dwz/0000rQ"
                }, 
                {
                    "dataId": 108, 
                    "name": "购物", 
                    "img": "", 
                    "jumpType": 2, 
                    "url": "http://go.10086.cn/hao/dwz/0000rP"
                }, 
                {
                    "dataId": 109, 
                    "name": "软件", 
                    "img": "", 
                    "jumpType": 2, 
                    "url": "http://go.10086.cn/hao/dwz/0000rw"
                }
            ]
        }, 
        {
            "cardId": 2, 
            "name": "图片广告", 
            "desc": "广告，让生活更精彩", 
            "type": "ad", 
            "controls": 2, 
            "sort": 2, 
            "dataList": [
                {
                    "dataId": 201, 
                    "name": "图片1", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group1/M00/00/06/rBAMVVdIBjOAGXG_AAA6hfGDazo740.jpg", 
                    "url": "http://rd.go.10086.cn:9090/ndisi/adclk?id=1zhYZ3gn&mcpid=h7ZgPZ5vi&show_form=0&bn=1&channel_num=&mobile=&ad_showform="
                }, 
                {
                    "dataId": 202, 
                    "name": "图片2", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group3/M00/00/05/rBAMWVdQ-iCAM9KUAAAt0SgM8NQ921.png", 
                    "url": "http://rd.go.10086.cn:9090/ndisi/adclk?id=1xOHZ3jV&mcpid=h7ZgPZ5va&show_form=0&bn=1&channel_num=&mobile=&ad_showform="
                }, 
                {
                    "dataId": 203, 
                    "name": "图片3", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group1/M00/00/08/rBAMVVeMaaiAXTziAAA7j8MUoqM860.jpg", 
                    "url": "http://rd.go.10086.cn:9090/ndisi/adclk?id=1zhYZ3Et&mcpid=h7ZgPZ5AX&show_form=0&bn=1&channel_num=&mobile=&ad_showform="
                }
            ]
        }, 
        {
            "cardId": 3, 
            "name": "推荐站点", 
            "desc": "最热门的站点推荐", 
            "type": "site", 
            "controls": 3, 
            "sort": 3, 
            "dataList": [
                {
                    "dataId": 301, 
                    "name": "冲浪导航", 
                    "img": "http://go.10086.cn/favicon.ico", 
                    "url": "http://go.10086.cn"
                }, 
                {
                    "dataId": 302, 
                    "name": "百度一下", 
                    "img": "https://m.baidu.com/favicon.ico", 
                    "url": "https://m.baidu.com"
                }, 
                {
                    "dataId": 303, 
                    "name": "腾讯首页", 
                    "img": "http://www.qq.com/favicon.ico", 
                    "url": "http://www.qq.com"
                }, 
                {
                    "dataId": 304, 
                    "name": "人民网", 
                    "img": "http://www.people.com.cn/favicon.ico", 
                    "url": "http://www.people.com.cn"
                }, 
                {
                    "dataId": 305, 
                    "name": "优酷", 
                    "img": "http://www.youku.com/favicon.ico", 
                    "url": "http://www.youku.com"
                }, 
                {
                    "dataId": 306, 
                    "name": "淘宝网", 
                    "img": "https://www.taobao.com/favicon.ico", 
                    "url": "https://www.taobao.com"
                }, 
                {
                    "dataId": 307, 
                    "name": "京东商城", 
                    "img": "http://www.jd.com/favicon.ico", 
                    "url": "http://www.jd.com"
                }, 
                {
                    "dataId": 308, 
                    "name": "凤凰网", 
                    "img": "http://www.ifeng.com/favicon.ico", 
                    "url": "http://www.ifeng.com"
                }, 
                {
                    "dataId": 309, 
                    "name": "冲浪导航", 
                    "img": "http://go.10086.cn/favicon.ico", 
                    "url": "http://go.10086.cn"
                }, 
                {
                    "dataId": 310, 
                    "name": "百度一下", 
                    "img": "https://m.baidu.com/favicon.ico", 
                    "url": "https://m.baidu.com"
                }, 
                {
                    "dataId": 311, 
                    "name": "腾讯首页", 
                    "img": "http://www.qq.com/favicon.ico", 
                    "url": "http://www.qq.com"
                }, 
                {
                    "dataId": 312, 
                    "name": "人民网", 
                    "img": "http://www.people.com.cn/favicon.ico", 
                    "url": "http://www.people.com.cn"
                }, 
                {
                    "dataId": 313, 
                    "name": "优酷", 
                    "img": "http://www.youku.com/favicon.ico", 
                    "url": "http://www.youku.com"
                }, 
                {
                    "dataId": 314, 
                    "name": "淘宝网", 
                    "img": "https://www.taobao.com/favicon.ico", 
                    "url": "https://www.taobao.com"
                }, 
                {
                    "dataId": 315, 
                    "name": "京东商城", 
                    "img": "http://www.jd.com/favicon.ico", 
                    "url": "http://www.jd.com"
                }
            ]
        }, 
        {
            "cardId": 4, 
            "name": "每日快讯", 
            "desc": "实时呈现个性化热点推荐", 
            "type": "news", 
            "controls": 3, 
            "sort": 4, 
            "newsList": [
                {
                    "id": 8157694, 
                    "title": "妻子成了我高中同学的情人，离婚后她又回头求我原谅！", 
                    "desc": "曾以为自己会娶一个爱自己的女人一直幸福的生活下去，哪怕日子过得稍微辛苦一点，只要两个人肯努力，什么都会解决。可是她的背叛", 
                    "time": 1468984779000, 
                    "source": "温暖情书", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/1355327/20160720/8157694/m/8157694_id_2_4100.webp", 
                    "newsUrl": "http://toutiao.com/group/6309238941740597506/", 
                    "isTop": -1, 
                    "type": 1, 
                    "coid": 1355327, 
                    "isTopOrderby": 0, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "referer": "tp_cijibco_tp", 
                    "rectype": 2, 
                    "ctype": 0, 
                    "webView": 0
                }, 
                {
                    "id": 8157126, 
                    "title": "外媒：至少14艘土耳其军舰未回港 或开往希腊", 
                    "time": 1468998826000, 
                    "source": "参考消息网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8157126/8157126.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442863.webp", 
                    "newsUrl": "http://i.ifeng.com/news/sharenews.f?aid=111270388", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8157166, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "iconPath": "http://go.10086.cn/surfnews/usr/surf/icon/20151209174928.png", 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -4410, 
                    "positive_count": 0, 
                    "negative_count": 131, 
                    "total_energy": -4410, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "dm": "218*143", 
                    "updateTime": 1468999142000, 
                    "showType": 1001, 
                    "newsId": 8157126, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8157082, 
                    "title": "台游览车起火事故初验:逃生门上栓 需用工具打开", 
                    "time": 1468997708000, 
                    "source": "澎湃新闻网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8157082/8157082.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8157082/m/8157082_id_1_3462.webp", 
                    "newsUrl": "http://news.163.com/16/0720/14/BSE44KUA0001124J.html", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8157083, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -4744, 
                    "positive_count": 0, 
                    "negative_count": 179, 
                    "total_energy": -4744, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468997590000, 
                    "showType": 1001, 
                    "newsId": 8157082, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8155430, 
                    "title": "上海金山一水上飞机试飞时撞桥 已救出5人", 
                    "time": 1468991887000, 
                    "source": "澎湃新闻网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8155430/8155430.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8155430/m/8155430_id_1_1737.webp", 
                    "newsUrl": "http://news.163.com/16/0720/13/BSDVFRBJ00014SEH.html", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8157069, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "iconPath": "http://go.10086.cn/surfnews/usr/surf/icon/20151209174928.png", 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 41462, 
                    "negative_energy": -147796, 
                    "positive_count": 22, 
                    "negative_count": 194, 
                    "total_energy": -106334, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468995419000, 
                    "showType": 1001, 
                    "newsId": 8155430, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8157063, 
                    "title": "特大暴雨导致河北邯郸涉县8村失联", 
                    "time": 1468996486000, 
                    "source": "央广网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8157063/8157063.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442775.jpg", 
                    "newsUrl": "http://news.sohu.com/20160720/n460117886.shtml", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8157063, 
                    "webp_flag": "0", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -12912, 
                    "positive_count": 0, 
                    "negative_count": 196, 
                    "total_energy": -12912, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "dm": "218*143", 
                    "updateTime": 1468996661000, 
                    "showType": 1001, 
                    "newsId": 8157063, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8152831, 
                    "title": "[视频]车匪路霸光天化日持刀抢劫货车", 
                    "time": 1468978462000, 
                    "source": "新浪视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442255.jpg", 
                    "newsUrl": "http://v.sina.cn/xkx/jingxuan/2016-07-20/miaopai-ifxuapvw2392403.d.html?vt=4&pos=91", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8155531, 
                    "open_type": 1, 
                    "iconPath": "http://go.10086.cn/surfnews/usr/surf/icon/20150929174346.png", 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 0, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468978615000, 
                    "showType": 1001, 
                    "newsId": 8152831, 
                    "channelId": 4061, 
                    "webView": 1, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8156342, 
                    "title": "暴雨袭城 天津进入“看海模式”", 
                    "time": 1468998765000, 
                    "source": "光明网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8156342/8156342.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8156342/m/8156342_id_0_7481.webp", 
                    "newsUrl": "http://photo.gmw.cn/2016-07/20/content_21047089.htm", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8155075, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "multiImgUrl": [
                        {
                            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8156342/m/8156342_id_0_7481.png"
                        }, 
                        {
                            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442859.png"
                        }, 
                        {
                            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8156342/m/8156342_id_3_2799.png"
                        }
                    ], 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -4479, 
                    "positive_count": 0, 
                    "negative_count": 133, 
                    "total_energy": -4479, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "dm": "218*143", 
                    "updateTime": 1468998725000, 
                    "showType": 1002, 
                    "newsId": 8156342, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8157067, 
                    "title": "2016高考状元调查:八成上普通幼儿园 父母多本科", 
                    "time": 1468996670000, 
                    "source": "澎湃新闻网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8157067/8157067.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442782.webp", 
                    "newsUrl": "http://news.163.com/16/0720/14/BSE2OE0B00014SEH.html", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8153764, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 8862, 
                    "negative_energy": 0, 
                    "positive_count": 196, 
                    "negative_count": 0, 
                    "total_energy": 8862, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "dm": "218*143", 
                    "updateTime": 1468996919000, 
                    "showType": 1001, 
                    "newsId": 8157067, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "shareCount": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8155531, 
                    "title": "13岁孙女被奶奶送人圆房治病 留遗书跳楼自杀", 
                    "time": 1468995481000, 
                    "source": "四川在线", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8155531/8155531.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442747.jpg", 
                    "newsUrl": "http://news.sina.com.cn/s/wh/2016-07-20/doc-ifxuapvs8919252.shtml", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8153704, 
                    "webp_flag": "0", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -67936, 
                    "positive_count": 0, 
                    "negative_count": 147, 
                    "total_energy": -67936, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "dm": "218*143", 
                    "updateTime": 1468995487000, 
                    "showType": 1001, 
                    "newsId": 8155531, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "shareCount": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8154951, 
                    "title": "养老院一张床24万起 26年内可住可租可卖", 
                    "time": 1468996960000, 
                    "source": "南方都市报", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8154951/8154951.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442783.webp", 
                    "newsUrl": "http://3g.oeeee.com/nis/201607/20/451768m.html?channel=jr_yaowen", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8153658, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -4146, 
                    "positive_count": 0, 
                    "negative_count": 182, 
                    "total_energy": -4146, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "dm": "218*143", 
                    "updateTime": 1468996814000, 
                    "showType": 1001, 
                    "newsId": 8154951, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8154382, 
                    "title": "爱国青年质疑抵制肯德基非理性遭围堵人士殴打", 
                    "time": 1468986574313, 
                    "source": "新京报", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8154382/8154382.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442584.webp", 
                    "newsUrl": "http://news.163.com/16/0720/00/BSCJGH6A00011229.html", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8153657, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 36, 
                    "negative_energy": -17203, 
                    "positive_count": 2, 
                    "negative_count": 178, 
                    "total_energy": -17167, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "dm": "218*143", 
                    "updateTime": 1468986540000, 
                    "showType": 1001, 
                    "newsId": 8154382, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8155073, 
                    "title": "台湾8立委今登太平岛 渔民10人船队也将前往", 
                    "time": 1468985726000, 
                    "source": "中国台湾网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8155073/8155073.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442602.jpg", 
                    "newsUrl": "http://i.ifeng.com/news/sharenews.f?aid=111266571", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8153600, 
                    "webp_flag": "0", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 13987, 
                    "negative_energy": 0, 
                    "positive_count": 172, 
                    "negative_count": 0, 
                    "total_energy": 13987, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "dm": "218*143", 
                    "updateTime": 1468986591044, 
                    "showType": 1001, 
                    "newsId": 8155073, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8153481, 
                    "title": "艾滋病感染者信息泄露 诈骗电话打到单位患者辞职", 
                    "time": 1468981052000, 
                    "source": "中国网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8153481/8153481.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8153481/m/8153481_id_0_264.webp", 
                    "newsUrl": "http://i.ifeng.com/news/sharenews.f?aid=111257578", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8153110, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 0, 
                    "negative_energy": -35513, 
                    "positive_count": 0, 
                    "negative_count": 88, 
                    "total_energy": -35513, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468981182000, 
                    "showType": 1001, 
                    "newsId": 8153481, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "shareCount": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8152477, 
                    "title": "[视频]网上美照的背后 真相太感人", 
                    "time": 1468978509000, 
                    "source": "新浪视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442261.jpg", 
                    "newsUrl": "http://v.sina.cn/xkx/jingxuan/2016-07-20/miaopai-ifxuapvw2395080.d.html?vt=4&pos=91", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8151275, 
                    "open_type": 1, 
                    "iconPath": "http://go.10086.cn/surfnews/usr/surf/icon/20150929174346.png", 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 0, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468984328000, 
                    "showType": 1001, 
                    "newsId": 8152477, 
                    "channelId": 4061, 
                    "webView": 1, 
                    "content_type": 1, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8132562, 
                    "title": "里约奥运中国体育代表团成立", 
                    "time": 1468895335000, 
                    "source": "快讯", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160719/back/1468892871468.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/usr/surf/special/20160719/1786.html", 
                    "isTop": 1, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 8151032, 
                    "open_type": 1, 
                    "iconPath": "http://go.10086.cn/surfnews/usr/surf/icon/20151209174953.png", 
                    "rectype": 0, 
                    "ctype": 0, 
                    "multiImgUrl": [
                        {
                            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8156342/m/8156342_id_0_7481.png"
                        }, 
                        {
                            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/back/1468910442859.png"
                        }, 
                        {
                            "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8156342/m/8156342_id_3_2799.png"
                        }
                    ], 
                    "is_energy": 0, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468912291000, 
                    "showType": 1002, 
                    "newsId": 8132562, 
                    "channelId": 4061, 
                    "webView": 1, 
                    "content_type": 2, 
                    "hasVideo": 0
                }, 
                {
                    "id": 8157174, 
                    "title": "台银行盗领案剩余2000万现金 在公园垃圾堆寻获", 
                    "time": 1468998934000, 
                    "source": "中国新闻网", 
                    "content_url": "http://go.10086.cn/surfnews/images/4061/20160720/8157174/8157174.html", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4061/20160720/8157174/m/8157174_id_1_7332.webp", 
                    "newsUrl": "http://news.163.com/16/0720/15/BSE6DK9C00014JB6.html", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4061, 
                    "isTopOrderby": 0, 
                    "webp_flag": "1", 
                    "open_type": 0, 
                    "rectype": 0, 
                    "ctype": 0, 
                    "is_energy": 1, 
                    "positive_energy": 4126, 
                    "negative_energy": 0, 
                    "positive_count": 103, 
                    "negative_count": 0, 
                    "total_energy": 4126, 
                    "isComment": 0, 
                    "comment_count": 0, 
                    "updateTime": 1468998916000, 
                    "showType": 1001, 
                    "newsId": 8157174, 
                    "channelId": 4061, 
                    "webView": 0, 
                    "content_type": 1, 
                    "hasVideo": 0
                }
            ]
        }, 
        {
            "cardId": 5, 
            "name": "搞笑视频", 
            "desc": "最新热门爆笑视频", 
            "type": "funny", 
            "controls": 3, 
            "sort": 5, 
            "newsList": [
                {
                    "id": 8156942, 
                    "title": "冬眠的猫喵喵:美人鱼", 
                    "time": 1468999529412, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156942/8156942_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156942", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84563485.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156942, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156948, 
                    "title": "比基尼大作战，满满的都是肉疼", 
                    "time": 1468999525503, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156948/8156948_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156948", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84563129.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156948, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156946, 
                    "title": "【混剪联盟】致敬阿汤哥 09", 
                    "time": 1468999521244, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156946/8156946_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156946", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84563395.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 2, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "recommendation_list": [
                        {
                            "channelId": "4079", 
                            "newsId": "8129772", 
                            "updateTime": 1468909889000, 
                            "newsTitle": "两架叛军战机曾锁定土总统专机 未开火成迷", 
                            "newsType": "1", 
                            "content_url": "/usr/surf/4079/20160719/8129772/8129772.xml", 
                            "source": "华商报", 
                            "imgurl": "/images/4079/20160719/back/1468818048709.jpg", 
                            "channelName": "国际"
                        }, 
                        {
                            "channelId": "4079", 
                            "newsId": "8128416", 
                            "updateTime": 0, 
                            "newsTitle": "土耳其军队和司法系统6000人被捕 30名省长被解职", 
                            "newsType": "1", 
                            "content_url": "/usr/surf/4079/20160718/8128416/8128416.xml", 
                            "source": "中国新闻网", 
                            "imgurl": "/images/4079/20160718/8128416/m/8128416_id_0_2492.jpeg;", 
                            "channelName": "国际"
                        }, 
                        {
                            "channelId": "4061", 
                            "newsId": "8115108", 
                            "updateTime": 0, 
                            "newsTitle": "美国智库:土耳其打压军方将涣散军心", 
                            "newsType": "1", 
                            "content_url": "/usr/surf/4061/20160718/8115108/8115108.xml", 
                            "source": "环球网", 
                            "channelName": "热推"
                        }
                    ], 
                    "newsId": 8156946, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156947, 
                    "title": "【混剪联盟 】致敬史密斯夫妇 08", 
                    "time": 1468999517062, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156947/8156947_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156947", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84563172.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156947, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156939, 
                    "title": "【MagicTV】工作中的谜之错觉", 
                    "time": 1468999512101, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156939/8156939_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156939", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84563019.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156939, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156362, 
                    "title": "大姨Ma来了-带点特产回来", 
                    "time": 1468994366148, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156362/8156362_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156362", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84562780.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156362, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156357, 
                    "title": "惊呆了！老司机吐槽《神犬小七2》神级卖萌", 
                    "time": 1468994360487, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156357/8156357_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156357", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84562835.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156357, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156356, 
                    "title": "激情夜生活、嗨的过了火！", 
                    "time": 1468994352830, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156356/8156356_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156356", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84562860.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156356, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156361, 
                    "title": "麦子手工:水晶蝴蝶一字夹", 
                    "time": 1468994348222, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156361/8156361_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156361", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84562785.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156361, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8156358, 
                    "title": "天线宝宝配上恐怖音乐就成恐怖片了 胆小误点", 
                    "time": 1468994344465, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8156358/8156358_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8156358", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84562762.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8156358, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8154667, 
                    "title": "搞笑：不亲亲也有好电影《微信疯传》2016720期", 
                    "time": 1468984918737, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8154667/8154667_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8154667", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84559278.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8154667, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8154657, 
                    "title": "柳岩为《男人装》拍摄的性感写真大片", 
                    "time": 1468984912876, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8154657/8154657_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8154657", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84559898.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8154657, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8154658, 
                    "title": "当小三的下场,泼粪水,然后一顿打", 
                    "time": 1468984906362, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8154658/8154658_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8154658", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84559910.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8154658, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8154655, 
                    "title": "在悬崖峭壁上开车，你见过吗？惊心动魄 胆太大了", 
                    "time": 1468984898746, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8154655/8154655_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8154655", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84560282.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8154655, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8154664, 
                    "title": "wuli青柠欧尼酱:鲜奶糖粒蛋糕", 
                    "time": 1468984891932, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8154664/8154664_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8154664", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84559794.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8154664, 
                    "channelId": 4007281, 
                    "showType": 1001
                }, 
                {
                    "id": 8153337, 
                    "title": "豁牙男屌丝菜刀上阵", 
                    "time": 1468982458173, 
                    "source": "搜狐视频", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/4007281/20160720/8153337/8153337_0.jpg", 
                    "newsUrl": "http://go.10086.cn/surfnews/suferDeskInteFace/redirectnews?id=8153337", 
                    "isTop": 0, 
                    "type": 1, 
                    "coid": 4007281, 
                    "isTopOrderby": 0, 
                    "webp_flag": "0", 
                    "webView": 1, 
                    "open_type": 1, 
                    "content_type": 1, 
                    "hot": 0, 
                    "content_url": "http://m.tv.sohu.com/u/vw/84559509.shtml", 
                    "imageScale": 1, 
                    "intimacyDegree": 2000, 
                    "comment_count": 0, 
                    "is_energy": 0, 
                    "recommendType": 1, 
                    "imgc": 0, 
                    "hasVideo": 2, 
                    "newsId": 8153337, 
                    "channelId": 4007281, 
                    "showType": 1001
                }
            ]
        }, 
        {
            "cardId": 6, 
            "name": "轻松一刻", 
            "desc": "不开心不要钱", 
            "type": "joke", 
            "controls": 3, 
            "sort": 6, 
            "newsList": [
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "大学毕业后，同学们各奔东西。几年后的一次聚会，同宿舍的大壮问大林：“听说你毕业留校了，领导哪个部门？”大林谦虚地笑笑：“哪里是什么领导！在‘接生办’混口饭吃！”大壮羡慕滴接着不解地问：“还是个妇科部门？你这工作很刺激啊！”大林笑了：“想什么呢？‘接生办’是接待新生办公室！”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8157888/8157888.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "大学毕业后，同学们各奔东西。几年后的一次聚会，同宿舍的大壮问大林：“听说你毕业留校了，领导哪个部门？”大林谦虚地笑笑：“哪里是什么领导！在‘接生办’混口饭吃！”大壮羡慕滴接着不解地问：“还是个妇科部门？你这工作很刺激啊！”大林笑了：“想什么呢？‘接生办’是接待新生办公室！”", 
                    "downCount": 325, 
                    "hasVideo": 0, 
                    "id": 8157888, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8157888, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 95, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468999800000, 
                    "title": "大学毕业后，同学们各奔东西。几年...", 
                    "type": 1, 
                    "upCount": 1918, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "听说剧组来招聘群众演员，二强立即去报名。“招聘要求看了吗？”剧组人员问。“看了，我是最符合你们的招聘条件的。”二强说。“你能演几个角色？”“多了去了，妻管严、受气包、跪搓衣板的怂男人。”“啊？你就演这个啊，还会演别的不？”“会啊，动物类的哈巴狗、小猴子、大猩猩、狗熊、水牛、山羊，没有我不会演的。”“你都是在哪里演啊？”“在家里啊，我老婆经常逼我演。”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8157602/8157602.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "听说剧组来招聘群众演员，二强立即去报名。“招聘要求看了吗？”剧组人员问。“看了，我是最符合你们的招聘条件的。”二强说。“你能演几个角色？”“多了去了，妻管严、受气包、跪搓衣板的怂男人。”“啊？你就演这个啊，还会演别的不？”“会啊，动物类的哈巴狗、小猴子、大猩猩、狗熊、水牛、山羊，没有我不会演的。”“你都是在哪里演啊？”“在家里啊，我老婆经常逼我演。”", 
                    "downCount": 215, 
                    "hasVideo": 0, 
                    "id": 8157602, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8157602, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 92, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468998000000, 
                    "title": "听说剧组来招聘群众演员，二强立即...", 
                    "type": 1, 
                    "upCount": 1857, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "以前人求转运会去庙里拜拜，现在都是上网找条锦鲤帖子转转，真是越来越不把迷信当回事了。", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8156787/8156787.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "以前人求转运会去庙里拜拜，现在都是上网找条锦鲤帖子转转，真是越来越不把迷信当回事了。", 
                    "downCount": 157, 
                    "hasVideo": 0, 
                    "id": 8156787, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8156787, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 65, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468996200000, 
                    "title": "以前人求转运会去庙里拜拜，现在都...", 
                    "type": 1, 
                    "upCount": 1316, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "给公司招了前台，董事长把我叫去臭骂了一顿，说是太丑了影响公司形象．我说：“就你定的那工资，三个月了才来一个应聘的，我没的挑。再说了，我作为总经理连招个前台的权力都没吗？”董事长气坏了：“你说咱俩合伙开个小吃店容易吗？全公司就咱俩，你还整天跟我拧着干？”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8156472/8156472.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "给公司招了前台，董事长把我叫去臭骂了一顿，说是太丑了影响公司形象．我说：“就你定的那工资，三个月了才来一个应聘的，我没的挑。再说了，我作为总经理连招个前台的权力都没吗？”董事长气坏了：“你说咱俩合伙开个小吃店容易吗？全公司就咱俩，你还整天跟我拧着干？”", 
                    "downCount": 54, 
                    "hasVideo": 0, 
                    "id": 8156472, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8156472, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 64, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468994400000, 
                    "title": "给公司招了前台，董事长把我叫去臭...", 
                    "type": 1, 
                    "upCount": 1288, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "美眉对护肤专家说：“我觉得你的面膜挺好用，一片可以顶好几片，就是价钱贵点！”护肤专家说：“一片顶好几片还贵吗？”妹妹说：“贵啊！以前我用的是黄瓜片！”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8156250/8156250.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "美眉对护肤专家说：“我觉得你的面膜挺好用，一片可以顶好几片，就是价钱贵点！”护肤专家说：“一片顶好几片还贵吗？”妹妹说：“贵啊！以前我用的是黄瓜片！”", 
                    "downCount": 171, 
                    "hasVideo": 0, 
                    "id": 8156250, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8156250, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 101, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468992600000, 
                    "title": "美眉对护肤专家说：“我觉得你的面...", 
                    "type": 1, 
                    "upCount": 2022, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "狮子是狮王，老虎是丛林之王，狼什么名号也没有，但是狼从未在马戏团表演出现过。", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8156098/8156098.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "狮子是狮王，老虎是丛林之王，狼什么名号也没有，但是狼从未在马戏团表演出现过。", 
                    "downCount": 434, 
                    "hasVideo": 0, 
                    "id": 8156098, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8156098, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 88, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468990800000, 
                    "title": "狮子是狮王，老虎是丛林之王，狼什...", 
                    "type": 1, 
                    "upCount": 1764, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "好看的妹子笑起来才是花枝乱颤，丑的那叫中风抽搐。", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8155842/8155842.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "好看的妹子笑起来才是花枝乱颤，丑的那叫中风抽搐。", 
                    "downCount": 299, 
                    "hasVideo": 0, 
                    "id": 8155842, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8155842, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 59, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468989000000, 
                    "title": "好看的妹子笑起来才是花枝乱颤，丑...", 
                    "type": 1, 
                    "upCount": 1188, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "别嫌男生太幼稚，那是他喜欢你，要是不喜欢你，他比你爹还成熟！", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8155591/8155591.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "别嫌男生太幼稚，那是他喜欢你，要是不喜欢你，他比你爹还成熟！", 
                    "downCount": 273, 
                    "hasVideo": 0, 
                    "id": 8155591, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8155591, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 99, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468987200000, 
                    "title": "别嫌男生太幼稚，那是他喜欢你，要...", 
                    "type": 1, 
                    "upCount": 1992, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "“为什么男人把女人追到手态度就变了呢？”“你考完试还会复习吗？”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8154890/8154890.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "“为什么男人把女人追到手态度就变了呢？”“你考完试还会复习吗？”", 
                    "downCount": 412, 
                    "hasVideo": 0, 
                    "id": 8154890, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8154890, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 50, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468985400000, 
                    "title": "“为什么男人把女人追到手态度就变...", 
                    "type": 1, 
                    "upCount": 1010, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "我警告我的朋友，如果你们谁再敢打扰我写作业的话，我就和你们一起玩。", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8154464/8154464.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "我警告我的朋友，如果你们谁再敢打扰我写作业的话，我就和你们一起玩。", 
                    "downCount": 140, 
                    "hasVideo": 0, 
                    "id": 8154464, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8154464, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 129, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468983600000, 
                    "title": "我警告我的朋友，如果你们谁再敢打...", 
                    "type": 1, 
                    "upCount": 2592, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "在火车站附近买水，老板问：“要泡面吗？”我说：“行，拿一盒。”他就指着店里仅有的老坛和卤肉面问：“你要汪涵口味的还是何炅口味的？”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8154125/8154125.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "在火车站附近买水，老板问：“要泡面吗？”我说：“行，拿一盒。”他就指着店里仅有的老坛和卤肉面问：“你要汪涵口味的还是何炅口味的？”", 
                    "downCount": 267, 
                    "hasVideo": 0, 
                    "id": 8154125, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8154125, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 91, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468981800000, 
                    "title": "在火车站附近买水，老板问：“要泡...", 
                    "type": 1, 
                    "upCount": 1832, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "下班到家进电梯，正好邻居带着读小学的儿子走了进来，他一脸的不高兴。我逗他说：“怎么啦，开学不高兴吗，能重新见到小伙伴多开心呀！”他冷笑道：“你很久不上班会想同事吗？”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8153174/8153174.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "下班到家进电梯，正好邻居带着读小学的儿子走了进来，他一脸的不高兴。我逗他说：“怎么啦，开学不高兴吗，能重新见到小伙伴多开心呀！”他冷笑道：“你很久不上班会想同事吗？”", 
                    "downCount": 445, 
                    "hasVideo": 0, 
                    "id": 8153174, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8153174, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 66, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468980000000, 
                    "title": "下班到家进电梯，正好邻居带着读小...", 
                    "type": 1, 
                    "upCount": 1325, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "朋友见一千娇百媚，婀娜多姿的美女在路边抹泪。过去安慰，得知其没有完成业绩被老板骂了。心肠一软，把美女拉旁边酒馆喝酒。一瓶下肚，美女慢慢有了笑容。朋友问她开心点了吗？美女说：“嗯，再来一瓶就超额完成这月的业绩了！！！”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8153111/8153111.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "朋友见一千娇百媚，婀娜多姿的美女在路边抹泪。过去安慰，得知其没有完成业绩被老板骂了。心肠一软，把美女拉旁边酒馆喝酒。一瓶下肚，美女慢慢有了笑容。朋友问她开心点了吗？美女说：“嗯，再来一瓶就超额完成这月的业绩了！！！”", 
                    "downCount": 380, 
                    "hasVideo": 0, 
                    "id": 8153111, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8153111, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 142, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468979400000, 
                    "title": "朋友见一千娇百媚，婀娜多姿的美女...", 
                    "type": 1, 
                    "upCount": 2842, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "同样是肉肉，为什么长在胸部就很受人喜欢，而长在肚子上却那么令人讨厌？这算不算地域歧视？", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8153108/8153108.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "同样是肉肉，为什么长在胸部就很受人喜欢，而长在肚子上却那么令人讨厌？这算不算地域歧视？", 
                    "downCount": 167, 
                    "hasVideo": 0, 
                    "id": 8153108, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8153108, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 53, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468978800000, 
                    "title": "同样是肉肉，为什么长在胸部就很受...", 
                    "type": 1, 
                    "upCount": 1080, 
                    "webView": 0, 
                    "webp_flag": "0"
                }, 
                {
                    "channelId": 4648023, 
                    "coid": 4648023, 
                    "content": "老师：“现在我给你们五分钟的时间，你们每个小朋友可要抓紧哟，看哪位小朋友能用这五分钟的时间记下这两个英语单词”。五分钟过后，全部的同学都能默写了这两个单词，唯有一个同学还在闭着眼睛，伸着双手小手板一动不动。老师问：“这位小朋友你在干吗，还不抓紧时间？”这位小朋友说：“老师，我伸了这么久的手，你是不是忘记给我时间呀？”", 
                    "content_url": "http://go.10086.cn/surfnews/images/4648023/20160720/8152907/8152907.html", 
                    "count": 0, 
                    "ctype": 0, 
                    "desc": "老师：“现在我给你们五分钟的时间，你们每个小朋友可要抓紧哟，看哪位小朋友能用这五分钟的时间记下这两个英语单词”。五分钟过后，全部的同学都能默写了这两个单词，唯有一个同学还在闭着眼睛，伸着双手小手板一动不动。老师问：“这位小朋友你在干吗，还不抓紧时间？”这位小朋友说：“老师，我伸了这么久的手，你是不是忘记给我时间呀？”", 
                    "downCount": 196, 
                    "hasVideo": 0, 
                    "id": 8152907, 
                    "isComment": 0, 
                    "isTop": 0, 
                    "newsId": 8152907, 
                    "open_type": 0, 
                    "rectype": 0, 
                    "shareCount": 145, 
                    "showType": 1003, 
                    "source": "手机冲浪", 
                    "time": 1468978200000, 
                    "title": "老师：“现在我给你们五分钟的时间...", 
                    "type": 1, 
                    "upCount": 2901, 
                    "webView": 0, 
                    "webp_flag": "0"
                }
            ]
        }, 
        {
            "cardId": 7, 
            "name": "美女", 
            "desc": "热辣、清纯、御姐...", 
            "type": "beauty", 
            "controls": 3, 
            "sort": 7, 
            "newsList": [
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157148, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720150723609.jpg", 
                    "intimacyDegree": 5299, 
                    "isTop": 0, 
                    "newsId": 8157148, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468998443000, 
                    "title": "浴缸里的长发美女清新明亮", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157100, 
                    "imageScale": 1, 
                    "imageTag": "白皙,写真,青春,淑女", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720150250047.jpg", 
                    "intimacyDegree": 6826, 
                    "isTop": 0, 
                    "newsId": 8157100, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468998170000, 
                    "title": "甜美古装妹子荷花池边", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157097, 
                    "imageScale": 1, 
                    "imageTag": "甜美,粉嫩,萝莉,可爱", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720150141491.jpg", 
                    "intimacyDegree": 6325, 
                    "isTop": 0, 
                    "newsId": 8157097, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468998101000, 
                    "title": "短发少女明眸皓齿表情清纯无辜", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157089, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720145816077.jpg", 
                    "intimacyDegree": 7605, 
                    "isTop": 0, 
                    "newsId": 8157089, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468997896000, 
                    "title": "美女秀性感美腿与妖娆身姿", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157086, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720145353541.jpg", 
                    "intimacyDegree": 6798, 
                    "isTop": 0, 
                    "newsId": 8157086, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468997633000, 
                    "title": "俏皮可爱美少女甜品相伴甜美迷人", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157083, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720145253113.jpg", 
                    "intimacyDegree": 5787, 
                    "isTop": 0, 
                    "newsId": 8157083, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468997573000, 
                    "title": "粉嫩甜美可爱少女闺房梦幻写真", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157071, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,甜美,清纯,可爱", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720143901392.jpg", 
                    "intimacyDegree": 7457, 
                    "isTop": 0, 
                    "newsId": 8157071, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468996741000, 
                    "title": "柔美白皙少女甜美性感", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157069, 
                    "imageScale": 1, 
                    "imageTag": "白皙,写真,青春,淑女", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720143738730.jpg", 
                    "intimacyDegree": 5278, 
                    "isTop": 0, 
                    "newsId": 8157069, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468996658000, 
                    "title": "和服漂亮美女蓝天娇艳颜色斑斓", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8157062, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720143151197.jpg", 
                    "intimacyDegree": 5244, 
                    "isTop": 0, 
                    "newsId": 8157062, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468996311000, 
                    "title": "逆光下的高颜值背心美女", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8155499, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720134350218.jpg", 
                    "intimacyDegree": 3917, 
                    "isTop": 0, 
                    "newsId": 8155499, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468993430000, 
                    "title": "中分女神校服户外清新漂亮", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8156414, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720134215465.jpg", 
                    "intimacyDegree": 7687, 
                    "isTop": 0, 
                    "newsId": 8156414, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468993335000, 
                    "title": "邻家甜美姐姐日常居家照", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8155497, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720133757595.jpg", 
                    "intimacyDegree": 6231, 
                    "isTop": 0, 
                    "newsId": 8155497, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468993077000, 
                    "title": "花海中的黑发氧气漂亮美少女", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8153881, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720105418436.jpg", 
                    "intimacyDegree": 6803, 
                    "isTop": 0, 
                    "newsId": 8153881, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468983258000, 
                    "title": "白皙芭蕾舞少女身姿妖娆", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8153874, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720105244753.jpg", 
                    "intimacyDegree": 7542, 
                    "isTop": 0, 
                    "newsId": 8153874, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468983164000, 
                    "title": "农田间的清纯漂亮牛仔背带短裤女孩", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8153868, 
                    "imageScale": 1, 
                    "imageTag": "嫩肤,白皙,清纯,青春", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720104838743.jpg", 
                    "intimacyDegree": 5429, 
                    "isTop": 0, 
                    "newsId": 8153868, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468982918000, 
                    "title": "90后清纯美女盛夏户外连衣裙装", 
                    "type": 1, 
                    "webView": 0
                }, 
                {
                    "channelId": 1024229, 
                    "coid": 1024229, 
                    "comment_count": 0, 
                    "count": 0, 
                    "ctype": 0, 
                    "dm": "600*800", 
                    "id": 8153318, 
                    "imageScale": 1, 
                    "imageTag": "白皙,写真,青春,淑女", 
                    "imgUrl": "http://go.10086.cn/surfnews/images/beautyPic/20160720100056812.jpg", 
                    "intimacyDegree": 5727, 
                    "isTop": 0, 
                    "newsId": 8153318, 
                    "rectype": 0, 
                    "showType": 1001, 
                    "time": 1468980056000, 
                    "title": "可爱丸子头金鱼少女甜美写真", 
                    "type": 1, 
                    "webView": 0
                }
            ]
        }, 
        {
            "cardId": 8, 
            "name": "最常访问", 
            "desc": "智能推荐最常访问的地址", 
            "type": "latest", 
            "controls": 3, 
            "sort": 8
        }, 
        {
            "cardId": 9, 
            "name": "实时热点", 
            "desc": "热门搜索风云榜", 
            "type": "hot", 
            "controls": 3, 
            "sort": 9, 
            "dataList": [
                {
                    "dataId": 901, 
                    "name": "北京暴雨橙色预警"
                }, 
                {
                    "dataId": 902, 
                    "name": "婴儿车抢车位"
                }, 
                {
                    "dataId": 903, 
                    "name": "土耳其首都大火"
                }, 
                {
                    "dataId": 904, 
                    "name": "重庆遭暴雨袭击"
                }, 
                {
                    "dataId": 905, 
                    "name": "千万超跑失控撞毁"
                }, 
                {
                    "dataId": 906, 
                    "name": "暴雨最牛公交掉头"
                }, 
                {
                    "dataId": 907, 
                    "name": "违章男子成表情包"
                }, 
                {
                    "dataId": 908, 
                    "name": "王思聪直播被砸场"
                }, 
                {
                    "dataId": 909, 
                    "name": "英国爆发2亿老鼠"
                }, 
                {
                    "dataId": 910, 
                    "name": "抓精灵闯军事基地"
                }, 
                {
                    "dataId": 911, 
                    "name": "北京暴雨橙色预警"
                }, 
                {
                    "dataId": 912, 
                    "name": "婴儿车抢车位"
                }, 
                {
                    "dataId": 913, 
                    "name": "土耳其首都大火"
                }, 
                {
                    "dataId": 914, 
                    "name": "重庆遭暴雨袭击"
                }, 
                {
                    "dataId": 915, 
                    "name": "千万超跑失控撞毁"
                }, 
                {
                    "dataId": 916, 
                    "name": "暴雨最牛公交掉头"
                }, 
                {
                    "dataId": 917, 
                    "name": "土耳其首都大火"
                }, 
                {
                    "dataId": 918, 
                    "name": "重庆遭暴雨袭击"
                }
            ]
        }, 
        {
            "cardId": 10, 
            "name": "小说", 
            "desc": "养成读书的好习惯", 
            "type": "novel", 
            "controls": 3, 
            "sort": 10, 
            "dataList": [
                {
                    "dataId": 1001, 
                    "name": "图片小说1", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group3/M00/00/0B/rBAMWFeIg4mAYA2WAAA7g6FDQrA641.jpg", 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40657542&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3DS&mcpid=hnZhHZ5Nl&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=1&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1002, 
                    "name": "图片小说2", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group3/M00/00/07/rBAMWVeHARuAc--HAAA6L0WJsy8968.jpg", 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=1&linkId=40659870&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ2Qx&mcpid=hnZhHZ5Nk&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=2&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1003, 
                    "name": "图片小说3", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group1/M00/00/08/rBAMVVeHL4uAexdmAAA8P77qEzk258.jpg", 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40658971&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3Dg&mcpid=hnZhHZ5Nq&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=3&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1004, 
                    "name": "图片小说4", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group3/M00/00/0B/rBAMWFeIg4mAYA2WAAA7g6FDQrA641.jpg", 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40657542&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3DS&mcpid=hnZhHZ5Nl&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=1&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1005, 
                    "name": "图片小说5", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group3/M00/00/07/rBAMWVeHARuAc--HAAA6L0WJsy8968.jpg", 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=1&linkId=40659870&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ2Qx&mcpid=hnZhHZ5Nk&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=2&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1006, 
                    "name": "图片小说6", 
                    "showType": 1, 
                    "img": "http://rd.go.10086.cn:9099/group1/M00/00/08/rBAMVVeHL4uAexdmAAA8P77qEzk258.jpg", 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40658971&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3Dg&mcpid=hnZhHZ5Nq&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=3&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1007, 
                    "name": "文字小说1", 
                    "showType": 2, 
                    "readCount": 178, 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40658971&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3Dg&mcpid=hnZhHZ5Nq&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=3&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1008, 
                    "name": "文字小说2", 
                    "showType": 2, 
                    "readCount": 78, 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40658971&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3Dg&mcpid=hnZhHZ5Nq&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=3&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1009, 
                    "name": "文字小说3", 
                    "showType": 2, 
                    "readCount": 1178, 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40658971&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3Dg&mcpid=hnZhHZ5Nq&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=3&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }, 
                {
                    "dataId": 1010, 
                    "name": "文字小说4", 
                    "showType": 2, 
                    "readCount": 178, 
                    "url": "http://go.10086.cn/rd/go/dh/awstatsNav.do?categoryId=22944935&ct=10&linkId=40658971&linkUrl=http://rd.go.10086.cn:9090/ndisi/adclk?id=1yVvZ3Dg&mcpid=hnZhHZ5Nq&show_form=0&bn=1&channel_num=&mobile=&ad_showform=&posIndex=3&isNavArch=1&siteId=22&pageDeep=1&pageId=579500&coc=5zDqzz6A&cid=m_ea1a02e1e87045209dd2dffc6fded774"
                }
            ]
        }, 
        {
            "cardId": 11, 
            "name": "话费流量查询", 
            "desc": "及时查询手机话费流量余额", 
            "type": "flow", 
            "controls": 3, 
            "sort": 11
        }
    ]
}
```