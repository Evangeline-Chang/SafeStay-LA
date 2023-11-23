# SafeStay LA: Airbnb Safety Dashboard

This repository is used to create an interactive Tableau-based dashboard, enabling users to effortlessly search for secure Airbnb's by incorporating safety filters on an intuitive map interface.

The final result is available at the following link:
[SafeStay LA: Airbnb Safety Dashboard](https://public.tableau.com/views/SafeStayLAAirbnbSafetyDashboard/SafeStayLA?:language=en-US&:display_count=n&:origin=viz_share_link)

In this repository, there are 8 notebooks, each containing a part of data cleaning and XXXX performed on the datasets to be used in the dashboard.

### [01_zipcode](01_zipcode.ipynb)
#### Convert the latitude and longitude data from crime data to zip codes.

The original dataset has nearly 800,000 rows, and after filtering for the year 2020 (using the function get_year), there are approximately 199,000 rows. I then split the dataset into 10 parts (and saved each of them just in case). After the 10 splits, I converted the latitude and longitude to zip codes using the function `latlong_zip` and saved the results respectively. Finally, I concatenated them back together.

To test the code without running it for 27 hours, I extracted the top 20 rows from the original dataset and saved them as `Crime_Data_head20.csv` for testing and review.

### [02_fillna](02_fillna.ipynb)
#### Automate the process of inputting zip code data for rows with missing latitude and longitude data

I have created the `fill_same` function to iterate through the rows and identify those without zip code values, while checking if they share the same location and cross street as others. After running `fill_same` several times, approximately 550 rows have been filled out, leaving only 65 rows for manual input.

For code testing, I extracted a subset of rows from the original dataset and saved them as `crime_data_2020_zip_na.csv` for further review.

For the 65 rows that could not be filled using the code mentioned above, I manually inputted the values in Excel.

### [03_crime_rate](03_crime_rate.ipynb)
#### Calculate crime rates by zip code

By combining the crime count and the population of each zip code, I have defined a function `crime_rate` to calculate the crime rate within each zip code. Since I wasn't able to upload the whole csv file for crime data 2020, I have uploaded `crime_count.csv`, which is a file of crime counts in each zip code. The final resulting data frame is saved as `crime_rate_2020.csv`.

### [04_airbnb_basics](04_airbnb_basics.ipynb)
#### Obtain basic information about each listing from the Airbnb file that I got from Inside Airbnb

There are two functions: `superhost` and `basic_info` in this file. The former checks whether the listings are from Superhosts or not, while the latter retrieves basic information such as the number of bedrooms, beds, baths, etc. Finally, the file will be saved as Airbnb_BasicInfo.csv, including only the columns needed for future use.

The original file contains approximately 45,000 rows. To ensure a smooth review process, I created a subset of the first 100 rows, named `detailed_listings_test.csv`, for reviewing.

### [05_review_topics](05_review_topics.ipynb)
#### Perform data cleaning on the reviews and conduct topic modeling on listings with more than 100 reviews

Note: This file was not used at the end because keyword extraction performed better on the reviews and do not require as many reviews to generate meaningful result.

##### Data Cleaning
First, filter comments from the year 2020 or later using the `get_year` function. Then, apply a series of data cleaning operations, including stripping, removing rows with less than 5 characters, handling empty lines (`empty_lines`), removing host information (`remove_host_names`, `remove_host`), and retaining only English reviews (`english_only`) for improved topic modeling results.

##### LDA Topic Modeling
In this part, I utilized two functions, `clean_text` and `lda`, to perform LDA topic modeling on listings with more than 100 reviews. After getting the results of LDA topic modeling, I retrieved the top five words for each topic and then combine the results with the full listings dataframe, making it suitable for use in Tableau.

### [06_keyword_extraction](06_keyword_extraction.ipynb)
#### Perform keyword extraction on listings with more than 15 reviews since 2020
The first part involves data cleaning. The methods used were not exactly the same as the ones I applied in topic modeling. After data cleaning, I excluded a lot of stopwords, which are words that need to be removed if they are determined as keywords by the model. Then, I extracted the top seven keywords and incorporated them into the listings file. Pleasantly, the results from `yake` are better than the LDA model; `yake` can even generate phrases like 'great location' and 'wonderful host'.

### [07_safety_related](07_safety_related.ipynb)
#### Identify safety-related reviews and determine unsafe listings
For this task, I created a list of safety-related keywords, which includes `['safe', 'security', 'danger', 'unsafe', 'safety', 'dangerous']`. For listings that didn't undergo keyword extraction, I checked if the reviews contained any of these keywords using `is_safety_related`. If they did, I extracted the three words before and after the keywords with `extract_context` to later determine whether the listings were unsafe or not. In the end, I merged everything back into the listing file, and hopefully, this will be sufficient for me to work on the dashboard.

### [08_crime_bar](08_crime_bar.ipynb)
#### Categorize the crime types and grouped them by quarters and ZIP codes
In this file, I categorized the crimes into 6 groups, Theft, Assault, Burglary, Vandalism, Sex-Related, and Others. Then, I grouped them by quarters, ZIP codes, and crime types. The output file is used to create a bar chart showing the number of crimes in each area through 2020 Q1 to 2023 Q3.


In the end, everything is merged back into the listing file, which should be sufficient for creating the dashboard.