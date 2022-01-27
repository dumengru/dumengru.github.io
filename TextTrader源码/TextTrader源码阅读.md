# **TextTrader源码(CTP接口入门)**

## **主体架构(面向过程编程)**
>
> 1.文件替换.
>
>> - 配置文件: TextTrader.ini
>> - 行情文件: thostmduserapi_se.dll
>> - 交易文件: thosttraderapi_se.dll
>
> 2.定义状态宏.
>
>> - 连接断开: `CONNECTION_STATUS_DISCONNECTED`
>> - 连接成功: `CONNECTION_STATUS_CONNECTED`
>> - 登录成功: `CONNECTION_STATUS_LOGINOK`
>> - 登录失败: `CONNECTION_STATUS_LOGINFAILED`
>
> 3.定义类
>
>> - 交易类: `class CTradeRsp:public CThostFtdcTraderSpi`
>> - 行情类: `class CQuoteRsp:public CThostFtdcMdSpi`
>
> 4.定义结构体
>
>> - 报价: `quotation_t`
>> - 持仓: `stPosition_t`
>> - 账户: `stAccount_t`
>
> 5.CTP主函数(订阅行情等)
>
> 6.其他函数(界面显示等)
>

## **类对象**

### **CTradeRsp**
>
> `void CTradeRsp::HandleFrontConnected()`
>
>> - 1. 显示当前状态(连接)
>> - 2. 判断是否有 `UserProductInfo` 字段
>> - 3. 若有 `UserProductInfo` 字段, 调用 `pTradeReq->ReqAuthenticate` 函数登录认证(穿透式监管)
>> - 4. 若无 `UserProductInfo` 字段, 调用 `pTradeReq->ReqUserLogin` 函数登录认证
>
> `void OnFrontConnected()`
> **已连接**
>
>> - 调用 `&CTradeRsp::HandleFrontConnected` 登录认证
>
> `void CTradeRsp::HandleFrontDisconnected(int nReason)`
>
>> - 1. 显示当前状态(断开)
>> - 2. 判断当前工作窗口
>> - 3. 刷新界面
>
> `void OnFrontDisconnected(int nReason)`
> **未连接**
>
>> - 调用 `&CTradeRsp::HandleFrontDisconnected` 断开连接
>
> `void CTradeRsp::HandleRspAuthenticate`
>
>> - 1. 显示当前状态(终端认证成功/失败)
>> - 2. 调用 `pTradeReq->ReqUserLogin` 函数登录认证
>
> `void OnRspAuthenticate`
> **认证响应**
>
>> - 调用 `&CTradeRsp::HandleRspAuthenticate` 进行终端认证
>
> `void CTradeRsp::HandleRspUserLogin`
>
>> - 1. 显示当前状态(登录成功/失败)
>> - 2. 清除多余订单
>> - 3. 刷新窗口
>> - 4. 调用 `pTradeReq->ReqSettlementInfoConfirm` 确认结算单
>> - 5. 调用 `pTradeReq->ReqQryInstrument` 查询合约
>
> `void OnRspUserLogin`
> **登陆应答**
>
>> 1. 请求结构体 CThostFtdcRspUserLoginField
>> 2. 响应结构体 CThostFtdcRspInfoField
>> 3. 调用 `&CTradeRsp::HandleRspUserLogin` 函数登录
>
>
>
>
>
### **CQuoteRsp**
>
>
>
>
>
>
>
>
>
>
>
>
>
>
>
## **结构体**
>

## **主函数**
>

## **其他函数**
>
>> - `int subscribe(const char *product_id);`
>> - `int unsubscribe(const char *product_id);`
>> - `const char *apistrerror(int e);`
>> - `void init_screen();`
>> - `void on_key_pressed(int ch);`
>> - `void time_thread();`
>> - `void work_thread();`
>> - `void HandleTickTimeout();`
>> - `void HandleStatusClear();`
>