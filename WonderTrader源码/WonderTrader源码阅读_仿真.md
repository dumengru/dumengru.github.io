# **WonderTrader仿真**

## **Parsers(行情接口)**

CTPApi文件

 1. error.dtd
 2. error.xml
 3. ThostFtdcMdApi.h            定义了客户端接口
 4. ThostFtdcUserApiDataType.h  定义了客户端接口使用的业务数据类型
 5. ThostFtdcUserApiStruct.h    定义了客户端接口使用的业务数据结构

### **CThostFtdcMdSpi**
>
> "ThostFtdcMdApi.h"
> 连接CTP行情
>
> 接口方法
>>
>> "virtual void OnFrontConnected"
>>
>> - 当客户端与交易后台建立起通信连接时（还未登录前），该方法被调用
>>
>> "virtual void OnFrontDisconnected"
>>
>> - 当客户端与交易后台通信连接断开时，该方法被调用
>>
>> "virtual void OnHeartBeatWarning"
>>
>> - 心跳超时警告。当长时间未收到报文时，该方法被调用
>>
>> "virtual void OnRspUserLogin"
>>
>> - 登录请求响应
>>
>> "virtual void OnRspUserLogout"
>>
>> - 登出请求响应
>>
>> "virtual void OnRspError"
>>
>> - 错误应答
>>
>> "virtual void OnRspSubMarketData"
>>
>> - 订阅行情应答
>>
>> "virtual void OnRspUnSubMarketData"
>>
>> - 取消订阅行情应答
>>
>> "virtual void OnRspSubForQuoteRsp"
>>
>> - 订阅询价应答
>>
>> "virtual void OnRspUnSubForQuoteRsp"
>>
>> - 取消订阅询价应答
>>
>> "virtual void OnRtnDepthMarketData"
>>
>> - 深度行情通知
>>
>> "virtual void OnRtnForQuoteRsp"
>>
>> - 询价通知
>>
### **CThostFtdcMdApi**
>
> "ThostFtdcMdApi.h"
> 订阅行情
>
> 接口方法
>
>> "static CThostFtdcMdApi *CreateFtdcMdApi"
>>
>> - 创建MdApi
>>
>> "static const char *GetApiVersion"
>>
>> - 获取API的版本信息
>>
>> "virtual void Release"
>>
>> - 不再使用本接口对象时,调用该函数删除接口对象
>>
>> "virtual void Init"
>>
>> - 初始化运行环境,只有调用后,接口才开始工作
>>
>> "virtual int Join"
>>
>> - 等待接口线程结束运行
>>
>> "virtual const char *GetTradingDay"
>>
>> - 获取当前交易日
>>
>> "virtual void RegisterFront"
>>
>> - 注册前置机网络地址
>>
>> `virtual void RegisterNameServer`
>>
>> - 注册名字服务器网络地址
>>
>> `virtual void RegisterFensUserInfo`
>>
>> - 注册名字服务器用户信息
>>
>> `virtual void RegisterSpi`
>>
>> - 注册回调接口
>>
>> `virtual int SubscribeMarketData`
>>
>> - 订阅行情
>>
>> `virtual int UnSubscribeMarketData`
>>
>> - 退订行情
>>
>> `virtual int SubscribeForQuoteRsp`
>>
>> - 订阅询价
>>
>> `virtual int UnSubscribeForQuoteRsp`
>>
>> - 退订询价
>>
>> `virtual int ReqUserLogin`
>>
>> - 用户登录请求
>>
>> `virtual int ReqUserLogout`
>>
>> - 登出请求
>>
### **IParserSpi**
>
> "Includes/IParserApi.h"
> 行情解析模块回调接口
>
> 接口函数
>
>> `virtual void handleEvent`
>>
>> - 处理模块事件(如连接、断开、登录、登出)
>>
>> `virtual void handleSymbolList`
>>
>> - 处理合约列表(基础元素为WTSContractInfo)
>>
>> `virtual void handleQuote`
>>
>> - 处理实时行情
>>
>> `virtual void handleOrderQueue`
>>
>> - 处理委托队列数据（股票level2）
>>
>> `virtual void handleOrderDetail`
>>
>> - 处理逐笔委托数据（股票level2）
>>
>> `virtual void handleTransaction`
>>
>> - 处理逐笔成交数据
>>
>> `virtual void handleParserLog`
>>
>> - 处理解析模块的日志
>>
### **IParserApi**
>
> "Includes/IParserApi.h"
> 行情解析模块接口
>
>> `virtual bool init`
>>
>> - 初始化解析模块
>>
>> `virtual void release`
>>
>> - 释放解析模块
>>
>> `virtual bool connect`
>>
>> - 开始连接服务器
>>
>> `virtual bool disconnect`
>>
>> - 断开连接
>>
>> `virtual bool isConnected`
>>
>> - 是否已连接
>>
>> `virtual void subscribe(const CodeSet& setCodes)`
>>
>> - 订阅合约列表
>>
>> `virtual void unsubscribe(const CodeSet& setCodes)`
>>
>> - 退订合约列表
>>
>> `virtual void registerSpi`
>>
>> - 注册回调接口
>>
### **ParserCTP**
>
> "ParserCTP/ParserCTP.h"
> 解析CTP行情接口
>
> 属性和方法
>>
>> `CThostFtdcMdApi* m_pUserAPI`;
>>
>> 行情Api对象
>>
>> `inline uint32_t strToTime(const char* strTime)`
>>
>> - 字符串转时间
>>
>> `inline double checkValid(double val)`
>>
>> - 检查浮点数是否有效
>>
>> `bool ParserCTP::init(WTSParams* config)`
>>
>> - 1. 从配置文件获取 front, broker, user, pass, flowdir(文件目录)
>> - 2. 从配置文件获取 ctpmodule 名称, 并加载 ctpmodule.dll
>> - 3. 调用 `m_pUserAPI = m_funcCreator`, 创建MdApi对象
>> - 4. 调用 `m_pUserAPI->RegisterSpi` 和 `m_pUserAPI->RegisterFront`
>>
>> `void ParserCTP::release()`
>>
>> `bool ParserCTP::connect()`
>>
>> `bool ParserCTP::disconnect()`
>>
>> - 断开, 连接, 断开接口
>>
>> `void ParserCTP::registerSpi`
>>
>> - 注册父类Spi接口
>>
>> `void ParserCTP::subscribe(const CodeSet &vecSymbols)`
>>
>> - 订阅合约列表接口
>>
>> `void ParserCTP::SubscribeMarketData()`
>>
>> - 调用 `m_pUserAPI->SubscribeMarketData` 订阅合约
>>
### ****
>>
>>
>>
>>
>>
>>
>>
>>
>>
>>
>>
>>
>>

## **Traders(交易接口)**
>
>
>
>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>
>> ``
>>
>> -
>>