# Politician Article Quality on English Wikipedia
Data 512: A2 - Bias in Data

<!-- ABOUT THE PROJECT -->
## About the Project:
The goal of this assignment is to explore the concept of bias in data through an analysis of article quality for politicial figures on English Wikipedia from a variety of countries. This analysis uses a datasets of Wikipedia articles and country populations and employs the ORES machine learning service to estimate article quality.

<!-- PREREQUISITES -->
## Prerequisites:

* python3
* pandas
* numpy
* json
* requests

<!-- PROJECT STRUCTURE -->
## Project Structure:

The raw data is stored in the `raw_data` directory, any processing or incremental files are stored in `prelim_data`, and the final output is in `final_data`. The analysis is done in a jupyter notebook (`hcds-a2-bias.ipynb`).
```
.
├── LICENSE
├── README.md
├── raw_data
│   ├── page_data.csv
│   ├── WPDS_2020_data.csv
├── prelim_data
│   ├── country_prelim.csv
│   ├── country_region_mapping.csv
│   ├── no_ORES_scores.csv
│   ├── ORES_scores.csv
│   ├── politicians_prelim.csv
│   ├── region_prelim.csv
├── final_data
│   ├── wp_wpds_countries-no_match.csv
│   ├── wp_wpds_politicians_by_country.csv
└── hcds-a2-bias.ipynb
```


<!-- SPECIAL CONSIDERATIONS -->
## Special Considerations:

Note that we have made assumptions that `WPDS_2020_data.csv` is ordered where each region is at the same level and a country belongs to the region nearest above it. There is a `Type` field that we did not use for demarcating country/region to allow for the inclusion of the Channel Islands in our analysis.

The ORES REST API is limited to 50 revision_ids at a time. This is discussed further in `hcds-a2-bias.ipynb`.

If you would like to read more about Wikipedia content assessment guidelines via ORES, please see the documentation below. At a high level, the 6 categories are as follows with the top 2 (FA & GA) indicating an article is "high-quality":

1. FA - Featured article
2. GA - Good article
3. B - B-class article
4. C - C-class article
5. Start - Start-class article
6. Stub - Stub-class article


<!-- DATA -->
## Data:

### Raw Data:
The raw data is contained in 2 CSV files formatted as follows:

* Filename: `page_data.csv`
* Data Structure:

| Column  | Description                                           |
|---------|-------------------------------------------------------|
| page    | Wikipedia article name associated with politician     |
| country | Country name associated with politician               |
| rev_id  | Revision ID associated with a given Wikipedia article |

* Filename: `WPDS_2020_data.csv`
* Data Structure:

| Column     | Description                                      |
|------------|--------------------------------------------------|
| FIPS       | Country code or Region name                      |
| Name       | Geographic name (such as Country or Region name) |
| Type       | Geographic type (World, Sub-Region, Country)     |
| TimeFrame  | Population year                                  |
| Data (M)   | Population in Millions by type                   |
| Population | Population by type                               |

### Prelim Data:
Any preliminary data is contained in 6 CSV files as briefly described below. The most notable of which is `country_region_mapping.csv` for which you can see the data structure below. A more complete discussion of the other intermediary files can be found in `hcds-a2-bias.ipynb`.

* Filename: `country_prelim.csv`
* Brief Description: contains all country entries (including the Channel Islands) from `WPDS_2020_data.csv` 

* Filename: `country_region_mapping.csv`
* Data Structure:

| Column             | Description                      |
|--------------------|----------------------------------|
| country            | Country name                     |
| country_population | Country population               |
| region             | Region name                      |
| region_population  | Population for the entire region |

* Filename: `no_ORES_scores.csv`
* Brief Description: all articles / rev_ids for which an ORES score is NOT found

* Filename: `ORES_scores.csv`
* Brief Description: all articles / rev_ids for which an ORES score is found

* Filename: `politicians_prelim.csv`
* Brief Description: cleaned `page_data.csv` to remove non-article entries (occurs where `page` begins with 'Template:')

* Filename: `region_prelim.csv`
* Brief Description: contains all region entries (identified as any `Type` in ALL CAPS) from `WPDS_2020_data.csv` 

### Final Data:
The final data is contained in 2 CSV files each formatted as follows:

* Data Structure:

| Column               | Description                                           |
|----------------------|-------------------------------------------------------|
| country              | Country name                                          |
| article_name         | Wikipedia article name associated with politician     |
| revision_id          | Revision ID associated with a given Wikipedia article |
| article_quality_est. | ORES score (FA, GA, B, C, Start, Stub)                |
| population           | Population by country                                 |

* Filename: `wp_wpds_countries-no_match.csv`
* Brief Description: contains any rev_ids WITH A SCORE that were unable to be mapped by country
* Note: for rev_ids WITHOUT A SCORE check `prelim_data/no_ORES_scores.csv`

* Filename: `wp_wpds_politicians_by_country.csv`
* Brief Description: contains any rev_ids that were mapped to both an ORES score and a country

<!-- REFLECTIONS & IMPLICATIONS -->
## Reflections & Implications:

Overall, I found this process of this assignment to exercise to be very interesting. I enjoyed that some of my expectations for the analysis outcomes held up while others did not in ways I hadn’t initially anticipated, particularly when looking at article-per-population. This experience gave me some ideas on how to enhance and expand this analysis to get a more complete picture on the relationship between population and article count/quality on English Wikipedia. See continued below for further discussion of my reflections throughout this process: 

__What biases did you expect to find in the data (before you started working with it), and why?__

Prior to working with this data, I expected to see a bias towards countries with a higher proportion of English for both articles-per-population and high-quality article proportion on English Wikipedia. I assumed this would result in higher scores for countries/regions in North America and across Europe. My reasoning for this was that people from these areas would be more likely to see benefit from more, higher quality articles on English Wikipedia.

I assumed we would see higher relative quality coverage for countries with increased levels of social or political unrest within the past few decades. This could be a result of more people looking for this type of information and contributing to article quality. I also expected to see a bias towards more coverage for countries with increased internet accessibility – this could be due to a larger segment of the population engaging with Wikipedia in general. I anticipated lower coverage rates for smaller countries or those with relatively less English speakers.

__What might your results suggest about the internet and global society in general?__

The results suggested that I had underestimated the presence of articles-per-population for smaller countries – my anticipated results were actually the opposite of what the analysis showed for “top 10 countries by coverage” where the largest country in that group was Iceland with a population of 368k. Upon further review, this could be due to a few confounding factors such as proportion of the population that are politicians. For example, as per [Wikipedia](https://en.wikipedia.org/wiki/Parliament_of_Tuvalu), Tuvalu (the top country by coverage) has 16 seats in parliament for a population of 10k. If that ratio held true for the population of the United States, we would have over 500k seats in our legislative body.

Despite this, the results for relative quality of article were aligned with my expectations. Anecdotally, I would argue that much of the “top 10 countries by relative quality” are countries that are frequently in the English media such as North Korea or Syria, which may correlate with higher quality articles.
In general, these results have inherent limitations, I would not attempt to draw conclusions on worldwide Wikipedia usage or article quality. Any discussion of this data should note the English-language bias in the results.

__How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?__

If a researcher wanted to correct for some of these limitations, they could perform some normalization on estimated proportion of politicians in the population before looking at coverage as defined by article-per-population. It would be interesting to see if this analysis would result in some larger countries in the top 10, as discussed earlier, our entire “top 10 countries by coverage” is made up of countries in the smallest 20% by population.

If the goal was to generalize these results to Wikipedia usage or article quality on a global scale, a researcher could pull article quality scores for a range of Wikipedia projects rather than limiting their analysis to enwiki. This could pose a challenge in joining on politician name or even country across languages as well as in getting a valid ORES score but it would be interesting to explore further.


<!-- LICENSE -->
## License:

Distributed under the MIT License. See `LICENSE.txt` for more information.


<!-- CONTACT -->
## Contact:

Nicole Riggio - riggionicole@gmail.com

Project Link: [https://github.com/nriggio/data-512-a2](https://github.com/nriggio/data-512-a2)


<!-- ACKNOWLEDGMENTS -->
## Acknowledgements:

* [A2: Bias in Data Assignment](https://docs.google.com/document/d/11eswL84T-H6bli8aX_-XndCN6tAZ4bIb9Z2ywiIf2fE/edit#)
* [Sample API Code](https://public.paws.wmcloud.org/User:Jtmorgan/data512_a1_example.ipynb)
* [Figshare: Politicians by Country Dataset](https://figshare.com/articles/Untitled_Item/5513449)
* [World Population Data Sheet (WPDS)](https://www.prb.org/international/indicator/population/table/)
* [WPDS_2020_data.csv (sourced from above)](https://docs.google.com/spreadsheets/d/1CFJO2zna2No5KqNm9rPK5PCACoXKzb-nycJFhV689Iw/edit?usp=sharing)
* [Wikipedia Content Assessment Guidelines](https://en.wikipedia.org/wiki/Wikipedia:Content_assessment)
* [ORES REST API Documentation](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model)
* ['batches' Function Help](https://www.geeksforgeeks.org/break-list-chunks-size-n-python/)
* [MIT License](https://opensource.org/licenses/MIT)
* [Best-README-Template: othneildrew](https://github.com/othneildrew/Best-README-Template)
