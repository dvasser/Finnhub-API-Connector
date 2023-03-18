# Instructions on how to use the Finnhub-API-Connector

Establish your connector variable:
## connector = FinnhubConnector(api_key = 'YOUR_API_KEY')

This empty function returns the symbols of all stocks (free tier gives access to only North American ones) in a pandas data frame that could be used as input for other functions.
# connector.get_north_american_stocks()

The look up stock function allows you to search stocks based on query, text can be symbol, name, isin, or cusip. Returns a data frame with possible results and their symbols which again could be used as input for other functions.
# connector.look_up_stock('microsoft') 

Returns a data frame with company news in according dates, start/end date format: yyyy-mm-dd . Free tier only gives 1 year of records so the end date cannot be more that one year behind current date, also the API call returns a data frame that goes no more than 240 records records away from the end date, so it is highly preferable to enter dates that are only a few days apart
# connector.get_company_news('AMCR', '2022-01-01', '2023-01-01')

Returns a dictionary with annual, quarterly and past year basic financial info of a given stock. For best efficiency use the outline below in order to assign the dictionaries to a value and then access them based on your specific needs.
# FRC = connector.get_basic_financials('FRC')
# FRC['annual'] or FRC['quarterly'] or FRC['past_year']

Simply put in a stock symbol and get a data frame with earnings surprises of the given stock with dates in ascending order as index.
# connector.get_earnings_surprises('ADC')

Get the current quote of a given stock with price	change, percent change, high,	low, open, close and time.
# connector.get_current_quote('AMZN')

Get candles of a stock in a time frame. Supported resolution includes 1, 5, 15, 30, 60, D, W, M. Some timeframes might not be available depending on the exchange. Please specify the date in the following format: yyyy-mm-dd . Time is an optional argument set to default: 00:00:00. Make sure that you specify arguments in the following order: symbol, resolution, date from, date to, time from, time to.
# connector.get_stock_candles('TSLA', '1', '2022-11-10', '2022-12-31')

Get crypto symbols for a speficic exchange. List of exchanges includes;
"FXPIG", "KUCOIN", "GEMINI", "BITTREX", "POLONIEX", "HUOBI", "BINANCEUS", "COINBASE", "BITFINEX", "KRAKEN", "HITBTC", "OKEX", "BITMEX", "BINANCE"
# connector.get_crypto_symbols('KUCOIN')

Gets crypto candles the same as the get_stock_candles function above. Make sure that the symbol input is in the following format: 'EXCHANGE:SYMBOL' i.e 'BINANCE:BTCUSDT'. The list of symbols can be generated by calling get_crypto_symbols and would be in the 'Symbols' column of the returned data frame. Time arguments are optional, refer to get_stock_candles for list of possible time resoution arguments.
# connector.get_crypto_candles('BINANCE:BTCUSDT', '1', '2022-11-10', '2022-12-31', '13:14:24', '17:14:24')

Stream real time live updates for a given stock or cryptocurrency. Data cleaning was not done here because with websocket connection speed is most important and doing any manipulation with the output dictionaries would take up CPU time. Response Attributes: type: Message type. data: List of trades or price updates. s: Symbol. p: Last price. t: UNIX milliseconds timestamp. v: Volume. c: List of trade conditions. A comprehensive list of trade conditions code can be found here: https://docs.google.com/spreadsheets/d/1PUxiSWPHSODbaTaoL2Vef6DgU-yFtlRGZf19oBb9Hp0/edit#gid=0
Simply interrupt the kernel when you want to stop running the stream and you'll see a disconnect statement.
# connector.stream_websocket('AAPL')
# connector.stream_websocket('BINANCE:BNBUSDT')
