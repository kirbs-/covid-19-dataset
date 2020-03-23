# covid-19-dataset
US county level COVID-19 case data.

Daily snapshots of US cases by county. 

## Project structure
```
/data
    |
    - {state}_by_county_{scraper_timestamp_in_EDT}.txt
/source_page_backup
    |
    - {state}_county_{scrape_timestamp}.html # depends on data source
- main.ipynb # triggers 
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