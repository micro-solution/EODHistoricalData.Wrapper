# EODHistoricalData.Wrapper

Made with Microsoft Visual Studio

[![.NET Version](https://img.shields.io/badge/.NET-6.0+-blue.svg)](https://shields.io/)

# Contents

1. [General description](#general-description-arrow_up)
2. [Requirements](#requirements-arrow_up)
3. [Installation](#installation-arrow_up)
4. [Demo](#demo-arrow_up)
5. [Documentation](#documentation-arrow_up)
	- [Historical Prices, Splits and Dividends Data APIs](#historical-prices-splits-and-dividends-data-apis-arrow_up)
	- [Fundamental and Economic Financial Data APIs](#fundamental-and-economic-financial-data-apis-arrow_up)
6. [Disclaimer](#disclaimer-arrow_up)

## General description [:arrow_up:](#eod-historical-data-sdk)
This library is the C# .NET SDK for the EOD Historical data REST API. It's intended to be used for data extraction for financial valuations, macroeconomic analyses, sentiment analysis, option strategies, technical analysis, development of machine learning models, and more!

## Requirements [:arrow_up:](#eod-historical-data-sdk)
- You need to request an API key with the EOD team. Create your account at the following [link](https://eodhistoricaldata.com/)
	- ***Please be aware of the pricing plans and policies. Different plans have different data accesses.***
- ```C#``` >= 6.0

## Installation [:arrow_up:](#eod-historical-data-sdk)

## Demo [:arrow_up:](#eod-historical-data-sdk)

## Documentation [:arrow_up:](#eod-historical-data-sdk)

Please be aware that some descriptions will come directly from the API's documentation because no further explanations were needed for the specific method. Additionally, for the sake of simplicity, I will use the following convention along with the whole document: 

```c#
using EOD;
// create the instance of the SDK
apiToken = "YOUR_API_KEY_GOES_HERE";
var _api = new API(apiToken);
```

### Historical Prices, Splits and Dividends Data APIs [:arrow_up:](#eod-historical-data-sdk)

- **Stock Price Data API (End-Of-Day Historical Data)**: Retrieve end-of-day data for Stocks, ETFs, Mutual Funds, Bonds (Government and Corporate), Cryptocurrencies, and FOREX pairs.
	- Parameters:
		- ```tiker```(string): Required - ticker consists of two parts: {SYMBOL_NAME}.{EXCHANGE_ID}, then you can use, for example, AAPL.MX for Mexican Stock Exchange. or AAPL.US for NASDAQ.
		- ```from```(DateTime) and ```to```(DateTime): the beginning and end of the desired dates
		- ```period```(HistricalPeriod): search data interval. By default, daily prices will be shown.
		- ```order```(Order): Optional - sorting data by dates. By default, dates are shown in ascending order.
	- Usage:
```c#
// AngloAmerican stock that trades from January 1 to December 11 in the London Stock Exchange
List<HistoricalStockPrice>? response = await _api.GetEndOfDayHistoricalStockPriceAsync('AAL.LSE', new DateTime(2021, 1, 10), new DateTime(2021, 12, 11), HistoricalPeriod.Daily, Order.Ascending);
```
- **Live (Delayed) Stock Prices API**: The method supports almost all symbols and exchanges worldwide, and the prices provided have a 15-20 minutes delay. The method also offers combinations of multiple tickers with just one request. The only supported interval is the 1-minute interval. **The UNIX standard is used for the timestamp**.
	- Parameters:
		- ```tiker```(string): Required - ticker consists of two parts: {SYMBOL_NAME}.{EXCHANGE_ID}, then you can use, for example, AAPL.MX for Mexican Stock Exchange. or AAPL.US for NASDAQ.
	- Usage:
```c#
// An example of live (delayed) stock prices API for AAPL (Apple Inc)
List<HistoricalStockPrice>? response = await _api.GetLiveStockPricesAsync("AAPL.US");
```
- **Historical Splits and Dividends API**: Get the historical dividends, splits for any stock.
	- Parameters:
		- ```tiker```(string): Required - ticker consists of two parts: {SYMBOL_NAME}.{EXCHANGE_ID}, then you can use, for example, AAPL.MX for Mexican Stock Exchange. or AAPL.US for NASDAQ.
		- ```from```(DateTime) and ```to```(DateTime): the beginning and end of the desired dates
	- Usage:
```c#
// An example of historical dividends for AAPL (Apple Inc)
List<HistoricalDividend>? response = await _api.GetHistoricalDividendsAsync("AAPL.US", new DateTime(2000, 1, 1), new DateTime(2021, 12, 1));
//  An example of historical splits for AAPL (Apple Inc)
List<HistoricalSplit>? response = await _api.GetHistoricalSplitsAsync("AAPL.US", new DateTime(2000, 1, 1), new DateTime(2022, 01, 1));
```
- **Intraday Historical Data API**: Get intraday historical stock price data for US (NYSE and NASDAQ), Canada, and MOEX tickers. The 1-minute interval includes the pre-market and after-hours trading data from 2004 (more than 15 years of the data), and for the 5-minute intervals, the data starts from October 2020. For other tickers (mainly for international instruments), it is only available the 5-minute intervals and only from October 2020.
	- Parameters:
		- ```symbol```(string): Required - Name of the instrument to retrieve data.
		- ```interval```(IntradayHistoricalInterval): Required - the possible intervals: 5-minutes, 1 hour and 1-minute
		- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates. The maximum periods between ‘from’ and ‘to’ are 120 days for 1-minute interval, 600 days for 5-minute interval and 7200 days for 1 hour interval.
	- Usage:
```c#
// An example of every hour intraday historical stock price data for AAPL (Apple Inc)
List<IntradayHistoricalStockPrice>? response = await _api.GetIntradayHistoricalStockPriceAsync("AAPL.US", new DateTime(2021, 12, 01), new DateTime(2021, 12, 31), IntradayHistoricalInterval.OneHour);
```
- **Options Data API**:  Stock options data for top US stocks from NYSE and NASDAQ, the data for Options starts from April 2018. Options data is updated daily; however, the API does not provide a history for options contracts prices or other related data. That means: for each contract, there is only the current price, bid/ask, etc. 
1. **IMPORTANT!** For backward compatibility, you should use the ```from``` parameter with any value before the expiration date, the API recommends '2000-01-01'. 
2. **Note**: option greeks and some additional value are available only for options with expiration date Feb 15, 2019, or later.

	- Parameters:
		- ```ticker```(string): Required - Could be any supported symbol. No default value.
		- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates.
		- ```trade_date_from```(DateTime): Optional - filters OPTIONS by ```lastTradeDateTime```. Default value is blank.
		- ```trade_date_to```(DateTime): Optional - filters OPTIONS by ```lastTradeDateTime```. Default value is blank.
		- ```contract_name```(string): Optional - Name of a particular contract.
	- Usage:
```c#
// An example of Stock options data for AAPL (Apple Inc)
List<IntradayHistoricalStockPrice>? response = await _api.GetOptionsDataAsync("AAPL.US");
```
### Fundamental and Economic Financial Data APIs [:arrow_up:](#eodhistoricaldata.wrapper)
- **Economic events API**: It provides the past and future events from all around the world like Retail Sails, Bond Auctions, PMI Releases and many other Economic Events data.
	- Parameters:
		- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates.
		- ```country```(string). Optional - The country code in ISO 3166 format, 2 symbols.
		- ```comparison```(string): Optional - Filter events by their periodicity.
		- ```offset```(int): Optional - Possible values from 0 to 1000.
		- ```limit```(int): Optional - The maximum amount of data to retrieve. Possible values from 0 to 1000.
	- Usage:
```c#
// An example of Economic Events Calendar data for the USA in 2021
List<EconomicEventData>? response = await _api.GetEconomicEventsDataAsync(new DateTime(2021, 01, 1), new DateTime(2021, 12, 31), "US");
```
- **Insider Transactions API**: The insider transactions API data is available for all US companies that report Form 4 to the SEC. Insider trading involves trading in a public company’s stock by someone who has non-public, material information about that stock for any reason.
	- Parameters:
		- ```limit```(int): Optional - The limit for entries per result, from 1 to 1000. Default value: 100.
		- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates.
		- ```ticker```(string): Optional - ticker of the company to retrieve data. By default, all possible symbols will be displayed.
	- Usage:
```c#
// An example of Insider Transactions the data only for Apple Inc in 2021
List<InsiderTransaction>? response = await _api.GetInsiderTransactionsAsync(null, new DateTime(2021, 01, 1), new DateTime(2021, 12, 31), "AAPL.US");
```
- **Fundamental Data: Stocks, ETFs, Mutual Funds, Indices**: Access to fundamental data API for stocks, ETFs, Mutual Funds, and Indices from different exchanges and countries. Almost all major US, UK, EU, India, LATAM, and Asia exchanges are available.
	- Parameters:
		- ```ticker```(string): Required - Name of the instrument to retrieve data.
		- ```filters```(string): Optional - Multi-layer filtering helps to reduce the output of the request. Different layers are divided with ```::``` and it’s possible to have as many layers as you need. Additionally, you can request multiple fields from a particular layer using ```,```. Be aware that the order of the layers is from the macro keys to the micro-level.
	- Usage:
```c#
// An example of fundamental data feed for Apple Inc
FundamentalData? response = await _api.GetFundamentalDataAsync("AAPL.US");
// An example of filtered fundamental data feed for Apple Inc
FundamentalData? response = await _api.GetFundamentalDataAsync("AAPL.US", "General::Code,General,Earnings");
// An example of ETFs data for VTI.US
FundamentalData? response = await _api.GetFundamentalDataAsync("VTI.US");
```
- Bulk fundamentals API
	- Parameters (Bulk fundamentals):
		- ```ticker```(string): Required - consists of two parts: {SYMBOL_NAME}.{EXCHANGE_ID}, then you can use, for example, AAPL.MX for Mexican Stock Exchange.
		- ```offset```(int): Optional - the first symbol you will get. Default value is 0.
		- ```limit```(int): Optional - the number of symbols you will get. Default value is 1000. Max value is 1000.
		- ```symbols```(string): Optional - To get the data for several symbols instead of the entire exchange. If not empty, the first parameter will be ignored.
	- Usage:
```c#
// An example of bulk fundamental data feed for NASDAQ exchange
BulkFundamental? response = await _api.GetBulkFundamentalsDataAsync("NASDAQ", 500, 500);
```
- **Calendar API. Upcoming Earnings, Trends, IPOs and Splits**
	- **Upcoming Earnings**
		- Parameters:
			- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates. If ```from``` is not provided, today will be used. If ```to``` is not provided, today + 7 days will be used.
			- ```ticker```(string): Optional - You can request specific symbols to get historical and upcoming data. If ‘symbols’ used, then ‘from’ and ‘to’ parameters will be ignored. You can use one symbol: ‘AAPL.US’ or several symbols separated by a comma: ‘AAPL.US, MS’
		- Usage:
		```c#
		// An example to get upcoming earnings for Apple Inc
		UpcomingEarning? response = await _api.GetUpcomingEarningsAsync(, , "AAPL.US");
		```
	- **Earning Trends**
		- Parameters:
			- ```ticker```(string): Required - You can request specific symbols to get historical and upcoming data.
        /// You can use one symbol: ‘AAPL.US’ or several symbols separated by a comma: ‘AAPL.US, MS’.
		- Usage:
		```c#
		// An example to get earning trends for several symbols
		EarningTrend response = await _api.GetEarningTrendsAsync("AAPL.US, MS");
		```
	- **Upcoming IPOs**
		- Parameters:
			- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates for IPOs. If ```from``` is not provided, today will be used. If ```to``` is not provided, today + 7 days will be used.
		- Usage:
		```c#
		// An example to get all upcoming IPOs for April 2022
		UpcomingIPO response = await _api.GetUpcomingIPOsAsync(new DateTime(2022, 04, 1), new DateTime(2022, 04, 31));
		```
	- **Upcoming Splits**
		- Parameters:
			- ```from```(DateTime) and ```to```(DateTime): Optional - the beginning and end of the desired dates for IPOs. If ```from``` is not provided, today will be used. If ```to``` is not provided, today + 7 days will be used.
		- Usage:
		```c#
		// An example to get all splits from December 2, 2018, to December 6, 2018
		UpcomingIPO response = await _api.GetUpcomingSplitsAsync(new DateTime(2018, 12, 2), new DateTime(2018, 12, 6));
		```
- **Macro Indicators API**: Macroeconomics is a part of economics dealing with the performance, structure, behavior, and decision-making of an economy as a whole. The Macroeconomics data API includes regional, national, and global economies. EOD provides the data for more than 30 macro indicators such as GDP, unemployment rates, national income, price indices, inflation rates, consumption, international trades, and many other significant indicators.
	- Parameters:
		- ```country```(string): Required - Defines the country for which the indicator will be shown. The country should be defined in the Alpha-3 ISO format. Possible values: CHL, FRA, DEU, etc.
		- ```indicator```(string): Optional - Defines which macroeconomics data indicator will be shown. The default value is ```'gdp_current_usd'```.
	- Usage:
```c#
// Get the available macroindicators names
List<MacroIndicator> response = await _api.GetMacroIndicatorsAsync();
// Request the Chilean interest rate
List<MacroIndicator> response = await _api.GetMacroIndicatorsAsync("CHL", "real_interest_rate");
```
- **Bonds Fundamentals API**: Bond covenants details.
	- Parameters:
		- ```code```(string): Required - CUSIP of a particular bond, it’s also could be an ISIN. Other IDs are not supported at the moment.
	- Usage:
```c#
// Request bonds fundamental data feed for CUSIP: US00213MAS35
BondsFundamentalData response = await _api.GetBondsFundamendalDataAsync("US00213MAS35")
```
## Disclaimer [:arrow_up:](#eod-historical-data-sdk)
This document is not an offer to buy or sell financial instruments. Never invest more than you can afford to lose. You should consult a registered professional advisor before making any investment.
