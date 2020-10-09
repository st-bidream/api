### Bidream OTC Open Api
##### 一、指引
<hr/>
● 获取API Key<br/>
API Key由Bidream进行分配，提供相关接入信息后，随机生成一组API Access Key和Secret Key，利用这一组数据可以进行程序化交易。<br/>

	请不要泄露API Access Key和Secret Key信息，以免造成资产损失
<br/>
● 接口调用方式说明<br/>
Api提供的接口调用方式为REST API方式，提供币种信息查询、生成地址、充/提币等功能<br/>
同时，所有请求基于Https协议，请求头信息中的Content-Type需要统一设置为表单格式：<br/>
Content-Type:application/x-www-form-urlencoded<br/>
同时，所有私有请求需要进行签名认证，详见签名说明<br/>
<br/>
● 联系我们<br/>
<br/>

##### 二、签名说明
<hr/>
API 请求在通过网络传输的过程中极有可能被篡改，为了确保请求未被更改，所有的私有接口均必须使用您的 API Key 做签名认证，以校验参数或参数值在传输途中是否发生了更改。<br/><br/>

签名步骤示例：<br/>
1、示例接口：/newAddress<br/>
2、示例密钥：<br/>

	API Access Key: 7302D2GMixZ3542g2333Q050l01Rfqqi<br/>
	Secret Key: 9EJ2g15l3Do294fjwU7n271l0y73P4C0<br/>
3、签名步骤：<br/>

	3.1 按照ASCII码的顺序对参数名进行排序
	
		□ 原始参数顺序为：
		symbol=eth
		time=1573529660
		apiKey=7302D2GMixZ3542g2333Q050l01Rfqqi
		
		□ 按照ASCII码顺序对参数名进行排序后：
		apiKey =7302D2GMixZ3542g2333Q050l01Rfqqi
		symbol=eth
		time=1573529660
	
	3.2 所有参数按"参数名=参数值&"格式拼接在一起组成要签名计算的字符串
		apiKey =7302D2GMixZ3542g2333Q050l01Rfqqi&symbol=eth&time=1573529660
		
	3.3 签名计算的字符串与秘钥(SecretKey)拼接形成最终计算的字符串，使用32位MD5算法进行计算生成数字签名
		□ 拼接SecretKey后：
		apiKey =7302D2GMixZ3542g2333Q050l01Rfqqi&symbol=eth&time=15735296609EJ2g15l3Do294fjwU7n271l0y73P4C0
		□ 进行MD5加密并全部转换为小写：
		sign=073a35b6801d72402659c8196c52b39b
		
	3.4 将生成的数字签名加入到请求的路径参数里
		最终请求的url为：
		https://www.bidream.cn/public/v1/wallet/api/newAddress?symbol=eth&time=1573529660&apiKey=7302D2GMixZ3542g2333Q050l01Rfqqi&sign=073a35b6801d72402659c8196c52b39b


##### 三、REST API
<hr/>
● 接入URL地址前缀：<br/>
测试服：http://47.88.216.134:8080/public/v1/otc/api<br/>
正式服：https://www.bidream.cn/public/v1/otc/api

● 接口列表<br/>
1 <a href="#1">币种信息</a>	GET /{symbol}/info<br/>
2 <a href="#2">预下单</a>		POST /pre-order<br/>
3 <a href="#3">下单</a>		POST /order<br/>
4 <a href="#4">已完成支付</a>	POST /{tradeNo}/payed<br/>
5 <a href="#5">取消订单</a>	POST /{tradeNo}/cancel<br/>
6.<a href="#6">买币交易完成通知</a><br/>

### <a name="1">1 币种信息</a>
###### 说明
	请求币种信息
	
###### 请求接口
	GET /{symbol}/info

###### 请求参数
	{
		"symbol":"usdt-trc20"		// 币种，通过请求地址中传入
		"currencyType":"CNY"		// 计价货币类型 人民币="CNY"；卢比="INR"
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": {
	    	"cnyPrice":6.93,		// 单个币的价格
	    	"minSell":1.0,			// 最小卖出数量
	    	"minBuy":1.0,			// 最小买入数量
	    },	
	    "message": ""
	}

### <a name="2">2 预下单</a>
###### 说明
	根据输入的币种数量或购买金额计算实际买入的数量及实际支付的价格
	
###### 请求接口
	POST /pre-order

###### 请求参数
	{
		"symbol":"usdt-trc20"			// 币种
		"time":"1573529660"			// 时间戳
		"apiKey":【API Access Key】		// 申请的API Access Key
		"amount":1000				// 订单的数量
		"price":10000.00			// 订单的价格（price与amount二选一）
		"side":1				// =1,买入;=2,卖出
		"currency":"CNY"			// 计价货币类型（可选，默认="CNY"）人民币="CNY"，卢比="INR"
		"sign":"19dd26b4e80df932a02ca7ed52f6b69a"
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": {
	    	"amount":1000,				// 实际买入/卖出的数量
	    	"price":10000.00,			// 实际支付的金额
	    },
	    "message": ""
	}

### <a name="3">3 下单</a>
###### 请求接口
	POST /order

###### 请求参数
	{
		"amount":100					// 买入/卖出数量
		"price":1000.00					// 订单金额（price与amount二选一）
		"symbol":"usdt_trc20"				// 币种
		"address":"TXMbnzgwy8UZraghZ5eSJ8qqCKjUnQogot"	// 地址
		"time":"1573529660"				// 时间戳
		"payType":1					// =1,银行卡;=2,支付宝;=3,微信;=4,UPI；
		"side":1					// =1,买入;=2,卖出;
		"apiKey":【API Access Key】			// 申请的API Access Key
		"currency":"CNY"				// 计价货币类型（可选，默认="CNY"）人民币="CNY"，卢比="INR"
		"sign":"19dd26b4e80df932a02ca7ed52f6b69a"
	}
	
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"amount":100,					// 实际买入/卖出的数量
	    	"price":1000.00,				// 实际支付的金额
	    	"tradeNo":"c491dabf7ad96ca33d912c460b761a5c",	// 订单号
	    	"accountName":"张三",			       // 收款人姓名
	    	"province":"北京",			       // 开户行所在省份
	    	"city":"北京",				       // 开户行所在城市
	    	"branch":"招行朝阳门支行",			    // 开户行
	    	"accountNo":"8888888888888888",			// 账户
	    	"qrCode":"http://xxx/xxx/xxx.png"		// 二维码连接
	    }
	    "message": ""
	}

### <a name="4">4 已支付</a>
###### 说明
	用户完成支付后请求
	
###### 请求接口
	POST /{tradeNo}/payed

###### 请求参数
	{
		"time":"1573529660"				// 时间戳
		"bankRefer":"129304392012"			// 银行流水单号
		"apiKey":【API Access Key】			// 申请的API Access Key
		"sign":"19dd26b4e80df932a02ca7ed52f6b69a"
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": "",	
	    "message": ""
	}	
	
### <a name="5">5 取消订单</a>
###### 说明
	取消订单
	
###### 请求接口
	POST /{tradeNo}/cancel

###### 请求参数
	{
		"time":"1573529660"				// 时间戳
		"apiKey":【API Access Key】			// 申请的API Access Key
		"sign":"19dd26b4e80df932a02ca7ed52f6b69a"
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": "",	
	    "message": ""
	}	
	
### <a name="6">6 买币交易完成通知</a>
###### 说明
	需接入方实现，在买币交易完成后，会调用接入方的deposit_url地址，通知交易完成
	
###### 请求接口
	POST 接入方提供的回调地址

###### 请求参数
	{
		"orderNo":"2df3fd173c2c5ffaf29149bc8b15745b"	// 订单号
		"time":"1573529660"				// 时间戳
		"apiKey":【API Access Key】			// 申请的API Access Key
		"sign":"19dd26b4e80df932a02ca7ed52f6b69a"
	}
	
###### 返回值
	{
	    "status": 0,
	    "message": ""
	}	
<br/>

