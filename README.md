# CZ4034 Information Retrieval

## Quick Start  
1. Obtain your twitter api keys and fill in the key.csv file.  
```
access_key,  <your access key>
access_secret,  <your access secret>
consumer_key,  <your consumer key>
consumer_secret,  <your consumer secret>
```  
  
2. Run SOLR on your local machine  
```
cd \solr-8.8.1\bin  
solr start -c -p 8983 -s ../example/cloud/node1/solr
```
Go to your web browser and go to URL *http://localhost:8983/solr/#/*  
Take note you need to have SOLR running before running scripts `inject_data.py` and `twitter_stream_to_db.py`
  
To stop,
```
cd \solr-8.8.1\bin  
solr stop -all
```

## Here are the functions of the 3 files:
### twitter_scraping.py
Scrape 2000 documents of data from each of our designated 5 sources to come up with a total of 10,000 documents.  
`['CNBC', 'WSJmarkets', 'nytimesbusiness', 'LizAnnSonders', 'FinancialNews']`
  
Output file will follow the naming convention:  
`news_<%Y%m%d_%H%M%S>.csv` *where <> represents the datetime when it was scraped*  
  
### inject_data.py
Pre-populate our solr non-SQL database with our 10k data. Change the variable `CSV_FILEPATH in line 48` to the new file name.  
  
### twitter_stream_to_db.py
Running this script will allow us to listen to the above-mentioned 5 twitter pages.  
And so when there is a new tweet, it will:  
a. Add that tweet to our db, then  
b. Delete the oldest tweet based on tweetcreatedts  
  
Therefore, we will always maintain a dynamic database of size 10,000 documents  
