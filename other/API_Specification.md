# API规范

## 请求头
0. 格式

	```
	Content-Type: application/json; charset=utf-8
	```
	不推荐使用表单格式：application/x-www-form-urlencoded，无法提交有层级的参数
1. 命名，驼峰格式，如houseList，不推荐house_list

## 返回
0. 返回头格式，请返回JSON格式，不要返回 text/html，text/css 等不相关返回头

	```
	Content-Type: application/json; charset=utf-8
	```
0. 数据格式
	0. code 请求结果码
		* 成功:0
		* 客户端错误：-4XX
		* 服务端错误：-5XX
	0. message 错误描述
		* 请给出详细错误信息，比如缺少参数是缺少哪个参数。
	0. data 返回结果，所有结果放在data里便于统一处理，data里再按照层级来放，比如
	
		```
		code : 0,
		message : 成功,
		data : {
			houseAreaList : [],
			houseTraitList : [],
			areaPriceList : [],
			houseList : []
		}
		
		```
	0. 为减小请求大小，不用的信息不要返回，不要直接 select * from ...，请指定要获取的字段

## 参数签名
0. 概述

	API需要通过签名来访问，签名的过程是将请求参数串以及APP密钥根据一定签名算法生成的签名值，作为新的请求参数从而提高访问过程中的防篡改性。签名值的生成详见下面的描述。
0. URL签名生成规则

	所有API的有效访问URL包括以下三个部分： 
	1. 资源访问路径，如/v1/deal/find_deals; 
	2. 请求参数：即API对应所需的参数名和参数值param=value，多个请求参数间用&连接，如deal_id=1-85462&appkey=00000； 
	3. 签名串，由签名算法生成
 
0. 签名算法如下： 
	1. 对除appkey以外的所有请求参数进行字典升序排列； 
	2. 将以上排序后的参数表进行字符串连接，如key1value1key2value2key3value3...keyNvalueN； 
	3. 将app key作为前缀，将app secret作为后缀，对该字符串进行SHA-1计算，并转换成16进制编码； 
	4. 转换为全大写形式后即获得签名串 

签名串获得后，将其作为sign参数附加到对应的URL中，即可正常访问API。 参考[大众点评API签名](http://developer.dianping.com/app/documentation/signature)

## 通用参数

```
// 手机型号
device = "iPhone8,1"
// 设备UUID
deviceID = "38FBD577-9BE4-49EA-8D16-D040EF6CA80E";
// 网络情况，如WiFi，4G，3G，2G；可能有需求做不同的网络环境适配
networkType = WiFi;
// 手机品牌型号
phoneBrand = "Apple iPhone8,1";
// 系统 1 Android 2 iOS
refer = 2;
// 参数签名
sign = 69D3AE5C037847959E3670783E6ACBC37FD77B51;
// 操作系统
sys = "iPhone OS 9.3.5";
// app版本
ver = “1.1";
```