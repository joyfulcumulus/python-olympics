# Exploratory Analysis: 120 Years of Olympics

## Introduction
* Motivation: Since the 2024 Olympics in Paris is over, I thought it would be interesting to do an exploratory data analysis on Olympics data
* Objective: Be proficient in Python libraries - Pandas, Seaborn, Matplotlib

## Technologies Used
### Language
* Python

### Libraries
* Pandas
* Seaborn
* Matplotlib

## Dataset Used
* I obtained an Olympics Dataset from Kaggle: https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results
* Each row in this dataset is an athlete-event, which describes who competed in which game(s), in which event, and the result (medal).
* Each row also contains details about the athlete's physical data (i.e. sex, height, weight, age)

## Business Problem Identification
As an exploratory data analysis, there is no business problem. Instead, I thought of common questions that people may ask about the Olympic games:
1. Who (at country level) participates in the Olympics?
2. The ancient Olympic games only started with male participation. Since then, has female representation increased?
3. Who (at country level) dominates the Olympics?
4. Since these athletes have trained very hard, they must be very fit. Does it follow that they also have a healthy BMI? 

## Data Extraction
* I used a Jupyter notebook hosted on Kaggle, which makes connecting to the dataset easy
```python
filepath = "/kaggle/input/120-years-of-olympic-history-athletes-and-results/athlete_events.csv"
olympics_data = pd.read_csv(filepath, index_col="ID")
```

## Data Cleaning / Checks
To ensure the data was fit for analysis, I first checked the quality of the data
* Size of data set (using shape)
* Data types of each column so I would manipulate them using the appropriate functions (using dtypes)
* Missing values, unrealistic outliers (e.g. min/max) (using describe)
* Relationship between the columns, if any (e.g Sports and Events are 1:N)

Insights (that also broadened my general knowledge)
* Event and Team column data was not very useful
* There are indeed some elderly athletes taking part in relatively more "sedentary" events (e.g. Art Competitions)
* The Winter Olympics data needed recoding of the Year from 1992 onwards due to it being held in different years
* Age, Height, Weight and Medal fields had incomplete data. It was best to use data from 1960 onwards for analyses related to athlete's physique

## Data Exploration & Visualization
I describe my thought process in approaching the "Business Problems" below:

### Female Participation in the Olympics
* I decided that a time series analysis would be appropriate
* For each Olympiad year, I would count the no. of male athletes vs female athletes
* Did not split by Season (i.e. winter vs summer) to get an overall picture

### Geographical Participation in the Olympics
* I first checked the no. of countries that had participated over time
* This was done by plotting a line chart (i.e. time series)
* I could see that as of 2016, there were 200+ countries, which would make subsequent visualisations regarding proportion very messy due to the small proportions per country
* Tracking each of the 200+ countries' participation over time would also not be meaningful, and result in a chart with 200+ lines, making it hard to read

* Given there were 200+ countries, I decided to narrow my focus to the most recent Olympic data in the dataset, as people would be more interested in the latest situation
* I also decided to narrow my focus to visualise the top N countries to see who were the main contenders
* For further improvement to make use of all the data and to give a fuller picture of the situation, it would be ideal to use geospatial data packages in subsequent iterations (e.g. Kepler GL, geopandas)

### Countries that Dominate the Olympics
* Given the previous insight that there were 200+ countries that participated in the Olympics, in order to find out who dominated the games, I would just need to narrow down to the top few
* Therefore, I felt that it was sufficient to tally the top 3 countries (by medal count) per Olympiad year to work with a more manageable dataset
* This whittles the no. of countries that need to be presented on the chart to 19
* As the idea of "domination" comes with an extended period of time (i.e. not a once-off endeavour), I charted the medal counts of these countries over Year as the x-axis. Then I could see which country was consistently present year on year, as opposed to a country that was intermittently present

### BMI by Sport
* As the male and female physical body is different, I split the dataset when making the analysis
* To represent the BMI distribution for multiple sports without overwhelming the chart with plots or lines, I chose a box plot
* To facilitate easier comparison to the normal BMI range, I added 2 red reference lines

## Main Takeaways
* Female participation in the Olympics increased gradually in the 1940s, then rapidly in the 1980s
* Attendance rose over time, from 12 countries in the first Olympics, to 207 in the 2016 Olympics. The only exception where attendance dipped in 1976 and 1980 was due to boycotts.
* Some countries participate in both summer and winter strongly (e.g. USA), but others may have less representation in winter sports (e.g. Australia), and vice versa (e.g. Norway more representation in winter), possibly due to the climate in their countries
* Countries that dominates the Olympics are USA, Russia, Germany
* Athletes competing in sports that require more strength / muscle mass (e.g. judo, rugby, curling, weighlifting and wrestling) usually had athletes with BMI above the normal range

## Key Learnings
* Important to do the data cleaning / checks first, so that I understood what preparation was required for each business question. For example, when analysing anything related to height, weight, age, I would only take data from 1960 onwards due to incompleteness issue.
* Always try to investigate abnormal trends. I realised that it was from there where I learnt more historical knowledge about the Olympics, such as how the winter and summer olympics were split from the same year from 1992 onwards, and the boycotts that took place etc. The historical knowledge then guided my data cleaning. For example, in the case of the splitting of the winter and summer olympics, I had to add additional logic to recode the Year value in the winter olympics:

```python
def recode_winter_olympics_year(row):
    if row.Year in [1994, 1998, 2002, 2006, 2010, 2014]:
        row.Year += 2
    return row

olympics_data_recoded = olympics_data.apply(recode_winter_olympics_year, axis='columns')
``` 
