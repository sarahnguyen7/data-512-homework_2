# data-512-homework_2
# Considering Bias in Data

## Project Goal
The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. This assignment will consider articles on political figures from different countries. This will be done by combining a dataset of Wikipedia articles with a dataset of country populations, and with the combined dataset, use a machine learning service called ORES to estimate the quality of each article.

Analysis will be performed of how the coverage of politicians on Wikipedia and the quality of articles about politicians varies among countries. Analysis consists of a series of tables that show:
The countries with the greatest and least coverage of politicians on Wikipedia compared to their population.
The countries with the highest and lowest proportion of high quality articles about politicians.
A ranking of geographic regions by articles-per-person and proportion of high quality articles.

## Table of Contents

- [License](#license)
- [API Documentation](#api-documentation)
- [Data Files](#data-files)
- [Data Schema](#data-schema)
- [Instructions for Use](#instructions-for-use)
- [Functions](#functions)
- [Research Implications](#research-implications)
- [Known Issues](#known-issues)
- [Special Considerations](#special-considerations)

## Source Data
The Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries. This data is in the repository as `politicians_by_country.AUG.2024.csv`.
The population data is available in CSV format as `population_by_country_AUG.2024.csv` from repository. This dataset was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau.

## License
The code written was developed in combination of myself, ChatGPT, Dr. David. W. McDonald for use in Data 512, a UW MS Data Science Degree program. The authors of the code will be referenced in the specifc code they were used in. Licenses for all are listed down below. 

#### Wikipedia API Example Code
The source data is governed by the [Wikimedia Foundation terms of use](https://foundation.wikimedia.org/wiki/Terms_of_use). The datasets created from this project is subject to the same terms, allowing for reuse and redistribution with appropriate attribution.

#### Liftwing ML Service API Example Code
The Liftwing ML service provides a flexible and scalable API for deploying machine learning models in production. All use of the service is governed by the [Liftwing API terms of use]((https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing/Usage)). Any datasets generated or used by this service are subject to these terms, which allow for reuse and redistribution with proper attribution, where applicable. 

#### Dr. David W. McDonald's Example Code
This code example was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024

#### Sarah Nguyen's Code
This code was developed by Sarah Nguyen for use in DATA 512 homework, which is coursework provided under the [MIT License](https://chatgpt.com/c/67048ab0-3d24-8001-88ce-c354bb934b32#:~:text=under%20the%20MIT-,License,-.).

#### GPT-4
Portions of this project were assisted by GPT-4, an AI model by OpenAI.  These are indicated in the notebook and subject to Open AI's [Terms of Use](https://openai.com/policies/row-terms-of-use/). It was also used to help rewrite some of my documentation to help standardize format.

## API Documentation

For more details about the Wikimedia API used in this project, please refer to the following resources:
- [Wikimedia REST API](https://wikimedia.org/api/rest_v1/)

#### Liftwing ML Service API
- Wikimedia Foundation (WMF) is implementing a new Machine Learning (ML) service infrastructure that they call [LiftWing](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing). This notebook illustrates how to generate article quality estimates for article revisions using the LiftWing version of [ORES](https://www.mediawiki.org/wiki/ORES). The [ORES API documentation](https://ores.wikimedia.org) can be accessed from the main ORES page. The [ORES LiftWing documentation](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing/Usage).

## Data Files
#### Input Files:
- `population_by_country_AUG.2024.csv`
- `politicians_by_country.AUG.2024.csv`

Intermediary Files:
- `ores_scores.json`
- `failed_articles.json`
- `article_quality_per_capita.csv`

Output Files:
- `wp_countries-no_match.txt`
- `wp_politicians_by_country.csv`

## Data Schema
### Input Files:
##### 1. `population_by_country_AUG.2024.csv`
 **Description**: Contains population data for countries and regions, with regions distinguished by ALL CAPS values in the `Geography` column. These regional rows should not be matched with the country values in `politicians_by_country_AUG.2024.csv`, but they are essential for regional reporting.

| Column     | Data Type | Description                                                                           |
|------------|-----------|---------------------------------------------------------------------------------------|
| Geography  | String    | Country or region name. Regions are in ALL CAPS (e.g., `AFRICA`, `ASIA`, `EUROPE`).   |
| Population | Float     | Population value (in millions) for the respective country or region.                   |


##### 2. `politicians_by_country_AUG.2024.csv`
##### **Description**: Contains a list of politicians categorized by their respective countries.

| Column   | Data Type |
|----------|-----------|
| name     | String    |
| url      | String    |
| country  | String    |
---

### Intermediary Files:
##### 3. `ores_scores.json`
**Description**: Stores the ORES (Objective Revision Evaluation Service) quality scores for politicians' Wikipedia articles, along with their revision IDs.

**JSON Structure**:
```json
{
   "Politician Name": {
      "enwiki": {
         "models": {
            "articlequality": {
               "version": "0.9.2"
            }
         },
         "scores": {
            "revision_id": {
               "articlequality": {
                  "score": {
                     "prediction": "Quality Class",
                     "probability": {
                        "FA": 0.12345,
                        "GA": 0.23456,
                        "B": 0.34567,
                        "C": 0.45678,
                        "Start": 0.56789,
                        "Stub": 0.67890
                     }
                  }
               }
            }
         }
      }
   }
}
```

##### 4. `failed_articles.json`
**Description**: Contains articles that could not retrieve an ORES score
[
   "Article Title 1",
   "Article Title 2",
   "Article Title 3",
   ...
]
##### 5. `article_quality_per_capita.csv`
**Description**: A CSV file containing the number of articles and high-quality articles per capita for each country and region. The population values are stored in individuals (converted from millions).

| Column                          | Data Type |                                                                  
|----------------------------------|-----------|
| country                         | String    | 
| region                          | String    | 
| total_articles                  | Integer   | 
| high_quality_articles           | Integer   | 
| population                      | Float     | 
| articles_per_capita             | Float     |
| high_quality_articles_per_capita | Float     |

---

### Output Files:
##### 6.  `wp_countries-no_match.txt`
**Description**: Contains countries that there are no matches for after merging the wikipedia data and population data.

Country Name 1

Country Name 2

Country Name 3


##### 7. `wp_politicians_by_country.csv`
**Description**: A CSV file containing the list of politicians, their countries, ORES quality scores, and population information (from the merged datasets). The population is also in millions here.

| Column          | Data Type |
|-----------------|-----------|
| country         | String    |
| region          | String    |
| population      | Float     |
| article_title   | String    |
| revision_id     | Integer   |
| article_quality | String    |


## Instructions for Use
### Installation
Ensure you have Python 3.8 and above to install the required packages:
- `pip install requests`
- `pip install pandas` 

Ensure you have an API Access key in order to use the ORES API. Directions on how to do so are in the notebook.

### Requirements for ORES Request
Access to the ORES API will require that you request an API access key. The sample code for making ORES requests includes links to information about how to request a key. A "best practice" for any code that requires an API key is to make sure that the key does not appear in the plain text of the code or notebook. In the notebook, I have discussed how to request the key.

## Functions
- `request_pageviews_per_article` (created by Dr. McDonald) is an API request made using one function to make the code resuable. It has paramters but relies on constants listed in the Notebook.
- `batch_request_pageinfo` calls the API to get the page information for multiple pages at the same time, by separating the page titles with the vertical bar "|" character. However, this approach has limits. You should probably check the API documentation if you want to do multiple pages in a single request - and limit the number of pages in one request reasonably. At the time of writing, the limit is 50 pages in a single request.
- `filter_pageinfo` filters the page information we just got into `title:lastrevid`
- `request_ores_score_per_article` (created by Dr. McDonald) is an ORES API whose main parameter is `article_revid`.
- `score_all_articles` is a function to perform ORES scoring for all articles.
  
## Research Implications
In analyzing article quality and coverage for politicians from Wikipedia, there are some key takeaways about the scope and limits of using it as a data source. I’ve learned that it mostly reflects the interests and efforts of the people contributing to it.

For example, regions like Northern, Southern, Eastern, and Western Europe stand out as having the highest quality coverage (with the Caribbean and Southern Africa also ranking high). However, what really surprised me was the uneven representation in the total number of articles for smaller countries like Antigua and Barbuda, the Federated States of Micronesia, and the Marshall Islands, which are at the top for articles per capita. I expected that bigger countries with more global influence would dominate in terms of presence. The analysis also revealed some biases in the geographic and cultural spread of articles. Countries involved in international tourism or diplomacy tend to get more coverage compared to larger, less globally connected nations. For instance, small countries like Bhutan and Seychelles might get more visibility because of their unique cultural or environmental significance. These patterns likely exist because some contributors are more willing to cover lesser-known topics. Please view the `Analysis` section in the notebook, as I feel you can really see the bias as the countries and regions had the most coverage and highest quality are documented.

In conclusion, while Wikipedia is a huge source of information, it still mostly reflects the contributors’ interests, and those people aren't spread equally across the world. I was surprised at how few articles there are for countries with geopolitical influence, like Romania or the Philippines, which might be due to regional or language gaps. It also suggests that wealthier, English-speaking countries, or those with active online communities, might be overrepresented. All this points to potential limits of using Wikipedia as a global data source, especially for certain types of research.

**What biases did you expect to find in the data (before you started working with it), and why?**
Before starting, I expected to find a bias toward content produced in wealthier, English-speaking countries, as this was also what we found in this week's reading in Duarte, N., Llanso, E., & Loup, A. (2018). Mixed Messages? The Limits of Automated Social Media Content Analysis. Proceedings of the 1st Conference on Fairness, Accountability and Transparency, 81, 106. In Duarte et al., we find that English-language tools may have disparate accuracy levels for minority populations. I figured that services like Wikipedia were more likely to be geared toward where contributions are more likely to come from regions with greater internet access, higher literacy rates.

**What might your results suggest about the internet and global society in general?** 
These results show some of the bigger societal imbalances when it comes to global access to information and technology. Even though the internet seems open and available to everyone, it still highlights the gaps in infrastructure, education, and cultural influence. Topics that get more attention usually line up with regions or people who have better access to education, resources, and the platforms needed to contribute. In a way, it reflects the same biases we see in society, where certain voices end up dominating global conversations more than others.

**Can you think of a realistic data science research situation where using these data might create biased or misleading results?** 
Yes, using this data to train models for things like predicting global sentiment, summarizing knowledge, or even making business decisions (like market segmentation by region) could lead to biased or misleading results. For example, if you trained a model to rank global political importance based on the number or quality of Wikipedia articles, it might seriously underrepresent smaller or less developed countries. Similarly, using this data for studies on social or gender issues, without accounting for the coverage bias, could lead to wrong conclusions about the representation of women or other minorities.

## Known Issues
A few articles were classified as "Korean" for their country, as they were dated before North and South Korea split. These articles went to neither country. "Korean" can be found in `wp_countries-no_match.txt`. In the merge to create `wp_countries-no_match.txt`, I originally found that some countries were spelled differently in the input `.csv` files. They were renamed as the following with `politicians_by_country_AUG.2024.csv`'s format on the right to `politicians_by_country_AUG.2024.csv` on the left:

    'Guinea-Bissau': 'GuineaBissau',  
    'Korea, South': 'Korea (South)',  
    'Korea, North': 'Korea (North)'   

Additionally, the following 8 politicians are missing from REST API Call:

Barbara Eibinger-Miedl, Mehrali Gasimov, Kyaw Myint, André Ngongang Ouandji, Tomás Pimentel, Richard Sumah, Segun ''Aeroland'' Adewale, Bashir Bililiqo

As this was to explore bias, it is important to note that no articles from Canada or the US were crawled and therefore, not present in the results. I am unsure if this was an inconsistency due to the crawl, or due to the Wikipedia dataset.

The last issue, I manually summed up the countries for all geographic regions instead of using the population given in the table for for the geographic region. This is because I felt it may be more accurate and granular to do so, but if you are replicating the data, you may get different results because of this.

## Special Considerations
You should be a little careful with the data. Crawling Wikipedia categories to identify relevant page subsets can result in misleading and/or duplicate category labels. Naturally, the data crawl attempted to resolve these, but not all may have been caught. As well, Wikipedia categories are folksonomic, meaning there is very little control over how they are applied to pages. This means that the set of pages is very likely some kind of subset, and may have pages that are not actually about individual politicians. You should look for any data inconsistencies and document how you handle inconsistencies that you find.
The inconsistencies I found are in the section [Known Issues](#known-issues).

