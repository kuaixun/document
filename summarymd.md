重要说明
=======
1、以下字段客户端每次请求时都应该携带，记录日志，为后续的统计做好准备
在后续的接口说明中列举的是服务器端需要使用的参数

| 字段名 | 备注 | 示例
| -- | -- | 
| os | 手机操作系统 | android 
| did | 设备号 | ca67c8f7-d4ca-3089-986f-fdb2c7260f34
| pm | 手机型号 | M35t
| cid | 渠道号 | 2ehveeiy
| sdkv | sdk版本 | 10
| vername | 版本名称 | 5.0.1
| vercode | 版本号 | 90

2、如果参数的值(包括客户端请求和服务器响应)为空或者Null的情况下，请求或者响应的json不需要携带这个key

3、下述接口文档中的JSON 实例只是例子，以字段说明为准

4、服务器端返回Json key的顺序，除URL前缀 (apicPass)固定在第一位之外，其他key不保证其顺序

5、调用接口说明  
服务器地址：http://go.10086.cn/surfnews/suferDeskInteFace/surfdesk  
参数：method=\*\*\*\*  
jsonRequest={\*\*\*\*}











