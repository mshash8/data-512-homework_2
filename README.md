# data-512-homework_2
# Project Goal
The primary goal of this project is to conduct an in-depth analysis of Wikipedia article traffic patterns spanning from 2015 to 2023. To achieve this objective, we leverage the capabilities of the Wikimedia REST API. The API provides a wealth of information, including article view counts, article titles, access methods (mobile or desktop), and more. Our aim is to store this data in JSON files and subsequently perform comprehensive data visualization and analysis.

# Data Acquistion and API Documentation

## Data Acquistion
The section below details the steps for data acquistion. We have:

1. A csv [file](https://drive.google.com/file/d/1khouDmMaZyKo0y5WkFj4lu7g8o35x_98/view?usp=drive_link) defining the subset of the city names that have articles on Wikipedia. 
2. A [file](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html) that contains estimated populations of all US states for 2022. This can be downloaded from: https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html 
3. A [spreadsheet](ttps://docs.google.com/spreadsheets/d/14Sjfd_u_7N9SSyQ7bmxfebF_2XpR8QamvmNntKDIQB0/edit?usp=drive_link) listing the states in each regional division.

## API Documentation
In this project, two API endpoint were called. 
1. [API Info](https://www.mediawiki.org/wiki/API:Info)
  * Input Parameters: This API takes page_title as a parameter, taken from the first csv file mentioned above.
  * Output: This returns and range of metadata on an article, including the most current revision ID of the article page. This revision ID is used for the ORES API Call.
  * A code snippet is used in the Data Acquistion phase from [notebook](https://drive.google.com/file/d/15UoE16s-IccCTOXREjU3xDIz07tlpyrl/view?usp=drive_link) and this sample code is licensed [CC-BY](https://creativecommons.org/licenses/by/4.0/). 
3. [ORES](https://www.mediawiki.org/wiki/ORES)
  * Input Parameters: This API requires a specific revision ID of an article to make a label prediction.
  * Output: Metadata about the article and probabilities of it belonging to a label and a final prediction.
  * A code snippet is used in the Data Acquistion phase from [notebook](https://drive.google.com/file/d/17C9xsmR9U3lJeD52UTbAedlHDetwYsxs/view?usp=drive_link). This code was modified to include HTTPS status code checks.
  * The [labelings](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment) were learned based on articles in Wikipedia that were peer-reviewed using the Wikipedia content assessment procedures.

# Platform
This project was run on jupyter notebook on localhost. 

# Steps to Reproduce these Results
Data Acquistion, Data Preparation, and Data Analysis are key phases within this project:

* Step 1: Getting the Article, Population and Region Data
  - Data is obtained from the data sources provided by the instructor and this is used to make API calls to get further information required for analysis.
* Step 2: Getting Article Quality Predictions
  - The data from the previous step is used to call the Page Info and ORES API to obtain information required for analysis.
* Step 3: Combining the Datasets
  - After retrieving and including the ORES data for each article, it is required to merge the wikipedia data and population data together. This is further merged with the US Census regional-division.
* Step 4: Analysis
  - Analyis included calculating total-articles-per-population (a ratio representing the number of articles per person) and high-quality-articles-per-population among other metrics.
* Step 5: Results
  - Results are obtained by calculating certain metrics like:
      1. Top 10 US states by coverage: The 10 US states with the highest total articles per capita (in descending order) .
      2. Bottom 10 US states by coverage: The 10 US states with the lowest total articles per capita (in ascending order) .
      3. Top 10 US states by high quality: The 10 US states with the highest high quality articles per capita (in descending order) .
      4. Bottom 10 US states by high quality: The 10 US states with the lowest high quality articles per capita (in ascending order).
      5. Census divisions by total coverage: A rank ordered list of US census divisions (in descending order) by total articles per capita.
      6. Census divisions by high quality coverage: Rank ordered list of US census divisions (in descending order) by high quality articles per capita.

# Documentation
This project adheres to recommended best practices for replication and documentation:

Jupyter Notebook: detailed comments for data acquistion, processing, and analysis.

README File: Offers a general overview of the project, outlining its objectives, data sources, data processing procedures, and standards for reproducibility.

LICENSE File: Contains the MIT LICENSE for the project's code, ensuring it is freely usable.

# License
This project is open-source and follows the MIT License. You are free to use and modify the code according to the terms of the MIT License.

# Output file

| Column            | datatype 
| :---------------- | :------: 
| state             |   string 
| regional_division |   string 
| population        |   int    
| article_title     |   string 
| revision_id       |   int
| article_quality   |   string

**Note**
The article quality can take values: 
1. FA - Featured article
2. GA - Good article (sometimes called A-class)
3. B - B-class article
4. C - C-class article
5. Start - Start-class article
6. Stub - Stub-class article


# Research Implications
1. What biases did you expect to find in the data (before you started working with it), and why?

Before working with the data, I anticipated discovering more information for popular and highly populated cities, while expecting less information for cities that are both less populated and less renowned. Considering Wikipedia's open nature for public editing, I inferred that cities with larger populations and more attractions would likely have more extensive content on their pages. However, during the analysis, I found this assumption to be incorrect. The top 10 states in the analysis also surprised me, as I initially expected them to be more densely populated and frequently traveled to.

Furthermore, I expected potential bias within the content itself, as there could be a higher representation of one group submitting edits to Wikipedia. This might perpetuate their beliefs as the truth within the articles, potentially causing the most popular opinion to be accepted as the sole truth.


2. What might your results suggest about (English) Wikipedia as a data source?

The research from this study suggests that Wikipedia continues to serve as a vast repository of information on a wide range of topics. Information is available for almost every city in the United States, and one could argue that there is data on nearly every city that has been visited and documented on Wikipedia. This wealth of information is valuable for individuals conducting exploratory data analysis in virtually any field, as it covers a significant number of topics.

In our analysis, it was fascinating to observe that the top 10 states with the highest number of articles per capita were not necessarily the most popular or frequently visited states. This finding suggests that Wikipedia may not exhibit the biases one might initially assume. There appears to be a fair representation of information about states from across the country, which is interesting.

Wikipedia, as a data source, also offers numerous opportunities for active engagement with researchers. It provides various APIs for accessing page information and assessing content quality, as demonstrated in this assignment. The results obtained in this assignment were made possible because the API was accessible.

3. Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might still be appropriate and useful, despite its inherent limitations and biases?

Utilizing Wikipedia data can prove valuable in various research phases, including exploratory data analysis and literature surveys. Wikipedia's enduring presence and comprehensive coverage make it a frequent top result in Google searches, ensuring access to a wide array of information.

Furthermore, with a 22-year history, Wikipedia serves as a constantly updated repository of historical data across diverse subjects. This historical context can be invaluable when conducting research to gain insights into the past and explore related topics.

Finally, Wikipedia's open editing model allows individuals from around the world to contribute their discoveries to the platform. This collaborative approach resembles a form of crowdsourcing, where society collectively gathers data, contributing to reduced bias in data collection.





