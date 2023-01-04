232023-01-04

Style: 

Domain:

Tags:

# DataOps Part 2 - using data to find data

# Data-driven Data Catalog

A **data catalog** is a detailed inventory of all the data in your data infrastructure. Like an inventory list for a real physical warehouse, if you didn’t have this list, it would be pretty hard to find anything useful.

There’s a saying that **_if you don’t have a data catalog, your data lake is a data swamp_**. A poorly documented data lake means you have no idea what’s going in, what’s coming out and what’s festering inside.

Furthermore, as part of the EU GDPR data protection rules, you may also need a data catalog to demonstrate compliance with the law.

As with the above theme, you can use data-driven approaches to manage a data catalog.

## Analogy to real life - home contents

In my travels, I find that an organisation’s catalog or stocktake of what data they have is analogous to many people’s home and contents insurance. It is recommend as part of having [home and contents insurance](https://moneysmart.gov.au/home-insurance/contents-insurance), you keep a catalog/list of all the items in your house that covers:

1.  purchase price (if applicable)
2.  location in house
3.  receipts or ownership history

Now when was the last house you know someone that actually keeps such a detailed list? Probably not likely and even if they did, it would likely be out of date or incomplete.

However, if you were to ask most people to name say the top 10 most important/expensive items in their home and provide the above information, they would likely be able to.

If you apply this analogy to data in organisations:

1.  Most organisations have a good grasp and information on their most critical data
2.  However, most organisations would not be able to catalog of the data they own, or if they did, it would most likely be out of date.

## Data catalogs need accessible and searchable metadata

All the metadata you obtain and generate through the data workflow and pipeline should be made available to users in the data catalog. That is, every single piece of data should have metadata that describes:

-   **Data lineage** - where the data came from
    
-   **Data currency** - when the data was ingested, processed or last updated
    
-   **Data quality check and validation results** - as described earlier
    
-   **Data linkages** - how it relates to other data (described below)
    
-   **Data hierarchy** - which dataset is the preferred source of truth. Having a hierarchy will prevent scenarios where two different departments provide different numbers to the same metric.
    
-   **Data security classification** - whether it is restricted/sensitive data and subject to privacy laws with limited use (e.g. can’t use it for predictive analytics, only regulatory reporting)
    

Furthermore, having **standard naming conventions** for business units/departments, data types, data purpose, etc. makes life so much easier and will go a long way in making even the name itself descriptive.

All the above feeds into the most important purpose of a data catalog - **begin able to search for what data you need**. It’s estimated that [60 to 73% of data](https://go.forrester.com/blogs/hadoop-is-datas-darling-for-a-reason/) an organisation has is unused for analytics.

If you can’t find the data or don’t know what the heck it is, you can’t use it! In a nutshell, you need a search engine inside your data catalog that searches not just the column or table names, but the attributes and metadata too.

There’s many data catalog tools out there that have this built out-of-the-box, but I personally find the capabilities of [ElasticSearch](https://www.elastic.co/elasticsearch/) pretty cool. It’s an open-source analytics engine that basically is like a search engine out-of-the-box!

**ElasticSearch** is within the **ElasticSearch-Logstash-Kibana (ELK)** stack, which includes the data analytics Web UI Kibana. Furthermore, it supports multiple languages to query the data, including Python, SQL and a REST API.

ElasticSearch is a powerful search engine because it implements two things:

1.  **Fuzzy matching**
    
2.  **Similiarity model**
    

ElasticSearch [implements fuzzy matching](https://www.elastic.co/blog/found-fuzzy-search) using **Damerau-Levenshtein distance**. In essence, this is the distance between two texts measures how different the words are (e.g. ‘fox’ and ‘box’ are 1 swap apart, ‘axe’ and ‘aex’ are 1 swap apart and ‘face’ and ‘fac’ are 1 insert apart).

A ranking is then given for how similiar two words are based on this distance is. By default, the maximum allowed distance is based on the length of the search word - e.g. max distance of 1 for words between 3 to 5 characters long.

## Use data-driven analytics and machine learning

As I mentioned before, if metadata is treated as data, then really you are using **data about data to train ML to identify data related to the data**!

### Clustering, Correlations and Trends

Since you have generated all the metadata about the data, this data can be used to train a ML model that will automatically identify and tag relevant data.

Data-driven approaches based on data types can also identify the probability of two datasets have correlations. For example two datasets could be correlated and flagged as ‘related’ if:

-   have the same start and end times with the same amount of data points
-   were generated at the same time

You may notice identifying these characteristics is similiar to the **Exploratory Data Analysis** (EDA) step in ML/data science. In EDA, there is a focus on identifying trends, correlations, statistical relationships as well as just generally how the data relates to each other.

ML can even monitor usage and access metrics to determine and rank which datasets are the most popular by using a **recommendation system** (similiar to how Netflix recommends movies for you to watch based on people’s viewing patterns).

A great out-of-the-box open-source ML library is [scikit-surprise](http://surpriselib.com/), which allows you to pick different types of algorithms, including the one that won a Netflix prize.

### Automatically generating tags and descriptions

Natural-language processing machine learning (NLP ML) already exists in many other fields and domains. Some use cases for NLP ML include:

-   semantic analysis of keywords to determine the overall ‘theme’ of a dataset
-   automatically generating descriptions based on metadata.

For example, using the recently released [open-source NLP ML library GPT 3](https://github.com/openai/gpt-3)created by OpenAPI, you can create paragraphs of text using some keywords. The new GPT-3 is already pre-trained with over 175 billion parameters so out-of-the-box and for some generated news articles, it fooled [88% of human participants](https://arxiv.org/pdf/2005.14165.pdf) into thinking it was written by a real person!

Having a ML library generate natural language descriptions of the datasets would greatly benefit a data catalog, including the search capabilities mentioned above.

___
# References
[DataOps Part 2 - using data to find data](https://ayc-data.com/data_engineering/2020/08/01/dataops-part-2-data-to-find-data.html)