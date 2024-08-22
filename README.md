# Data Portfolio - Dawid Wolanin: Power Query, SQL (BigQuery), R (RStudio), Tableau

This is my Cyclistic Capstone Project in the form of a case study, the completion of which is required to achieve the Google Data Analytics Professional Certificate.


# Table of contents 

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Types of Visuals](#types-of-visuals)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration](#data-exploration)
  - [Data Cleaning](#data-cleaning)
  - [Data Transformation](#data-transformation)
- [R code](#r-code)
  - [Preparation](#preparation)
  - [Calculation and Visuals](#calculation-and-visuals)
- [Tableau Dashboard](#tableau-dashboard)
  - [Results](#results)
  - [Setup and Tests](#setup-and-tests)
- [Analysis](#analysis)
  - [Key Findings](#key-findings)
- [Recommendations](#recommendations)
  - [Potential Courses of Actions](#potential-courses-of-actions)
- [Conclusion](#conclusion)


# Objective
- What is the main goal of the analysis?

The director of marketing of Cyclistic, a fictional bike-share company in Chicago, has set a clear goal: convert casual riders into annual members. The marketing team needs to better understand how annual members and casual riders use Cyclistic bikes differently in order to design appropriate marketing strategies. My task as a data analyst working on the team is to create a report which will help the company's executives make informed decisions.

- What does the ideal solution include?

A report containing a description of all data sources used, documentation of any cleaning or manipulation of data, a summary of my analysis, supporting visualizations and key findings, my top three recommendations based on my analysis.

## User story
As a data analyst working on the marketing analytics team at Cyclistic, I want to consider previous tiers of bike riders and based on their recurring behaviour, provide accurate recommendations for marketing strategy development.

I want to base my report on creating efficient scripts, which can be reusable with oncoming data for calculating main metrics, presented on clear visualizations, as well as a dashboard for the purpose of presentation in front of the company's executives.

The dashboard should allow me to quickly identify differences in bike usage between casual and annual members, based on metrics such as ride counts on various time periods, average ride durations and placing them on a density map. I want to be able to share the dashboard with the executives after the presentation, thus it must work intuitively and efficiently.


# Data source
Data was provided in advance on a first-party basis which ensures a high level of integrity (Note: The datasets have a different name because Cyclistic is a fictional company. For the purposes of this case study, the datasets are appropriate and will enable me to answer the business questions. The data has been made available by Motivate International Inc. under this [license](https://divvybikes.com/data-license-agreement).) 

To achieve the objective, perform the analysis and identify trends, 12 months of Cyclistic trip data is used - from January 2023 to December 2023. This timeframe allows for seasonal impact observation within a single calendar year.

# Stages

- Design
- Developement
- Analysis
- Recommendations

# Design

## Types of visuals

Some of the data visuals that may be appropriate for solving our business questions include:

1. Pie chart - presenting the total ride counts per membership type
2. Horizontal bar charts - presenting total ride counts per day of the week and per month for each membership type
3. Split horizontal bar chart - presenting average ride duration per day of the week for each membership type
4. Vertical bar charts - presenting which of the offered bike types are most frequently used
5. Dashboard containing the following visuals:
  - Line chart - presenting the most popular start times between each membership type
  - Summary table/heat map - presenting seasonal activity of users from each membership type
  - Treemap - presenting the most popular routes from each membership type
  - Density map - presenting the most popular overall locations, with switching between each membership type functionality

## Tools

| Tool | Purpose |
| --- | --- |
| Power Query | Exploring the data |
| SQL (BigQuery) | Cleaning, testing, and analyzing the data |
| R (RStudio) | Performing calculations, creating charts |
| Tableau | Visualizing the data via an interactive dashboard |
| GitHub | Hosting the project documentation and version control |


# Development

## Pseudocode

- What is the general approach in solving this business problem from start to finish?

1. Download and store the data, identify its organization
2. Explore the data in Power Query
3. Load the data into BigQuery, merge datasets
4. Clean the data with SQL
5. Create calculated fields with R and test the data with SQL and R alternately
6. Visualize the data with R (using the ggplot2 library)
7. Create an interactive dashboard in Tableau
8. Generate the findings and provide clear insights with recommendations
9. Write the documentation + commentary
10. Publish the report to GitHub Pages

## Data exploration

I imported the data to Power Query as a whole folder, containing 12 .csv files from each month and merged them into a single table through the following steps:

![import_to_PQ](assets/images/PQ1.jpg)
![import_to_PQ_2](assets/images/PQ2.jpg)

- What are the inital observations with this dataset? What has caught my attention?

1. First, using the 'Count rows' founction, I counted the total number of records, after checking for any initial errors:

![count_total_rows](assets/images/PQ10_total_rows_deleted_errors.jpg)
   
2. Next, I checked for errors and null values. By filtering each column, I have found null values in the 'start_station_name' and 'end_station_name' columns:

![start_null_values](assets/images/PQ3.jpg)
![start_null_values_2](assets/images/PQ4.jpg)
![start_null_values_count_rows](assets/images/PQ5.jpg)
![end_null_values_count rows](assets/images/PQ8_end_station_name_nulls.jpg)

Using the 'Count rows' function, I have found that there are 875716 missing values in the 'start_station_name' column and 929202 missing values in the 'end_station_name' column.

In a real-life scenario I would ask the stakeholders, engaged in the project, or someone above me in the chain of command to discuss possible causes of those null values and what would be the steps to avoid them in the future. I would also hold discussions about way to compensate for those missing values, without introducing any kind of bias into the data.

For this case study, I have decided to keep all of the records, as most of the analysis is time/date oriented, only omitting the missing values when creating the geolocation portion of the interactive dashboard in Tableau.

3. In the 'member_casual' column, the 'casual' string value refers to any rides that were not membership rides (there is no distinction between single-ride or day-pass users). This is due to Customer Protection laws, which do not allow to store data about card payments. For this reason we also can not record how many times a single customer made recurring purchases.

4. The dataset does not contain information about the distance rode per trip, which would be a valuable piece of information. Most of the insights are therefore based on the ride duration and ride count metrics, supported by geolocation records.


## Data cleaning

- The aim is to refine the dataset to ensure it is structured and ready for analysis.
- Only relevant columns should be retained.
- Certain columns have to be split into a number of new ones to generate more insights.
- Certain columns can to be merged to keep the data readable and more usable for analysis.
- All data types should be appropriate for the contents of each column.

Below is a picture of the schema for the data before cleaning:

![dirty_data_schema](assets/images/SQL1.jpg)

What are the needed steps to cleand and shape the data into a desired format?

1. Merge the 12 datasets for each month in SQL, as the file size was too big to export using Excel/Power Query. Avoid creating duplicate rows doing so.
2. Remove unnecessary columns by not including them in the query.
3. Split the TIMESTAMP value columns ('started_at', 'ended_at') into time, date, day and month columns to allow for more specific analysis.
4. Rename columns using aliases.


## Data transformation


```sql
/*
Merge 12 datasets, utilising UNION DISTINCT to avoid duplicate rows.
*/

SELECT *
FROM `cyclistic-case-study-427019.Cyclistic_data_2023.df23_01`
UNION DISTINCT
SELECT *
FROM `cyclistic-case-study-427019.Cyclistic_data_2023.df23_02`
UNION DISTINCT
.
.
.
SELECT *
FROM `cyclistic-case-study-427019.Cyclistic_data_2023.df23_12

```

After running the query, use the BigQuery functionality to save the results as a BigQuery Table:
![save_as_BigQuery_Table](assets/images/SQL3.jpg)

Moving on to the data transformation itself:

```sql
/*
# 1. Select and rename appropriate columns using aliases
# 2. Split the started_at column by extracting appropriate values
# 3. Split the ended_at column by extracting appropriate values
# 4. Calculate the ride duration for each row
# 5. Merge columns to create a RouteName column, later used for ranking most popular bike routes
*/

-- 1.
SELECT
  member_casual AS MembershipType,
  rideable_type AS BikeType,
  started_at AS StartTimeStamp,

-- 2.
  EXTRACT(TIME FROM started_at) AS StartTime,
  EXTRACT(DATE FROM started_at) AS StartDate,
  FORMAT_DATE('%A', started_at) AS StartDay,
  FORMAT_DATE('%B', started_at) AS StartMonth,
  ended_at AS ReturnTimeStamp,

-- 3.
  EXTRACT(TIME FROM ended_at) AS ReturnTime,
  EXTRACT(DATE FROM ended_at) AS ReturnDate,
  FORMAT_DATE('%A', ended_at) AS ReturnDay,
  FORMAT_DATE('%B', ended_at) AS ReturnMonth,

-- 4.
  TIMESTAMP_DIFF(ended_at, started_at, MINUTE) AS Duration,

-- 5.
  CONCAT(start_station_name, " - ", end_station_name) AS RouteName,

-- Rename geolocation columns for consistency
  start_lat AS StartLat,
  start_lng AS StartLng,
  end_lat AS ReturnLat,
  end_lng AS ReturnLng

FROM
  `cyclistic-case-study-427019.Cyclistic_data_2023.df23_all`

```

Below is a picture of the schema for the data after cleaning and transformation:

![clean_data_schema](assets/images/SQL2.jpg)



# R code


## Preparation

In order to make the dataset useable in the RStudio and Tableau enviroments it is exported from BigQuery as a .csv file. Due to its size, it first has to be exported to an interconnected Google Drive, and then it can be downloaded locally.

Load the following packages which suit our analysis approach and load our dataset:

```R
library(tidyverse) # collection of packages used to import, tidy, transform, visualize and model data
library(ggrepel) # to repel overlapping text labels from the ggplot2 package from tidyverse
library(scales) # override the default breaks, labels, transformations and palettes used for scales by ggplot2
library(extrafont) # register custom fonts

trip_data <- read_csv("Data_cleaned.csv")

```


## Calculation and Visuals

- Creating a pie chart - presenting the total ride counts per membership type

```R
# Group the dataset by membership type and count the occurrences of each type
counts <- trip_data %>%
  group_by(MembershipType) %>%
  count() %>%
  ungroup() %>%
# Calculate the percentage for each membership type relative to the total ride count
  mutate(perc = `n` / sum(`n`)) %>%
# Arrange the membership types by their percentage in ascending order
  arrange(perc) %>%
# Create a new column with adequately formatted percentages
  mutate(labels = scales::percent(perc))

# Calculate positions for centered percentage label placement
positions <- counts %>% 
  mutate(csum = rev(cumsum(rev(perc))), 
         pos = perc/2 + lead(csum, 1),
         pos = if_else(is.na(pos), perc/2, pos))

# Pie chart plotting using ggplot2
# Split by membership type, map the plot
ggplot(counts, aes(x = "", y = perc, fill = MembershipType)) +
  geom_col(color = "white") +
# Create ride count labels in a centered position for each membership type
  geom_text(aes(label = comma(n)),
            position = position_stack(vjust = 0.5), color = "white") +
# Introduce the polar coordinate system, resulting in a pie chart
  coord_polar(theta = "y") +
# Choose a predefined color palette
  scale_fill_brewer(palette = "Dark2") +
# Create percentage labels, using previously calculated positions, repel them to avoid overlap
  geom_label_repel(data = positions,
                   aes(y = pos, label = labels),
                   size = 4, nudge_x = 0.6, show.legend = FALSE, min.segment.length = Inf, color = "white", family = "Tahoma") +
# Additional formatting
  ggtitle(paste("Total riders in 2023:", comma(nrow(trip_data)))) +
  theme_void(base_family = "Tahoma") +  # Change base font family
  theme(
    plot.title = element_text(size = 14, face = "bold"),
    legend.title = element_text(size = 12, face = "bold"),
    legend.text = element_text(size = 10)
  )

```

Running the code above results in creating the following chart:
![Rplot1](assets/images/Rplot1.png)

- Creating a horizontal bar chart - presenting the total ride count per day of the week for each membership type 

```R
# Define the order of days to be viewed in the plot (order inverted due to later usage of the coord_flip() function)
days_descending <- c("Sunday", "Saturday", "Friday", "Thursday", "Wednesday", "Tuesday", "Monday")

# Bar chart plotting using ggplot2
ggplot(data = trip_data) +
# Split by day of the week and membership type, map the plot, use dodged/side-by-side bar position 
  geom_bar(mapping = aes(x = factor(StartDay, level = days_descending), fill = MembershipType),
           position = position_dodge()) +
# Create ride count labels per day of the week for each membership type
  geom_text(
    stat = 'count', 
    aes(x = factor(StartDay, levels = days_descending), label = comma(..count..), group = MembershipType), 
    position = position_dodge(width = 0.9),
    hjust = 1.5,
    color = "white",
    size = 3,
    face = "bold",
    family = "Tahoma"
  ) +
# Format the plot
  xlab("Day of the week") + 
  ylab("Count") +
  scale_y_continuous(labels = comma) +
# Flip coordinates to convert to a horizontal bar chart
  coord_flip() +
  ggtitle("Daily Riders") +
  scale_fill_brewer(palette = "Dark2") +
  theme_minimal(base_family = "Tahoma") +  # Change base font family
  theme(
    plot.title = element_text(size = 14, face = "bold"),
    axis.title.x = element_text(size = 12, face = "bold", margin = margin(t = 10)),  # Move x-axis title
    axis.title.y = element_text(size = 12, face = "bold", margin = margin(r = 10)),  # Move y-axis title
    axis.text = element_text(size = 10),
    legend.title = element_text(size = 12, face = "bold"),
    legend.text = element_text(size = 10)
  )

```
Running the code above results in creating the following chart:
![Rplot2](assets/images/Rplot2.png)

- Creating a horizontal bar chart - presenting the total ride count per month for each membership type 
  
```R
# Define the order of months to be viewed in the plot (order inverted due to later usage of the coord_flip() function)
months_descending <- c("December", "November", "October", "September", "August", "July", "June", "May", "April", "March", "February", "January")

# Bar chart plotting using ggplot2
ggplot(data = trip_data) +
# Split by month and membership type, map the plot, use dodged/side-by-side bar position 
  geom_bar(mapping = aes(x = factor(StartMonth, level = months_descending), fill = MembershipType),
           position = position_dodge()) +
# Create ride count labels per month for each membership type
  geom_text(
    stat = 'count', 
    aes(x = factor(StartMonth, levels = months_descending), label = comma(..count..), group = MembershipType), 
    position = position_dodge(width = 0.9),
    hjust = 1.5,
    color = "white",
    size = 3,
    face = "bold",
    family = "Tahoma"
  ) +
# Format the plot
  xlab("Month") + 
  ylab("Count") +
  scale_y_continuous(labels = comma) +
# Flip coordinates to convert to a horizontal bar chart
  coord_flip() +
  ggtitle("Monthly Riders") +
  scale_fill_brewer(palette = "Dark2") +
  theme_minimal(base_family = "Tahoma") +  # Change base font family
  theme(
    plot.title = element_text(size = 14, face = "bold"),
    axis.title.x = element_text(size = 12, face = "bold", margin = margin(t = 10)),  # Move x-axis title
    axis.title.y = element_text(size = 12, face = "bold", margin = margin(r = 10)),  # Move y-axis title
    axis.text = element_text(size = 10),
    legend.title = element_text(size = 12, face = "bold"),
    legend.text = element_text(size = 10)
  )

```
Running the code above results in creating the following chart:
![Rplot3](assets/images/Rplot3.png)

- Creating a facet-wrapped horizontal bar chart - presenting average ride duration per day of the week for each membership type

```R
# Calculate the average ride duration for each combination of day of the week and membership type
averages <- trip_data %>%
  group_by(StartDay, MembershipType) %>%
  summarise(average_duration = mean(Duration)) %>% 
  as.data.frame()

# Bar chart plotting using ggplot2
ggplot(data = averages) +
# Split by day of the week and membership type, map the plot
  geom_col(mapping = aes(x = factor(StartDay, level = days_descending), y = average_duration, fill = MembershipType)) +
# Create average ride duration labels for each bar with adequate position adjustments and formatting
  geom_text(aes(x = factor(StartDay, level = days_descending), y = average_duration, label = round(average_duration, 1)), 
            position = position_dodge(width = 0.9),
            hjust = 2,
            color = "white",
            size = 3,
            face = "bold",
            family = "Tahoma"
            ) +
# Flip coordinates to convert to a horizontal bar chart
  coord_flip() + 
# Additional formatting
  xlab("Day of the week") + 
  ylab("Average ride duration (minutes)") + 
  ggtitle("Average ride duration") +
# Split plots to create separate ones for each membership type, stack them vertically
  facet_wrap(~MembershipType, ncol = 1) +
  scale_fill_brewer(palette = "Dark2") +
  theme_minimal(base_family = "Tahoma") +  # Change base font family
  theme(
    plot.title = element_text(size = 14, face = "bold"),
    axis.title.x = element_text(size = 12, face = "bold", margin = margin(t = 10)),  # Move x-axis title
    axis.title.y = element_text(size = 12, face = "bold", margin = margin(r = 10)),  # Move y-axis title
    axis.text = element_text(size = 10),
    legend.title = element_text(size = 12, face = "bold"),
    legend.text = element_text(size = 10)
  )

```
Running the code above results in creating the following chart:
![Rplot4](assets/images/Rplot4.png)

- Creating a vertical bar chart - presenting which of the offered bike types are most frequently used

```R   
# Bar chart plotting using ggplot2
ggplot(data = trip_data) +
# Split by bike type and membership type, use dodged/side-by-side bar position
  geom_bar(mapping = aes(x = BikeType, fill = MembershipType), position = position_dodge()) +
# Create count labels for each bar with adequate position adjustments and formatting
  geom_text(stat = "count", aes(x = BikeType, label = comma(..count..), group = MembershipType), 
            position = position_dodge(width = 0.9),
            vjust = 2,
            color = "white",
            size = 3,
            face = "bold",
            family = "Tahoma"
            ) +
# Additional formatting
  xlab("Bike Type") +
  ylab("Count") +
  ggtitle("Bike types vs Membership type") +
  scale_y_continuous(labels = comma) +
  scale_fill_brewer(palette = "Dark2") +
  theme_minimal(base_family = "Tahoma") +  # Change base font family
  theme(
    plot.title = element_text(size = 14, face = "bold"),
    axis.title.x = element_text(size = 12, face = "bold", margin = margin(t = 10)),  # Move x-axis title
    axis.title.y = element_text(size = 12, face = "bold", margin = margin(r = 10)),  # Move y-axis title
    axis.text = element_text(size = 10),
    legend.title = element_text(size = 12, face = "bold"),
    legend.text = element_text(size = 10)
  )

```
Running the code above results in creating the following chart:
![Rplot5](assets/images/Rplot5.png)


# Tableau Dashboard


## Results

- What does the dashboard look like and how does it operate?

![Tableau_Dashboard](assets/images/Tableau_Dashboard.gif)

It provides a clear represantation of each membership type user group and their respective recurring beahviours through these four visualizations:
  - Line chart - presenting the most popular start times between each membership type
  - Summary table/heat map - presenting seasonal activity of users from each membership type
  - Treemap - presenting the most popular routes from each membership type
  - Density map - presenting the most popular overall locations, with switching between each membership type functionality

The dashboard can also be interacted with using the following link:
[Tableau Public Dawid Wolanin](https://public.tableau.com/views/BikeRentalAnalysisDawidWolanin/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)


## Setup and Tests

### Data preparation

During the setup of Tableau worksheets, needed to create the dashboard, there were some issues discovered with the dataset that have been overlooked during data exploration and cleaning. The problem lied within geo-location columns, containing latitude and longitude values for each route's start and end points. These values were not considered up to this point in the analysis process.

The problem lied with slight, insignificant variations of latitude and longitude (by +/- 0.00001). When creating the density map, containing location points from only the most popular routes, there were more points plotted than expected. It turned out, each occurence off a single route was connected to a slightly different value, rather than the same one.

The problem was most likely a result of geo-location data collection from the users, who were starting and ending their bike rides at certain station, but their GPS location of every user was shifted from each other by a small, insignificant to our research amount.

To combat that, a small subset was created with distinct latitude and longitude values, rounded to the third decimal point and assigned to distinct routes, through the following query:

```sql
SELECT
DISTINCT RouteName,
ROUND(StartLat, 3) AS sLatitude,
ROUND(StartLng, 3) AS sLongitude,
ROUND(ReturnLat, 3) AS rLatitude,
ROUND(ReturnLng, 3) AS rLongitude

FROM
  `cyclistic-case-study-427019.Cyclistic_data_2023.df23_cleaned`

```

This subset was later downloaded as a .csv file and named 'locations_cleaned.csv'. It was then joined to the original dataset, using an INNER JOIN by the RouteName column, within the Tableau enviroment:

![Tableau6](assets/images/Tableau6.jpg)

Next, appropriate geographic data types, specific to the Tableau enviroment, were set:

![Tableau5](assets/images/Tableau5.jpg)


### Worksheet setup

With the data prepared for further visualization purposes, the following steps were taken to create the dashboard:

1. Start time line chart:
- Hour from StartTime set to Columns,
- MembershipType count measure set to Rows,
- MembershipType set to the Color Mark.

![Tableau1](assets/images/Tableau1.jpg)

2. Month heatmap table:
- StartMonth set to Columns,
- MembershipType set to Rows,
- MembershipType count measure set to the Color and Label Marks.
  
![Tableau2](assets/images/Tableau2.jpg)

3. Top 10 routes treemap:
- MembershipType count measure set to the Color and Label Marks,
- MembershipType and RouteName set to the Label mark,
- MembershipType filter applied with a toggle switch,
- Introduced index() function, used as a filter, which contains a Table Calculation, indexing the records by most popular routes, resetting when it encounters a change in membership type.

![Tableau3](assets/images/Tableau3.jpg)

4. Top location density map:
- Similar setup to previous worsheet with indexing records,
- Created calculated fields: Start Point and Return Point, aggregaring longitude and longitude values into one entity, which are then pasted onto the map on separate Marks Layers.

![Tableau7](assets/images/Tableau7.jpg)
![Tableau8](assets/images/Tableau8.jpg)
![Tableau4](assets/images/Tableau4.jpg)


# Analysis

## Key Findings

- There were over 5,7 Million trips using Cyclistic in 2023. 64% were completed by annual members, 36% by casual riders.
  
- Casual riders use the app more on the weekends, whereas the annual members take more trips during weekdays on their daily commute to work and school, especially in the central area of the city, as backed by the density map visualizations.
  
- The Cyclistic bike-sharing service is at its popularity peak during summer months. Casual riding falls off drastically during winter months.
  
- Casual riders take about twice as longer trips on average than annual members. The longest trips for casuals occur on weekend (over 30 minute on average). Members tend not to take trips longer than 15 minutes.
  
- Casual riders are more keen to try out electric bikes, members take about as many trips using regular bikes as with e-bikes.
  
- The most popular ride start times for annual members are 8 AM and 5 PM with large popularity spikes taking place. Casuals also hit their peak popularity at 5 PM, which could be the people who do not want to be stuck in traffic on their way back from work.
  
- The most popular routes for annual members start from one place and end in an another, which clearly communicates that they use the bikes to get to their workplace or other appointments located around the city center. The casuals however like to start and end their tripes at the same station, Streeter Dr & Grand Ave being a clear winner. This suggests using the bikes for leisure or maybe sightseeing, within the tourist demographic.
  
- Casual rides are more north-central based and member rides' locations shift towards southern parts of the city.

# Recommendations

## Potential Courses of Actions

### 1 - Reiteration

- There is some risk involved with moving forward with a conversion strategy from casual users to annual members.

- There may be more than meets the eye - even though the majority of bike rides came from annual members, the most popular overall route by a large margin was in the casual user's group, starting and ending at the same station.

- The data gathered was sufficient enogh to create some generalizations about each membership group, but it is not fully conclusive.

- User demographics are a key component that can have a significant impact on further development of the user base, Cyclistic's clients, and thus the company's profit.

- It might be worth considering to reiterate the data analysis process and dig deeper into why members and casuals behave the way they do to gather the required insights to inform a precise data-driven decision.

- This could involve conducting further research to better understand the motivations and behaviors of both members and casual riders. The research should provide more qualitative data from unbiased surveys, conducted through the Cyclistic app or other means.

- This would allow Cyclistic to develop more targeted and effective marketing campaigns to encourage casual riders to become members.


### 2 - Forge ahead with a conversion strategy while recognizing the potential risks of a premature marketing plan

- Create more value for current-tier annual members by including unlimited 3 hour rides instead of 45 minutes.
  
- Run location-targeted marketing campaigns in places with highest casual rider activity.

- Consider alternatives to full conversions such as new price offerings, e.g. a weekend pass: an annually billed subscription service providing an unlimited pass for every weekend.
  
- Another alternative to a full conversionL 3-month memberships for people who mostly use bike-sharing in the summer months, or an annual membership with ride times unlimited in the summer, but limited to 30 minutes in winter, spring and autumn.
  
- Run cost-optimized surveys, resulting in conveying interesting statistics that matter to casual riders, such as costs saved on fuel, calories burnt, CO2 emmisions saved. Include this additional functionality only in the annually billed membership tiers.
  
- Introduce higher priced offerings with additional privileges: bike reservations during peak season and peak hours, calculating range of electric bikes.
  
- Provide members with longer time limit for e-bikes usagem as it is the most popular bike type among casual users.


# Conclusion

- The provided data allows us to distinguish how casual riders and annual members use Cyclistic's bike-sharing service differently, but not conclusively why.
  
- A more detailed research and at least additional surveys would be recommended for more accurate strategy of conversion development. There are many assumptions stated during the analysis phase that ideally should be backed by more qualitative data about user motivations and demographics.
  
- The decision of which course of action to take depends on all of the shareholders' specific goals, resources, and risk tolerance.


