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
