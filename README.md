# Sentiment Analysis of News Articles

## Project Goal
This project aims to explore the correlation between the sentiment of news articles about a target country and the number of protests and riots in that same target country.

The ability to forecast social unrest could potentially save millions of lives by offering governments across the world the chance to better prepare for it.

## Project Scope
For the purpose of this project, data from the [Armed Conflict Location & Event Data Project (ACLED)](https://acleddata.com/) were used to determine the number of protests and riots in a country.

As a proof of concept, this project focuses on India as its target country.

## Choice of News Publisher
There are several options to consider when deciding on the news publisher to extract news articles from. The options available, datasets chosen for our investigation and its considerations are listed below.
    
1. Bucket of news publishers - [CommonCrawl](https://commoncrawl.org/)
    * Pros
        - Variety of news sources (data can potentially be more objective overall)
        - More perspectives of events considered
    * Cons
        - Issue of quality control. There could be unreliable news publishers included in the bucket that function as noise in the data
        - Dataset can get very large
2. Individual News Publisher (International) - Reuters (Articles were extracted from CommonCrawl as well)
    * Pros   
        - Better quality control (offers us the ability to choose a reputable publisher)
    * Cons
        - Lack of coverage in country of interest as the news publisher focuses mainly on international news
        - News published may not be as recent as compared to native news publishers
3. Individual News Provider (Native) - [Times of India](https://www.kaggle.com/therohk/india-headlines-news-dataset)
    * Pros
        - Better quality control
        - Could have more coverage on country of interest
        - News published may be more recent as compared to international news publishers
    * Cons    
        - There could be potential bias in the reporting of local events by the local media especially in countries where media is heavily controlled

My exploration attempts to answer which type of dataset is best for our use case of forecasting social unrest.

## Data Processing Pipeline
The following data processing pipeline was used.

![Data Processing Pipeline](/images/text_data_processing_pipeline.png?raw=true "Data Processing Pipeline")

Open source ready-made models were used in order to bootstrap development for a quick investigation as a proof-of-concept. Details of the models used can be found below.

1. Geoparser
    * Computes focus country of the article (ie which country is the article covering)
    
2. News Tag Classifier
    * Categorises news articles into 4 different news tags (World, Business, Sci/Tech, Sports)
    
3. Sentiment Analyzer
    * Categorises news articles into positive or negative
    
At the end of the data processing pipeline, a sentiment index can be computed for each news category by taking the number of negative articles divided by the total number of articles. The way the sentiment indexes is computed is such that it will always range from 0 to 1, whereby 0 means no negative articles were published while 1 means all the articles published were negative.

The rationale for filtering the articles into different categories was to examine if there are certain types of content of news articles that bear a stronger correlation with the number of ACLED protests and riots. If there happen to be such a relationship, we can then choose to streamline our investigation for future explorations.

## Insights

The correlation coefficients between the sentiment index of news articles published today and the number of ACLED protests and riots in the next day were computed and shown below.
### Correlation for CommonCrawl Dataset
![Correlation for CommonCrawl](/images/commoncrawl_correlation.png)

The sentiment index of Sports articles had the least correlation with the number of ACLED protests and riots of the next day.

![CommonCrawl Country Count](/images/commoncrawl_country_count.png)

There was a disproportionately large coverage for the Western countries like US and GB and much lower coverage for our target country, India.
### Correlation for Reuters Dataset
![Correlation for Reuters](/images/reuters_correlaton.png)

The results from the Reuters dataset were quite contradictory to the results obtained for the CommonCrawl dataset. More specifically, a moderate positive correlation was observed for the Business Sentiment Index of Reuters but a moderate negative correlation was observed for the Business Sentiment Index of CommonCrawl. This could possibly be due to the fact that CommonCrawl consists of many different news providers and upon deeper analysis, I found that many of the news providers included in CommonCrawl were not of high quality. This could serve as noise in the data and might explain our contrasting results.
        
Furthermore, the results obtained for the Reuters and CommonCrawl datasets should be taken with a pinch of salt, especially because the datasets only span across a mere 9 days. The results obtained from the Times of India dataset should be of more significance as it spanned across 20 years.

![Reuters Country Count](/images/reuters_country_count.png)

The same disproportionate coverage of countries was observed.
### Correlation for Times of India Dataset
![Correlation for Times of India](/images/times_of_india_correlation.png)
        
Although weak correlations were observed for Business and World Sentiment Indexes, they were still positive ones. Furthermore, weaker correlations were observed for Sci/Tech and Sports Sentiment Indexes which supports the hypothesis that certain categories of news could have a larger impact on the number of riots and protests.

![Times of India Country Count](/images/times_of_india_country_counts.png)
    
As expected, most of the articles published by Times of India, a native news publisher are about its local country, India.
    
In conclusion, it seems that native news publishers may be the best choice for forecasting of social unrest. International news providers may not have as much coverage as native news providers and this could possibly explain the poor results observed.