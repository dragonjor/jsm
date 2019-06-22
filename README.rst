
Overview
========

这是用来取得日本股票市场的股价，财务数据的工具。

各种数据是从yahoo财经<http://finance.yahoo.co.jp/>上爬虫爬下来的。

环境：Python2.5 - 2.7

Installation
============

从PyPI来安装很简单:

  easy_install jsm

Usage
=====

jsm.Quotesの安装:

  import jsm
  q = jsm.Quotes()

安装完后，可以获得他的各种数据。

获得当天的股价（例子里是Yahoo!指定的証券码）:

  q.get_price(4689)

获得历史股价数据:

  q.get_historical_prices(4689)

默认获得当日开始过去1个月的日线数据。

获得週間, 月間的股价:

  q.get_historical_prices(4689, jsm.WEEKLY) # 获得週間股价
  q.get_historical_prices(4689, jsm.MONTHLY) # 获得月間股价

获得指定时期的股价数据:

  import datetime
  start_date = datetime.date(2011, 1, 1)
  end_date = datetime.date(2011, 2, 1)
  q.get_historical_prices(4689, jsm.DAILY, start_date, end_date) # 2011/1/1 〜 2011/2/1的股价

2000/1/1至今的股价:

  q.get_historical_prices(4689, jsm.DAILY, all=True)

获得股价的方法（method）（get_price, get_historical_prices）的返回值是、PriceData（class）的实例（instance）。

关于PriceData、请参照以下的Data項。

jsm是将获取的股价用CSV形式保存的class。

用CSV形式保存:

  c = jsm.QuotesCsv()
  c.save_price(4689)
  c.save_historical_prices(4689) # 和get_historical_prices一样的使用方法

为了获得对象的财务情报，请使用以下的method

获得财务情报:

  q.get_finance(4689)

获得财务情报的method（get_finance）的返回值是、class FinanceData的instance。

关于FinanceData、请参照以下的Data項。

还有就是，jsm也可以获得按行业分的銘柄情報。

为了获得按行业分的銘柄情報，请使用以下的method。
# 銘柄情報：郭理解为会影响股价和股数的因素

获得按行业分的銘柄情報:

  q.get_brand() # 默认：所有行业

使用以下的ID可以获得不同行业的銘柄情報:
#不翻了2333郭应该用不上
  '0050' # 農林・水産業
  '1050' # 鉱業
  '2050' # 建設業
  '3050' # 食料品
  '3100' # 繊維製品
  '3150' # パルプ・紙
  '3200' # 化学
  '3250' # 医薬品
  '3300' # 石油・石炭製品
  '3350' # ゴム製品
  '3400' # ガラス・土石製品
  '3450' # 鉄鋼
  '3500' # 非鉄金属
  '3550' # 金属製品
  '3600' # 機械
  '3650' # 電気機器
  '3700' # 輸送機器
  '3750' # 精密機器
  '3800' # その他製品
  '4050' # 電気・ガス業
  '5050' # 陸運業
  '5100' # 海運業
  '5150' # 空運業
  '5200' # 倉庫・運輸関連業
  '5250' # 情報・通信
  '6050' # 卸売業
  '6100' # 小売業
  '7050' # 銀行業
  '7100' # 証券業
  '7150' # 保険業
  '7200' # その他金融業
  '8050' # 不動産業
  '9050' # サービス業

获得銘柄情報的method（get_brand）的返回值是、class BrandData的instance。

关于BrandData、请参考以下的Data項。

Data
====

PriceData::

  date      # 日時
  open      # 初値
  high      # 高値
  low       # 低値
  close     # 終値
  volume    # 出来高

FinanceData::

  market_cap        # 時価総額     #股票数*当前单价
  shares_issued     # 発行済株式数 #已经发行的股票数量
  dividend_yield    # 配当利回り   #分红利息
  dividend_one      # 1株配当      #一股分多少
  per               # 株価収益率   #股价收益率
  pbr               # 純資産倍率   #1股对应纯资产多少倍
  eps               # 1株利益      #1股买卖过程赚多少
  bps               # 1株純資産    #每股对应纯资产是多少
  price_min         # 最低購入代金  #单次最少买多少钱
  round_lot         # 単元株数      #一手多少股
  years_high        # 年初来高値    #当年最高价
  years_low         # 年初来安値    #当年最低价

BrandData::

  ccode     # 証券代码
  market    # 市場
  name      # 銘柄名
  info      # 銘柄情報
