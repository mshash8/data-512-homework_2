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
  * An [API Key](https://api.wikimedia.org/wiki/Authentication) is required to call this API. More information about this can be found in the notebook linked above.

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

# Special Considerations
* It should be noted that not all article titles might have ORES labels. However, in this projet all articles yielded a response.
* The ORES API accepts 5000 requests per hour per user.

# License
This project is open-source and follows the MIT License. You are free to use and modify the code according to the terms of the MIT License.

# Output file

**Path:** data/wp_scored_city_articles_by_state.csv

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

# Data Folder
The data folder contains the data files used for analysis and the intermediary ones produced in the process.
1. NST-EST2022-ALLDATA.csv - estimated populations of all US states for 2022.
2. US States by Region - US Census Bureau.xlsx - listing the states in each regional division.
3. us_cities_by_state_SEPT.2023.csv - File with subset of city names that have articles on Wikipedia
4. wp_scored_city_articles_by_state.csv - final output csv file
5. ORES_Response/ores_dict1.json - first ORES response
6. ORES_Response/ores_dict1.json - second ORES response
7. ORES_Response/ores_dict1.json - third ORES response
8. ORES_Response/ores_dict1.json - fourth ORES response
9. ORES_Response/consolidated_ores.json - the four consolidated ores response (21519 rows)
10. ORES_Response/predictions.json - A file mapping article_title to predicted ORES label
11. Page_Info_Response/title_revid_dict.json - Output of the Page Info API

# Research Implications
As part of this project, I had the opportunity to explore some of the biases present on Wikipedia, gain a better understanding of data acquisition through APIs, and uncover some surprising results. It was fascinating to learn about the limitations of APIs, such as the maximum number of requests allowed, and the significance of creating access keys while monitoring access to APIs. Additionally, our investigation yielded interesting insights, revealing that certain states considered 'traveled to' and popular had fewer articles per capita than others. Lastly, working with Python libraries like pandas, requests, JSON, and others enabled me to discover the methods and capabilities these libraries offer.
1. What biases did you expect to find in the data (before you started working with it), and why?

Before working with the data, I anticipated discovering more information for popular and highly populated cities, while expecting less information for cities that are both less populated and less renowned. Considering Wikipedia's open nature for public editing, I inferred that cities with larger populations and more attractions would likely have more extensive content on their pages. However, during the analysis, I found this assumption to be incorrect. The top 10 states in the analysis also surprised me, as I initially expected them to be more densely populated and frequently traveled to.

Furthermore, I expected potential bias within the content itself, as there could be a higher representation of one group submitting edits to Wikipedia. This might perpetuate their beliefs as the truth within the articles, potentially causing the most popular opinion to be accepted as the sole truth.


2. What might your results suggest about (English) Wikipedia as a data source?

The research from this study suggests that Wikipedia continues to serve as a vast repository of information on a wide range of topics. Information is available for almost every city in the United States, and one could argue that there is data on nearly every city that has been visited and documented on Wikipedia. This wealth of information is valuable for individuals conducting exploratory data analysis in virtually any field, as it covers a significant number of topics.

In our analysis, it was fascinating to observe that the top 10 states with the highest number of articles per capita were not necessarily the most popular or frequently visited states. This finding suggests that Wikipedia may not exhibit the biases one might initially assume. There appears to be a fair representation of information about states from across the country, which is interesting.

Wikipedia, as a data source, also offers numerous opportunities for active engagement with researchers. It provides various APIs for accessing page information and assessing content quality, as demonstrated in this assignment. The results obtained in this assignment were made possible because the API was accessible.

3. Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might still be appropriate and useful, despite its inherent limitations and biases?

Utilizing Wikipedia data can prove valuable in various research phases, including exploratory data analysis and literature surveys. Wikipedia's long-term presence makes it a frequent top result in Google searches, ensuring access to a wide array of information.

Furthermore, with a 22-year history, Wikipedia serves as a constantly updated repository of historical data across diverse subjects. This historical context can be useful when conducting research to gain insights into the past and explore related topics. This could be especially useful in time series analysis tasks.





