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
- [Known Issues](#known-issues)
- [Special Considerations](#special-considerations)
- [Instructions for Use](#instructions-for-use)
- [Functions](#functions)
- [Research Implications](#research-implications)

## Source Data
The Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries. This data is in the repository as `politicians_by_country.AUG.2024.csv`.
The population data is available in CSV format as `population_by_country_AUG.2024.csv` from repository. This dataset was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau.

## License
The code written was developed in combination of myself, ChatGPT, Dr. David. W. McDonald for use in Data 512, a UW MS Data Science Degree program. The authors of the code will be referenced in the specifc code they were used in. Licenses for all are listed down below. 

#### Wikipedia API Example Code
The source data is governed by the [Wikimedia Foundation terms of use](https://foundation.wikimedia.org/wiki/Terms_of_use). The datasets created from this project is subject to the same terms, allowing for reuse and redistribution with appropriate attribution.

#### Dr. David W. McDonald's Example Code
This code example was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024

#### Sarah Nguyen's Code
This code was developed by Sarah Nguyen for use in DATA 512 homework, which is coursework provided under the [MIT License](https://chatgpt.com/c/67048ab0-3d24-8001-88ce-c354bb934b32#:~:text=under%20the%20MIT-,License,-.).

#### GPT-4
Portions of this project were assisted by GPT-4, an AI model by OpenAI.  These are indicated in the notebook and subject to Open AI's [Terms of Use](https://openai.com/policies/row-terms-of-use/). It was also used to help rewrite some of my documentation to help standardize format.

## API Documentation

For more details about the Wikimedia API used in this project, please refer to the following resources:
- [Wikimedia REST API](https://wikimedia.org/api/rest_v1/)

## Data Files


## Data Schema

## Known Issues

## Special Considerations
You should be a little careful with the data. Crawling Wikipedia categories to identify relevant page subsets can result in misleading and/or duplicate category labels. Naturally, the data crawl attempted to resolve these, but not all may have been caught. As well, Wikipedia categories are folksonomic, meaning there is very little control over how they are applied to pages. This means that the set of pages is very likely some kind of subset, and may have pages that are not actually about individual politicians. You should look for any data inconsistencies and document how you handle inconsistencies that you find.
The population_by_country_AUG.2024.csv contains rows that provide cumulative regional population counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA). These rows should not match the country values in politicians_by_country.AUG.2024.csv, but you will want to retain them so that you can report coverage and quality by region as specified in the analysis section below.

## Instructions for Use
### Installation
Ensure you have Python 3.8 and above to install the required packages:
- `pip install requests`
- `pip install pandas` 
- `pip install matplotlib`

### Requirements for ORES Request
Access to the ORES API will require that you request an API access key. The sample code for making ORES requests includes links to information about how to request a key. A "best practice" for any code that requires an API key is to make sure that the key does not appear in the plain text of the code or notebook. One approach is to embed the key as an environment variable and retrieve the key from that variable. Another approach is to use a code based key manager that stores keys on your local machine. The apikeys user module is used in the ORES example code.

## Functions

## Research Implications


