# Data Analysis- Solcast
## Overview
This project focuses on the analysis and visualization of solar energy forecasting data of Solcast obtained from Rapid API. The dataset includes several important metrics, such as Global Horizontal Irradiance (GHI), Direct Normal Irradiance (DNI), cloud opacity, and time period. The primary goal is to explore the relationship between weather conditions and solar energy production and provide insights for optimizing solar energy systems.

## Quick Links
1. [Solar Data Analysis Notebook](https://github.com/adhiraammu/Solcast_Data_Analysis/blob/main/Solar%20data-analysis-notebook.ipynb)
2. [Power Bi Dashboard](https://app.powerbi.com/view?r=eyJrIjoiNjM1Y2NjNTgtNmIwZC00MTNkLTgyNzUtZjA4YTMzNDg0NjkxIiwidCI6ImVlMmQ2ZDcyLTk1MzUtNDI0Mi1hMDc3LWFjZjE4NTc4MmY5YiIsImMiOjF9)
   
## Project Workflow
### Step 1: Exploring the Data
After retrieving the solar energy forecasting data from the Solcast API, the first step is to understand its structure and contents. This involves examining the main components of the data, particularly the keys in the JSON response.
Key Exploration: The data typically contains several keys that represent different types of information. For example, you might find a key named "forecasts" that holds a list of entries with relevant data points.
Data Types: Each entry in the list can contain various fields, such as Global Horizontal Irradiance (GHI), Direct Normal Irradiance (DNI), cloud opacity, and timestamps. Understanding the types of data within these fields helps determine how they can be analyzed and visualized later.

### Step 2: Data Cleaning
Once the data structure is understood, the next step is data cleaning. This process is essential to ensure the dataset is accurate, consistent, and ready for analysis. Here are some key aspects of the data cleaning process:
Column Renaming: To enhance clarity, columns are renamed to more descriptive titles. For instance, "ghi" might be changed to "Global Horizontal Irradiance," making it easier for users to understand what each column represents.
Handling Missing Values: It’s important to identify and address any missing or null values in the dataset. Depending on the context, missing data can be removed or filled in using appropriate methods, such as interpolation or substitution based on the average.
Data Type Conversion: Ensuring that each column has the correct data type is crucial for analysis. For instance, timestamps should be converted to a date-time format, while numerical fields need to be in integer or float format for calculations.
Outlier Detection: Outliers can skew analysis results, so identifying and assessing them is vital. Depending on the nature of the data, outliers may be removed or treated to prevent them from affecting the overall insights derived from the dataset.

### Step 3: Connecting Pandas to SQL
To store the cleaned data, a database needs to be created. This involves using SQL to create a new database and connecting to it via SQLAlchemy:
  1. Create a Database
  2. Install Required Library: If not already installed, you can install the required library using
  3. Connect to the Database

### Step 4: SQL Queries for Data Filtering
Once the data is stored in the SQL database, various SQL queries can be executed to filter the data based on specific criteria. This filtering is important to focus on periods of significant solar energy production.
Filtering for Sufficient GHI Values: Create a new table that contains only the entries where the Global Horizontal Irradiance is above 50, which is generally sufficient for solar energy production.

```sql
Copy code
CREATE TABLE ghi_50_SE_production AS 
SELECT * FROM mytable 
WHERE Global Horizontal Irradiance > 50;
```

Filtering for Daylight Hours: Create a new table with data from times when solar energy production is highest, specifically between 9:00 AM and 4:00 PM.
```sql
Copy code
DROP TABLE IF EXISTS time_with_max_sunlight; 
CREATE TABLE time_with_max_sunlight AS 
SELECT * FROM mytable 
WHERE TIME(period_end) BETWEEN '09:00:00' AND '16:00:00';
```
![image](https://github.com/user-attachments/assets/c0712fa2-0d87-4b87-b639-6d933c0291ea)

Fig: Data with Max sunlight

Filtering for Low Cloud Opacity: Create a new table to identify times with low cloud opacity, which can help optimize energy production conditions.

```sql
Copy code
DROP TABLE IF EXISTS low_cloud_opacity; 
CREATE TABLE low_cloud_opacity AS 
SELECT * FROM mytable 
WHERE cloud_opacity < 50;
```
![image](https://github.com/user-attachments/assets/6bcaa878-7c7e-4d0c-8142-ad48d0781122)

Fig: Filtering with low cloud opacity

### Step 5: Data Analysis and Visualization
After successfully filtering the data, the filtered data can then be imported into Power BI for visual exploration, where various charts and graphs will be created to illustrate the insights gained.

### Power BI Visualization

![image](https://github.com/user-attachments/assets/30483e06-84e1-4f5f-9f3e-94a1012038af)

Line Graph
Filtered GHI values greater than 50 are plotted in a line graph for a 7-day period, alongside other factors like EHI, GHI 90, and GHI 10.
Definitions:
EHI: Extraterrestrial Horizontal Irradiance, the maximum amount of solar energy reaching the surface without atmospheric interference.
GHI 90: The amount of solar radiation reaching a panel oriented at a 90-degree angle to the ground.
Higher EHI values indicate more available solar energy, but it must still be captured by solar panels. The zig-zag pattern in the line chart could be due to fluctuations in solar irradiance levels. GHI 90 being consistently higher suggests that panels tilted at 90 degrees receive more sunlight than those at 10 degrees, although this doesn't necessarily make GHI 90 the best indicator for solar production.

Bar Graph
A bar chart displaying the average of DHI, DNI, and GHI for each day from March 12 to 17 can provide insights into solar energy production during those days.
Interpreting the Bar Chart:
Height of Bars: Represents average solar energy production, with taller bars indicating higher production.
Individual Components: Examine which component (DHI, DNI, GHI) contributed more to total energy production on specific days.
Trends Over Time: Identify patterns in solar energy production related to weather conditions, cloud cover, or time of day.
Overall, visualizations in Power BI can help solar companies understand the amount and variability of solar energy production during a specific time period, which can inform decision-making.

### Conclusion
This project provides valuable insights into the relationship between weather conditions and solar energy production. The findings can assist in optimizing the design and operation of solar energy systems and serve as a basis for further research and the development of more accurate solar energy forecasting models.


