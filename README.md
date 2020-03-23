# covid-19-dataset
US county level COVID-19 case data.

Daily snapshots of US cases by county. 

## County Data Status
| State | Scraper | Validator | Aggergator | Time Series |
|-------|---------|-----------|------------|-------------|
|   VA  |    Y    |     N     |     N      |      N      |
|   CA  |    Y    |     N     |     N      |      N      |
|   MD  |    Y    |     N     |     N      |      N      |
|   MI  |    Y    |     N     |     N      |      N      |
|   WY  |    Y    |     N     |     N      |      N      |
|   TN  |    Y    |     N     |     N      |      N      |
|   NJ  |    Y    |     N     |     N      |      N      |
|   NY  |    Y    |     N     |     N      |      N      |
|   CO  |    Y    |     N     |     N      |      N      |
|   OH  |    Y    |     N     |     N      |      N      |
|   FL  |    Y    |     N     |     N      |      N      |
|   AK  |    Y    |     N     |     N      |      N      |
|   TX  |    Y    |     N     |     N      |      N      |
|   WA  |    Y    |     N     |     N      |      N      |
|   PA  |    Y    |     N     |     N      |      N      |


## Project structure
```
/data  # county level snapshots by scrape timestamp.
    |
    - {state}_by_county_{scraper_timestamp_in_EDT}.txt # snapshot of scraped results as of timestamp.
/source_page_backup # backup of source pages by scrape timestamp.
    |
    - {state}_county_{scrape_timestamp}.html # backup of source page. Extension depends on data source.
- main.ipynb # triggers crawler
- config.yaml # shared scraper configurations
- {state}_by_county.ipynb # State specific scapers
```

## Scraper Format
Scrapers are simple python scripts or jupyter notebooks that implement a fetch, save, and run method.
#### fetch() 
_Returns_
	- DataFrame containing positive cases by county.
	- Source data - HTML page, etc.
	
Fetch is responsible for getting and processing a page into a Pandas DataFrame. Fetch must return a DataFrame must contain `county` and `positive_cases` columns (additional columns are fine) and a string containing the data source being scraped.

#### save(df, source)
_Params:_
	- df (DataFrame): DataFrame containing `county` and `positive_cases` columns (additional columns are fine) 
	- source (str): string containing the data source page that was scraped.

Save handles persisting the Data Frame and source data. df is saved as a pipe delimited text file in the data directory with the scraping timestamp in EDT.

#### run()
Handles fetch and save in one action. Used in main crawling job.