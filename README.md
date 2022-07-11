# steeleye_assignment
```
from pydoc import cli
import pymongo
from fastapi import FastAPI

# MongoDb atlas is used to create Database
client = pymongo.MongoClient("mongodb+srv://deepak:deepakbabu@cluster0.sbit1.mongodb.net/")

db = client['steeleye']
trades = db['trades']
app = FastAPI()

#  Here get_trade_data function is create to return a list which have no object id.

def get_trade_data():
    trade_data = []

    for data in trades.find({}, {"_id": 0}):  # remove object id key

        trade = data['trade_details']
        trade[0].pop('_id')

    # remove object id key

        data.update({'trade_details': trade[0]})

        trade_data.append(data)

    return trade_data

```
# Get list of all trades

```
@app.get("/all_trades")
def all_trades():
    return get_trade_data()


# Search trades by id

@app.get("/trade/{id}")
def trade_by_id(id):
    trade_data = get_trade_data()
    for i in range(0, len(trade_data)):
        if id in trade_data[i].values():
            return trade_data[i]

```
# search by different parameters
```
@app.get("/query")
def search(search):
    query_value = []
    trade_data = get_trade_data()
    for i in range(0, len(trade_data)):
        counterparty = (trade_data[i])['counterparty']
        instrumentId = (trade_data[i])['instrument_id']
        instrumentName = (trade_data[i])['instrument_name']
        trader = (trade_data[i])['trader']

        if search in counterparty or search in instrumentId or search in instrumentName or search in trader:
            query_value.append(trade_data[i])

    return query_value
```
# 4th
```
@app.get("/filter")
def filter_data(assetClass=None, end=None, maxprice=None, minprice=None, start=None, tradeType=None):
    # trade_data=get_trade_data()
    # return trade_data
    # trade_data = get_trade_data()
    # trade_list=[]
    # for i in range(0, len(trade_data)):
    # #     for j in range(minprice,maxprice+1):
    #         if minprice <= (trade_data[0])['price']<=maxprice:
    #             trade_list.append[trade_data[0]['price']]
    # return trade_list
```
