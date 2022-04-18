# 指标计算的中间结果和最终结果

***All of the calculation would be processed in PYTHON codes, MYSQL only has result data and staging data***

> 算法理念： 模型是基于周期内的真实的交易成本，结合完全换手周期来建立起来的。其中，周期内的真实的交易成本就是成交额除以成交量的数值; 完全换手周期就是累计换手率超过100的天数。
``` python
周期内真实交易成本 real_price = sum(amount)/sum(vol)
完全换手周期（天）full_turnover_period = when sum(turnover_rate) > 100
```

# 需要课题
- P2 完全换手率的周期哪一个更好？ 需要比较100,161.8,200，不急，先按100的来进行预测，以后再微调
- P1 [课题1： 完全换手的必要性](完全换手的必要性)，比较数值的差别和趋势，看看哪一个更合理
  - 5日成本价
  - 20日成本价
  - 完全换手成本价
- P1 完全换手率的周期内的真实交易成本real_price_full_turnover相关的各种指标， 比值应该比差值更好：
  - 每天平均交易成本价格 real_price_daily / real_price_full_turnover
  - close / real_price_daily
  - close / real_price_full_turnover
  - 收盘价 / 5日成本价
  - 收盘价 / 20日成本价
  - 收盘价 / 完全换手成本价

- 判别涨跌指标，+1, +3, +5的涨幅，需要进一步比较出来哪一个指标更好，更准：
  - 明日涨幅
  - 3日涨幅
  - 5日涨幅


## 课题1： 完全换手的必要性
### 相关指标


### 数据库
``` sql

`vol` 成交量（手）
`amount` 成交量（万？千？）

 real_daily_price = `amount` / `vol` * 10

`close_next` float DEFAULT NULL COMMENT '明天收价',
`close_next5` float DEFAULT NULL COMMENT '明5收价',
`pct_chg` float DEFAULT NULL COMMENT '涨跌幅',
`pct_chg5` float DEFAULT NULL COMMENT '5日涨跌幅',


`index_start` INT NULL,
`trade_date_start` DATE NULL,
`periods` INT NULL COMMENT '完全换手的周期的天数',
`high_max` FLOAT NULL,
`low_min` FLOAT NULL,
`vol_sum` INT,
`amount_sum` INT,
```

## [talib 的指标](./talib)

* [Overlap Studies 重叠研究](func_groups/overlap_studies.md)
各种移动平均MA的计算，计划： 以完全换手的周期来MA（RealPrice,ClosedPrice，high,low,close）
* [Momentum Indicators 动量指标](func_groups/momentum_indicators.md)
useless
趋势判断的指标，MACD  
* [Volume Indicators 成交量指标](func_groups/volume_indicators.md)
  * 函数名：AD
* [Volatility Indicators 波动性指标](func_groups/volatility_indicators.md)
useless
以 N 天的指数移动平均数平均後的交易波动幅度
* [Price Transform 价格指标](func_groups/price_transform.md)
useless
中位数价格 。。。
* [Cycle Indicators 周期指标](func_groups/cycle_indicators.md)
值得研究一下
* [Pattern Recognition 形态识别](func_groups/pattern_recognition.md)
useless
K线的形态
* [Statistic Functions 统计函数](func_groups/statistic_functions.md)
useless
线性回归函数
* [Math Transform 数学变换](func_groups/math_transform.md)
useless
三角函数
* [Math Operators 数学运算符](func_groups/math_operators.md)
useless
向量的加减乘除

## 有用的指标
### MA
以完全换手的周期来MA（RealPrice,high,low,close）
> real = MA(close, timeperiod=30, matype=0)
### AD
> real = AD(high, low, close, volume)
### [Cycle Indicators 周期指标](func_groups/cycle_indicators.md)
> real = HT_DCPERIOD(close)
> real = HT_DCPHASE(close)
> inphase, quadrature = HT_PHASOR(close)
> sine, leadsine = HT_SINE(close)
> integer = HT_TRENDMODE(close)
