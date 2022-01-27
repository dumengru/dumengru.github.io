# **Backtest**

## **WtBtCore**

## **WtBtPorter**

## **WtBtRunner**

### **mdump.cpp**

> - 通用进程内插件，用于从应用程序崩溃中生成崩溃转储

### **WtBtRunner.cpp**

> - `WTSLogger::init("logcfg.json")`
>   - 加载 logcfg.json 文件
>
> - `install_signal_hooks`
>   - 添加一个匿名函数钩子
>
> - `StdFile::read_file_content(filename.c_str(), content);`
>   - 加载 config.json 文件内容到 content
>
> - `root.Parse(content.c_str())`
>   - 解析文件内容到 Document 对象中 root
>
> - `jsonToVariant(root, cfg);`
>   - 将 json 类型转化为 WT 系统容器类型
>
> - `replayer.init(cfg->get("replayer"));`
>   - 传入"replayer", 初始化回测引擎
>   - 根据"basefiles", 加载其他配置文件
>
> - `WTSVariant* cfgEnv = cfg->get("env");`
>   - 获取策略执行环境
>
> - `const char* mode = cfgEnv->getCString("mocker");`
>   - 获取回测模式
>
> - `int32_t slippage = cfgEnv->getInt32("slippage");`
>   - 获取滑点
>
> - `if (strcmp(mode, "cta") == 0)`
>
>   - 选择对应的回测模式
>
> - `CtaMocker* mocker = new CtaMocker(&replayer, "cta", slippage);`
>
> - 根据不同回测模式创建不同模型管理器(将replayer作为参数传入)
>
> - `mocker->init_cta_factory(cfg->get("cta"));`
>
> - 利用调度器根据对应文件配置创建对应策略模型
>
>
> - `replayer.prepare();`
>   - 准备播放历史数据
> - `replayer.run();`
>   - 播放历史数据
> - `WTSLogger::stop();`
>   - 策略停止

---
---
---

## **数据类型**

### **WTSTypes**
>
> "Includes/WTSTypes.h"
> WonderTrader基本数据类型定义文件(合约分类, K线周期, 价格类型等)
>
### **WTSStruct**
>
> "Includes/WTSStruct.h"
> Wt基础结构体定义(K线, 订单, 委托结构体)
>

### **DataDefine**
>
> "WtBtCore/DataDefine.h"
>
> 各种数据类型结构体数组(实时K线数据块, 逐笔成交数据块)

## **辅助类对象**

### **CodeHelper**
>
> "Share/CodeHelper.hpp"
> 代码辅助类, 封装到一起方便使用
>
> 预定义
>
>> `static const char* SUFFIX_HOT = ".HOT";`
>>
>> `static const char* SUFFIX_2ND = ".2ND";`
>>
> 属性和方法
>
>> - 主力和次主力合约后缀
>>
>> `_CodeInfo`
>>
>> 合约信息
>>
>> `static inline std::size_t find(const char* src, char symbol = '.', bool bReverse = false)`
>>
>> - 查询合约列表中某合约索引
>>
>> `static inline bool isStdStkCode(const char* code)`
>>
>> - 是否是标准股票合约
>>
>> `static inline bool isStdFutHotCode(const char* stdCode)`
>>
>> - 是否标准期货主力合约
>>
>> `static inline bool isStdFut2ndCode(const char* stdCode)`
>>
>> - 是否标准期货次主力合约
>>
>> `static inline bool isStdFutOptCode(const char* code)`
>>
>> - 是否标准期货期权合约
>>
>> `static inline bool isStdFutCode(const char* code)`
>>
>> - 是否标准期货合约
>>
>> `static inline std::string stdCodeToStdCommID(const char* stdCode)`
>>
>> - 标准代码转标准品种ID
>>
>> `static inline std::string rawFutCodeToRawCommID(const char* code)`
>>
>> - 从基础合约代码提取基础品种代码
>>
>> `static inline std::string rawFutCodeToStdCode(const char* code, const char* exchg, bool isComm = false)`
>>
>> - 基础合约代码转标准码
>>
>> `static inline std::string rawStkCodeToStdCode(const char* code, const char* exchg, const char* pid)`
>>
>> - 原始股票代码转标准码
>>
>> `static inline std::string rawFutOptCodeToStdCode(const char* code, const char* exchg)`
>>
>> - 期货期权代码标准化
>>
>> `static inline std::string stdCodeToStdHotCode(const char* stdCode)`
>>
>> - 标准合约代码转主力代码
>>
>> `static inline std::string stdCodeToStd2ndCode(const char* stdCode)`
>>
>> - 标准合约代码转次主力代码
>>
>> `static inline CodeInfo extractStdFutCode(const char* stdCode)`
>>
>> - 提取标准期货合约代码信息
>>
>> `static inline CodeInfo extractStdStkCode(const char* stdCode)`
>>
>> - 提取标准股票合约代码
>>
>> `static inline CodeInfo extractStdFutOptCode(const char* stdCode)`
>>
>> - 提取标准期货期权代码信息
>>
>> `static CodeInfo extractStdCode(const char* stdCode)`
>>
>> - 提取标准代码的信息
>>
### **CsvHelper.h**
>
> "WTSTools/CsvHelper.h"
> Csv文件处理辅助类
>
> 属性和方法
>
>> `CsvReader(const char* item_splitter = ",");`
>>
>> - 初始化时添加分隔符
>>
>> `bool load_from_file(const char* filename);`
>>
>> - 加载文件
>>
### **WTSCmpHelper**
>
> "WTSTools/WTSCmpHelper"
> 数据压缩辅助类,利用zstdlib压缩
>
> 属性和方法
>>
>> `static std::string compress_data(const void* data, uint32_t dataLen, uint32_t uLevel = 1)`
>>
>> - 压缩数据
>>
>> `static std::string uncompress_data(const void* data, uint32_t dataLen)`
>>
>> - 解压数据
>>
## **decimal**
>
> "Share/decimal.h"
> 浮点数据辅助类
>
> 主要用于各种浮点数的比较
>
### **IDataFactory**
>
> "Includes/IDataFactory.h"
> 数据拼接工厂接口定义
>
> 虚基类, 用于各种数据的拼接
>>
>> `virtual WTSBarStruct* updateKlineData(WTSKlineData* klineData, WTSTickData* tick, WTSSessionInfo* sInfo)`
>>
>> - 利用tick数据更新K线
>>
>> `virtual WTSBarStruct* updateKlineData(WTSKlineData* klineData, WTSBarStruct* newBasicBar, WTSSessionInfo* sInfo)`
>>
>> - 利用基础周期K线数据更新K线
>>
>> `virtual WTSKlineData* extractKlineData(WTSKlineSlice* baseKline, WTSKlinePeriod period, uint32_t times, WTSSessionInfo* sInfo, bool bIncludeOpen = true)`
>>
>> - 从基础周期K线数据提取非基础周期的K线数据
>>
>> `virtual WTSKlineData* extractKlineData(WTSTickSlice* ayTicks, uint32_t seconds, WTSSessionInfo* sInfo, bool bUnixTime = false)`
>>
>> - 从tick数据提取秒周期的K线数据
>>
>> `virtual bool mergeKlineData(WTSKlineData* klineData, WTSKlineData* newKline)`
>>
>> - 合并K线
>>
### **WTSDataFactory**
>
> "WTSTools/WTSDataFactory.h"
> 数据拼接工厂类定义
>
> 1. 实现父类更新K线方法
>
> 2. 更新固定周期K线方法
>
> 3. 提取固定周期K线数据
>

---

### **StrUtil**
>
> "Share/StrUtil.hpp"
> 字符串处理的封装
>
> `static inline void trim(std::string& str, const char* delims = " \t\r", bool left = true, bool right = true)`
>
> `static inline std::string trim(const char* str, const char* delims = " \t\r", bool left = true, bool right = true)`
>
> -
>
> `static inline void trimAllSpace(std::string &str)`
>
> - 去掉所有空格
>
> `static inline void trimAll(std::string &str,char ch)`
>
> - 去掉所有特定字符
>
> `static inline StringVector split( const std::string& str, const std::string& delims = "\t\n ", unsigned int maxSplits = 0)`
>
> `static inline void split(const std::string& str, StringVector& ret, const std::string& delims = "\t\n ", unsigned int maxSplits = 0)`
>
> - 分割字符串
>
> `static inline void toLowerCase( std::string& str )`
>
> `static inline void toUpperCase( std::string& str )`
>
> `static inline std::string makeLowerCase(const char* str)`
>
> `static inline std::string makeUpperCase(const char* str)`
>
> - 大小写转换
>
> `static inline float toFloat( const std::string& str )`
>
> `static inline double toDouble( const std::string& str )`
>
> `将字符串转为数字`
>
> `static inline bool startsWith(const std::string& str, const std::string& pattern, bool lowerCase = true)`
>
> `static inline bool endsWith(const std::string& str, const std::string& pattern, bool lowerCase = true)`
>
> - 判断字符串以何开始或结束
>
> `static inline std::string standardisePath( const std::string &init, bool bIsDir = true)`
>
> - 标准化路径
>
> `static inline void splitFilename(const std::string& qualifiedName,std::string& outBasename, std::string& outPath)`
>
> - 分割目录和文件名
>
> `static inline bool match(const std::string& str, const std::string& pattern, bool caseSensitive = true)`
>
> - 字符串模式匹配
>
> `static inline const std::string BLANK()`
>
> - 空字符串
>
> `static inline std::string printf(const char *pszFormat, ...)`
>
> `static inline std::string printf2(const char *pszFormat, ...)`
>
> `static inline std::string printf2(const char *pszFormat,va_list argptr)`
>
> `static inline std::string printf(const char* pszFormat, va_list argptr)`
>
> - 地球人都知道,恶心的std::string是没有CString的Format这个函数的,所以我们自己造字符串格式化
>
> `static inline std::string extend(const char* str, uint32_t length)`
>
> - 将字符串扩充到指定长度
>
> `static inline std::string right(const std::string &src,size_t nCount)`
>
> `static inline std::string left(const std::string &src,size_t nCount)`
>
> - 取得右/左边 N 个字符
>
> `static inline size_t charCount(const std::string &src,char ch)`
>
> - 统计字符串中某字符个数
>
> `static inline void replace(std::string& str, const char* src, const char* des)`
>
> - 字符串替换
>
> `static inline std::string fmtInt64(int64_t v)`
>
> `static inline std::string fmtUInt64(uint64_t v)`
>
> - 格式化输出
>

### **StdFile**
>
> "Share/StdUtils.hpp"
>
> 文件辅助类
>
> `static inline uint64_t read_file_content(const char* filename, std::string& content)`
>
> - 读取文件内容
>
> `static inline void write_file_content(const char* filename, const std::string& content)`
>
> `static inline void write_file_content(const char* filename, const void* data, std::size_t length)`
>
> - 将内容写入文件
>
> `static inline bool exists(const char* filename)`
>
> - 判断文件是否存在
>

### **"TimeUtils"**
>
> "Share/TimeUtils.hpp"
> 时间处理的封装
>
> 属性和方法
>>
>> `static inline int64_t getLocalTimeNow(void)`
>>
>> `static inline int64_t getLocalTimeNano(void)`
>>
>> - 获取本地时间微秒/纳秒
>>
>> `static inline std::string getLocalTime(bool bIncludeMilliSec = true)`
>>
>> `static inline std::string now(void)`
>>
>> `static inline std::string getYYYYMMDD(void)`
>>
>> `static inline uint64_t getYYYYMMDDhhmmss()`
>>
>> `static inline std::string getYYYYMMDD_hhmmss(void)`
>>
>> - 获取日期时间字符串
>>
>> `static inline void getDateTime(uint32_t &date, uint32_t &time)`
>>
>> - 分别获取日期和时间
>>
>> `static inline uint32_t getCurDate()`
>>
>> `static inline uint32_t getWeekDay(uint32_t uDate = 0)`
>>
>> `static inline uint32_t getCurMin()`
>>
>> - 获取当前日期, 周, 分钟
>>
>> `static inline int64_t makeTime(std::string time_str)`
>>
>> - 日期字符串 -> 毫秒
>>
>> `static inline int64_t makeTime(long lDate, long lTime)`
>>
>> - 日期时间 -> 毫秒
>>
>> `static std::string timeToString(int64_t mytime)`
>>
>> - 时间 -> 字符串
>>
>> `static uint32_t getNextDate(uint32_t curDate, int days = 1)`
>>
>> - 日期计算
>>
>> `static uint32_t getNextMinute(int32_t curTime, int32_t mins = 1)`
>>
>> - 分钟计算
>>
>> `static uint32_t getNextMonth(uint32_t curMonth, int months = 1)`
>>
>> - 月份计算
>>
>> `static inline uint32_t timeToMinBar(uint32_t uDate, uint32_t uTime)`
>>
>> `static inline uint32_t minBarToDate(uint32_t minTime)`
>>
>> `static inline uint32_t minBarToTime(uint32_t minTime)`
>>
>> - minBar和日期时间转换
>>
>> `static inline bool isWeekends(uint32_t uDate)`
>>
>> - 判断是否周末
>>
> ### **class Time32**
>
> 自定义时间类, 时间形如: 20220119
>> `uint32_t _msec;`
>>
>> - 毫秒
>>
>> `struct tm t;`
>>
>> - 时间结构体
>>
>> `void from_local_time(uint64_t _time)`
>>
>> - 获取时间结构体
>>
>> `uint32_t date()`
>>
>> `uint32_t time()`
>>
>> `uint32_t time_ms()`
>>
>> `const char* fmt(const char* sfmt = "%Y.%m.%d %H:%M:%S", bool hasMilliSec = false) const`
>>
>> - 获取日期, 秒时间, 毫秒时间, 格式化时间
>>
> ### **class Ticker**
>
>> 高精度计时器类
>>

---
---
---

## **类对象**

### **WTSObject**

> "Includes/WTSObject.hpp"
> Wt基础Object定义
>
>> `volatile std::atomic<uint32_t> m_uRefs;`
>>
>> - volatile 防止编译器优化
>> - atomic 原子操作对象
>> - m_uRefs 为创建的对象计数

### **WTSArray**

> "Includes/WTSCollection.hpp"
> 平台数组容器
>
> 内部使用vector实现, 数据使用WTSObject指针对象, 所有WTSObject的派生类都可以使用, 用于平台内使用
>
> 变量和方法
>> `std::vector<WTSObject*> _vec;`
>>
>> - 装载对象的容器
>>
>> `std::atomic<bool> _holding;`
>>
>> - 原子操作对象
>>
>> `static WTSArray* create()`
>>
>> - 创建数组对象
>>
>> `uint32_t size()`
>>
>> - 获取数据长度
>>
>> `void resize(uint32_t _size)`
>>
>> - 重新分配内存, 默认值为 NULL
>>
>> `WTSObject* at(uint32_t idx)`
>>
>> - 读取指定索引元素
>>
>> `uint32_t idxOf(WTSObject* obj)`
>>
>> - 获取对象索引
>>
>> `WTSObject* operator [](uint32_t idx)`
>>
>> - [] 符号重载, 用法同 at()
>>
>> `WTSObject* grab(uint32_t idx)`
>>
>> - 获取指定索引元素, 用法同 at(), 但会增加引用计数
>>
>> `void append(WTSObject* obj, bool bAutoRetain = true)`
>>
>> - 末尾添加元素, 引用计数+1
>>
>> `void set(uint32_t idx, WTSObject* obj, bool bAutoRetain = true)`
>>
>> - 设置指定位置元素, 引用计数+1
>>
>> `void append(WTSArray* ay)`
>>
>> - 末尾合并另一个数组元素
>>
>> `void clear()`
>>
>> - 清空数组
>>
>> `virtual void release()`
>>
>> - 释放数组对象
>>
>> `void sort(SortFunc func)`
>>
>> - 数组排序

### **WTSValueArray**
>
> "Includes/WTSDataDef.hpp"
> 数值数组的内部封装
>
>采用std::vector实现, 包含数据格式化字符串, 数值的数据类型为double
>
> 属性和方法
>>
>> `vector<double> m_vecData;`
>>
>> - 存储数据
>>
>> `static WTSValueArray* create()`
>>
>> - 创建数值数组对象
>>
>> `inline uint32_t size() const`
>>
>> `inline bool empty()`
>>
>> - 获取数组长度
>>
>> `inline double at(uint32_t idx) const`
>>
>> - 获取指定位置数据
>>
>> `double maxvalue(int32_t head, int32_t tail, bool isAbs = false) const`
>>
>> `double minvalue(int32_t head, int32_t tail, bool isAbs = false) const`
>>
>> - 找到指定范围最大/最小值
>>
>> `inline void append(double val)`
>>
>> - 末尾添加数据
>>
>> `inline void set(uint32_t idx, double val)`
>>
>> - 设置指定位置数据
>>
>> `inline void resize(uint32_t uSize, double val = INVALID_DOUBLE)`
>>
>> - 为数组重新分配内存并设置默认值
>>
>> `inline double& operator[](uint32_t idx)`
>>
>> - 重载[]
>>
>> `inline std::vector<double>>& getDataRef()`
>>
>> - 获取数据对象 m_vecData
>>
### **WTSKlineSlice**
>
> "Includes/WTSDataDef.hpp"
> K线数据切片
>
>这个比较特殊,因为要拼接当日和历史的, 所以有两个开始地址
>
> 属性和方法
>>
>> `WTSKlinePeriod m_kpPeriod;`
>>
>> - K线周期, 默认1分钟
>>
>> `WTSBarStruct* m_bsHisBegin;`
>>
>> - 历史K线开始地址
>>
>> `int32_t m_iHisCnt;`
>>
>> - 历史数据量
>>
>> `WTSBarStruct* m_bsRtBegin;`
>>
>> - 当日K线开始地址
>>
>> `int32_t m_iRtCnt;`
>>
>> - 当日数据量
>>
>> `static WTSKlineSlice* create(const char* code, WTSKlinePeriod period, uint32_t times, WTSBarStruct* hisHead, int32_t hisCnt, WTSBarStruct* rtHead = NULL, int32_t rtCnt = 0)`
>>
>> - 创建K线切片指针对象
>>
>> `inline WTSBarStruct* get_his_addr()`
>>
>> - 获取历史数据开始地址
>>
>> `inline int32_t get_his_count()`
>>
>> - 获取历史数据量
>>
>> `inline WTSBarStruct* get_rt_addr()`
>>
>> - 获取当日数据开始地址
>>
>> `inline int32_t get_rt_count()`
>>
>> - 获取当日数据量
>>
>> `inline WTSBarStruct* at(int32_t idx)`
>>
>> - 获取指定位置K线, 从历史数据开始数
>>
>> `double maxprice(int32_t head, int32_t tail) const`
>>
>> - 指定范围内最高价的最大值
>>
>> `double minprice(int32_t head, int32_t tail) const`
>>
>> - 指定范围最低价的最小值
>>
>> `inline int32_t size() const`
>>
>> `inline bool empty() const`
>>
>> - 返回K线大小
>>
>> `inline const char* code()`
>>
>> `inline void setCode(const char* code)`
>>
>> - 返回/获取K线对象合约代码
>>
>> `double open(int32_t idx) const`
>>
>> `double high(int32_t idx) const`
>>
>> `double low(int32_t idx) const`
>>
>> `double close(int32_t idx) const`
>>
>> `uint32_t volume(int32_t idx) const`
>>
>> `uint32_t openinterest(int32_t idx) const`
>>
>> `int32_t additional(int32_t idx) const`
>>
>> `double money(int32_t idx) const`
>>
>> `uint32_t date(int32_t idx) const`
>>
>> `uint32_t time(int32_t idx) const`
>>
>> - 获取指定位置 open, high, low, close, volume, openinterest(总持仓), additional(增仓), money(成交额), date, time
>>
>> `WTSValueArray* extractData(WTSKlineFieldType type, int32_t head = 0, int32_t tail = -1) const`
>>
>> - 获取指定范围某特定字段全部数据并保存到数组中
>>
### **WTSKlineData**
>
> "Includes/WTSDataDef.hpp"
> K线数据
>
> K线数据的内部数据使用WTSBarStruct, WTSBarStruct是一个结构体, 因为K线数据单独使用的可能性较低, 所以不做WTSObject派生类的封装
>
> 属性和方法
>>
>> `typedef std::vector<WTSBarStruct>> WTSBarList;`
>>
>> - 保存K线结构体的容器
>>
>> `char m_strCode[32];`
>>
>> - 品种代码
>>
>> `WTSKlinePeriod m_kpPeriod;`
>>
>> - K线周期, 默认分钟
>>
>> `uint32_t m_uTimes;`
>>
>> - 时间戳
>>
>> `bool m_bUnixTime;`
>>
>> - 是否是时间戳, 对秒周期有效
>>
>> `bool m_bClosed;`
>>
>> - 是否是闭合K线
>>
>> `static WTSKlineData* create(const char* code, uint32_t size)`
>>
>> - 创建K线数据对象
>>
>> `inline void setClosed(bool bClosed)`
>>
>> `inline bool isClosed() const`
>>
>> - 设置/获取K线闭合标志
>>
>> `inline void setPeriod(WTSKlinePeriod period, uint32_t times = 1){ m_kpPeriod = period; m_uTimes = times; }`
>>
>> `inline void setUnixTime(bool bEnabled = true)`
>>
>> - 设置时间周期和时间戳标志
>>
>> `inline WTSKlinePeriod period() const`
>>
>> `inline uint32_t times() const`
>>
>> `inline bool isUnixTime() const`
>>
>> - 获取周期/ 时间 和时间戳
>>
>> `inline double maxprice(int32_t head, int32_t tail) const`
>>
>> - 获取指定范围最高价的最大值
>>
>> `inline double minprice(int32_t head, int32_t tail) const`
>>
>> - 获取指定范围最低价的最小值
>>
>> `inline uint32_t size() const`
>>
>> `inline bool IsEmpty() const`
>>
>> - 获取K线大小
>>
>> `inline const char* code() const`
>>
>> `inline void setCode(const char* code)`
>>
>> - 获取/设置品种代码
>>
>> `inline double open(int32_t idx) const`
>>
>> `inline double high(int32_t idx) const`
>>
>> `inline double low(int32_t idx) const`
>>
>> `inline double close(int32_t idx) const`
>>
>> `inline uint32_t volume(int32_t idx) const`
>>
>> `inline uint32_t openinterest(int32_t idx) const`
>>
>> `inline int32_t additional(int32_t idx) const`
>>
>> `inline double money(int32_t idx) const`
>>
>> `inline uint32_t date(int32_t idx) const`
>>
>> `inline uint32_t time(int32_t idx) const`
>>
>> - 获取指定位置 open, high, low, close, volume, openinterest, additional, money, date, time
>>
>> `WTSValueArray* extractData(WTSKlineFieldType type, int32_t head = 0, int32_t tail = -1) const`
>>
>> - 获取指定范围某特定字段全部数据
>>
### **WTSTickData**
>
> "Includes/WTSDataDef.hpp"
> Tick数据对象
>
> 内部封装WTSTickStruct, 封装的主要目的是出于跨语言的考虑
>
> 属性和方法
>>
>> `WTSTickStruct m_tickStruct;`
>>
>> - Tick结构体
>>
>> `uint64_t m_uLocalTime;`
>>
>> - 本地时间
>>
>> `static inline WTSTickData* create(const char* stdCode)`
>>
>> `static inline WTSTickData* create(WTSTickStruct& tickData)`
>>
>> - 创建Ticks数据对象
>>
>> `inline void setCode(const char* code)`
>>
>> `inline const char* code() const`
>>
>> - 设置/获取合约代码
>>
>> `inline const char* exchg() const`
>>
>> `inline double price() const`
>>
>> `inline double open() const`
>>
>> `inline double high() const`
>>
>> `inline double low() const`
>>
>> - 获取交易所代码, 最新价, open, high, low
>>
>> `inline double preclose() const`
>>
>> `inline double presettle() const`
>>
>> `inline int32_t preinterest() const`
>>
>> - 获取昨日收盘价, 结算价, 总持仓量
>>
>> `inline double upperlimit() const`
>>
>> `inline double lowerlimit() const`
>>
>> `inline uint32_t totalvolume() const`
>>
>> `inline uint32_t volume() const`
>>
>> - 获取涨/跌停板价格, 总成交量, 成交量
>>
>> `inline double settlepx() const`
>>
>> `inline uint32_t openinterest() const`
>>
>> `inline int32_t additional() const`
>>
>> `inline double totalturnover() const`
>>
>> `inline double turnover() const`
>>
>> `inline uint32_t tradingdate() const`
>>
>> `inline uint32_t actiondate() const`
>>
>> `inline uint32_t actiontime() const`
>>
>> - 获取结算价, 总持仓量, 增仓量, 总成交额, 成交额, 交易日, 数据发生日期, 数据发生时间
>>
>> `inline double bidprice(int idx) const`
>>
>> `inline double askprice(int idx) const`
>>
>> `inline uint32_t bidqty(int idx) const`
>>
>> `inline uint32_t askqty(int idx) const`
>>
>> 获取指定挡位委买价bid, 委卖价ask, 委买量, 委卖量
>>
>> `inline WTSTickStruct& getTickStruct(){ return m_tickStruct; }`
>>
>> - 返回Tick结构体引用
>>
>> `inline uint64_t getLocalTime() const { return m_uLocalTime; }`
>>
>> - 返回本地时间
>>
### **WTSBarData**
>
> "Includes/WTSDataDef.hpp"
> K线数据封装对象
>
> 派生于通用的基础类,便于传递
>
> 属性和方法
>>
>> `WTSBarStruct m_barStruct;`
>>
>> - Bar结构体
>>
>> `uint16_t m_market;`
>>
>> - 市场编号
>>
>> `std::string m_strCode;`
>>
>> - 合约代码
>>
>> `inline static WTSBarData* create()`
>>
>> `inline static WTSBarData* create(WTSBarStruct& barData, uint16_t market, const char* code)`
>>
>> - 创建Bar数据对象
>>
>> `inline WTSBarStruct& getBarStruct(){return m_barStruct;}`
>>
>> - 获取Bar结构体引用
>>
>> `inline uint16_t getMarket()`
>>
>> - 获取市场编号
>>
>> `inline const char* getCode()`
>>
>> - 获取合约代码
>>
### **WTSOrdQueData**
>
> "Includes/WTSDataDef.hpp"
> 委托队列数据
>
> 属性和方法
>>
>> `WTSOrdQueStruct m_oqStruct;`
>>
>> - 委托队列结构体
>>
>> `static inline WTSOrdQueData* create(const char* code)`
>>
>> `static inline WTSOrdQueData* create(WTSOrdQueStruct& ordQueData)`
>>
>> - 创建委托队列对象
>>
>> `inline WTSOrdQueStruct& getOrdQueStruct(){return m_oqStruct;}`
>>
>> - 获取委托队列结构体
>>
>> `inline const char* exchg() const`
>>
>> `inline const char* code() const`
>>
>> `inline uint32_t tradingdate() const`
>>
>> `inline uint32_t actiondate() const`
>>
>> `inline uint32_t actiontime() const`
>>
>> - 获取交易所代码, 合约代码, 交易日, 自然日, 发生时间(毫秒)
>>
>> `inline void setCode(const char* code)`
>>
>> - 设置合约代码
>>
### **WTSOrdDtlData**
>
> "Includes/WTSDataDef.hpp"
> 逐笔委托数据
>
> 属性和方法
>>
>> `WTSOrdDtlStruct m_odStruct;`
>>
>> - 逐笔委托结构体
>>
>> `static inline WTSOrdDtlData* create(const char* code)`
>>
>> `static inline WTSOrdDtlData* create(WTSOrdDtlStruct& odData)`
>>
>> - 创建逐笔委托对象
>>
>> `inline WTSOrdDtlStruct& getOrdDtlStruct(){ return m_odStruct; }`
>>
>> - 获取逐笔委托结构体
>>
>> `inline const char* exchg() const`
>>
>> `inline const char* code() const`
>>
>> `inline uint32_t tradingdate() const`
>>
>> `inline uint32_t actiondate() const`
>>
>> `inline uint32_t actiontime() const`
>>
>> - 获取交易所代码, 合约代码, 交易日, 自然日, 发生时间(毫秒)
>>
>> `inline void setCode(const char* code)`
>>
>> - 设置合约代码
>>
### **WTSTransData**
>
> "Includes/WTSDataDef.hpp"
> 逐笔成交数据
>
> 属性和方法
>>
>> `WTSTransStruct m_tsStruct;`
>>
>> - 逐笔成交结构体
>>
>> `static inline WTSTransData* create(const char* code)`
>>
>> `static inline WTSTransData* create(WTSTransStruct& transData)`
>>
>> - 设置逐笔成交结构体
>>
>> `inline const char* exchg() const`
>>
>> `inline const char* code() const`
>>
>> `inline uint32_t tradingdate() const`
>>
>> `inline uint32_t actiondate() const`
>>
>> `inline uint32_t actiontime() const`
>>
>> - 获取交易所代码, 合约代码, 交易日, 自然日, 发生时间
>>
>> `inline WTSTransStruct& getTransStruct()`
>>
>> - 获取逐笔成交结构体
>>
>> `inline void setCode(const char* code)`
>>
>> - 设置合约代码
>>
### **WTSHisTickData**
>
> "Includes/WTSDataDef.hpp"
> 历史Tick数据数组
>
> 内部使用WTSArray作为容器
>
> 属性和方法
>>
>> `char m_strCode[32];`
>>
>> - 合约代码
>>
>> `std::vector<WTSTickStruct>> m_ayTicks;`
>>
>> - Tick结构体列表
>>
>> `bool m_bValidOnly;`
>>
>> -
>>
>> `static inline WTSHisTickData* create(const char* stdCode, unsigned int nSize = 0, bool bValidOnly = false)`
>>
>> `static inline WTSHisTickData* create(const char* stdCode, const std::vector<WTSTickStruct>>& ayTicks, bool bValidOnly = false)`
>>
>> - 创建历史Tick数据对象
>>
>> `inline uint32_t size() const`
>>
>> `inline bool empty() const`
>>
>> - 获取数据条数
>>
>> `inline const char* code() const`
>>
>> - 获取合约代码
>>
>> `inline WTSTickStruct* at(uint32_t idx)`
>>
>> - 获取指定位置Tick数据
>>
>> `inline std::vector<WTSTickStruct>>& getDataRef()`
>>
>> - 获取Tick结构体列表
>>
>> `inline void appendTick(const WTSTickStruct& ts)`
>>
>> - 追加一条Tick
>>
### **WTSTickSlice**
>
> "Includes/WTSDataDef.hpp"
> Tick数据切片
>
> 从连续的tick缓存中做的切片, 切片并没有真实的复制内存, 而只是取了开始和结尾的下标, 这样使用虽然更快,但是使用场景要非常小心,因为他依赖于基础数据对象
>
> 属性和方法
>>
>> `char m_strCode[MAX_INSTRUMENT_LENGTH];`
>>
>> - 合约代码
>>
>> `WTSTickStruct* m_ptrBegin;`
>>
>> - Tick结构体
>>
>> `uint32_t m_uCount;`
>>
>> - Tick结构体数量
>>
### **WTSOrdDtlSlice**
>
> "Includes/WTSDataDef.hpp"
> 逐笔委托数据切片
>
> 从连续的逐笔委托缓存中做的切片
>
> 属性和方法(略)
>
### **WTSOrdQueSlice**
>
> "Includes/WTSDataDef.hpp"
> 委托队列数据切片
>
> 从连续的委托队列缓存中做的切片
>
> 属性和方法(略)
>
### **WTSTransSlice**
>
> "Includes/WTSDataDef.hpp"
> 逐笔成交数据切片
>
> 从连续的逐笔成交缓存中做的切片
>
> 属性和方法(略)
>

---

### **WTSMap**

> "Includes/WTSCollection.hpp"
> 平台数组容器
>
> 内部采用std:map实现, 模版类型为key类型, 数据使用WTSObject指针对象, 所有WTSObject的派生类都适用
>
> 变量和方法
>> `std::map<T, WTSObject*> _map;`
>>
>> - 标准库map容器, 键为 T, 值为 WTSObject 指针
>>
>> `static WTSMap<T>* create()`
>>
>> - 创建 map 容器
>>
>> `uint32_t size() const`
>>
>> - 返回容器大小
>>
>> `WTSObject* get(const T &_key)`
>>
>> - 获取指定键的值
>>
>> `WTSObject* operator[](const T &_key)`
>>
>> - 重载[], 用法同get()
>>
>> `WTSObject* grab(const T &_key)`
>>
>> - 获取指定键的值, 引用计数 +1
>>
>> `void add(T _key, WTSObject* obj, bool bAutoRetain = true)`
>>
>> - 新增一个数据, 引用计数 +1
>>
>> `void remove(T _key)`
>>
>> - 根据Key删除一条数据, 引用计数 -1
>>
>> `WTSObject* last()`
>>
>> - 获取最后一条数据
>>
>> `void clear()`
>>
>> - 清空容器
>>
>> `virtual void release()`
>>
>> - 释放数据对象

### **WTSHashMap**

> "Includes/WTSCollection.hpp"
> tsl::robin_map, 快速hash map 和 hash set, 提供快速的插入、查找和删除操作
>
> 内部采用std:map实现, 模版类型为key类型, 数据使用WTSObject指针对象, 所有WTSObject的派生类都适用
>
> 变量和方法
>>
>> `tsl::robin_map<T, WTSObject*> _map;`
>>
>> - 定义数据容器, 快速hash map 和 hash set, 提供快速的插入、查找和删除操作
>>
>> `static WTSHashMap<T>* create()`
>>
>> - 创建 map 容器
>>
>> `uint32_t size() const`
>>
>> - 返回容器大小
>>
>> `WTSObject* get(const T &_key)`
>>
>> - 获取键对应的值
>>
>> `WTSObject* grab(const T &_key)`
>>
>> - 获取键对应的值, 引用计数 +1
>>
>> `void add(const T &_key, WTSObject* obj, bool bAutoRetain = true)`
>>
>> - 新增一条数据, 引用计数 +1
>>
>> `void remove(const T &_key)`
>>
>> - 删除一条数据, 引用计数 -1
>>
>> `void clear()`
>>
>> - 清空容器
>>
>> `virtual void release()`
>>
>> - 释放容器

### **WTSQueue**

> "Includes/WTSCollection.hpp"
> 平台数组容器
>
> 内部采用std:deque实现
>
> 变量和方法
>> `std::deque<WTSObject*> _queue;`
>>
>> - 队列对象, 可存储 WTSObject 指针对象
>>
>> `static WTSQueue* create()`
>>
>> - 创建对象
>>
>> `void pop()`
>>
>> - 移除容器头部的元素
>>
>> `void push(WTSObject* obj, bool bAutoRetain = true)`
>>
>> - 尾部添加元素, 引用计数 +1
>>
>> `WTSObject* front(bool bRetain = true)`
>>
>> - 返回第一个元素的引用, 引用计数 +1
>>
>> `WTSObject* back(bool bRetain = true)`
>>
>> - 返回最后一个元素的引用, 引用计数 +1
>>
>> `uint32_t size() const`
>>
>> - 返回实际元素个数
>>
>> `bool empty()`
>>
>> - 判断容器中是否有元素
>>
>> `void release()`
>>
>> - 释放容器
>>
>> `void clear()`
>>
>> - 清空容器
>>
>> `void swap(WTSQueue* right)`
>>
>> - 交换两个容器的所有元素

### **WTSParams**

> "Includes/WTParams.hpp"
> WT参数对象定义
>
> 属性和方法
>> `ValueHolder`
>>
>> - 共用体(union)属性, 所有的共用体成员共用一个空间，并且同一时间只能储存其中一个成员变量的值
>>
>> `ValueHolder _value;`
>>
>> - 数据值
>>
>> `ValueType _type;`
>>
>> - 数据类型
>>
>> `std::string _desc;`
>>
>> - 数据描述信息字符串
>>
>> `ValueType`
>>
>> - 自定义数据类型
>>
>> `static WTSParams* create(int32_t i32, const char* desc="")`
>>
>> `static WTSParams* create(uint32_t u32, const char* desc="")`
>>
>> `static WTSParams* create(double _real, const char* desc="")`
>>
>> `static WTSParams* create(const char* _string, const char* desc="")`
>>
>> `static WTSParams* create(bool _bool, const char* desc="")`
>>
>> `static WTSParams* createObject(const char* desc="")`
>>
>> `static WTSParams* createArray(const char* desc="")`
>>
>> - 创建 WTSParams 对象
>>
>> `int32_t asInt32() const`
>>
>> `uint32_t asUInt32() const`
>>
>> `double asDouble() const`
>>
>> `std::string asString() const`
>>
>> `const char* asCString() const`
>>
>> `bool asBoolean() const`
>>
>> - 将 _value 值转变为指定类型
>>
>> `WTSParams* get(const char* name) const`
>>
>> `WTSParams* get(const std::string& name) const`
>>
>> `WTSParams* get(uint32_t idx) const`
>>
>> - 获取 _type 为 VT_Object 或 VT_Array 对象的值
>>
>> `int32_t getInt32(const char* name) const`
>>
>> `uint32_t getUInt32(const char* name) const`
>>
>> `double getDouble(const char* name) const`
>>
>> `std::string getString(const char* name) const`
>>
>> `const char* getCString(const char* name) const`
>>
>> `bool getBoolean(const char* name) const`
>>
>> - 将获取的 _type 为 VT_Object 或 VT_Array 对象的值转为指定类型
>>
>> `bool append(const char* _name, const char* _string, const char* desc="")`
>>
>> `bool append(const char* _name, int32_t _i32, const char* desc="")`
>>
>> `bool append(const char* _name, uint32_t _u32, const char* desc="")`
>>
>> `bool append(const char* _name, double _real, const char* desc="")`
>>
>> `bool append(const char* _name, bool _bool, const char* desc="")`
>>
>> `bool append(const char* _name, WTSParams *item, bool bAutoRetain = true)`
>>
>> `bool append(int32_t _i32, const char* desc="")`
>>
>> `bool append(uint32_t _u32, const char* desc="")`
>>
>> `bool append(double _real, const char* desc="")`
>>
>> `bool append(bool _bool, const char* desc="")`
>>
>> `bool append( WTSParams *item, bool bAutoRetain = true)`
>>
>> - 向 _type 为 VT_Object 或 VT_Array 的对象添加元素
>>
>> `uint32_t size() const`
>>
>> - 返回容器大小
>>
>> `MemberNames memberNames() const`
>>
>> - 返回 _type 为 VT_Object 对象的所有键名
>>
>> `virtual void release()`
>>
>> - 释放对象
>>
>> `inline void setDescription(const char* desc)`
>>
>> - 添加描述信息
>>
>> `inline const char* description() const`
>>
>> - 获取描述信息

### **WTSVariant**

> "Includes/WTSVariant.hpp"
> Wt通用变量对象定义
>
> TSVariant是一个通用数据容器,设计目标是Json的Value类和Json不同的地方在于,WTSVariant满足WT系统内的派生关系可以通过引用计数管理数据,从而减少数据复制
>
> 属性和方法
>> `ValueHolder`
>>
>> - 共用体(union)属性, 所有的共用体成员共用一个空间，并且同一时间只能储存其中一个成员变量的值
>>
>> `ValueHolder _value;`
>>
>> - 数据值
>>
>> `ValueType _type;`
>>
>> - 数据类型
>>
>> `ValueType`
>>
>> - 自定义数据类型
>>
>> `typedef WTSArray ChildrenArray;`
>>
>> `typedef WTSHashMap<std::string> ChildrenMap;`
>>
>> `typedef std::vector<std::string> MemberNames;`
>>
>> - 定义成员属性
>>
>> `static WTSVariant* create(int32_t i32)`
>>
>> `static WTSVariant* create(uint32_t u32)`
>>
>> `static WTSVariant* create(int64_t i64)`
>>
>> `static WTSVariant* create(uint64_t u64)`
>>
>> `static WTSVariant* create(double _real)`
>>
>> `static WTSVariant* create(const char* _string)`
>>
>> `static WTSVariant* create(bool _bool)`
>>
>> - 创建不同类型 WTSVariant 对象,_type记录类型,_value以字符串形式保存
>>
>> `static WTSVariant* createObject()`
>>
>> - 创建 VT_Object 对象,_type = VT_Object,_value=ChildrenMap::create();
>>
>> `static WTSVariant* createArray()`
>>
>> - 创建 VT_Array 对象,_type = VT_Array,_value=ChildrenArray::create();
>>
>> `bool has(const char* key)`
>>
>> - 判断 WTSVariant 对象中是否有 key
>>
>> `int32_t asInt32() const`
>>
>> `uint32_t asUInt32() const`
>>
>> `int64_t asInt64() const`
>> `uint64_t asUInt64() const`
>> `double asDouble() const`
>> `std::string asString() const`
>> `const char* asCString() const`
>> `bool asBoolean() const`
>>
>> - 将 _type 为 string类型的数据转为对应类型
>>
>> `WTSVariant* get(const char* name) const`
>>
>> `WTSVariant* get(const std::string& name) const`
>>
>> `WTSVariant* get(uint32_t idx) const`
>>
>> - 如果_type是VT_Object或VT_Array,可以通过get()获取_value值
>>
>> `int32_t getInt32(const char* name) const`
>>
>> `uint32_t getUInt32(const char* name) const`
>>
>> `int64_t getInt64(const char* name) const`
>>
>> `uint64_t getUInt64(const char* name) const`
>>
>> `double getDouble(const char* name) const`
>>
>> `std::string getString(const char* name) const`
>>
>> `const char* getCString(const char* name) const`
>>
>> `bool getBoolean(const char* name) const`
>>
>> - 通过get()获取_value值之后再将值转化为指定类型
>>
>> `bool append(const char* _name, const char* _string)`
>>
>> `bool append(const char* _name, int32_t _i32)`
>>
>> `bool append(const char* _name, uint32_t _u32)`
>>
>> `bool append(const char* _name, int64_t _i64)`
>>
>> `bool append(const char* _name, uint64_t _u64)`
>>
>> `bool append(const char* _name, double _real)`
>>
>> `bool append(const char* _name, bool _bool)`
>>
>> `bool append(const char* _name, WTSVariant *item, bool bAutoRetain = true)`
>>
>> `bool append(const char* _str)`
>>
>> `bool append(int32_t _i32)`
>>
>> `bool append(uint32_t _u32)`
>>
>> `bool append(int64_t _i64)`
>>
>> `bool append(uint64_t _u64)`
>>
>> `bool append(double _real)`
>>
>> `bool append(bool _bool)`
>>
>> `bool append(WTSVariant *item, bool bAutoRetain = true)`
>>
>> - 如果_type是VT_Object或VT_Array,可以通过append()添加元素
>>
>> `uint32_t size() const`
>>
>> - 如果_type是VT_Object或VT_Array,可以通过 size() 获取大小
>>
>> `MemberNames memberNames() const`
>>
>> - 如果_type是VT_Object,可以通过 memberNames() 获取所有键名
>>
>> `virtual void release()`
>>
>> - 释放对象
>>
>> `inline ValueType type() const`
>>
>> - 获取对象类型_type

---

> ### **WTSSessionInfo**
>
> "Includes/WTSSessionInfo.hpp"
> Wt交易时间模板对象定义
>
> 属性和方法
>>
>> `TradingTimes m_tradingTimes`
>>
>> `TradingSection m_auctionTime;`
>>
>> `int32_t m_uOffsetMins;`
>>
>> `std::string m_strID;`
>>
>> `std::string m_strName;`
>>
>> - 交易时间段列表, 集合竞价时间段, 时间偏移, id, name
>>
>> `static WTSSessionInfo* create(const char* sid, const char* name, int32_t offset = 0)`
>>
>> - 创建对象
>>
>> `int32_t getOffsetMins() const`
>>
>> - 获取时间偏移
>>
>> `void addTradingSection(uint32_t sTime, uint32_t eTime)`
>>
>> - 添加交易时间段
>>
>> `void setAuctionTime(uint32_t sTime, uint32_t eTime)`
>>
>> - 设置集合竞价时间段
>>
>> `void setOffsetMins(int32_t offset)`
>>
>> - 设置时间偏移
>>
>> `uint32_t getSectionCount() const`
>>
>> - 获取时间段数量
>>
>> `uint32_t getOffsetDate(uint32_t uDate = 0, uint32_t uTime = 0)`
>>
>> - 计算偏移后的日期, 夜盘偏移日期都是下一日
>>
>> `uint32_t timeToMinutes(uint32_t uTime, bool autoAdjust = false)`
>>
>> `uint32_t minuteToTime(uint32_t uMinutes, bool bHeadFirst = false)`
>>
>> - 分钟和时间互换
>>
>> `uint32_t timeToSeconds(uint32_t uTime)`
>>
>> `uint32_t secondsToTime(uint32_t seconds)`
>>
>> - 时间和秒互换
>>
>> `inline uint32_t getOpenTime(bool bOffseted = false) const`
>>
>> `inline uint32_t getAuctionStartTime(bool bOffseted = false) const`
>>
>> `inline uint32_t getCloseTime(bool bOffseted = false) const`
>>
>> - 获取开盘时间, 集合竞价开始时间,
收盘时间
>>
>> `inline uint32_t getTradingSeconds()`
>>
>> `inline uint32_t getTradingMins()`
>>
>> - 获取交易的秒/分钟数
>>
>> `bool isInTradingTime(uint32_t uTime, bool bStrict = false)`
>>
>> - 是否处于交易时间
>>
>> `inline bool isLastOfSection(uint32_t uTime)`
>>
>> `inline bool isFirstOfSection(uint32_t uTime)`
>>
>> `inline bool isInAuctionTime(uint32_t uTime)`
>>
>> 是否是最后一段交易时间, 是否是第一段交易时间, 是否是集合竞价时间
>>
>> `inline uint32_t offsetTime(uint32_t uTime) const`
>>
>> `inline uint32_t originalTime(uint32_t uTime) const`
>>
>> - 获取偏移时间, 原始时间
>>
### **WTSCommodityInfo**
>
> "Includes/WTSContractInfo.hpp"
> Wt品种信息定义文件
>
> 属性和方法
>
>> `std::string m_strName;`
>>
>> `std::string m_strExchg;`
>>
>> `std::string m_strProduct;`
>>
>> `std::string m_strCurrency;`
>>
>> `std::string m_strSession;`
>>
>> `std::string m_strTrdTpl;`
>>
>> `std::string m_strFullPid;`
>>
>> - 品种名称, 交易所代码, 品种ID, 币种, 交易时间模板, 节假日模板, 全品种ID
>>
>> `uint32_t m_uVolScale;`
>>
>> `double m_fPriceTick;`
>>
>> `uint32_t m_uPrecision;`
>>
>> `uint32_t m_buyQtyUnit;`
>>
>> `uint32_t m_selQtyUnit;`
>>
>> - 合约乘数, 最小价格变动单位, 价格精度
>>
>> `ContractCategory m_ccCategory;`
>>
>> `CoverMode m_coverMode;`
>>
>> `PriceMode m_priceMode;`
>>
>> `TradingMode m_tradeMode;`
>>
>> - 品种分类, 平仓类型, 价格类型, 交易类型
>>
>> `faster_hashset<std::string> m_setCodes;`
>>
>> - 合约代码列表
>>
>> `static WTSCommodityInfo* create(const char* pid, const char* name, const char* exchg, const char* session, const char* trdtpl, const char* currency = "CNY")`
>>
>> - 创建品种信息对象
>>
>> `inline void setVolScale(uint32_t volScale)`
>>
>> `inline void setPriceTick(double pxTick)`
>>
>> `inline void setCategory(ContractCategory cat)`
>>
>> `inline void setCoverMode(CoverMode cm)`
>>
>> `inline void setPriceMode(PriceMode pm)`
>>
>> `inline void setTradingMode(TradingMode tm)`
>>
>> - 设置相关属性
>>
>> `inline bool canShort() const`
>>
>> - 交易类型是否多空都能做
>>
>> `inline bool isT1() const`
>>
>> - 交易类型是否T+1交易
>>
>> `inline void setPrecision(uint32_t prec)`
>>
>> - 设置价格精度
>>
>> `inline const char* getName() const`
>>
>> `inline const char* getExchg() const`
>>
>> `inline const char* getProduct() const`
>>
>> `inline const char* getCurrency() const`
>>
>> `inline const char* getSession() const`
>>
>> `inline const char* getTradingTpl() const`
>>
>> `inline const char* getFullPid() const`
>>
>> `inline uint32_t getVolScale() const`
>>
>> `inline double getPriceTick() const`
>>
>> `inline uint32_t getPrecision() const`
>>
>> `inline ContractCategory getCategoty() const`
>>
>> `inline CoverMode getCoverMode() const`
>>
>> `inline PriceMode getPriceMode() const`
>>
>> `inline TradingMode getTradingMode() const`
>>
>> - 获取相关属性
>>
>> `inline void addCode(const char* code)`
>>
>> - 向合约列表中插入合约代码
>>
>> `inline const faster_hashset<std::string>& getCodes() const`
>>
>> - 获取合约列表
>>
>> `inline bool isOption() const`
>>
>> - 是否是期权合约
>>
>> `inline uint32_t getBuyQtyUnit() const`
>>
>> `inline uint32_t getSellQtyUnit() const`
>>
>> - 获取买入/卖出数量单位
>>
>### **WTSContractInfo**
>
> "Includes/WTSContractInfo.hpp"
> Wt合约信息定义文件
>
> 属性和方法
>>
>> `std::string m_strCode;`
>>
>> `std::string m_strExchg;`
>>
>> `std::string m_strName;`
>>
>> `std::string m_strProduct;`
>>
>> `std::string m_strFullPid;`
>>
>> `std::string m_strFullCode;`
>>
>> - 合约代码, 交易所, 合约名称, 品种ID, 全部品种ID, 全部合约代码
>>
>> `uint32_t m_maxMktQty;`
>>
>> `uint32_t m_maxLmtQty;`
>>
>> - 限仓
>>
>> `static WTSContractInfo* create(const char* code, const char* name, const char* exchg, const char* pid)`
>>
>> - 创建合约信息对象
>>
>> `inline void setVolumeLimits(uint32_t maxMarketVol, uint32_t maxLimitVol)`
>>
>> - 设置限仓属性
>>
>> `...get...()`
>>
>> - 获取其他属性(略)
>>
### **WTSHotItem**
>
> "Includes/WTSHotItem.hpp"
> 主力切换规则对象定义文件
>
> 类属性和方法
>
>> `std::string _exchg;`
>>
>> `std::string _product;`
>>
>> `std::string _hot;`
>>
>> `std::string _from;`
>>
>> `std::string _to;`
>>
>> `uint32_t _dt;`
>>
>> `double _oldclose;`
>>
>> `double _newclose;`
>>
>> - 交易所, 品种名称, 主力合约, 从 from 合约变为 to 合约, 日期, 上一个合约收盘价, 新合约收盘价
>>
>> `static WTSHotItem* create(const char* exchg, const char* product, const char* from, const char* to, uint32_t dt, double oldclose = 0, double newclose = 0)`
>>
>> - 创建主力切换对象
>>

---
---
---

### **IBaseDataMgr**
>
> "Includes/IBaseDataMgr.h"
> 基础数据管理器接口定义
>
> 虚基类
>
> 定义容器
>>
>> `typedef faster_hashset<std::string> ContractSet;`
>>
>> `typedef faster_hashset<uint32_t> HolidaySet;`
>>
>> - 定义合约列表和节假日列表
>
> 属性和方法
>>
>> `virtual WTSCommodityInfo* getCommodity(const char* exchgpid)`
>>
>> `virtual WTSCommodityInfo* getCommodity(const char* exchg, const char* pid)`
>>
>> `virtual WTSCommodityInfo* getCommodity(WTSContractInfo* ct)`
>>
>> - 获取品种信息
>>
>> `virtual WTSContractInfo* getContract(const char* code, const char* exchg = "")`
>>
>> `virtual WTSArray* getContracts(const char* exchg = "")`
>>
>> - 获取合约信息
>>
>> `virtual WTSSessionInfo* getSession(const char* sid)`
>>
>> `virtual WTSSessionInfo* getSessionByCode(const char* code, const char* exchg = "")`
>>
>> `virtual WTSArray* getAllSessions()`
>>
>> - 获取某交易时间段信息和全部时间段信息
>>
>> `virtual bool isHoliday(const char* pid, uint32_t uDate, bool isTpl = false)`
>>
>> - 判断是否时节假日
>>
>> `virtual uint32_t calcTradingDate(const char* stdPID, uint32_t uDate, uint32_t uTime, bool isSession = false)`
>>
>> - 计算准确的回测结束日期
>>
>> `virtual uint64_t getBoundaryTime(const char* stdPID, uint32_t tDate, bool isSession = false, bool isStart = true)`
>>
>> - 获取时间边界
>>
### **WTSBaseDataMgr**
>
> "WTSTools/WTSBaseDataMgr.h"
> 基础数据管理器实现
>
> 属性和方法
>>
>> `TradingDayTplMap m_mapTradingDay;`
>>
>> `SessionCodeMap m_mapSessionCode;`
>>
>> `WTSExchgContract* m_mapExchgContract;`
>>
>> `WTSSessionMap* m_mapSessions;`
>>
>> `WTSCommodityMap* m_mapCommodities;`
>>
>> `WTSContractMap* m_mapContracts;`
>>
>> - 信息映射字典: 节假日, 合约代码列表, 交易所合约, 交易时间段, 品种, 合约
>>
>> `bool loadSessions(const char* filename);`
>>
>> `bool loadCommodities(const char* filename);`
>>
>> `bool loadContracts(const char* filename);`
>>
>> `bool loadHolidays(const char* filename);`
>>
>> - 加载交易时间文件, 品种文件, 合约文件, 节假日文件
>>
>> `uint32_t getTradingDate(const char* stdPID, uint32_t uOffDate = 0, uint32_t uOffMinute = 0, bool isTpl = false);`
>>
>> `uint32_t getNextTDate(const char* stdPID, uint32_t uDate, int days = 1, bool isTpl = false);`
>>
>> `uint32_t getPrevTDate(const char* stdPID, uint32_t uDate, int days = 1, bool isTpl = false);`
>>
>> - 获取交易日, 前一日, 后一日
>>
>> `bool isTradingDate(const char* stdPID, uint32_t uDate, bool isTpl = false);`
>>
>> - 判断是否是交易日
>>
>> `void setTradingDate(const char* stdPID, uint32_t uDate, bool isTpl = false);`
>>
>> - 设置交易日
>>
>> `CodeSet* getSessionComms(const char* sid);`
>>
>> -
>>
>> `const char* getTplIDByPID(const char* stdPID);`
>>
>> -

---

### **IHotMgr**
>
> "Includes/IHotMgr.h"
> 主力合约管理器接口定义
>
> 虚基类
>
> 接口方法
>
>> `virtual const char* getRawCode(const char* exchg, const char* pid, uint32_t dt) = 0;`
>>
>> - 获取主力合约的分月代码
>>
>> `virtual const char* getPrevRawCode(const char* exchg, const char* pid, uint32_t dt) = 0;`
>>
>> - 获取上一个主力合约的分月代码
>>
>> `virtual const char* getHotCode(const char* exchg, const char* rawCode, uint32_t dt) = 0;`
>>
>> - 获取主力合约代码
>>
>> `virtual bool isHot(const char* exchg, const char* rawCode, uint32_t dt) = 0;`
>>
>> - 判断是否是主力合约
>>
>> `virtual bool splitHotSecions(const char* exchg, const char* hotCode, uint32_t sDt, uint32_t eDt, HotSections& sections) = 0;`
>>
>> - 将主力合约在某时段的分月合约全部提取
>>
>> `virtual const char* getSecondRawCode(const char* exchg, const char* pid, uint32_t dt) = 0;`
>>
>> `virtual const char* getPrevSecondRawCode(const char* exchg, const char* pid, uint32_t dt) = 0;`
>>
>> `virtual const char* getSecondCode(const char* exchg, const char* rawCode, uint32_t dt) = 0;`
>>
>> `virtual bool isSecond(const char* exchg, const char* rawCode, uint32_t dt) = 0;`
>>
>> `virtual bool splitSecondSecions(const char* exchg, const char* hotCode, uint32_t sDt, uint32_t eDt, HotSections& sections) = 0;`
>>
>> - 获取此主力合约相关方法(同主力合约)
>>
### **WTSHotMgr**
>
> "WTSTools/WTSHotMgr.h"
> 主力合约管理器实现
>
> 定义容器
>>
>> `typedef WTSMap<uint32_t> WTSDateHotMap;`
>>
>> `typedef WTSMap<std::string> WTSProductHotMap;`
>>
>> `typedef WTSMap<std::string> WTSExchgHotMap;`
>>
>> - 换月主力映射, 品种主力映射, 分市场主力映射
>
> 属性和方法
>>
>> `WTSExchgHotMap* m_pExchgHotMap;`
>>
>> `WTSExchgHotMap* m_pExchgScndMap;`
>>
>> - 分市场主力合约和次主力合约
>>
>> `faster_hashmap<std::string, std::string>> m_curHotCodes;`
>>
>> `faster_hashmap<std::string, std::string>> m_curSecCodes;`
>>
>> - 主力合约代码和次主力合约代码
>>
>> `bool loadHots(const char* filename);`
>>
>> `bool loadSeconds(const char* filename);`
>>
>> - 加载主力合约和次主力合约文件
>>
>> `void getHotCodes(const char* exchg, std::map<std::string, std::string> &mapHots);`
>>
>> - 获取主力合约代码
>>
>> `void WTSHotMgr::release()`
>>
>> - 释放对象
>>

---

### **Document**
>
> RapidJSON 是一个 C++ 的 JSON 解析器及生成器, 只需把 include/rapidjson 目录复制至系统或项目的 include 目录中
>
> [详情参考RapidJSON中文文档](http://rapidjson.org/zh-cn/)

### **jsonToVariant**
>
> "Share/JsonToVariant.hpp"
> rapidjson转WTSVariant的辅助函数
>
> `bool static jsonToVariant(const rj::Value& root, WTSVariant* params)`
>
> - root 解析后的json对象(只能是 Object 或 Array)
>
> - params 将要存放 json 数据的 WTSVariant 对象(只能是 VT_Object 或 VT_Array)
>
### **WTSLogger**

> "WTSTools/WTSLogger.h"
> 日志模块定义
>
> 主要包装了 spdlog/spdlog.h 日志库的接口
>
> `inline spdlog::level::level_enum str_to_level( const char* slvl)`
>
> - 将字符串转为日志级别
>
> `inline void checkDirs(const char* filename)`
>
> - 检查目录, 没有则创建新目录
>
> `inline void format_impl(char* buf, const char* fmt, va_list& args)`
>
> - 格式化输出字符串
>
> `inline void print_timetag(bool bWithSpace = true)`
>
> - 打印当前日期时间
>
> `void WTSLogger::initLogger(const char* catName, WTSVariant* cfgLogger)`
>
> - 初始化日志对象, 根据 logcfg.json 配置
>
> `void WTSLogger::init(const char* propFile /* = "logcfg.json" */, bool isFile /* = true */, ILogHandler* handler /* = NULL */, WTSLogLevel logLevel /* = LL_INFO */)`
>
> - 读取日志文件 logcfg.json, 初始化日志配置
>
> `void WTSLogger::registerHandler(ILogHandler* handler /* = NULL */, WTSLogLevel logLevel /* = LL_ALL */)`
>
> - 注册日志句柄, 设置日志级别
>
> `void WTSLogger::stop()`
>
> - 关闭日志
>
> `void WTSLogger::debug(const char* format, ...)`
>
> `void WTSLogger::info(const char* format, ...)`
>
> `void WTSLogger::warn(const char* format, ...)`
>
> `void WTSLogger::error(const char* format, ...)`
>
> `void WTSLogger::fatal(const char* format, ...)`
>
> - 输出相关级别日志
>
> `void WTSLogger::log(WTSLogLevel ll, const char* format, ...)`
>
> `void WTSLogger::info_imp(SpdLoggerPtr logger, const char* message)`
>
> `void WTSLogger::warn_imp(SpdLoggerPtr logger, const char* message)`
>
> `void WTSLogger::error_imp(SpdLoggerPtr logger, const char* message)`
>
> `void WTSLogger::fatal_imp(SpdLoggerPtr logger, const char* message)`
>
> - 添加不同级别日志句柄
>
> `SpdLoggerPtr WTSLogger::getLogger(const char* logger, const char* pattern /* = "" */)`
>
> - 当作动态日志的处理
>
> `void WTSLogger::freeAllDynLoggers()`
>
> - 删除所有日志

### **EventNotifier**
>
> "WtBtCore/EventNotifier.h"
> UDP广播对象定义
>
> 定义函数指针
>>
>> `typedef unsigned long(*FuncCreateMQServer)(const char*, bool);`
>>
>> `typedef void(*FuncDestroyMQServer)(unsigned long);`
>>
>> `typedef void(*FundPublishMessage)(unsigned long, const char*, const char*, unsigned long);`
>>
>> `typedef void(*FuncLogCallback)(unsigned long, const char*, bool);`
>>
>> `typedef void(*FuncRegCallbacks)(FuncLogCallback);`
>>
>> - 函数指针
>>
>> `std::string m_strURL;`
>>
>> - URL地址
>>
>> `uint32_t _mq_sid;`
>>
>> -
>>
>> `FuncCreateMQServer _creator;`
>>
>> -
>>
>> `FuncDestroyMQServer _remover;`
>>
>> -
>>
>> `FundPublishMessage _publisher;`
>>
>> -
>>
>> `FuncRegCallbacks _register;`
>>
>> -
>>
>> `bool init(WTSVariant* cfg);`
>>
>> - 通过 WtMsgQue.lib 文件初始化UDP通讯组件
>>
>> `void notifyEvent(const char* evtType);`
>>
>> - 通知事件
>>
>> `void notifyData(const char* topic, void* data , uint32_t dataLen);`
>>
>> - 发送数据
>>
>> `void notifyFund(const char* topic, uint32_t uDate, double total_profit, double dynprofit, double dynbalance, double total_fee);`
>>
>> - 发送 Json 数据
>>

### **HisDataReplayerr**

> "WtBtCore/HisDataReplayer.h"
>
> 回调函数
>>
>> - obj 回传用的, 原样返回即可
>>
>> - key 数据缓存的key
>>
>> - bar K线数据
>>
>> - count K线条数
>>
>> - factor 复权因子, 如果前复权, factor为1.0; 如果后复权, factor不为1.0
>>
>> - stdCode 合约代码
>>
>> `typedef void(*FuncReadBars)(void* obj, WTSBarStruct* firstBar, uint32_t count);`
>>
>> `typedef void(*FuncReadTicks)(void* obj, WTSTickStruct* firstTick, uint32_t count);`
>>
>> - 历史数据加载器的回调函数
>>
>> `typedef void(*FuncReadFactors)(void* obj, const char* stdCode, uint32_t* dates, double* factors, uint32_t count);`
>>
>> - 复权因子回调函数
>
> IBtDataLoader
>
> - 加载历史数据
>>
>> `virtual bool loadFinalHisBars(void* obj, const char* stdCode, WTSKlinePeriod period, FuncReadBars cb) = 0;`
>>
>> - 加载最终数据(系统不再加工)
>>
>> `virtual bool loadRawHisBars(void* obj, const char* stdCode, WTSKlinePeriod period, FuncReadBars cb) = 0;`
>>
>> - 加载原始数据
>>
>> `virtual bool loadAllAdjFactors(void* obj, FuncReadFactors cb) = 0;`
>>
>> - 加载全部除权因子
>>
>> `virtual bool loadAdjFactors(void* obj, const char* stdCode, FuncReadFactors cb) = 0;`
>>
>> - 根据合约加载除权因子
>>
>> `virtual bool loadRawHisTicks(void* obj, const char* stdCode, uint32_t uDate, FuncReadTicks cb) = 0;`
>>
>> - 加载历史Tick数据
>
> ## **HisDataReplayer**
>>
>> `IDataSink* _listener;`
>>
>> - 策略管理器接口(继承自 `ICtaStraCtx` 和 `IDataSink`)
>>
>> `IBtDataLoader* _bt_loader;`
>>
>> - 数据加载管理器接口
>>
>> `std::string _stra_name;`
>>
>> -
>>
>> `TickCache _ticks_cache;`
>>
>> `OrdDtlCache _orddtl_cache;`
>>
>> `OrdQueCache _ordque_cache;`
>>
>> `TransCache _trans_cache;`
>>
>> `BarsCache _bars_cache;`
>>
>> `BarsCache _unbars_cache;`
>>
>> - Tick数据, 逐笔订单, 订单队列, 成交订单, K线, 为订阅K线缓存
>>
>> `TaskInfoPtr _task;`
>>
>> - 是否有时间调度任务
>>
>> `std::string _main_key;`
>>
>> - 主K线
>>
>> `std::string _min_period;`
>>
>> - 最小K线周期, 主要用于未订阅品种信号处理
>>
>> `bool _tick_enabled;`
>>
>> - 是否开启了tick回测
>>
>> `bool _tick_simulated;`
>>
>> - 是否需要模拟tick
>>
>> `std::map<std::string, WTSTickStruct> _day_cache;`
>>
>> - 每日Tick缓存, 当tick回放未开放时会用到该缓存
>>
>> `uint32_t _cur_date;`
>>
>> `uint32_t _cur_time;`
>>
>> `uint32_t _cur_secs;`
>>
>> `uint32_t _cur_tdate;`
>>
>> `uint32_t _closed_tdate;`
>>
>> `uint32_t _opened_tdate;`
>>
>> - 当前日期, 时间, 秒, 交易日, 收盘价交易日, 开盘价交易日
>>
>> `WTSBaseDataMgr _bd_mgr;`
>>
>> `WTSHotMgr _hot_mgr;`
>>
>> - 基础数据管理, 主力合约管理
>>
>> `uint64_t _begin_time;`
>>
>> `uint64_t _end_time;`
>>
>> - 数据文件夹, 模式, 起始时间, 结束时间
>>
>> `bool _running;`
>>
>> `bool _terminated;`
>>
>> - 开始回测标志, 控制台终止标值
>>
>> `typedef struct _FeeItem`
>>
>> - 手续费模板
>>
>> `typedef faster_hashmap<std::string, double> PriceMap;`
>>
>> `AdjFactorMap _adj_factors;`
>>
>> - 除权因子列表
>>
>> `const AdjFactorList& getAdjFactors(const char* code, const char* exchg, const char* pid);`
>>
>> - 获取除权因子列表
>>
>> `bool init(WTSVariant* cfg, EventNotifier* notifier = NULL, IBtDataLoader* dataLoader = NULL);`
>>
>> - cfg: 配置变量, notifier: 事件消息, dataLoader: 加载历史数据
>> - 1. 获取回测模式和文件路径
>> - 2. 是确定回测区间
>> - 3. 是否是tick级别回测
>> - 4. 获取基础文件json对象
>>
>> 基础数据管理器 _bd_mgr
>>
>> - 5. 加载交易时间段配置文件
>> - 6. 加载商品信息配置文件
>> - 7. 加载合约信息配置文件
>> - 8. 加载节假日配置文件
>>
>> 主力合约管理器 _hot_mgr
>>
>> - 9. 加载主力合约配置文件
>> - 10. 加载次主力合约配置文件
>>
>> 当前类方法
>>
>> - 11. 加载手续费配置文件
>> - 12. 加载除权因子配置文件
>>
>> `bool loadStkAdjFactorsFromFile(const char* adjfile);`
>>
>> `bool loadStkAdjFactorsFromLoader();`
>>
>> - 加载除权因子数据
>>
>> `void register_task(uint32_t taskid, uint32_t date, uint32_t time, const char* period, const char* trdtpl = "CHINA", const char* session = "TRADING");`
>>
>> - 定时调度任务
>>
>> `void clear_cache();`
>>
>> - 清理数据缓存
>>
>> `void reset();`
>>
>> - 重置数据, 不清理缓存, 避免重复加载
>>
>> `void dump_btstate(const char* stdCode, WTSKlinePeriod period, uint32_t times, uint64_t stime, uint64_t etime, double progress, int64_t elapse);`
>>
>> - 保存状态数据到文件
>>
>> `void notify_state(const char* stdCode, WTSKlinePeriod period, uint32_t times, uint64_t stime, uint64_t etime, double progress);`
>>
>> - 消息通知状态
>>
>> `uint32_t locate_barindex(const std::string& key, uint64_t curTime, bool bUpperBound = false);`
>>
>> - 定位bar数据索引
>>
>> `bool prepare();`
>>
>> - 准备回测
>>
>> `void stop();`
>>
>> - 停止回测
>>
>> `void run(bool bNeedDump = false);`
>>
>> - 开始回测
>>
>> - 1. 如果没有定时调度任务
>>      - 1. 确定主K线
>>      - 2. 如果订阅了K线, 按照主K线回放数据
>>      - 3. 如果允许tick回测, 按照tick数据回放
>> - 2. 如果有调度任务, 运行调度任务
>>
>> `void run_by_ticks(bool bNeedDump = false);`
>>
>> - tick级回测
>>
>> - 略
>>
>> `void run_by_bars(bool bNeedDump = false);`
>>
>> - bar回放
>>
>> `void run_by_tasks(bool bNeedDump = false);`
>>
>> - 定时任务
>>
>> `class HftDataList`
>>
>> - 高频数据列表类
>>
>> `typedef faster_hashmap<std::string, HftDataList<WTSTickStruct>> TickCache;`
>>
>> `typedef faster_hashmap<std::string, HftDataList<WTSOrdDtlStruct>> OrdDtlCache;`
>>
>> `typedef faster_hashmap<std::string, HftDataList<WTSOrdQueStruct>> OrdQueCache;`
>>
>> `typedef faster_hashmap<std::string, HftDataList<WTSTransStruct>> TransCache;`
>>
>> - 高频数据Tick数据, 逐笔委托单, 委托单队列, 成交单缓存
>>
>> `typedef struct _BarsList`
>>
>> - Bars数据列表
>>
>> `typedef faster_hashmap<std::string, BarsList> BarsCache;`
>>
>> - Bars数据列表缓存
>>
>> `bool cacheRawBarsFromBin(const std::string& key, const char* stdCode, WTSKlinePeriod period, bool bForBars = true);`
>>
>> - 从自定义数据文件缓存历史数据
>>
>> `bool cacheRawBarsFromCSV(const std::string& key, const char* stdCode, WTSKlinePeriod period, bool bSubbed = true);`
>>
>> - 从 csv 文件缓存历史数据
>>
>> `bool cacheRawTicksFromBin(const std::string& key, const char* stdCode, uint32_t uDate);`
>>
>> - 从自定义数据文件缓存历史tick数据
>>
>> `bool cacheRawTicksFromCSV(const std::string& key, const char* stdCode, uint32_t uDate);`
>>
>> - 从csv文件缓存历史tick数据
>>
>> `bool cacheFinalBarsFromLoader(const std::string& key, const char* stdCode, WTSKlinePeriod period, bool bSubbed = true);`
>>
>> `bool cacheRawTicksFromLoader(const std::string& key, const char* stdCode, uint32_t uDate);`
>>
>> - 从外部加载器缓存历史/tick数据
>>
>> `bool cacheIntegratedFutBars(const std::string& key, const char* stdCode, WTSKlinePeriod period, bool bSubbed = true);`
>>
>> - 缓存整合的期货合约历史K线(针对.HOT//2ND)
>>
>> `bool cacheAdjustedStkBars(const std::string& key, const char* stdCode, WTSKlinePeriod period, bool bSubbed = true);`
>>
>> - 缓存复权股票K线数据
>>
>> `void onMinuteEnd(uint32_t uDate, uint32_t uTime, uint32_t endTDate = 0, bool tickSimulated = true);`
>>
>> -
>>
>> `void loadFees(const char* filename);`
>>
>> - 加载手续费文件
>>
>> `bool replayHftDatas(uint64_t stime, uint64_t etime);`
>>
>> `uint64_t replayHftDatasByDay(uint32_t curTDate);`
>>
>> `void replayUnbars(uint64_t stime, uint64_t etime, uint32_t endTDate = 0);`
>>
>> -
>>
>> `inline bool checkTicks(const char* stdCode, uint32_t uDate);`
>>
>> `inline bool checkOrderDetails(const char* stdCode, uint32_t uDate);`
>>
>> `inline bool checkOrderQueues(const char* stdCode, uint32_t uDate);`
>>
>> `inline bool checkTransactions(const char* stdCode, uint32_t uDate);`
>>
>> `void checkUnbars();`
>>
>> -
>>
>> `bool checkAllTicks(uint32_t uDate);`
>>
>> -
>>
>> `inline uint64_t getNextTickTime(uint32_t curTDate, uint64_t stime = UINT64_MAX);`
>>
>> `inline uint64_t getNextOrdQueTime(uint32_t curTDate, uint64_t stime = UINT64_MAX);`
>>
>> `inline uint64_t getNextOrdDtlTime(uint32_t curTDate, uint64_t stime = UINT64_MAX);`
>>
>> `inline uint64_t getNextTransTime(uint32_t curTDate, uint64_t stime = UINT64_MAX);`
>>
>> -
>>
>> `inline void set_time_range(uint64_t stime, uint64_t etime)`
>>
>> - 设置数据起始和结束时间
>>
>> `inline void enable_tick(bool bEnabled = true)`
>>
>> - 是否使用ticks数据
>>
>> `inline void register_sink(IDataSink* listener, const char* sinkName)`
>>
>> - 策略管理器**策略实例对象在策略管理器中**和策略管理器名称
>>
>> `WTSKlineSlice* get_kline_slice(const char* stdCode, const char* period, uint32_t count, uint32_t times = 1, bool isMain = false);`
>>
>> `WTSTickSlice* get_tick_slice(const char* stdCode, uint32_t count, uint64_t etime = 0);`
>>
>> `WTSOrdDtlSlice* get_order_detail_slice(const char* stdCode, uint32_t count, uint64_t etime = 0);`
>>
>> `WTSOrdQueSlice* get_order_queue_slice(const char* stdCode, uint32_t count, uint64_t etime = 0);`
>>
>> `WTSTransSlice* get_transaction_slice(const char* stdCode, uint32_t count, uint64_t etime = 0);`
>>
>> - 获取切片数据
>>
>> `WTSTickData* get_last_tick(const char* stdCode);`
>>
>> -
>>
>> `uint32_t get_date() const`
>>
>> `uint32_t get_min_time() const`
>>
>> `uint32_t get_raw_time() const`
>>
>> `uint32_t get_secs() const`
>>
>> `uint32_t get_trading_date() const`
>>
>> -
>>
>> `double calc_fee(const char* stdCode, double price, double qty, uint32_t offset);`
>>
>> - 计算手续费
>>
>> `get_session_info(const char* sid, bool isCode = false);`
>>
>> `get_commodity_info(const char* stdCode);`
>>
>> `double get_cur_price(const char* stdCode);`
>>
>> - 获取交易时间, 合约信息, 品种价格
>>
>> `void sub_tick(uint32_t sid, const char* stdCode);`
>>
>> `void sub_order_queue(uint32_t sid, const char* stdCode);`
>>
>> `void sub_order_detail(uint32_t sid, const char* stdCode);`
>>
>> `void sub_transaction(uint32_t sid, const char* stdCode);`
>>
>> -
>>
>> `inline bool is_tick_enabled() const{ return _tick_enabled; }`
>>
>> - 返回是否使用tick数据标值
>>
>> `inline bool is_tick_simulated() const`
>>
>> - 返回是否使用模拟tick
>>
>> `inline void update_price(const char* stdCode, double price)`
>>
>> - 更新指定品种价格
>>

---
---
---

## **策略相关类**
>
### **ICtaStraCtx**
>
> "Includes/ICtaStraCtx.h"
>
> CTA策略管理器接口(报错下单, 获取持仓等)
>
> 接口的实现在"WtBtCore/CtaMocker.h"
>
>> `typedef std::function<void(const char*, double)> FuncEnumCtaPosCallBack;`
>>
>> - 回调函数
>>
### **IDataSink**
>
> "WtBtCore/HisDataReplayer.h"
> 策略回调函数接口
>
> 方法
>>
>> `virtual void handle_tick(const char* stdCode, WTSTickData* curTick)`
>>
>> `virtual void handle_order_queue(const char* stdCode, WTSOrdQueData* curOrdQue)`
>>
>> `virtual void handle_order_detail(const char* stdCode, WTSOrdDtlData* curOrdDtl)`
>>
>> `virtual void handle_transaction(const char* stdCode, WTSTransData* curTrans)`
>>
>> `virtual void handle_bar_close(const char* stdCode, const char* period, uint32_t times, WTSBarStruct* newBar)`
>>
>> `virtual void handle_schedule(uint32_t uDate, uint32_t uTime)`
>>
>> `virtual void handle_init()`
>>
>> `virtual void handle_session_begin(uint32_t curTDate)`
>>
>> `virtual void handle_session_end(uint32_t curTDate)`
>>
>> `virtual void handle_replay_done()`
>>
>> -
>>
### **CtaMocker**
>
> "WtBtCore/CtaMocker.h"
> 策略数据处理接口, 继承自 `ICtaStraCtx` 和 `IDataSink`
>
> 属性和方法
>
>> `uint32_t _context_id;`
>>
>> -
>>
>> `HisDataReplayer* _replayer;`
>>
>> - 历史数据回放对象
>>
>> `uint64_t _total_calc_time;`
>>
>> - 总计算时间
>>
>> `uint32_t _emit_times;`
>>
>> - 总计算次数
>>
>> `int32_t _slippage;`
>>
>> - 成交滑点
>>
>> `uint32_t _schedule_times;`
>>
>> - 调度次数
>>
>> `std::string _main_key;`
>>
>> -
>>
>> `KlineTags`
>>
>> - K线标签
>>
>> `PriceMap`
>>
>> - 记录不同品种最新价
>>
>> `DetailInfo`
>>
>> - 细节信息
>>
>> `_pos_map`
>>
>> - 持仓信息
>>
>> `_total_closeprofit`
>>
>> -
>>
>> `_sig_map`
>>
>> -
>>
>> `std::stringstream _trade_logs;`
>>
>> `std::stringstream _close_logs;`
>>
>> `std::stringstream _fund_logs;`
>>
>> `std::stringstream _sig_logs;`
>>
>> - 交易数据, 收盘价数据, 资金数据, 信号数据
>>
>> `CondEntrustMap _condtions;`
>>
>> -
>>
>> `bool _is_in_schedule;`
>>
>> - 是否处于调度中
>>
>> `StringHashMap _user_datas;`
>>
>> `bool _ud_modified;`
>>
>> - 用户数据; 是否修改用户数据
>>
>> `StraFundInfo _fund_info;`
>>
>> - 利润和手续费详情
>>
>> `StraFactInfo _factory;`
>>
>> - 策略工厂对象
>>
>> `CtaStrategy* _strategy;`
>>
>> - 策略对象
>>
>> `EventNotifier* _notifier;`
>>
>> - 事件通知
>>
>> `StdUniqueMutex _mtx_calc;`
>>
>> -
>>
>> `StdCondVariable _cond_calc;`
>>
>> -
>>
>> `bool _has_hook;`
>>
>> - 人为控制是否启用钩子
>>
>> `bool _hook_valid;`
>>
>> - 根据是否是异步回测确定钩子是否可用
>>
>> `std::atomic<uint32_t> _cur_step;`
>>
>> - 控制状态临时变量
>>
>> `bool _in_backtest;`
>>
>> -
>>
>> `bool _wait_calc;`
>>
>> -
>>
>> `bool _persist_data;`
>>
>> -
>>
>> `CtaMocker::_name`
>>
>> - 策略管理器名称("cta")
>>
>> `void CtaMocker::dump_outputs()`
>>
>> 输出结果保存到本地
>>
>> `void CtaMocker::log_signal`
>>
>> - 记录信号信息
>>
>> `void CtaMocker::log_trade`
>>
>> - 记录交易信息
>>
>>
>> `void CtaMocker::log_close`
>>
>> - 记录收盘信息
>>
>> `bool CtaMocker::init_cta_factory(WTSVariant* cfg)`
>>
>> - 根据配置信息初始化策略工厂(包括加载策略dll文件, 创建策略对象)
>>
>> `void CtaMocker::handle_init() {this->on_init();}`
>>
>> **IDataSink父类下的回调函数会调用相应的子类方法**
>>
>> `void CtaMocker::on_bar(const char* stdCode, const char* period, uint32_t times, WTSBarStruct* newBar)`
>>
>> - 先判断是否出现新K线, 若有则调用策略中的 `_strategy->on_bar(this, code, period, newBar);` 函数
>>
>> `void CtaMocker::on_init()`
>>
>> - 将回测标识设为true, 然后调用 `_strategy->on_init(this);`
>>
>> `void CtaMocker::update_dyn_profit(const char* stdCode, double price)`
>>
>> - 计算并更新损益相关数据
>>
>> `void CtaMocker::on_tick(const char* stdCode, WTSTickData* newTick, bool bEmitStrategy /* = true */)`
>>
>> - 检查信号与条件单逻辑, 然后执行策略中的 `on_tick_updated(stdCode, newTick);` 函数
>>
>> `void CtaMocker::on_bar_close(const char* code, const char* period, WTSBarStruct* newBar)`
>>
>> - 执行策略中的 on_bar
>>
>>
>> `void CtaMocker::install_hook()`
>>
>> `void CtaMocker::enable_hook(bool bEnabled /* = true */)`
>>
>> -
>>
>> `bool CtaMocker::step_calc()`
>>
>> - 执行计算步骤
>>
>> `bool CtaMocker::on_schedule(uint32_t curDate, uint32_t curTime)`
>>
>> - K 线调度, 并执行策略中的 `_strategy->on_schedule(this, curDate, curTime);`
>>
>> `void CtaMocker::on_session_begin(uint32_t curTDate)`
>>
>> - 每个交易日开始
>>
>> `void CtaMocker::enum_position(FuncEnumCtaPosCallBack cb)`
>>
>> - 枚举所有持仓数据
>>
>> **策略接口**
>>
>> `void CtaMocker::stra_enter_long`
>>
>> `void CtaMocker::stra_enter_short`
>>
>> `void CtaMocker::stra_exit_long`
>>
>> `void CtaMocker::stra_exit_short`
>>
>> - 多开, 空开, 多平, 空平
>>
>> `double CtaMocker::stra_get_price(const char* stdCode)`
>>
>> - 获取当前价格
>>
>> `void CtaMocker::stra_set_position`
>>
>> - 通过设置添加交易信号
>>
>> `void CtaMocker::append_signal`
>>
>> - 添加信号数据
>>
>> `void CtaMocker::do_set_position`
>>
>> - 设置目标仓位, 改变持仓
>>
>> `WTSKlineSlice* CtaMocker::stra_get_bars(const char* stdCode, const char* period, uint32_t count, bool isMain /* = false */)`
>>
>> - 获取切片 bar 数据
>>
>> `WTSTickSlice* CtaMocker::stra_get_ticks(const char* stdCode, uint32_t count)`
>>
>> - 获取tick数据切片
>>
>> `WTSTickData* CtaMocker::stra_get_last_tick(const char* stdCode)`
>>
>> - 获取最新的tick数据
>>
>> `void CtaMocker::stra_sub_ticks(const char* code)`
>>
>> -
>>
>> `WTSCommodityInfo* CtaMocker::stra_get_comminfo(const char* stdCode)`
>>
>> - 获取品种信息
>>
>> `uint32_t CtaMocker::stra_get_tdate()`
>>
>> - 获取交易日期
>>
>> `uint32_t CtaMocker::stra_get_date()`
>>
>> - 获取当前日期
>>
>> `uint32_t CtaMocker::stra_get_time()`
>>
>> - 获取当前时间
>>
>> `double CtaMocker::stra_get_fund_data(int flag)`
>>
>> - 获取资金信息
>>
>> `void CtaMocker::stra_log_info`
>>
>> `void CtaMocker::stra_log_debug`
>>
>> `void CtaMocker::stra_log_error`
>>
>> - 输出日志信息
>>
>> `const char* CtaMocker::stra_load_user_data`
>>
>> `加载用户数据`
>>
>> `uint64_t CtaMocker::stra_get_first_entertime`
>>
>> - 获取首次入场时间
>>
>> `uint64_t CtaMocker::stra_get_last_entertime`
>>
>> - 获取最后一次入场时间
>>
>> `uint64_t CtaMocker::stra_get_last_exittime`
>>
>> - 获取最后一次离场时间
>>
>> `double CtaMocker::stra_get_last_enterprice`
>>
>> - 获取最后一次离场价格
>>
>> `double CtaMocker::stra_get_position`
>>
>> - 获取持仓
>>
>> `double CtaMocker::stra_get_position_avgpx(const char* stdCode)`
>>
>> - 获取持仓均价
>>
>> `double CtaMocker::stra_get_position_profit(const char* stdCode)`
>>
>> - 获取持仓利润
>>
>> `uint64_t CtaMocker::stra_get_detail_entertime(const char* stdCode, const char* userTag)`
>>
>> - 获取某笔持仓时间
>>
>> `double CtaMocker::stra_get_detail_cost`
>>
>> - 获取某笔持仓价格
>>
>> `double CtaMocker::stra_get_detail_profit`
>>
>> 获取某笔持仓利润
>>
### **HftMocker**
>
> "WtBtCore/HftMocker.h"
>
### **SelMocker**
>
> "WtBtCore/SelMocker.h"
>
### **ExecMocker**
>
> "WtBtCore/ExecMocker.h"
>
>>

### **CtaStrategy**

> CTA策略基类, 虚基类, 只有接口没有实现
>
> 属性和方法
>
>> `std::string _id;`
>>
>> - 策略ID
>>
>> `virtual const char* id() const`
>>
>> - 获取策略ID
>>
>> `virtual const char* getName()`
>>
>> - 执行单元名称
>>
>> `virtual const char* getFactName()`
>>
>> - 所属执行器工厂名称
>>
>> `virtual bool init(WTSVariant* cfg)`
>>
>> - 参数初始化
>>
>> `virtual void on_init(ICtaStraCtx* ctx)`
>>
>> - 数据管理器回调
>>
>> `virtual void on_session_begin(ICtaStraCtx* ctx, uint32_t uTDate)`
>>
>> - 交易日开始
>>
>> `virtual void on_session_end(ICtaStraCtx* ctx, uint32_t uTDate)`
>>
>> - 交易日结束
>>
>> `virtual void on_schedule(ICtaStraCtx* ctx, uint32_t uDate, uint32_t uTime)`
>>
>> - 主体逻辑执行入口
>>
>> `virtual void on_schedule_done(ICtaStraCtx* ctx, uint32_t uDate, uint32_t uTime)`
>>
>> - 主体逻辑执行完成
>>
>> `virtual void on_tick(ICtaStraCtx* ctx, const char* stdCode, WTSTickData* newTick)`
>>
>> - tick数据
>>
>> `virtual void on_bar(ICtaStraCtx* ctx, const char* stdCode, const char* period, WTSBarStruct* newBar)`
>>
>> - K线闭合
>>
>>
### **ICtaStrategyFact**
>
> 策略工厂接口, 虚基类
>
> ### **函数指针接口**
>
>> `FuncEnumStrategyCallback`
>>
>> 策略工厂接口
>>
>> `FuncCreateStraFact`
>>
>> - 创建执行工厂
>>
>> `FuncDeleteStraFact`
>>
>> - 删除执行工厂
>>
> 属性和方法
>>
>> `virtual const char* getName()`
>>
>> - 获取工厂名
>>
>> `virtual void enumStrategy(FuncEnumStrategyCallback cb`
>>
>> - 枚举策略
>>
>> `virtual CtaStrategy* createStrategy(const char* name, const char* id)`
>>
>> - 根据名称创建K线级别策略
>>
>> `virtual bool deleteStrategy(CtaStrategy* stra)`
>>
>> - 删除策略
>>
### **WtStraFact**
>
> 策略工厂实例化
>
>> `virtual const char* getName()`
>>
>> - 获取工厂名称
>>
>> `virtual CtaStrategy* createStrategy(const char* name, const char* id)`
>>
>> - 创建策略
>>
>> `virtual void enumStrategy(FuncEnumStrategyCallback cb)`
>>
>> - 策略枚举
>>
>> `virtual bool deleteStrategy(CtaStrategy* stra)`
>>
>> - 删除策略
>>
### **WtStraDualThrust**
>
> DualThrust策略类
>
> 属性和方法
>>
>> `double _k1;`
>>
>> `double _k2;`
>>
>> `uint32_t _days;`
>>
>> - 指标参数
>>
>> `std::string _period;`
>>
>> - 数据周期
>>
>> `uint32_t _count;`
>>
>> - K线条数
>>
>> `std::string _code;`
>>
>> - 合约代码
>>
>> `bool _isstk;`
>>
>> - 是否是股票
>>
>> `virtual const char* getFactName() override;`
>>
>> - 获取策略工厂名称
>>
>> `virtual const char* getName() override;`
>>
>> - 获取策略名称
>>
>> `virtual bool init(WTSVariant* cfg) override;`
>>
>> - 所有参数初始化, **策略参数从配置文件中传入**
>>
>> `virtual void on_schedule(ICtaStraCtx* ctx, uint32_t curDate, uint32_t curTime) override;`
>>
>> - 全部K线闭合触发, 主逻辑入口
>>   - 1. 判断回测品种是否是股票, 若是股票, 代码后需要添加"Q"标识
>>   - 2. 利用 ctx 获取K线切片指针对象 kline
>>   - 3. 判断 kline 是否有数据
>>   - 4. 设置交易单元, 默认1, 股票 100
>>   - 5. 利用 kline 获取 (-_days, -2) 天内的最高价和最低价, **最新的K线默认下标是 -1**
>>   - 6. 利用 kline->extractData(KFT_CLOSE) 获取收盘价数据指针 closes, 然后获取 (-_days, -2) 天内收盘价的最大值和最小值
>>   - 7. 释放指针 closes->release()
>>   - 8. 获取最后一根K线开盘价, 最高价, 最低价
>>   - 9. 计算上轨和下轨
>>   - 10. 利用 ctx 获取合约信息对象
>>   - 11. 利用 ctx 获取当前持仓
>>   - 12. 浮点数比较持仓数据 `decimal::eq(curPos,0)`
>>   - 13. 利用 ctx 执行交易动作
>>     - 入场做多: stra_enter_long
>>     - 入场做空: stra_enter_short
>>     - 离场平多: stra_exit_long
>>     - 离场平空: stra_exit_short
>>   - 14. 保存用户数据 `ctx->stra_save_user_data`
>>   - 15. 释放指针 `kline->release();`
>>
>> `virtual void on_init(ICtaStraCtx* ctx) override;`
>>
>> - 策略数据管理器初始化
>> - 1. 判断若回测品种是股票则代码加后缀"Q"
>> - 2. 判断是否可以正常获取K线切片数据, 并牢记释放指针
>>
>> `virtual void on_tick(ICtaStraCtx* ctx, const char* stdCode, WTSTickData* newTick) override;`
>>
>> - 不用处理
>>