### Bidream Fund Api

<hr/>
● 接入URL地址前缀：https://www.bidream.cn（测试服：http://47.88.216.134:8080）

● 接口列表<br/>
1 <a href="#1">用户注册</a>	POST /public/v1/users/sign-up<br/>
2 <a href="#2">获取短信验证码</a>		POST /public/v1/users/send-msg<br/>
3 <a href="#3">登录</a>		POST /public/v1/users/sign-in<br/>
4 <a href="#4">基金列表</a>	GET /public/v1/fund/list<br/>
5 <a href="#5">基金详情</a>	GET /public/v1/fund/{fundId}/info<br/>
6 <a href="#6">买入下单</a>	POST /v1/fund/order<br/>
7 <a href="#7">订单列表</a>	GET /v1/fund/orders<br/>
8 <a href="#8">赎回下单</a>	POST /v1/fund/redeem<br/>
9 <a href="#9">获取充币地址</a>	GET /v1/chain/address/{symbol}<br/>
10 <a href="#10">获取资产</a>	GET /v1/chain/account/{token}<br/>
11 <a href="#11">提币</a>	POST /v1/chain/withdraw<br/>
12 <a href="#12">获取净值列表</a>	GET /public/v1/fund/{fundId}/npvs<br/>
13 <a href="#13">用户资产估值及明细</a>	GET /v1/fund/assets<br/>
14 <a href="#14">站内信列表</a>	GET /v1/fund/msgs<br/>
15 <a href="#15">站内信详情</a>	GET /public/v1/fund/msg<br/>
16 <a href="#16">用户资产及收益</a>	GET /v1/fund/{fundId}/profit<br/>
17 <a href="#17">站内信未读数量</a>	GET /v1/fund/msg/unread<br/>
18 <a href="#18">站内信标为已读</a>	POST /v1/fund/msg/read<br/>

### <a name="1">1 用户注册</a>	
###### 请求接口
	POST /public/v1/users/sign-up
###### 请求参数
	{
		"areacode":"86"					// 地区编码
		"phone":"13912345678"				// 手机号
		"code":"1234"					// 短信验证码
		"password":"12345678"				// 登录密码
		"inviteCode":"jk1234"				// 邀请码
		"identity":7					// 基金用户，固定=7
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": TOKEN,					// 用户token
	    "message": ""
	}

### <a name="2">2 获取短信验证码</a>	
###### 请求接口
	POST /public/v1/users/send-msg
###### 请求参数
	{
		"behavior":3					// 动作标识，用户注册=3；提币=5；
		"areacode":"86"					// 地区编码
		"phone":"13912345678"				// 手机号
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": ,
	    "message": ""
	}

### <a name="3">3 登录</a>
###### 请求接口
	POST /public/v1/users/sign-in
###### 请求参数
	{
		"areacode":"86"					// 地区编码
		"phone":"13912345678"				// 手机号
		"password":"12345678"				// 登录密码
	}
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"token":TOKEN,					// 用户token
	    }
	    "message": ""
	}

### <a name="4">4 基金列表</a>	
###### 请求接口
	GET /public/v1/fund/list
###### 请求参数
	{
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": [{
	    	"id":123,					// 基金id
	    	"name":"基金1",					// 名称
	    	"open_investment":"2020-01-01",			// 开放购买起始日期
	    	"open_redemption":"2020-01-01",			// 开放赎回起始日期
	    	"fee_investment":1.0,				// 买入手续费（百分比）
	    	"extent_year":180,				// 近一年涨跌幅（百分比）
	    	"profit":50,					// 预期收益率（百分比）
	    	"desc":"",					// 说明
	    	"risk":""					// 风险提示
	    }
	    ...
	    ],	
	    "message": ""
	}	

### <a name="5">5 基金详情</a>	
###### 请求接口
	GET /public/v1/fund/{fundId}/info
###### 请求参数
	{
		"fundId":123					// 基金id，请求地址中传入
	}
	
###### 返回值
	{
	    "status": 0,
	    "data": {
	    	"id":123,					// 基金id
	    	"name":"基金1",					// 名称
	    	"open_investment":"2020-01-01",			// 开放购买起始日期
	    	"open_redemption":"2020-01-01",			// 开放赎回起始日期
	    	"fee_investment":1.0,				// 买入手续费（百分比）
	    	"extent_year":180,				// 近一年涨跌幅（百分比）
	    	"profit":50,					// 预期收益率（百分比）
	    	"desc":"",					// 说明
	    	"risk":""					// 风险提示
	    	"extent_daily":12				// 日涨跌幅（百分比）
	    	"npv":1.89					// 净值
	    },	
	    "message": ""
	}	

### <a name="6">6 买入下单</a>
###### 请求接口
	POST /v1/fund/order
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
		"fundId":123					// 基金id
		"amount":10000					// 买入的usdt数量
	}
###### 返回值
	{
	    "status": 0,
	    "data":"",
	    "message": ""
	}
	
### <a name="7">7 订单列表</a>
###### 请求接口
	GET /v1/fund/orders
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
		"fundId":123					// 基金id，可不传，默认所有
		"type":1					// 订单类型，可不传，默认所有。 =1，买入；=2，赎回
		"page":1					// 页数
		"rows":20					// 每页条数
	}
###### 返回值
	{
	    "status": 0,
	    "data":[{
	    	"name": "基金1",					// 基金名称
	    	"order_no": aci291kc9olds,			// 订单号
	    	"type": 1,					// 交易类型 =1，买入；=2，赎回
	    	"amount": 10000,				// usdt数量
	    	"portion": 990.89,				// 份额
	    	"npv": 1.89,					// 净值
	    	"fee": 20,					// 手续费
	    	"status": 0					// 订单状态 =0,待处理；=1，已完成；
	    }
	    ...
	    ]
	    "message": ""
	}
	
### <a name="8">8 赎回下单</a>
###### 请求接口
	POST /v1/fund/redeem
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
		"fundId":123					// 基金id
		"portion":30					// 赎回份额
	}
###### 返回值
	{
	    "status": 0,
	    "data":""
	    "message": ""
	}	
<br/>

### <a name="9">9 获取充币地址</a>
###### 请求接口
	GET /v1/chain/address/{symbol}
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
		"symbol":"eth"					// 币种代码
	}
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"address":"0xf781dfd6def0f3b44be1a26650ab620dce0c0ac2"
	    }
	    "message": ""
	}
<br/>

### <a name="10">10 获取资产</a>
###### 请求接口
	GET /v1/chain/account/{token}
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
		"token":"usdt"					// 币种代码
	}
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"available":"100.0000",				// 可用
	    	"hold":"100.0000"				// 冻结
	    }
	    "message": ""
	}
<br/>

### <a name="11">11 提币</a>
###### 请求接口
	POST /v1/chain/withdraw
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
		"currency_id":19					// 币种id，usdt=19
		"address":"0xB2199C06F617CC252dDB34b9dfb1076F12A3344A"	// 提币地址
		"size":20.00						// 提币数量
		"code":"1234"						// 短信验证码
	}
###### 返回值
	{
	    "status": 0,
	    "data":
	    "message": ""
	}
<br/>

### <a name="12">12 获取净值列表</a>
###### 请求接口
	GET /public/v1/fund/{fundId}/npvs
###### 请求参数
	{
		"fundId":123					// 基金id
		"start":10000000				// 起始时间戳，单位：毫秒（可选，默认三个月数据）
		"end":100000000					// 结束时间戳，单位：毫秒（可选，默认三个月数据）
	}
###### 返回值
	{
	    "status": 0,
	    "data":[{
	    	"npv": 1.0923,					// 基金净值
	    	"day": "2020-01-01",				// 日期
	    }
	    ...
	    ]
	    "message": ""
	}
<br/>

### <a name="13">13 用户资产估值及明细</a>
###### 请求接口
	GET /v1/fund/assets
###### HEADER请求参数
	{
		"Authorization":TOKEN				// 用户token
	}
###### 请求参数
	{
	}
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"asset": 1000.00,				// 总估值
	    	"asset_cny": 7000.00,				// 总估值（人民币） 
	    	"assets": [					// 明细
	    		{
	    			"available": 100.00,		// 可用
	    			"hold":0.00,			// 冻结
	    			"available_worth":100.00,	// 可用估值（人民币）
	    			"hold_worth":0.00,		// 冻结估值（人民币）
	    		}
	    		...
	    	]
	    }
	    "message": ""
	}
<br/>

### <a name="14">14 站内信列表</a>
###### 请求接口
	GET /v1/fund/msgs
###### 请求参数
	{
	}
###### 返回值
	{
	    "status": 0,
	    "data":[{
	    	"id": 1,				// id
	    	"title": "",				// 标题
	    	"content":"",				// 内容
		"read":0,				// =0,未读；=1,已读（登录后返）
	    	"create_time":"2020-01-01"		// 添加日期
	    }
	    ...
	    ]
	    "message": ""
	}

<br/>

### <a name="15">15 站内信详情</a>
###### 请求接口
	GET /public/v1/fund/msg
###### 请求参数
	{
		"msgId": 1
	}
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"title": "",				// 标题
	    	"content":"",				// 内容
	    	"create_time":"2020-01-01"		// 添加日期
	    }
	    ...
	    "message": ""
	}

<br/>

### <a name="16">16 用户资产及收益</a>
###### 请求接口
	GET /v1/fund/{fundId}/profit
###### HEADER请求参数
	{
		"Authorization":TOKEN			// 用户token
	}
###### 请求参数
	{
		"fundId": 100				// 基金id，请求地址中传入
	}
###### 返回值
	{
	    "status": 0,
	    "data":{
	    	"asset": 1000.00,			// 净资产
	    	"profit":10.00,				// 今日收益
	    	"profit_total":100.00			// 持有收益
		“profit_rate":23.00			// 持有收益率（百分比）
		"amount":190.00				// 持有金额
		"amount_unconfirmed":10.00		// 待确认金额
		"cost":0.917				// 持仓成本
		"portion":190.00			// 持有份额
		"extent_daily":0.98			// 日涨幅（百分比）
		"npv":1.01				// 净值
		"day":1611763200000			// 日期（时间戳）
	    }
	    ...
	    "message": ""
	}

<br/>

### <a name="17">17 站内信未读数量</a>
###### 请求接口
	GET /v1/fund/msg/unread
###### HEADER请求参数
	{
		"Authorization":TOKEN			// 用户token
	}
###### 请求参数
	{
	}
###### 返回值
	{
	    "status": 0,
	    "data": 0,					// 未读数量
	    "message": ""
	}

<br/>

### <a name="18">18 站内信标为已读</a>
###### 请求接口
	POST /v1/fund/msg/read
###### HEADER请求参数
	{
		"Authorization":TOKEN			// 用户token
	}
###### 请求参数
	{
		"msgId":1				// 站内信id，如把所有都标为已读，传0
	}
###### 返回值
	{
	    "status": 0,
	    "data": "",
	    "message": ""
	}

<br/>
